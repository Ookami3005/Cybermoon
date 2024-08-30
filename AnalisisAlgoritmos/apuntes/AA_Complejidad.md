[<- Índice](../AnalisisAlgoritmos.md)
# Justificación y diseño de algoritmos

Por una parte, nos interesa identificar conjuntos más amplios de algoritmos que tienen un ==comportamiento similar==, el cual sólo es posible notar con un análisis menos preciso.

Por otra, aunque intentemos determinar el tiempo de ejcución exacto, dicha precisión suele no valer la pena pues para entradas de tamaño suficientemente grandes, las constantes multiplicativas y términos de menor orden terminan siendo dominadas por los ==términos de mayor orden==.

## Notación asintótica

Vamos a expresar la tasa de crecimiento del tiempo de ejcución (y de otras funciones) de manera que se ignoren los **factores constantes** y los **términos de menor orden**.

> Consideramos funciones entre valores no negativos.

### Notación O grande (Cota asintótica superior)

Sea $T(n)$ una función, digamos, el tiempo de ejcución en el **peor caso** de un algoritmo para una entrada de tamaño $n$.

Dada otra función $g(n)$ decimos que $T(n)$ está en $O(g(n))$ denotado como:

> $T(n) \; = \; O(g(n))$

si para $n$ suficientemente grande, la función $T(n)$ está acotada por arriba por una constante multiplicativa de $g(n)$.

De forma más precisa:

$T(n) \; = \; O(g(n))$ si existen constantes positivas $c$ y $n_0$ tales que para toda $n \geq n_0$ se cumpla $T(n) \leq c \cdot g(n)$

Notemos que $c$ no puede depender de $n$, es decir, necsitamos una constante $c$ que no dependa del tamaño de la entrada. ==Una constante c fija==.

Por otro lado $O(\cdot)$ expresa una cota superior y no una tasa de crecimiento exacta.
Por ejemplo, notemos que un algoritmo que está en $O(n^2)$ tambien cumple (por definición de $O$ grande) con estar en $O(n^3)$

### Notación $\Omega$ grande

# Enlaces

[<- Anterior](AA_Introduccion.md)