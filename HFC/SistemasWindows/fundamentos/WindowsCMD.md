[<- Índice](../SistemasWindows.md)
# Windows Command Line (CMD)

> Aunque potencialmente obsoleto, el ***CMD*** de *Windows* sigue siendo una forma efectiva y simple de interactuar con el sistema operativo, además de que para dispositivos más antiguos podría ser la única forma de interacción.

Es de alguna manera similar a los *Shells* de Linux y los comandos principales son:

##### Básicos

Los comandos básicos para el uso del ***CMD*** son:

- `cd`: Permite moverte de directorio.
- `dir`: Permite listar el contenido de una carpeta.
- `tree`: Otra manera de listar el contenido de carpetas
- `ver`: Muestra la versión del sistema operativo.
- `systeminfo`: Brinda información sobre el dispositivo, el sistema operativo y demás.
- `type`: Muestra el contenido de un archivo en pantalla
- `copy`: Copia un archivo
- `del`: Borra un archivo
- `mkdir`: Crea una carpeta
- `rmdir`: Borra una carpeta
- `more`: Muestra por partes el contenido de un archivo
- `set`: Lista las variables de entorno de la *shell*
- `where`: Busca archivos por su nombre
- `find`: Similar a `grep`, busca patrones de texto en archivos.
- `echo`: Repite el argumento recibido
- `cls`: Limpia la pantalla.

```bat
type user.txt

# Busca flag.txt en todo el disco
where /R C:\ flag.txt

set

echo "Hola"
```

##### Red

Los siguientes comandos pueden brindar valiosa información del estado del dispositivo en la red y demás datos útiles.

- `netstat`: Brinda información sobre las conexiones y sesiones de red activas en el dispositivo.
- `ipconfig`: Brinda información y configuraciones de red
- `nslookup`: Resuelve nombres de dominio a direcciones *IP*

##### Procesos

El principal comando que muestra información sobre los procesos en el sistema es `tasklist`. Su uso puede resultar extraño al inicio, pero toda la información necesaria se encuentra con la bandera `/?`.

Si se necesita matar procesos, existe el comando `taskkill /PID [PID]`. Basta con concer el *PID* del proceso para dar el comando anterior y detener su ejecución.

```bat
tasklist

tasklist /FI "imagename eq sshd.exe"

taskkill /PID 4752
```

---

Sobre todo, el comando `help` es capaz de listar la **mayoría** de los comandos disponibles, y si se le indica uno, despliega las opciones disponibles de tal comando.

```bat
help

help find
```

Al iniciar a aprender sobre una nueva *shell*, sin duda es importante la documentación de las opciones disponibles así que es crucial conocer las banderas u opciones de ayuda.

### Banderas

> En este tipo de terminal, las banderas **mayormente** se indican como **/x** donde **x** varía según la opcion que deseemos.

Por ejemplo, para obtener aun más información de `ipconfig` podemos indicar la bandera `/all`.

La bandera para desplegar información de ayuda, suele ser `/?`. De este modo podemos investigar que banderas nos sirven según nuestras necesidades.

Sin embargo, según el comando, puede variar pues algunos reciben banderas en formato *Unix* como `netstat` o de alguna otra manera.

```bat
# Información extra sobre la configuración de la red
ipconfig /all

# Listado de archivos ocultos
dir /a

netstat -ab
```

### Redirecciones

> ***CMD*** también soporta las redirecciones típicas de *Unix*, para salida estandar, entrada estándar, salida de error y el tan valioso **pipe**.

No me entretendre mucho en esta sección pues las redirecciones son completamente iguales, sin embargo puedo mencionar que existe la palabra clave `nul` que se comporta como `/dev/null` de los sistemas *Unix*, es decir, es un vacío de información para los datos que no deseamos.

```bat
echo "Hola Mundo" > saludo.txt
echo "Adios" >> saludo.txt

dir /s C:\Windows > nul

more < flag.txt

type C:\Windows 2> nul

type prueba.txt | find "FLAG"
```

### Variables

### Scripting

# Enlaces

[<- Seguridad](Seguridad.md) |