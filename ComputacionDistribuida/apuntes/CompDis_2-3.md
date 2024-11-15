[<- Índice](../ComputacionDistribuida.md)
# Multidifusión

Dado un grupo de nodos $g$, la ***multidifusión*** consiste en enviar un mismo mensaje a todos los miembros de $g$.

Si $g$ es la totalidad de nodos de la red entonces esta **multidifusión** (*Multicast*) es equivalente a un **Broadcast**.

Además de la idea básica de un *Multicast*, hay algunas propiedades deseables dados ciertos contextos, por ejemplo:

- ***Fiabilidad***: Se garantiza que todos los miembros reciban *n*
- ***Orden***: Los mensajes son entrgados de acuerdo a algún criterio de orden

A continuación nombrearé las variantes más importantes de **multidifusión**.

### Multidifusión básica (*B-multicast*)

Sea $m$ el mensaje que se desea enviar y $g$ el grupo al que se desea enviar el mensaje...

```pseudo
\begin{algorithm}
\caption{Emisor (B-Multicast)}
\begin{algorithmic}
\For{$r \in g$}
	\State $send(r,m)$
\EndFor
\end{algorithmic}
\end{algorithm}
```

```pseudo
\begin{algorithm}
\caption{Receptor (B-Multicast)}
\begin{algorithmic}
	\State $recv(e,m)$
\end{algorithmic}
\end{algorithm}
```

Como podemos ver, es efectivamente **muy** simple, por lo que se presentan algunso problemas.

Uno de los más grandes es la probabilidad de *ACK-implosion*, pues la red podría saturarse por el tráfico excesivo de acuses de recibo *ACK*.

Para solucionar este detalle, se implemento un método de detección de perdida de mensajes, donde se envía un *NACK* como acuse de no recibo en caso de detectar una perdida, de modo que el emisor pueda **reenviar** el mensaje.

### Multidifusión IP (*IP-Multicast*)

> Modelo de multidifusión incluido en el protocolo *TCP/IP*.

El grupo se determina por una dirección especial reservada para el proposito de realizar un *Multicast* que va desde 224.0.0.0 hasta 239.255.255.255.

El emisor envía el mensaje a la *IP* que corresponda al grupo deseado y es el *Router* el que se encarga de enviar todos los mensajes correspondientes a cada proceso del grupo.

Además no se envían *ACK* por si solos para evitar la saturaciónd de tráfico, si no que se van recopilando y adjuntando en un solo paquete denominado *"piggyback"*

### Multidifusión fiable

Para poder enunciar un **Multicast** como ***fiable*** debe cumplir 3 propiedades:

- **Integridad**: El receptor recibe un mensaje ***a lo más*** una vez (no hay mensajes duplicados)
- **Validez**: El emisor sabe si el mensaje ha sido recibido satisfactoriamente.
- **Acuerdo**: Si un nodo recibe el mensaje, entonces **todos** los nodos lo reciben

Los tipos más famosos de ***Multicast Fiable*** son:

#### Multidifusión de Orden FIFO (*OF-Multicast*)

En este tipo un orden **FIFO**, es decir...

> *"Si un proceso $p$ realiza un Multicast con el mensaje $m$ al grupo $g$ y despues otro Multicast al mismo grupo con mensaje $m'$, entonces todo $q$ en $g$ recibe $m$ antes que $m'$"*

Dicho de otra manera, los mensajes se reciben en el mismo orden que el emisor los envía, pero es importante recalcar que esto ==no garantiza este orden para mensajes de distintos emisores==, por lo que se considera que poseen un ***ordenamiento con respecto al emisor***.

Se implementa gracias a contadores o secuencias numéricas contenidas en los mensajes enviados para que cada recpetor pueda verificar el correcto orden de los mensajes.

#### Multidifusión de Orden Causal

> En este tipo de *Multicast* los mensajes se envían respetando relaciones de **Causalidad**, es decir, si el envío de un mensaje $m$ del *Multicast*, genera otro *Multicast* con el mensaje $m'$ por carte de otro proceso, todos los procesos recibiran $m$ antes que $m'$.

Esto se realiza gracias a que los emisores y receptores poseen vectores lógicos, conocidos como **vectores de tiempo** que registran la cantidad de los mensajes recibidos desde otros nodos y los mensajes enviados por el propio nodo.

#### Multidifusión de Orden Total

> Este tipo de *Multicast* asegura que ==todos los mensajes enviados por diferentes emisores== llegan a todos los receptores respetando el orden de emisión.

Se dice que es un ordenamiento *"con respecto a la recepción"*, además de no enfocarse en el tiempo físico en que se envian los mensajes, solamente que se reciban en el mismo orden.

No implica necesariamente orden causal ni *FIFO*.

## Algoritmo *Vsync* (antes is-is)

> Es un protocolo de comunicación que ofrece multidifusión ordenada y consistente en sistemas distribuidos.

Desarrollado principalmente para resolver problemas de sincronización y consistencia, este algoritmo garantiza que todos los nodos de un grupo reciban los mismos mensajes en el mismo orden y en el mismo contexto, incluso antes cambios importantes en el grupo o gráfica.

***Descripción***:

1. $p$ multidifunde un mensaje $m$
2. Cadaa receptor propone un número de secuencia a $p$
3. $p$ decide el número de secuencia definitivo y lo comunica al resto.

***A detalle***:

Cada proceso o nodo $p$ tiene:

- $Q_{g}^{p}$ : Una cola de prioridad para la retención de mensajes.
- $A_{g}^{p}$ : El mayor número de secuencia acordado para $p$ en el grupo $g$.
- $P_{g}^{p}$ : El mayor número de secuencia propuesto.

1. Primero el emisor $p$ realiza un *Multicast* $(g,<m,i>)$ donde $m$ es el mensaje y donde $i$ es un identificador **único** para $m$.
2. Cada receptor $q$ realiza el cálculo $P_{g}^{q} = max\{A_{g}^{q}, P_{g}^{q}\} + 1$ y asigna provisionalmente el número de secuencia de $m$ como el recien calculado $P_{g}^{q}$
	1. Además inserta $m$ a $Q_{g}^{q}$ y lo ordena
	2. Envía mensaje a $p$ con el valor de $P_{g}^{q}$
3. El emisor $p$ recopila las propuestas, asigna $a = max\{P_{g}^{q}\}$ y lo envía a todos los nodos.
4. Cada rececptor recibe y almacena $a$ como número de secuencia definitivo.
# Enlaces

[<- Anterior](CompDis_2-2.md) | [Siguiente ->](CompDis_2-4.md)