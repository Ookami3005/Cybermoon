# Tarea 1 - Computación Distribuida

#### Romero Cruz Fernando 319314256

### 1. ¿Cuál es la diferencia entre el cómputo concurrente, el cómputo paralelo y el cómputo distribuido?

El **cómputo concurrente** consiste en gestionar múltiples partes de una tarea simúltaneamente en ==una sola unidad de procesamiento==, pero ==no en el mismo instante==, de manera que mejoremos el tiempo de ejecución de dicha tarea.

El **cómputo paralelo** hace referencia a la ejecución simultánea de varias tareas o subtareas en ==múltiples unidades de procesamiento, en el mismo instante==.

Por otra parte el **cómputo distribuido** se refiere a la ejecución de tareas en ==múltiples nodos o máquinas== conectadas mediante una ==red==, con posibilidad de enviar y recibir mensajes a través de esta.

### 2. ¿Porqué hay un único modelo de cómputo distribuido?

La idea de unificar los módelos de cómputo distribuido es proponer una forma ==general de abordar los problemas== que resuelven los sistemas distribuidos.

Así propiciamos un estándar para el desarrollo y planificación de algoritmos distribuidos, así como para la implementación de estos mismos.

### 3. ¿Por qué el cómputo distribuido se relaciona a menudo con el término *Big Data*?

El **cómputo distribuido** se relaciona a menudo con **Big Data** ya que un enfoque distribuido suele ser una de las herramientas más poderosas y utilizadas en el **Big Data**.

Esto se debe a que reducen considerablemente la complejidad del análisis de los datos, así como ofrecen todos los beneficios de un sistema distribuido, como escalabilidad, tolerancia a fallos, paralelismo, etc.

### 4. Explica porqué es posible tener paralelismo sin concurrencia y concurrencia sin paralelismo

El **paralelismo sin concurrencia** puede darse pues podemos procesar una tarea dividida en múltiples subprocesos en un mismo instante sin la necesidad de intercalar tareas en alguna unidad de procesamiento.

Por otra parte, la **concurrencia sin paralelismo** puede darse, por ejemplo, en un sistema secuencial con una sola unidad de procesamiento.

En un sistema así, no puede existir el **paralelismo** pues no existen múltiples unidades de procesamiento.
Sin embargo, si puede haber concurrencia en el único procesador del sistema, pues es perfectamente capaz de intercalar varias tareas en instantes distintos, acelerando así el proceso completo.

### 5. ¿Cuáles son las diferencias entre un sistema síncrono, un sistema asíncrono, un algoritmo síncrono y un algoritmo asíncrono?

Un **sistema síncrono** es un sistema distribuido regido por un reloj global externo que regula el algoritmo por rondas.
De esta manera, el sistema regula los mensajes y los pasos del proceso apoyandose en las rondas.

Un **sistema asíncrono** es un sistema distribuido que no se encuentra regulado por ningún reloj, de manera que no hay una regulación o cota de cuando deban comunicarse los nodos.
En estos sistemas, los mensajes se consideran eventos que son procesados cuando son detectados en el orden en el que llegaron, interrumpiendo el proceso actual.

Un **algoritmo síncrono** es un algoritmo distribuido pensado para ejecutarse en un sistema síncrono.
Es decir, considera las rondas y la divisón en pasos de ejecución y sobre esas bases plantea un procedimiento.

Un **algoritmo asíncrono** es un algoritmo distribuido pensado para ejecutarse en un sistema asíncrono.
Más profundamente, este tipo de algoritmos se plantea considerando que el sistema en el que va a ejecutarse, no posee una cota de comunicación y puede ser interrumpido por la recepción de eventos.

### 6. ¿El internet es un sistema síncrono o asíncrono? Justifica tu respuesta

El internet es un ==sistema asíncrono==, pues ni los procesos ni la comunicación están regulados por pasos o cotas.
Además los servidores, por ejemplo de páginas, suelen estar en "espera" hasta que algún cliente realiza una solicitud, hasta entonces actuan y responden a dicha solicitud.

### 7. ¿Cómo se define la complejidad en tiempo para los algoritmos asíncronos?

Usualmente, la complejidad de un algoritmo síncrono se mide en el número de rondas de comunicación y/o procesamiento que se realizaron para obtener el resultado. Por supuesto esto puede dividirse en **complejidad computacional** y **complejidad de comunicación** como estudiamos en clase.

### 8. Explica con tus propias palabras el modelo *LOCAL* y el modelo *CONGEST*

El modelo ***LOCAL*** propone un sistema síncrono (regido por rondas) donde el tamaño del mensaje se supone ilimitado y el cálculo local se supone completamente libre.

El modelo ***CONGEST*** es bastante similar al modelo ***LOCAL***, pero los mensajes están limitados a $O(log(n))$.

### 9. ¿Qué es la contención de nodos?

Se refiere a la "competencia" entre procesos por acceder a recursos compartidos dentro de la red.

Que ocurra este acceso múltiple suele generar conflictos y retrasar el tiempo de ejecución global del sistema.

### 10. Demuestra el siguiente lema:

> Una gráfica no dirigida es un árbol si y solo si existe exactamente un camino simple entre cada par de vértices.

($\Rightarrow$): **Si una gráfica es un arbol, entonces existe exactamente un camino simple entra cada par de vértices**

Si la gráfica es un árbol, entonces por definición, es una gráfica conexa sin ciclos. Como es conexa, por definición de conexidad debería existir **al menos** un camino entre cualesquiera par de vértices.
A la vez, podemos afirmar que no existe más de uno, porque de existir 2 caminos (o más) podriamos exhibir un ciclo en la gráfica yendo de un vértice al otro por el primer camino y regresando por el segundo camino, contradiciendo así la definición de árbol.
Entonces hay exactamente un camino entre cualesquiera par de vértices.

($\Leftarrow$): **Si en una gráfica existe exactamente un camino simple entre cada par de vértices, entonces esa gráfica es un árbol**

Si existe un camino entre cada par de vértices, entonces es claro que la gráfica es conexa, pues esa es la definición de conexidad.
Por otra parte, supongamos que hay al menos un ciclo en la gráfica, de ser así podriamos enunciar 2 caminos distintos entre cualesquiera pares de vértices que formen dicho ciclo (segmentando el ciclo en 2 caminos), contradiciendo nuestra hipótesis.
Por tanto, no pueden existir ciclos en la gráfica, y al ser una gráfica conexa y sin ciclos podemos concluir que es un árbol, pues esa es la definición.

### 11. Explica con tus propias palabras el funcionamiento de un algoritmo distribuido que calcule la distancia entre la raíz de una gráfica y el nodo que se está visitando

Propongo como algoritmo distribuido, primero encontrar la secuencia de nodos que represente el camino más corto entre el nodo raíz y el nodo visitado.
Después el nodo raíz deberá enviar un mensaje al siguiente nodo de la secuencia con una variable inicializada en 0, dicho nodo deberá recibir el mensaje, aumentar el valor de la variable en una unidad y enviarlo al siguiente nodo de la secuencia, repitiendo este proceso hasta que llegue al nodo visitado

De esta manera, cuando la variable llegue al nodo visitado, recibirá la variable con el valor que representa la profundidad a la que se encuentra respecto a la raíz del sistema.

### 12. Para el algoritmo propuesto en el ejercicio anterior, argumenta cuántos mensajes es necesario enviar para diseminar el mensaje y cuál es el tamaño de cada mensaje enviado

Considerando que algoritmo para encontrar el camino más corto entre 2 nodos es, en el mejor de los casos, $O(n+e) \cdot log \; n$ con $n$ nodos en el sistema con $e$ conexiones (o aristas) entre ellos, además de que se envían $p$ mensajes, donde $p$ es la profundidad del nodo visitado respecto a la raíz, entonces puedo concluir que la complejidad total del algoritmo es, en el mejor de los casos:
$$
O((n+e)log \; n + p)
$$