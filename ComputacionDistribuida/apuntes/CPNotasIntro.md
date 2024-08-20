[<- Volver](../ComputacionDistrubuida.md)
# Introducción

A continuación, sentaremos bases con una serie de definiciones importantes.

#### Definición 1: Computación concurrente
La concurrencia es una técnica utilizada para disminuir el tiempo de respuesta del sistema que utiliza una sola unidad de procesamiento o procesamiento secuencial.
Una tarea se divide en varias partes, y su parte se procesa simultaneamente pero no en el mismo instante.

#### Definición 2: Computación paralela
El paralelismo se concibe con el fin de aumentar la velocidad de cálculo mediante el uso de múltiples procesos.
Es una técnica de ejecución simultánea de diferentes tareas en el mismo instante.
Implica varias unidades de procesamiento independientes, que operan en paralelo y realizan tareas con el fin de aumentar la velocidad de cálculo y mejorar el rendimiento.

#### Definición 3: Computación distribuida
Computadoras conectadas que trabajan para resolver un problema en común meidante el intercambio de mensajes.

#### Definición 4: Sistema distribuido
Un sistema distribuido es una colección de dispositivos de cómputo individuales que se pueden comunicar entre si.

#### Definición 5: Sistemas asíncronos
Un sistema es asíncrono si no hay límite superior fijo sobre cuanto tiempo tarda en entregar un mensaje o cuanto tiempo transcurre entre los pasos consecutivos de un procesador.

#### Definición 6: Sistemas síncronos
En el modelo síncrono, los procesadores se ejecutan paso a paso: la ejecución se divide en rondas, y en cada ronda cada procesador puede enviar un mensaje a cada vecino, los mensajes se entregan y cada procesador calcula basandose en los mensajes que acaba de recibir.
Este modelo, es muy conveniente para el diseño de algoritmos porque un algoritmo no necesita enfrentarse a mucha incertidumbre.

#### Definición 7: Proceso
Un sistema distribuido está formado por un conjunto de unidades de procesamiento, cada una de las cuales se abstrae mediante la noción de un proceso.
Los procesos cooperan en un objetivo en común, lo que significa que intercambian información de un modo u otro.

El conjunto de procesos es estático, se compone de *n* procesos y se denota como $\Pi = \{p_1,p_2,\cdots,p_n\}$.
Cada proceso $p_i$ es secuencial, es decir, ejecuta un paso cada vez.

#### Definición 8: Medio de comunicación
Los procesos se comunican enviando y recibiendo mensajes a través de canales.
Se supone qu cada canal es ==fiable== (no crea, modifica ni duplica mensajes).

En algunos casos suponemos que los canales son un "*first in first out*" (*FIFO*), lo que implica que los mensajes se reciben en el orden en que se han enviado.

Igualmente en la mayoría de los casos, se supone que cada canal es ==bidireccional== y tiene una capacidad infinita.
Aun así, en algunos casos particulares, se considera que son unidireccionales.

Cada proceso $p_i$, tiene un conjunto de vecinos, denominados $neighbors_i$.
Según el contexto, este conjunto contiene o bien las identidades locales de los canales que conectan con $p_i$ , o bien las identidades de estos procesos.

#### Definición 9: Algoritmo distribuido
Es una colección de *n* autómatas, uno por proceso.
Un autómata describe la secuencia de pasos ejecutados por el proceso correspondiente.

Además de la potencia de la máquina de Turing, un autómata se enriquece con 2 operadciones de comunicación que le permiten enviar un mensaje por un canal o recibir un mensaje por cualquier canal.
Las operaciones son *sen()* y *recieve()*.
