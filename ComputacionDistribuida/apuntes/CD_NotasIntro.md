[<- Volver](../ComputacionDistribuida.md)
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
Computadoras conectadas que trabajan para resolver un problema en común mediante el intercambio de mensajes.

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

> Cada proceso tiene asignado un *identificador* $id_i$ .

#### Definición 8: Medio de comunicación
Los procesos se comunican enviando y recibiendo mensajes a través de canales.
Se supone que cada canal es ==fiable== (no crea, modifica ni duplica mensajes).

En algunos casos suponemos que los canales son un "*first in first out*" (*FIFO*), lo que implica que los mensajes se reciben en el orden en que se han enviado.

Igualmente en la mayoría de los casos, se supone que cada canal es ==bidireccional== y tiene una capacidad infinita.
Aun así, en algunos casos particulares, se considera que son unidireccionales.

Cada proceso $p_i$, tiene un conjunto de vecinos, denominados $neighbors_i$.
Según el contexto, este conjunto contiene o bien las identidades locales de los canales que conectan con $p_i$ , o bien los identificadores de estos procesos.

#### Definición 9: Algoritmo distribuido
Es una colección de *n* autómatas, uno por proceso.
Un autómata describe la secuencia de pasos ejecutados por el proceso correspondiente.

Además de la potencia de la máquina de Turing, un autómata se enriquece con 2 operadciones de comunicación que le permiten enviar un mensaje por un canal o recibir un mensaje por cualquier canal.
Las operaciones son *send()* y *recieve()*.

#### Definición 10: Algoritmo síncrono distribuido
Es un algoritmo diseñado para ser ejecutado en un sistema síncrono distribuido.

Más profundamente, el progreso de dicho sistema se rige por un reloj global externo, y los procesos ejecutan colectivamente una secuencia de rondas, correspondiendo cada ronda a un valor del reloj global.

> En un algoritmo síncrono, los nodos operan en rondas síncronas.

En cada ronda, cada procesador ejecuta los siguientes pasos (puede ser en cualquier orden):

- ==Realiza algún cálculo local== [^1] (de complejidad razonable).

- ==Envia mensajes== a los vecinos de la gráfica [^2] (de tamaño razonable).

- ==Recibe mensajes== (enviados por los vecinos en la segunda sección de la misma ronda).

[^1]: En general, trataremos con algoritmos que realizan cálculos muy sencillos, entonces el cálculo en tiempo exponencial suele considerarse "tramposo".
[^2]: Los mensajes largos son considerados ineficientes o "tramposos".

#### Definición 11: Algoritmo asíncrono distribuido

Un algoritmo asíncrono distribuido es un algoritmo diseñado para ser ejecutado en un sistema distribuido asíncrono.

> En un sistema asíncrono, no existe la noción de tiempo externo.
> Por ello, los sistemas asíncronos se denomian a veces ==sistemas sin tiempo==.

En el modelo asíncrono, los algoritmos se rigen por eventos ("al recibir el mensaje ..., haz ...").

Como no existe un reloj global, los mensajes enviados de un procesador a otro llegaran en un tiempo finito pero ==ilimitado==.

- El progreso de un proceso está garantizado por su propio cálculo y los mensajes que podría recibir.

- En un modelo asíncrono, los mensajes toman que toman un camino más largo pueden llegar antes.

- Un proceso procesa un mensaje a la vez, es decir, no puede ser interrumpido por la llegada de un segundo mensaje. Se almacenan en un *buffer* de entrada del proceso receptor y seran procesados despues de haberse procesado todos los mensajes anteriores en este *buffer*.

#### Definición 12: Complejidad en tiempo para algoritmos síncronos
Para algoritmos síncronos la complejidad temporal es el número de ==rondas== hasta que el algoritmo termina.

#### Definición 13: Complejidad en tiempo para algoritmos asíncronos
Para los algoritmos asíncronos, la complejidad temporal es el número de ==unidades de tiempo== desde el inicio de la ejecución hasta su finalización en el peor de los casos, suponiendo que cada mensaje tiene un retraso de una unidad de tiempo como máximo.

> **No se puede utilizar el retardo máximo en el diseño del algoritmo**

#### Definición 14: Complejidad de mensajes
La complejidad de mensajes de un algoritmo síncrono o asíncrono viene determinada por el ==número de mensajes intercambiados== en cada entrada o escenario de ejecución.

#### Definición 15: Grado
El número de vecinos de un vértice $v$, denotado como $\delta(v)$, se denomina grado de $v$.

El vértice de grado máximo de una gráfica $G$ define el grado de la gráfica $\Delta(G) = \Delta$.

#### Definición 16: Sistemas anónimos
Un sistema es anónimo si los nodos no tienen identificadores únicos.

#### Definición 17: Algoritmo uniforme
Un algoritmo se denomina uniforme si el número de nodos $n$ es desconocido para el algoritmo. Si se conoce $n$, el algoritmo se denomina *no uniforme*.

#### Definición 18: Ejecución
Una ejecución de un algoritmo distribuido es una lista de eventos, ordenados por tiempo.
Un *evento* es un registro (hora, nodo, tipo, mensaje), donde tipo es **envía** o **recibe**.

- Suponemos que no hay 2 sucesos que ocurran exactamente al mismo tiempo.

- La ejecución de un algoritmo asíncrono no suele estar determinada sólo por el algoritmo, sino también por un planificador "divino". SI hay más de un mensaje en tránsito, el programador puede elegir cual llega primero.

- Si 2 mensajes se transmiten por la misma arista dirigida, a veces se exige que el mensaje transmitido primero tambien se reciba primero (*FIFO*).

- Solo aceptamos algoritmos uniformes en los que el nodo con el máximo identificador puede ser el lider. Además cualquier nodo que no sea el lider debe conocer la identidad del lider.

#### Definición 19: Modelo *LOCAL*
El tiempo de ejecución es igual al número de rondas síncronas hasta que todos los nodos se detienen y anuncian sus salidas locales; en cada ronda cada nodo puede enviar un mensaje a cada uno de sus vecinos; el tamaño del mensaje es *ilimitado*; el cálculo local es libre

#### Definición 20: Modelo *CONGEST*
Parecido al modelo anterior, pero los mensajes están limitados a $O(log(n))$ bits.

#### Definición 21: Complejidad en tiempo de algoritmos distribuidos
Los modelos estándar de computación distribuida suelen asumir que la computación local es libre.

Se deduce que en el modelo **LOCAL** se puede resolver cualquier problema de gráficas en tiempo $O(n)$, y en el modelo **CONGEST** se puede resolver cualquier problema de gráficas en tiempo $O(m)$ por fuerza bruta; aqui $n$ es el número de nodos y $m$ el número de aristas.

Sin embargo, en estos modelos no nos interesan estos tiempos de ejecución, si no los problemas que podemos resolver en dichos tiempos.

**LOCAL** y **CONGEST** suelen ser modelos equivocados si se está interesado en estudiar, por ejemplo, problemas *NP-duros*.

Sin embargo, si consideras problemas "faciles" estos modelos se vuelven mucho más interesantes.

#### Definición 22: Ordenamiento
Elegimos una gráfica con $n$ nodos $v_1,v_2,\cdots,v_n$.
Inicialmente, cada nodo almacena un valor, pero tras almacenar un algoritmo de ordenamiento, el nodo $v_k$ almacena el k-ésimo valor más pequeño.

#### Definición 23: Contención de nodos
En cada paso de un algoritmo síncrono, cada nodo sólo puede enviar y recibir $O(1)$ mensajes que contengan $O(1)$ valores, independientemente del número de vecinos que tenga el nodo.

# Enlaces

[Siguiente ->](CD_AlgoritmosDistribuidos.md)