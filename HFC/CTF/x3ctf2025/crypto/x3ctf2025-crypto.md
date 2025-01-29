[<- Volver](../x3ctf2025.md)
# Desafios de categoría `crypto`

### `sourceless-crypto`

> Este reto consistía en descifrar la encriptación que ejercía un programa sobre sus entradas, con el objetivo de descifrar la bandera cifrada que este nos proporcionaba.

El binario, alojado como un servicio en un servidor, permitía visualizar la bandera encriptada al seleccionar la opción 1, y encriptar cualquier entrada mediante la opción 2.

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
- Al resetear el programa (saliendo y entrando), se resetean tambien la secuencia de caracteres que recibe una misma entrada.
- Con la suficiente atención, es posible deducir el caracter de salida para el primer caracter de entrada, mediante una operación *XOR* con `0x2a`.