[WebHacking](../WebHacking.md)
# Desofuscación de Javascript

Antes de adentrarnos de lleno en este tema, primero hay que dejar claro que es la ***ofuscación de código***.

## Ofuscación

> La **ofuscación de código** es una técnica utilizada para dificultar la lectura y entendimiento del código por parte de una persona, sin perjudicar el funcionamiento del código aunque muchas veces si impacte su desempeño.

Este proceso puede realizarse tanto manualmente como con **herramientas automáticas**, que simplemente reescriben el código original respetando el diseño pero dificultando la lectura mediante algunas técnicas bien conocidas.

Específicamente, la ofuscación de ***Javascript***, es altamente utilizada pues estos scripts deben ser enviados al cliente en texto claro para que el navegador pueda interpretarlo, aunque realmente no se desee que el usuario husmee en este código.

Por supuesto, es una técnica mayormente usada en acciones maliciosas, aunque también se utilice de forma defensiva para entorpecer el entendimiento de la aplicación.

### Ofuscación básica

> Aunque las técnicas a continuación son relativamente sencillas, son bastante utilizadas en procesos de **ofuscación básica** de código.
##### Minimización

**Minimizar** consiste en "comprimir" todo el código en una sola línea (muy larga). Se realiza con la idea de optimizar el tiempo de carga del script y dificultar su lectura.

##### Empaquetado

Por otra parte, el **empaquetado** se refiere a convertir todas las palabras y símbolos del código en una lista o diccionario y se referencian usando una función `(p,a,c,k,e,d)` para reconstruir el código original durante la ejecución.

El nombre de la función puede variar según la herramienta de **empaquetación**, pero el objetivo es el mismo.

Aunque un **empaquetador** hace un gran trabajo reduciendo la legibilidad del código, aun podemos ver algunas cadenas importantes del programa en texto claro, dadonos algunas pistas del funcionamiento.

### Ofuscación avanzada

> La ofuscación avanzada consiste de técnicas más avanzadas y complejas que quedan fuera del alcance de esta nota, pero algunas herramientas comúnes son:

- *obfuscator.io*
- *JSFuck*
- *JJ Encode*
- *AA Encode*

Una vez que se aplican **ofuscaciones avanzadas** como con *JSFuck*, el desempeño del código puede verse muy afectado, pero es el riesgo que se acepta al aplicar estos procesos.

## Desofuscación

> Ahora que hablamos de la **ofuscación**, podemos empezar a ver como combatirla.

#### Beautifier/Prettier

**Beautifier.io** o **Prettier.io** son grandes herramientas que formatean un código minimizado a una versión más legible de este, ==combatiendo la **minimización**==.

Aunque aumenta considerablemente la legibilidad, no combate otras técnicas de **ofuscación** como el **empaquetado**.

#### UnPacker

Esta herramienta si que combate el **empaquetado**, devolviendo el código a su versión más legible, previo a la ofuscación.