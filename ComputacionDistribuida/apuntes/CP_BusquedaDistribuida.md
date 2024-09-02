[<- Índice](../ComputacionDistribuida.md)
# Búsqueda en ambientes distribuidos

En esta nota analizaremos 2 versiones de búsqueda en ambientes distribuidos:

1. Cuando el área de busqueda está concentrada en un nodo (**centralizada**)
2. Cuando los datos están distribuidos en la red (**descentralizada**)

### Búsqueda centralizada

En este tipo de búsqueda, el nodo coordinador gestiona la búsqueda, actuando como un controlador, asignando las tareas a los demás nodos y recopilando la información para poder brindar una respuesta.

##### Prerrequisitos

- $A$ : Área de busqueda (Arreglo, Lista, etc)
- $e$ : Elemento buscado
- $n$ : Número de nodos
- **Coordinador**: Nodo con rango o *id* 0

La idea principal del algoritmo, como se menciono anteriormente, es que el nodo **coordinador** reparta particiones del área de busqueda y les indique a los demás nodos *trabajadores* que busquen cierto elemento.

Recalco que el área de busqueda solo es reconocida inicialmente por el nodo **coordinador** y no por los demás, de ahi la necesidad de enviar las particiones a los otros nodos.

Después recopilara la respuesta de los nodos *trabajadores* y dará una respuesta positiva (devolvera el índice del elemento) o negativa (devolverá -1) dependiendo del hallazgo del elemento o la ausencia de este en alguna partición.

El algoritmo del coordinador sería:

```pseudo
\begin{algorithm}
\caption{BúsquedaCentralizada - \textbf{Coordinador}}
\begin{algorithmic}
	\State $tam = A/(n-1)$
	\For{$r=1$ hasta $(n-1)$}
		\State $A_{r}= A[(tam \times (r-1)) \; : \; (tam \times r)]$
		\State \textit{send}($r$, $A_r$)
		\State \textit{send}($r$, $e$)
    \EndFor
	\State $pos$ = -1
	\For{$r=1$ hasta $(n-1)$}
		\State \textit{recv}($r$, $pos_r$)
		\If{$pos_{r} \; \neq \; -1$}
			\State $pos$ = $pos_r$ + $tam \times (r-1)$
        \EndIf
    \EndFor
	\Return pos
\end{algorithmic}
\end{algorithm}
```

```pseudo
\begin{algorithm}
\caption{BúsquedaCentralizada - \textbf{Trabajador}}
\begin{algorithmic}
	\State \textit{recv}(0, $A$)
	\State \textit{recv}(0, $e$)
	\State $pos$ = \textit{busqSec($A$, $e$)}
	\State \textit{send}(0, $pos$)
\end{algorithmic}
\end{algorithm}
```

Ahora, analizando las métricas de desempeño:

$$
\displaystyle
S = S(n,p) = \frac{T(n,1)}{T(n,p)} = \frac{O(n)}{O(\frac{n}{p})} = \frac{n}{\frac{n}{p}} = p
$$
Por tanto, el algoritmo es óptimo en aceleración (*Speed Up*).

De igual manera, el algoritmo es óptimo en eficiencia:
$$
\displaystyle
E = \frac{S}{p} = \frac{p}{p} = 1
$$
Finalmente, la fracción seríal es nula:
$$
\displaystyle
F = \frac{\frac{p}{S}-1}{p-1} = \frac{\frac{p}{p} - 1}{p-1} = \frac{0}{p-1} = 0
$$
Es decir, no existen segmentos del algoritmo "inherentemente" secuenciales, por lo que todo el algoritmo es ==paralelizable==.

### Búsqueda descentralizada

En esta versión de búsqueda, no existe un nodo central o coordinador que gestione la búsqueda.
Todos los nodos trabajan de manera autónoma y colaboran entre sí para completar esta tarea.

Como se menciono al inicio de la nota, en este caso se supone que los datos ya se encuentran distribuidos por la red.

Más específicamente, se supone que cada nodo $i$ tiene almacenado un subarreglo de datos $A_i$ dentro de si mismo, lo que elimina la necesidad de asignarle una partición.

También cabe recalcar que aunque no hay un nodo coordinador como tal que gestione la búsqueda, el nodo 0 sigue recopilando la información de los demás nodos y brindando la respuesta del algoritmo.

Con esto dicho podemos plantear los siguientes algoritmos:

```pseudo
\begin{algorithm}
\caption{BúsquedaDescentralizada - \textbf{Nodo 0}}
\begin{algorithmic}
	\For{$r=1$ hasta $p-1$}
		\State \textit{send}($r$, $e$)
    \EndFor
    \State $pos$ = \textit{busqSec}($A_0$, $e$)
    \For{$r=1$ hasta $p-1$}
	    \State \textit{recv}($r$, $pos_r$)
	    \If{$pos$ == -1 y $pos_r$ $\neq$ -1}
			\State $pos$ = \textit{posicionGlobal}($pos_r$, $r$)
        \EndIf
    \EndFor
	\Return $pos$
\end{algorithmic}
\end{algorithm}
```

```pseudo
\begin{algorithm}
\caption{BúsquedaDescentralizada - \textbf{Nodo $i \neq 0$}}
\begin{algorithmic}
	\State \textit{recv}(0, $e$)
	\State $pos$ = \textit{busqSec}($A_i$, $e$)
	\State \textit{send}(0, $pos$)
\end{algorithmic}
\end{algorithm}
```

# Enlaces

[<- Anterior](CP_MetricasDesempeno.md) | [Siguiente ->](CP_OrdenamientosDistribuidos.md)