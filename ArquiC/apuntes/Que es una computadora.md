[[Arquitectura de Computadoras|<- Índice]]
## Datos, información y el concepto de programa almacenado

Procesar datos extrae su significado. Una computadora es una máquina de procesamiento de datos.
Los datos fluyen a la máquina como entrada, e información fluye desde la maquina como salida.

> **datos** (entrada) => **proceso** (computadora) => **información** (salida)

¿Qué hace a una computadora diferente a una calculadora, dado que ambas se utilizan para hacer operaciones?

Por ejemplo, al sumar, la calculadora encuentra el resultado de la suma, pero el usuario debe proveer el control mediante decidir que boton presionar.

A diferencia, una computadora procesa datos automáticamente. Se le debe proveer de un conjunto de instrucciones, en forma de un programa que la guíe.

El programa se almacena dentro de la máquina, y por esto, se le conoce con el nombre de programa almacenado.

==Una **computadora** es una máquina que procesa datos bajo el control de un programa almacenado a fin de obtener información.==

---
## La computadora y las cosas que se pueden hacer con ella

==La computadora no piensa, debe ser **programada**==, es decir, debe ser dirigida para realizar un conjunto específico de operaciones.

Tal programa (o secuencia de instrucciones) ha sido almacenado previamente en la computadora.

Las computadoras actuales pueden almacenar una gran cantidad de información. Tal información consiste tanto en programas que pueden ejecutarse, como de datos sobre los cuales se ejecutan los programas.

El **programa** puede dirigar a la computadora para realizar, paso a paso, complejas operaciones, usando tan solo operaciones aritméticas básicas (suma, resta, multiplicación y división) y algunos otros relativamente simples procedimientos.

---

## Ideas básicas sobre computadoras digitales

Las computadoras digitales operan sobre números. A veces, estos números son *códigos* que representan instrucciones de un programa, o datos tales como el nombre de una persona.

Por ejemplo, si se introduce el nombre de una persona a la computadora, debe convertirse primero en un conjunto de códigos númericos apropiados.

Las computadoras son esencialmente un conjunto de interruptores que se encuentran en uno de 2 estados: encendido (on) o apagado (off).

Así, los sistemas numéricos trabajan con un sistema numérico de 2 dígitos, llamado sistema binario, el cual usa 0 y 1 para representar dichos estados de apagado y encendido respectivamente.

---

## Componentes del sistema: componentes básicos

#### Unidad central de proceso (CPU)

Los sistemas de cómputo consisten en una unidad central (que a veces es referida como la computadora propiamente) y varios periféricos. La unidad central, que contiene la mayoría de los componentes electrónicos, se conecta mediante cables a los periféricos.

John von Neumann propone dividir el *hardware* de la computadora en las siguientes 5 partes principales:

- Unidad central de proceso
- Entrada
- Salida
- Memoria de trabajo
- Memoria permanente

La ==**unidad central de proceso** (CPU)== es el componente más importante de un sistema de cómputo. Bajo el control de prógramas almacenados, esta unidad manipula datos y almacena los resultados en memoria.

Normalmente, la CPU es un circuito único o microprocesador, en el que se conjuntan la ==unidad lógico aritmética (ALU por sus siglas en ingles)== y la ==unidad de control (CU)==; sin embargo, tal terminología no es completamente estandar, ya que en algunas computadoras ambas unidades aparecen como elementos independientes de una computadora.

- La **unidad lógico aritmética** se encarga de realizar todas las operaciones lógicas y aritméticas definidas sobre los valores numéricos binarios. Por ejemplo, dos números binarios pueden sumarse para producir un tercero.

- La **unidad de control** dirige la operación de la computadora. Es el elemento que se encarga de controlar el flujo de instrucciones que la computadora va llevando a cabo. Por ejemplo, puede dar las instrucciones a la ALU para que sume 2 números binarios.

#### Memoria y discos

Todos los datos y los programas cuyas instrucciones los modifican se encuentran almacenados en la memoria de la computadora digital.

La *memoria* de una computadora almacena tanto datos como programas

En realidad, en una computadora digital, existen varios tipos de unidades de memoria. Por ejemplo, la ==*unidad principal de memoria (MMU)*== es el almacenamiento principal y de trabajo de una computadora.

Normalmente en computadoras modernas la memoria principal consiste de circuitos semiconductores.

Un tipo particular de memoria semiconductora es la *==memoria de sólo lectura== (**Read Only Memory** o ROM)*.
Este tipo de memoria contiene datos permanente almacenados, como por ejemplo, podría ser una tabla de valores de funciones trigonométricas.
Tales datos generalmente, aunque no siempre, se incorporan a la memoria duratne su manufactura.

Otro tipo particular de memoria semiconductora, pero con mayor versatilidad en su uso durante la ejecución de instrucciones, es la *==memoria de acceso aleatorio== (**Random Access Memory** o RAM)*.
Esta memoria es capaz de modificar su estado mientras permanezca encendida, permitiendo tanto lectura como la escritura de valores númericos binarios al ejecutar instrucciones de un programa.

Además de la memoria principal, una computadora digital cuenta con también con *unidades de memoria auxiliar*.
Éstas son capaces de almacenar gran cantidad de información en componentes magnéticos, como son discos o cintas; se consideran de bajo costo en el sentido de que permiten almacenar muchos datos a un costo razonable; sin embargo, las operaciones para escribir y leer información toman mucho más tiempo comparado con el de acceso de la memoria principal semiconductora.

#### Periféricos

Un *==dispositivo periférico==* se encuentra alrededor de la computadora, a diferencia del procesador central o la memoria de trabajo, las cuales forman parte de la propia computadora.

El número y tipo de dispositivos periféricos dependen de la aplicación principal que tenga el sistema de cómputo; por ejemplo, teclados, monitores, impresoras, etc.
Se clasifican como:

- **Dispositivos de entrada**: Proveen facilidades para la entrada de datos (teclado por ejemplo)

- **Dispositivos de salida**: Proveen al usuario con datos y/o información procesada (monitor, impresora)

#### Buses

Los **==buses==** se encargan de conectar a la unidad central de proceso con todos los
demás componentes de la computadora. La computadora envía y recibe datos a
traves de los **buses**. Normalmente estos se clasifican como:

- *==bus del sistema==*
	Que conecta la unidad central de proceso con la memoria de trabajo RAM

- *==buses de entrada/salida==*
	Que conectan a la unidad central de procesamiento con otros componentes

El **bus** del sistema es la conexión más importante dentro de la computadora, en tanto los **buses de entrada/salida** se encargan de transportar datos, conectando los dispositivos de entrada/salida con la unidad central de proceso y la memoria.

El ==*ciclo de entrada/proceso/salida*== es la secuencia básica mediante la cual una computadora procesa datos.

![[Ciclo de entrada-proceso-salida.canvas|Ciclo de entrada-proceso-salida]]
#### Software

Los programas y datos sólo existen como pulsos electrónicos que se encuentran en la memoria, y se les conoce con el nombre genérico de *==software==*.

Una serie de instrucciones que realiza una tarea en particular se le llama *==programa==*.


Las dos categorías más importantes en que se clasifica el **software** son:

##### Software del sistema

El *==software del sistema==* se refiere a los programas que se utilizan para controlar la computadora y desarrollar y ejecutar **software de aplicación** (o simplemente aplicaciones).

##### Software de aplicación

El *==software de aplicación==* es cualquier entrada de datos, actualización, solicitud, reporte u otro programa que procesa datos para el usuario.

El **software de aplicación** incluye los paquetes genéricos de **software** (hojas de calculo, procesadores de palabras, programas de bases de datos, etc) así como programas de paquetería o hechos a la medida como nóminas, pagos, inventarios, y otros.

#### Hardware

Los componentes físicos de una computadora (unidad central de proceso, memoria y dispositivos de entrada/salida) se conocen con el nombre genérico de *==hardware==*.

El *==hardware==* se encarga del almacenamiento  y la transmisión. Mientras más memoria y almacenamiento en disco tenga una computadora, más trabajo puede realizar.
Mientras más rápida sea la memoria y los discos para transmitir datos e instrucciones al CPU, más rápido se realiza el trabajo.

---

## Estudiando la arquitectura de sistemas de cómputo

La siguiente tabla muestra como se construyen diferentes niveles de estudio y abstracción sobre descripciones más simples.

Aquí, de abajo hacia arriba, se comienza describiendo a los ==elementos lógicos== que forman los bloques básicos de construcción del **hardware** de un sistema de cómputo, y que se combinan para obtener una máquina simple de cómputo.

Después de esto, se provee alguna información introductoria de los ==sistemas operativos==.

Los dos últimos niveles más altos en el estudio de sistemas de cómputo, ==lenguajes de programación de alto nivel== y ==aplicaciones==, no forman parte del objeto de estudio de este curso.

| Aplicaciones                                |
| ------------------------------------------- |
| **Lenguajes de programación de alto nivel** |
| **Sistema Operativo**                       |
| **Máquina**                                 |
| **Elementos lógicos**                       |

[[Logica y Secuenciacion|Siguiente ->]]