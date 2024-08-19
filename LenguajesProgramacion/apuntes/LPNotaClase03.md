[<- Volver](../LenguajesProgramacion.md)
# Clasificación de los lenguajes de programación

Es importante ==categorizar== y entender las características de los distintos lenguajes de programación por varias razones:

- Comprensión de estilos de programación
- Facilita la elección del lenguaje apropiado
- Promueve el entendimiento de la evaluación de los lenguajes
- Mejora la educación en Ciencias de la Computación
- Para el desarrollo de nuevos lenguajes y herramientas
- Optimización y traducción del código

## Clasificación de acuerdo al nivel de abstracción

La clasificación de lenguajes de programación según el nivel de abstracción se refiere a como estos lenguajes se relacionan con el *hardware* subyacente y qué tan cercanos o alejados están de las operaciones de la máquina.

Podemos distinguir principalmente 3 categorías:

- **Lenguajes de bajo nivel**
- **Lenguajes de nivel medio**
- **Lenguajes de alto nivel**

### Lenguajes de bajo nivel

Estos lenguajes se encuentran muy cerca del *hardware* y proporcionan poca abstracción de los detalles de la maquina. Aqui podemos encontrar lenguajes como:

#### Lenguaje de máquina

Es el nivel más bajo de programación.
Consiste en instrucciones codificadas en binario que el procesador puede ejecutar directamente.
Cada tipo de procesador tiene su propio conjunto de instrucciones máquina.

#### Lenguaje ensamblador

Proporciona un nivel de abstracción ligeramente superior al lenguaje de máquina.
Usa mnemotécnicos (palabras abreviadas) en lugar de códigos binarios, lo que facilita la programación.
Cada instrucción de ensamblador se traduce directamente a una instrucción de máquina. Ejemplos incluyen el ensamblador *x86* y *ARM*.

---
### Lenguajes de nivel medio

Este tipo de lenguajes ofrece un balance entre abstracción y control sobre el *hardware*. Permiten manipular directamente la memoria y realizar operaciones de bajo nivel, pero también incluyen características de lenguajes de alto nivel.

Un clásico ejemplo de este tipo de lenguajes es *C*.
Este lenguaje proporciona acceso a operaciones de bajo nivel como la manipulación de memoria mediante apuntadores, pero tambien proporciona estructuras de alto nivel como funciones y estructuras de datos complejas.

### Lenguajes de alto nivel

Estos lenguajes están más alejados del *hardware*  uy proporcionan mucho mayor abstracción, facilitando la programación y lectura de código.

Lenguajes como *Python*, *Java* y *Ruby* caen dentro de esta clasificación.

Manejan detalles de bajo nivel como la administración de memoria automáticamente (por ejemplo, a través de recolectores de basura) y proporcionan características avanzadas como manejo de excepciones, estructuras de datos integradas y bibliotecas extensas.

---

La clasificación de lenguajes de programación de acuerdo al nivel de abstracción es importante para entender la relacioón entre el *software* y el *hardware*, permitiendo a las personas elegir herramientas adecuadas para cada tarea según su abstracción.

## Clasificación de acuerdo al propósito

La clasificación de lenguajes de programación según su proposito se refiere a la categorización de los mismos basándose en el tipo de problemas o tareas para los cuáles estan diseñados y optimizados.

### Lenguajes de propósito general

Estos lenguajes están diseñados para ser versátiles y aplicables a una amplia variedad de problemas y dominios, desde aplicaciones *web* hasta videojuegos.

### Lenguajes de propósito específico

Estos lenguajes están diseñados para resolver problemas en dominios particulares o para tareas específicas.
Su sintaxis y características están optimizadas para esas aplicaciones en particular.

Por ejemplo, *SQL*, *HTML* y *MATLAB*.

## Clasificación de acuerdo al paradigma

La clasificación de lenguajes de programación según su paradigma se refiere a clasificarlos según las diferentes filosofías/enfoques que estos lenguajes adoptan para estructurar y organizar el código.

Estos paradigmas, por supuesto, son cruciales para entender cómo un lenguaje puede influir en la manera en que pensamos y resolvemos problemas.

### Estilo de programación imperativa

En este estilo, los programas se constituyen mediante secuencias de instrucciones que cambian el estado del programa.
En este enfoque se centra en ==cómo== queremos resolver algo.

Podemos diferenciar 2 subestilos actualmente:

#### Programación Estructurada

Organiza los programas al puro estilo imperativo mediante distintos tipos de estructuras: (1) secuencias, (2) estructuras de decisión y (3) estructuras de repetición.
Promueve el uso de variables y referencias para cambiar y manipular datos en memoria.

El mayor representante de este lenguaje es *C*

#### Programación orientada a objetos

Organiza los programas en objetos, que son instancias de clases.
Cada objeto contiene datos y métodos que operan sobre esos datos.

La programación orientada a objetos promueve la reutilización de código y la modularidad.

Ejemplos clásicos: *Java*, *C++* y *Ruby*.

---
### Estilo de programación declarativa

En este estilo, los programas describen qué se quiere lograr o qué son las cosas en lugar de como hacerlo.

Los lenguajes declarativos se enfocan en la lógica del resultado deseado y el uso de definiciones y no en los pasos para obtenerlo.

Podemos encontrar subestilos como:

#### Programación funcional

Los programas se construyen mediante la aplicación de funciones.
Se enfoca en el uso de funciones puras, sin efectos secundarios y evita el uso de estados y datos mutables.

Ejemplos clásicos: *Haskell*, *Lisp* y *Scala*

#### Programación lógica

Se basa en la lógica matemática. Los programas se describen mediante un conjunto de hechos y reglas, y la computación se realiza mediante la aplicación de reglas de inferencia:

El principal representante de este estilo es *Prolog*.

# Enlaces

[<- Anterior](LPNotaClase02.md) | [Siguiente ->](LPNotaClase04.md)