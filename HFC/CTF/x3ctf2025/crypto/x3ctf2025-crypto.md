[<- Volver](../x3ctf2025.md)
# Desafios de categoría `crypto`

### `sourceless-crypto`

> Este reto consistía en descifrar la encriptación que ejercía un programa sobre sus entradas, con el objetivo de desencriptar la bandera cifrada que este nos proporcionaba.

El *Script*, alojado como un servicio en un servidor, permitía visualizar la bandera encriptada al seleccionar la opción 1, y encriptar cualquier entrada mediante la opción 2.

```shell-session
Welcome to sourceless-crypto, enjoy the pain
1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 1
Flag: b"\\F^itur%) /,B,'|0_p2pvd5emxyRo}wAD\x03m\x04\x05RPf\x00\x0b\x0b\rc\x0c\x0e\x19\x19\x17C\x14\x17\x1fEM\x1bV"

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 2
Enter plaintext: Hola
Encrypted plaintext: b'bB@N'

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 3
```

La dificultad, como el nombre indica, reside en que no poseemos el código fuente del programa ni ningún binario que analizar.
Entonces, nos vemos forzados a realizar pruebas exhaustivas con la interfaz del programa, analizando las salidas que obtenemos a partir de las entradas.

Tras mucha experimentación se notan algunas cosas interesantes.

- Cada encriptación, aunque reciba la misma entrada (digamos `a`) devuelve una salida distinta.
- Introducir varias veces el caracter `a`, por ejemplo `aaaaa`, devuelve la concatenación de los caracteres que devuelve al introducir `a` por si solo 5 veces.
- Al resetear el programa (saliendo y entrando), se resetea tambien la secuencia de caracteres que recibe una misma entrada.
- Con la suficiente atención y experimentación, es posible deducir el caracter de salida para el primer caracter de entrada, mediante una operación *XOR* con `0x2a` (`*`).
- El segundo caracter se obtiene a partir de de realizar un *XOR* con `0x2d` (`-`), el tercero con `0x2c` (`,`), etc.

```shell-session
Welcome to sourceless-crypto, enjoy the pain
1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 2
Enter plaintext: a
Encrypted plaintext: b'K'

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 2
Enter plaintext: a
Encrypted plaintext: b'L'

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 2
Enter plaintext: a
Encrypted plaintext: b'M'

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 3
```

```shell-session
Welcome to sourceless-crypto, enjoy the pain
1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 2
Enter plaintext: aaa
Encrypted plaintext: b'KLM'

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 3
```

```python
print(chr(ord('a') ^ 0x2a)
# 'K'

print(chr(ord('a') ^ 0x2d))
# 'L'
```

##### Mi solución

Con lo que ya sabemos, podemos obtener la "llave" con la que se esta encriptando esto pues la operación *XOR* es revertible. Para esto, debemos realizar un *XOR* entre la entrada que le damos al programa y el texto cifrado que devuelve.
De preferencia, con una cadena grande para visualizar de mejor manera esta llave.

```shell-session
Welcome to sourceless-crypto, enjoy the pain
1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 2
Enter plaintext: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Encrypted plaintext: b'KLMNO0123456789:;<=>? !"#$%&\'()*+,-./\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea'

1 -> Show Flag
2 -> Encrypt Plaintext
3 -> exit
Operation: 3
```

```python
from pwn import xor ; print(xor(b'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',b'KLMNO0123456789:;<=>? !"#$%&\'()*+,-./\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea'))

# b'*-,/.QPSRUTWVYX[Z]\\_^A@CBEDGFIHKJMLONqpsrutwvyx{z}|\x7f~a`cbedgfihkjmlon\x91\x90\x93\x92\x95\x94\x97\x96\x99\x98\x9b\x9a\x9d\x9c\x9f\x9e\x81\x80\x83\x82\x85\x84\x87\x86\x89\x88\x8b'
```

Si analizamos el valor hexadecimal de estos caracteres, podemos notar un secuencia similar a `2a 2d 2c 2f 2e 51 50 53 52 55 54 57 56 59 58 5b 5a 5c 5d 5f 5e 41 40 ...`.
En ésta, destaca un patron `1 0 3 2 5 4 7 6 9 8 b a d c f e` por el que iteran las unidades y luego las "decenas".

Además, la llave siempre inicia por `*` (`0x2a`) sin importar que entrada le demos, lo cual no debería ser el inicio de la llave, pues como vimos, el patrón inicia con `1`.
Entonces, con esto y con el hecho de que la llave se va "recorriendo" cada vez que cifras, surge una **idea**: ¿Y si utilizaron el inicio de la llave para cifrar la bandera?

Con esta idea en mente, podemos realizar un *script* que recree la llave y utilice los primeros caracteres para realizar un *XOR* con la bandera cifrada, de modo que podamos descifrarla por la propiedad reversible de esta operación.

```bash
#!/usr/bin/env python

# x3CTF 2025
# Sourceless-Crypto: https://github.com/x3ctf/challenges-2025/tree/main/crypto/sourceless-crypto

"""
My Solution
"""

# ------------------
# Ookami
# Hackers Fight Club
# ------------------

# IMPORTS
from pwn import xor

# Retrieved encrypted flag
encrypted_flag = b"\\F^itur%) /,B,'|0_p2pvd5emxyRo}wAD\x03m\x04\x05RPf\x00\x0b\x0b\rc\x0c\x0e\x19\x19\x17C\x14\x17\x1fEM\x1bV"

# Rechognized pattern
pattern=['1','0','3','2','5','4','7','6','9','8','b','a','d','c','f','e']

# Key array creation
key=[]
for first in pattern:
    for last in pattern:
       key.append(int(f'{first}{last}',16))

# Output flag
print(f'Decrypted Flag: {xor(encrypted_flag,key[:len(encrypted_flag)]).decode()}')
```

##### Solución oficial

La **solución oficial** a este reto, consistía en identificar la presencia de un ***nonce*** (*Number used once*), que es un número aleatorio o secuencial con el que se realiza un *XOR* a cada caracter de la contraseña, aunado al respectivo proceso de encriptación.
De este modo, aunque se conozca alguna técnica de descifrado, sin concer el ***nonce*** resultaría bastante complejo obtener el mensaje original.

En este reto, se utiliza un ***nonce*** secuencial, que aumenta en 1 cada vez que se utiliza.
Sabiendo esto, podemos revertir el ***nonce*** de cada caracter de la bandera y de los caracteres que posteriormente encriptemos con el servicio.
Analizando el cifrado sin ***nonce***, puede descubrirse que se trata de un algoritmo de cifrado por **sustitución**, de modo que solo hay que mapear todos los caracteres imprimibles con sus respectivas representaciones cifradas **sin nonce**.
Ya con esto, podemos utilizar dicho diccionario para descifrar la bandera **sin nonce**.

Siguiendo esta idea y haciendo una pequeña demostración de la librería `pwn`, podemos obtener la bandera con un *script* similar al siguiente:

```python
#!/usr/bin/env python

# x3CTF 2025
# Sourceless-Crypto: https://github.com/x3ctf/challenges-2025/tree/main/crypto/sourceless-crypto

"""
Official solution
"""

# ------------------
# Ookami
# Hackers Fight Club
# ------------------

# IMPORTS
from pwn import remote,context
from argparse import ArgumentParser
from string import printable

# Parser configuration
parser = ArgumentParser(description="Solución al reto Sourceless-Crypto de x3CTF 2025")
parser.add_argument("host", type=str, help="Dominio del host objetivo")
parser.add_argument("puerto", type=int, help="Puerto del host que aloja el reto")
parser.add_argument("-v", dest='verbosity', action='store_true')

# Parse Arguments
args = parser.parse_args()

if args.verbosity:
    context.log_level = 'debug'


#
# Starts connection
#
r = remote(args.host,args.puerto,ssl=True)

# Receive flag
r.sendlineafter(b'Operation: ',b'1')
encrypted_flag = r.recvline() 

# Send all printable chars and receive encrypted chars
r.sendlineafter(b'Operation: ',b'2')
r.sendlineafter(b'plaintext: ', printable.encode())
encrypted_printable = r.recvline()

# Finalize connection
r.close()
print()

# Format received data
encrypted_flag = eval(encrypted_flag.replace(b'Flag: ',b''))
encrypted_printable = eval(encrypted_printable.replace(b'Encrypted plaintext: ',b''))

# Variables
denonced_flag=b''
denonced_printable=b''

# Initialize nonce
nonce=0

# Undo the nonce XOR for the flag
for c in encrypted_flag:
    denonced_flag += (c ^ nonce).to_bytes(1)
    nonce += 1

# Undo the nonce XOR por printable chars
for c in encrypted_printable:
    denonced_printable += (c ^ nonce).to_bytes(1)
    nonce+=1

dictionary={chr(e):c for c,e in zip(printable,denonced_printable)}

# Print information
if args.verbosity:
    print('Created dictionary (Encrypted:Decrypted): ')
    print(dictionary)
    print()
print(f'Retrieved encrypted flag: {encrypted_flag}')
print()
print(f'Removed nonce flag: {denonced_flag}')
print()

#
# Decrypt flag with dictionary
#
flag=''
for c in denonced_flag:
    flag += dictionary[chr(c)]

print(f'Decrypted flag: {flag}')
```

# Enlaces

[<- rev](x3ctf2025-rev.md) |