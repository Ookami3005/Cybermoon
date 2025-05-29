# Práctica 1 - Compiladores 2025-2

1. Deberá tener instalado el compilador _gcc_ y trabajar en un ámbiente _Linux_.
2. Escriba el siguiente programa en lenguaje **_C_** (sin copiar y pegar) y nómbrelo *programa.c*
 
```c
#include <stdio .h>
#include <stdlib.h>
//# define PI 3.1415926535897

# ifdef PI
# define area(r)(PI * r * r)
# else
# define area(r)(3.1416 * r * r)
# endif


/**
* Compiladores 2025-2
*
*/
int main ( void ) {
printf ("Hola Mundo !\n"); //Función para imprimir hola mundo
float mi_area = area(3) ; //soy un comentario... hasta donde llegaré ?
printf ("Resultado : %f\n", mi_area);
return 0;
}
```

3. Use el siguiente comando: `cpp programa.c programa.i`.

Revise el contenido de _programa.i_ y conteste lo siguiente:

- ¿Qué ocurre cuando se invoca el comando cpp con esos argumentos? **Crea un archivo `programa.i` con más código en C.**
-  ¿Qué similitudes encuentra entre los archivos programa.c y programa.i? **En la última parte de `programa.i` hay un fragmento de código muy similar a `programa.c` pero con algunos detalles distintos.**
- ¿Qué pasa con las macros y los comentarios del código fuente original en programa.i? **Los macros y comentarios del programa fuente original son desarrollados y retirados respectivamente, en esta nueva versión.**
- Compare el contenido de programa.i con el de stdio.h e indique de forma general las similitudes entre ambos archivos. **Me parece que todo el contenido de `stdio.h` se encuentra en `programa.i`, introducido al inicio del código obtenido.
-  ¿A qué etapa corresponde este proceso? **Corresponde a la etapa de ==preprocesamiento==.**

4. Ejecute la siguiente instrucción: ``gcc -Wall -S programa.i``

 - ¿Para qué sirve la opción -Wall? **Activa las advertencias más importantes al momento de compilar.**
- ¿Qué le indica a gcc la opción -S? **Que se detenga antes de ensamblar el programa y genere un archivo en código en ensamblador.**
- ¿Qué contiene el archivo de salida y cuál es su extensión? **El archivo de salida `programa.s` contiene código ensamblador, la traducción del programa fuente.**
- ¿A qué etapa corresponde este comando? **Es la fase de compilación.**

5. Ejecute la siguiente instrucción: `as programa.s -o programa.o`

- Antes de revisarlo, indique cuál es su hipótesis sobre lo que debe contener el archivo con extensión  <i>.o</i>. **Debe contener código máquina ensamblado a partir del código en ensamblador que vimos anteriormente**
- Diga de forma general qué contiene el archivo <i>programa.o</i> y por qué se visualiza de esa manera. **Si contiene código máquina, no es legible porque utiliza un rango más amplio de caracteres que solamente los legibles, pues es código binario.**
- ¿Qué programa se invoca con *as*? **Se invoca el ensamblador de GNU.**
- ¿A qué etapa corresponde la llamada a este programa? **Este programa realiza la fase de ensamblaje.**

6. Encuentre la ruta de los siguientes archivos en el equipo de trabajo:

* *ld-linux-x86-64.so.2* : Se encuentra en `/usr/lib/ld-linux-x86-64.so.2`
* *Scrt1.o* (o bien, *crt1.o*) : `/usr/lib/ld-linux-x86-64.so.2`
* *crti.o* : `/usr/lib/crti.o`
* *crtbeginS.o* : `/usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/crtbeginS.o`
* *crtendS.o* :  `/usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/crtendS.o`
* *crtn.o* : `/usr/lib/crtn.o`

7. Ejecute el siguiente comando, sustituyendo las rutas que encontró en el paso anterior:

```bash
ld -o ejecutable -dynamic-linker /lib/ld-linux-x86-64.so.2 /usr/lib/Scrt1.o /usr/lib/crti.o programa.o -lc /usr/lib/crtn.o
```

- En caso de que el comando ld mande errores, investigue como enlazar un programa utilizando el comando <i>ld</i>. Y proponga una posible solución para llevar a cabo este proceso con éxito. **No recibí errores**
- Describa el resultado obtenido al ejecutar el comando anterior. **Se obtiene un archivo ejecutable, llamado *ejecutable* a partir del código máquina de la fase anterior.**

8. Una vez que se enlazó el código máquina relocalizable, podemos ejecutar el programa con la siguiente instrucción en la terminal: ```./ejecutable```

9. Quite el comentario de la macro _#define PI_ en el código fuente original y conteste lo siguiente:

- Genere nuevamente el archivo.i. De preferencia asigne un nuevo nombre.
- ¿Cambia en algo la ejecución final? **Cambia ligeramente elr esultado mostrado en pantalla**

10. Escribe un segundo programa en lenguaje **_C_** en el que agregue 4 directivas del preprocesador de _**C**_ (_cpp_). Las directivas elegidas deben jugar algún papel en el significado del programa, ser distintas entre sí y diferentes de las utilizadas en el primer programa (aunque no están prohibidas si las requieren). 

- Explique su utilidad general y su función en particular para su programa. **Utilicé `#warning`, `#define`, `#undef`, `#error` para lanzar una advertencia durante la compilación, definir una macro, eliminar esta macro, y finalmente lanzar un error.**

11. Redacte un informe detallado con sus resultados y conclusiones.

**Ahora que pude entender e inspeccionar de cerca cada fase del *procesamiento del lenguaje* en un compilador, tengo un mejor entendimiento de los procedimientos y comandos involucrados a lo largo del procedimiento, además puedo distinguir claramente entre cada una y puedo entender el impacto de mi código fuente inicial sobre éstas.**

**Para el final de ésta práctica, cuento con los archivos `programa.c`, `programa.i`, `programa.s`, `programa.o` y `ejecutable` que representan cada fase del sistema visto, además del programa que realice con las 4 directivas requeridas como `fer.c`.**