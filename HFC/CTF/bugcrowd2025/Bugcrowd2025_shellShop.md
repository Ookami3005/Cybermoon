# Bugcrowd Student Qualifier | Shell Shop Writeup

<p  align="center">
  <img  width="200"  src="https://media.licdn.com/dms/image/v2/D4E0BAQErtaHuzN8ehw/company-logo_200_200/company-logo_200_200/0/1719282098472?e=2147483647&v=beta&t=RXd3JJN8OVl21DW-MFoiXMUfpUsX18inBRQ7MknrthY"  alt="">
</p>

> **Autor**: *Ookami*

### Descripción

> **Categoría**: *Pwn*
> **Dificultad**: Media

Para este reto, se nos brindó un **comprimido** `pwn_shellstore.zip` del **ejecutable principal** `shell_shop`, acompañado de las librerías necesarias en un directorio `glibc`.
La idea es analizar estos recursos para preparar un `exploit` contra este binario como servicio, en un contenedor `docker` que desplegaba la página del *CTF*.

En cuanto al **ejecutable**, este despliega un menú básico para la compra de un par de productos fantasiosos y una opción para terminar el programa, no sin antes preguntar si deseas ser notificado cuando la tienda reaparezca.

![bugcrowd2_pwn_shellStore.png](imagenes/bugcrowd2_pwn_shellStore.png)

### Análisis

##### Manual

Antes de profundizar en el **análisis**, me dispuse a interactuar y probar a mano las distintas opciones en el menú del programa.

Particularmente, hubo un comportamiento que llamó mi atención. Con el saldo inicial de **1337** monedas, únicamente se puede comprar el objeto `X0 Armor`.
Tras comprar este objeto y optar por salir de la tienda, el programa nos ofrece un **"código de descuento"** distinto en cada ejecución:

![bugcrowd2025_pwnShellStore_discount.png](imagenes/bugcrowd2025_pwnShellStore_discount.png)

Este *"código"* parece más una dirección de memoria, por el formato **hexadecimal**.
Además, el hecho de que siempre inicie con los dígitos `7f` sugiere que se trata de una dirección en la sección de la `pila` en memoria.

##### Checksec

Por otra parte, utilicé la herramienta [checksec](https://github.com/slimm609/checksec) para identificar medidas de seguridad activas en el binario.

En especial, nos interesan los mecanismos más importantes que son:

- `Stack Canary`: Detección de **corrupción de datos** de la `pila`.

- `NX`: Bloqueo de **ejecución** de código en la `pila`.

- `PIE`: Aleatorización de las **direcciones de memoria** del ejecutable.

![bugcrowd2025_pwn_checksec.png](imagenes/bugcrowd2025_pwn_checksec.png)

Por los resultados, notamos que `Stack Canary` y `NX` estan **desactivados**, lo que quiere decir que el binario **podría** ser susceptible a ataques de tipo `Buffer Overflow`.

Sin embargo, `PIE` sí se encuentra **activo** lo que hace impredecible las direcciones de memoria, dificultando en cierta medida la `explotación`.

##### Análisis avanzado

Para un **análisis** más **avanzado** del binario, utilicé principalmente [Radare2](https://github.com/radareorg/radare2) con el *plugin* [R2Ghidra](https://github.com/radareorg/r2ghidra) y el `depurador` **GDB** acompañado de la extensión [PwnDBG](https://github.com/pwndbg/pwndbg).

###### Decompilado y desensamblado

Para empezar, obtuve y revisé el `desensamblado` y `decompilado` de varias **funciones** del binario con `radare2`.
Mi objetivo era identificar cualquier **vulnerabilidad** en el código y esclarecer el comportamiento del código de descuento.

Tras un tiempo, encontré esta sección interesante del código **decompilado** de la función `main` que responde ambas interrogantes:

![bugcrowd2025_pwn_vulns.png](imagenes/bugcrowd2025_pwn_vulns.png)

Para el código de descuento, podemos ver que imprime la dirección `&stack0xffffffffffffffc8` en formato de `apuntador` (`%p`).

Esta sintaxis de **Ghidra** se refiere a un desplazamiento de `0xffffffffffffffc8`, que representa `-56` bajo el **complemento a 2**, de una dirección base `&stack` relacionada a una sección de la `pila` de esta función.
Así confirmamos que el código de descuento es una dirección **específica** de la `pila`.

Después, hay una **vulnerabilidad** cuando se pregunta al usuario si desea ser notificado, debido a 2 razones:

1. El programa escribe la respuesta **directamente** en la pila, en la dirección `&stack0xffffffffffffffc6`.
2. Aunque solo se requiere un caracter en la respuesta, la función `fgets` permite la entrada de hasta **100** caracteres.

Esto abre completamente la posibilidad a un ataque de `Buffer Overflow` si logramos sobreescribir la dirección de retorno almacenada en la `pila`, y redirigir el flujo de ejecución a la misma `pila`.

Pero primero, dada la restricción del `PIE`, debemos determinar una **dirección de retorno** específica a la sección de la `pila` que podemos sobreescribir.

Antes se mencionó que el programa escribe nuestra respuesta en `&stack0xffffffffffffffc6` que, bajo la misma sintaxis del ejemplo anterior, representa la dirección `&stack` con un desplazamiento de `-58`.
Es decir, 2 `bytes` antes que cualquier dirección otorgada como código de descuento!

O sea que, a pesar de la **aleatorización** de las direcciones, siempre sabremos que la dirección filtrada como código de descuento se ubica 2 `bytes` después de donde se comienza a almacenar nuestra respuesta.

Finalmente, solo resta determinar si podemos sobreescribir la dirección de retorno de esta función, y en caso afirmativo, identificar el `offset` necesario para sobreescribirla.

###### Depuración

Para esta tarea, me apoyé de **PwnDBG**, primero generando una cadena de patrón ciclico con una longitud adecuada mediante el comando:

```gdb
pwndbg> cyclic 100
```

Después copié esta cadena y ejecute el binario con el siguiente comando:

```gdb
pwndbg> run
```

Tras solicitar terminar el programa, introduje dicha cadena como respuesta a si deseo ser notificado, obteniendo un **Segmentation Fault**.

![bugcrowd2025_pwn_sigsev.png](imagenes/bugcrowd2025_pwn_sigsev.png)

Esto siempre es buena señal, y con apoyo del `depurador` confirmé que es debido a corrupción en la dirección de retorno, pues el programa intentó retornar a una dirección específica:

![bugcrowd2025_pwn_ret.png](imagenes/bugcrowd2025_pwn_ret.png)

Esta dirección inexistente, es en realidad un fragmento de nuestra cadena de patrón ciclico, entonces confirmamos que es posible sobreescribir la dirección de retorno y podemos obtener el `offset` mediante el siguiente comando en **PwnDBG**:

```
pwndbg> cyclic -l 0x6169616161616161

Finding cyclic pattern of 8 bytes: b'aaaaaaia' (hex: 0x6161616161616961)
Found at offset 58
```

###### Conclusiones

Tras todo el análisis, repasemos la información que recopilamos de este binario y que será clave para el desarrollo del `exploit`. 

- Se aleatorizan las direcciones de memoria, pero no hay ningun mecanismo de protección en la `pila`.
- Filtra una dirección de memoria específica.
- Es susceptible a `Buffer Overflow` cuando pregunta si deseas ser notificado.
- La respuesta a esta pregunta comienza su escritura 2 `bytes` antes de la dirección filtrada.
- Tras los primeros 58 `bytes` escritos, los siguientes sobreescriben la dirección de retorno de la función `main`.

### Explotación

## Referencias