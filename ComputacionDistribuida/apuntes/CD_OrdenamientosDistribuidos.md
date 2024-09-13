[<- Índice](../ComputacionDistribuida.md)
# Ordenamientos en ambientes distribuidos

Aqui, igualmente que la busqueda, tenemos la versión **centralizada** y la versión **descentralizada**.

### Ordenamiento centralizado

Igualmente, se entiende que el acceso a los datos por ordenar los posee únicamente el nodo **coordinador** y por tanto, es este el que envia particiones del arreglo a los demás nodos.

Los algoritmos para **cordinador** y **trabajadores** se plantean a continuación:

```pseudo
\begin{algorithm}
\caption{OrdenamientoCentralizado - \textbf{Coordinador}}
\begin{algorithmic}
	\State $tam$ = $n/(p-1)$
	\For{$r=1$ hasta $p-1$}
		\State $A_r$ = $A[(tam \times (r-1)) \; : \; tam \times r]$
		\State \textit{send}($r$, $A_r$)
		\State \textit{send}($tam$)
    \EndFor
	\For{$r=1$ hasta $p-1$}
		\State \textit{recv}($r$, $A'_r$)
    \EndFor
	\State $A$ = \textit{kWayMerge}($A'_1$, $A'_2$, ..., $A'_{p-1}$)
	\Return $A$
\end{algorithmic}
\end{algorithm}
```

```pseudo
\begin{algorithm}
\caption{OrdenamientoCentralizado - \textbf{Trabajador i}}
\begin{algorithmic}
	\State \textit{recv}($0$, $A_i$)
	\State $A'_i$ = \textit{mergeSort}($A_i$)
	\State \textit{send}(0, $A'_i$)
\end{algorithmic}
\end{algorithm}
```

### Ordenamiento descentralizado

Queremos que los subarreglos de cada nodo estén ordenados localmente y que los arreglos guarden un orden local, es decir, para cualesquiera subarreglos en los nodos $i$ y $j$:
$$
i < j \Rightarrow A_{i}[x] < A_{j}[y] \quad \forall x,y
$$

El procedimiento de un ordenamiento descentralizado es:

1. Cada nodo ordena sus datos localmente

2. Cada nodo toma un muestro equidistante (con $k$ distanica) de sus datos partiendo por el primer dato 
	- Por ejemplo, sea $A[1,4,8,9]$ y $k=2$, tomariamos el muestreo $A'=[1,8]$

3. Los nodos envían sus muestreos al coordinador

4. El coordinador ordena los elementos de todos los muestreos

5. El coordinador toma un nuevo muestreo del arreglo ordenado con $p$ elementos (donde $p$ es el número de nodos)

6. El coordinador envía el nuevo muestreo a todos los nodos, indicandoles en que elemento debe iniciar cada nodo.

7. Cada nodo envía los datos al nodo que le corresponde según el muestreo del coordinador.

8. Cada nodo ordena los datos que recibió (o conservó).

# Enlaces

[<- Anterior](CD_BusquedaDistribuida.md) |