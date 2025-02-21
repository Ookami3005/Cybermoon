[<- Volver](SistemasUNIX.md)
## ¿Qué es una función `hash`?

Es un algoritmo matemático que transofroma cualquier bloque arbitario de datos en una nueva serie de caracteres con una longitud fija. Independientemente de la longitud de los datos de entrada, el valor *hash*.

## ¿Como cifra UNIX las contraseñas de `/etc/shadow`?

Al inicio de la contraseña *hasheada*, se indica entre simbolos \$ cual es el algoritmo de cifrado seguro utilizado.

\$1\$=*MD5*
\$2\$=*blowfish*
\$5\$=*SHA-256*
\$6\$=*SHA-512*

### Algoritmos

Los algoritmos *hash* toman un bloque arbitario de datos y devuelve una cadena con determinada longitud (*valor hash*).
