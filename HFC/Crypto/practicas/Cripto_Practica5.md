# Práctica 5
## Criptografía y seguridad 2025-2

> Fernando Romero Cruz - *319314256*

### Reporte

#### Preparativos

El primer desafío fue conseguir que el código muestra proporcionado se ejecutara y preparar la base de datos local para recibir la conexión de nuestro *Script*.

Por una parte, preparé las dependencias con apoyo de un entorno virtual gestionado con la utilidad `pipenv`.
Basto con instalar las dependencias ejecutando el siguiente comando en mi carpeta de trabajo de la práctica:

```bash
pipenv install pycryptodome pycryptodomex mysqlclient securestring
```

Una vez preparado el ambiente virtual, podemos activarlo con:

```bash
pipenv shell
```

O, podemos simplemente ejecutar un comando dentro del entorno con `pipenv run [comando]`, de modo que podemos indicarle un *Shebang* a nuestro script con el intérprete de python y brindarle permisos de ejecución para poder ejecutarlo más comodamente.
Es decir, una vez que la primera línea de nuestro *script* sea `#!/usr/bin/env python3` podemos ejecutar los siguientes comandos:

```bash
# Para brindar permisos de ejecución (solo se ejecuta una vez)
chmod +x encrypt.py

# De este modo se ejecuta el script cuando se desee
pipenv run ./encrypt.py
```

Una vez que se ejecuta, presentará un error a la hora de intentar almacenar nuestros datos en la base de datos, ya que ésta última no está presente aún.

Para configurarla, en mi caso instale el paquete `mariadb`, correspondiente a `mariadb-server` en otras distros *Linux*.
Por defecto, únicamente el usuario `root` tiene acceso, pero utilizar continuamente este usuario no es recomandable, de modo que creé un usuario especifica y únicamente para que se conecte y administre la base de datos relevante a esta práctica.

Claro, primero hay que crear la nueva base de datos con su respectiva tabla con apoyo del archivo provisto `hospital_schema.sql`.

```sql
CREATE DATABASE IF NOT EXISTS hospital;
USE hospital;
CREATE TABLE IF NOT EXISTS expediente (
  id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre varchar(255) NOT NULL,
  diagnostico varchar(450) NOT NULL,
  tratamiento varchar(450) NOT NULL,
  passwordSalt varchar(25) NOT NULL,
  diag_nonce varchar(12) NOT NULL,
  treat_nonce varchar(12) NOT NULL
```

Este archivo crea una base de datos `hospital` con una tabla `expediente` y define el *schema* de ésta última y podemos ejecutarlo con los siguientes comandos:

```bash
sudo mariadb < hospital_schema.sql
```

Una vez creada, despliegué la línea de comandos de *MariaDb* con el comando `sudo mariadb`, y ejecuté los siguientes comandos de sql para configurar un usuario `tonah` con los mínimos privilegios y con una contraseña censurada:

```sql
CREATE USER 'tonah'@'localhost' IDENTIFIED BY '[PASSWORD_REDACTED]';
GRANT ALL PRIVILEGES ON hospital.* TO 'tonah'@'localhost';
FLUSH PRIVILEGES;
```

Finalmente, para que el *script* pueda ejecutarse, creé un archivo `config.py` donde definí únicamente 3 variables que importa el *script* `encrypt.py` para su funcionamiento:

```python
user = 'tonah'
password = '[PASSWORD_REDACTED]'
dbname = 'hospital'
```

Con esto último, ya nuestro *script* debería ejecutarse sin problemas, conectarse a la abse de datos y almacenar la entrada indicada en el código.

#### Desarrollo

Ya con todo en su lugar, procedí a realizar las modificaciones solicitadas en la práctica.
Primero, definí una función `parse_json` que precisamente procesa el archivo *JSON* provisto `diagnosticos_tratamientos.txt` de la siguiente manera:

```python
def parse_json():
    with open('./diagnosticos_tratamientos.txt','r') as reader:
        data = reader.read()
        patients = []
        try:
            pacientes = json.loads(data)
            for _, info in pacientes.items():
                patients.append((info['name'],info['diagnosis'],info['treatment']))
        except json.JSONDecodeError:
            print("Error: El JSON proporcionado no es válido.")
            quit()
        except KeyError as e:
            print(f"Error: Falta la clave esperada en el JSON - {e}")
            quit()

        return patients
```

Tuve que arreglar un poquito el formato del archivo provisto para que pudiera procesarse ya que todas las claves debían estar entre comillas. Me parece que puede realizarse con `sed` de la siguiente manera:

```bash
sed -E 's/([0-9]+)/"\1"/g' diagnosticos_tratamientos.txt > temp
sed -e "s/'/\"/g" temp > diagnosticos_tratamientos.txt ; rm temp
```

Ahora, reorganicé el código principal del *Script* en 2 funciones principales, una para encriptar la información con el modo de operación **CBT** específicado y otra para conectarse a la base de datos y almacenar la información ya encriptada.
Realicé algunas optimizaciones sobre la conexión a la base de datos para que inserte todos los valores antes de cerrar la conexión, ademas de reducir las rondas de cifrado.

```python
def store_encrypted_data(data):
    mydb = None
    cursor = None

    # Guardar los datos en una base de datos relacional
    try:
        # Leemos las credenciales para la conexión del archivo config.py
        # El cifrado de la conexión se realizará en otra práctica
        mydb = MySQLdb.connect(user=config.user, password=config.password, database=config.dbname)
        cursor = mydb.cursor()

        insert_query = """ INSERT INTO expediente (nombre, diagnostico, tratamiento, passwordSalt, diag_nonce, treat_nonce) 
						   VALUES (%s,%s,%s,%s,%s,%s)"""

        cursor.executemany(insert_query,data)
        mydb.commit()
        print("Registros insertados exitosamente!")

    except Exception as err:
        print(f"\nSomething went wrong: {err}")
        sys.exit()

    finally:
        if mydb:
            if cursor:
                cursor.close()
            mydb.close()


def serialize():

    global password

    key=None
    patients = parse_json()
    encrypted_info = []
    for name,diagnosis,treatment in patients:

        # El algoritmo de derivación de llaves PBKDF2 necesita una salt,
        # por lo que generamos una secuencia pseudoaleatoria de 16 bytes. 
        passwordSalt = get_random_bytes(16)

        # En esta práctica emplearemos AES-256, por lo que necesitamos
        # que el algoritmo de derivación de llaves PBKDF2 nos proporcione
        # una llave 256 bits (32 bytes).
        key = PBKDF2(password, passwordSalt, 32, count=100, hmac_hash_module=SHA512)

        # Para el modo de operación CTR, se forma lo que se conoce como "Counter block"
        # de longitud 128 bits, y se compone de un "nonce" (number-used-once; valor aleatorio
        # de 64 bits) y un contador (que de manera predeterminada comienza en cero)
        diagnosis_nonce = get_random_bytes(8)
        treatment_nonce = get_random_bytes(8)
        diag_aes = AES.new(key, AES.MODE_CTR, nonce=diagnosis_nonce)
        treat_aes = AES.new(key, AES.MODE_CTR, nonce=treatment_nonce)

        # Cifrado de los campos considerados sensibles
        diagnosis_ciphertext = diag_aes.encrypt(diagnosis.encode())
        treatment_ciphertext = treat_aes.encrypt(treatment.encode())

        # Se codifica en base 64 tanto la passwordSalt como el 
        # criptograma para guardarlos en la base de datos
        passwordSalt = b64encode(passwordSalt)
        diagnosis_ciphertext = b64encode(diagnosis_ciphertext)
        treatment_ciphertext = b64encode(treatment_ciphertext)
        diagnosis_nonce = b64encode(diagnosis_nonce)
        treatment_nonce = b64encode(treatment_nonce)

        encrypted_info.append((name, diagnosis_ciphertext, treatment_ciphertext, passwordSalt, diagnosis_nonce, treatment_nonce))

        # Sobrescribir el contenido de las variables para evitar que se
        # puedan obtener los datos a través de un volcado de memoria RAM
        clearmem(diagnosis)
        clearmem(treatment)

    clearmem(password)
    clearmem(key)
    store_encrypted_data(encrypted_info)
```