[<- Índice](../AnalisisAlgoritmos.md)
# Justificación y diseño de algoritmos

Por una parte, nos interesa identificar conjuntos más amplios de algoritmos que tienen un ==comportamiento similar==, el cual sólo es posible notar con un análisis menos preciso.

Por otra, aunque intentemos determinar el tiempo de ejcución exacto, dicha precisión suele no valer la pena pues para entradas de tamaño suficientemente grandes, las constantes multiplicativas y términos de menor orden terminan siendo dominadas por los ==términos de mayor orden==.

## Notación asintótica

Vamos a expresar la tasa de crecimiento del tiempo de ejcución (y de otras funciones) de manera que se ignoren los **factores constantes** y los **términos de menor orden**.

> Consideramos funciones entre valores no negativos.

### - Notación O grande (Cota asintótica superior)

Sea $T(n)$ una función, digamos, el tiempo de ejcución en el **peor caso** de un algoritmo para una entrada de tamaño $n$.

Dada otra función $g(n)$ decimos que $T(n)$ está en $O(g(n))$ denotado como:

> $T(n) \; = \; O(g(n))$

si para $n$ suficientemente grande, la función $T(n)$ está acotada por arriba por una constante multiplicativa de $g(n)$.

De forma más precisa:

$T(n) \; = \; O(g(n))$ si existen constantes positivas $c$ y $n_0$ tales que para toda $n \geq n_0$ se cumpla $T(n) \leq c \cdot g(n)$

Notemos que $c$ no puede depender de $n$, es decir, necsitamos una constante $c$ que no dependa del tamaño de la entrada. ==Una constante c fija==.

Por otro lado $O(\cdot)$ expresa una cota superior y no una tasa de crecimiento exacta.
Por ejemplo, notemos que un algoritmo que está en $O(n^2)$ tambien cumple (por definición de $O$ grande) con estar en $O(n^3)$

### - Notación $\Omega$ grande (Cota asintótica inferior)

La notación Omega grande, nos permite expresar que para $n$ suficientemente grande, la función $T(n)$ está acotada por abajo por una constante multiplicativa de otra función de $g(n)$.

$T(n) = \Omega(g(n))$ si existen constantes $c > 0$ y $n_{0} > 0$ tales que $T(n) \geq c \cdot g(n)$
para toda $n \geq n_0$.

### - Notación $\Theta$ grande

La notación *Theta* indica que que una función $T(n)$ está tanto en $O(g(n))$ como en $\Omega(g(n))$.

$T(n) = \Theta(g(n))$ si existen constantes positivas $c_1$, $c_2$ y $n_0$ tales que $c_{1}\cdot g(n) \leq T(n) \leq c_{2}\cdot g(n)$ para toda $n \geq n_0$.

### - Notación o pequeña (Cota superior que no es asintóticamente justa)

$T(n) = o(g(n))$ si para **cualquier** constante $c > 0$ existe una constante $n_{0}> 0$ tal que $0 \leq T(n) < c \cdot g(n)$ para toda $n \geq n_0$.

Por supuesto, la diferencia más obvia con $O(\cdot)$ en $T(n) = o(g(n))$m la cota $0 \leq T(n) \leq c \cdot g(n)$ se cumple para toda constante $c > 0$.

En este caso, si $n$ es suficientemente grande, la función $T(n)$ se vuelve insignificante (varía practicamente nada) respecto a $g(n)$:

$$
\displaystyle
\lim_{n \rightarrow \infty} \frac{T(n)}{g(n)} = 0
$$

### - Notación $\omega$ pequeña (Cota inferior no asintóticamente justa)

$T(n) = \omega(g(n))$ si para **cualquier** constante $c > 0$ existe una constante $n_{0}>0$ tal que $0 \leq c \cdot g(n) < T(n)$ para toda $n \geq n_0$.

Para este caso, si $n$ es suficientemente grande, la función $T(n)$ se vuelve arbitrariamente grande respecto a $g(n)$:

$$
\displaystyle
\lim_{n \rightarrow \infty} \frac{T(n)}{g(n)} = \infty
$$

## Propiedades de los órdenes de crecimiento asintóticos

#### - Transitividad
- $f(n) = O(g(n)) \quad \land \quad g(n) = O(h(n)) \quad \Rightarrow \quad f(n) = O(h(n))$
- Se aplica esta misma regla para el resto de notaciones.

#### - Reflexividad
- $f(n) = O(f(n))$
- $f(n) = \Omega(f(n))$
- $f(n) = \Theta(f(n))$

#### - Simetría (Únicamente $\Theta$)
- $f(n) = \Theta(g(n))$ si y solo si $g(n) = O(f(n))$

#### - Transpuesta ($\Theta$ no)
- $f(n) = O(g(n))$ si y sólo si $g(n) = \Omega(f(n))$
- $f(n) = o(g(n))$ si y sólo si $g(n) = \omega(f(n))$

---
Para resumir:

Podemos hacer analogías entre las comparaciones asintóticas de 2 funciones $f$ y $g$ y la comparación aritmética de 2 números reales:

- $f(n) = O(g(n))$ es como $a \leq b$
- $f(n) = \Omega(g(n))$ es como $a \geq b$
- $f(n) = \Theta(g(n))$ es como $a = b$

- $f(n) = o(g(n))$ es como $a < b$
- $f(n) = \omega(g(n))$ es como $a > b$

## Relaciones de dominancia

Decimos que una función $g$ **domina** a la función $f$ si $f(n) = O(g(n))$ pero $g(n) \neq O(f(n))$.

Si es el caso decimos que $g(n)$ tiene un mayor orden de magnitud.

Nos fijaremos en las clases de dominancia más comunes que suelen surgir en el análisis de algoritmos

#### - Funciones constantes $f(n) = O(1)$
- Sumar 2 números, imprimir un mensaje corto, comparar 2 numeros ...
- No dependen del parámetro $n$

#### - Funciones logarítmicas $f(n) = O(log(n))$
- Algoritmos como búsqueda binaria
- Crecen lentamente respecto a $n$.

#### - Funciones lineales $f(n) = O(n)$
- Identificar el máximo o mínimo de una lista, computar un promedio...
- Por lo regular, surgen por revisar los $n$ elementos de una lista.

#### - Funciones súper lineales $f(n) = n \; log \; n$
- Algoritmos de ordenamiento como *MergeSort* o *QuickSort*
- Crecen un poco más rápido que lineal pero siguen considerandose eficientes.

#### - Funciones cuadráticas $f(n) = O(n^2)$
- *InsertionSort*, por ejemplo.
- Algoritmos que necesitan revisar todas las parejas de un conjunto de *n* elementos.

#### - Funciones cúbicas $f(n) = O(n^3)$
- Enumerar todas las tripletas de un conjunto

#### - Funciones polinomiales $f(n) = O(n^c)$ para $c > 3$

#### - Funciones exponenciales $f(n) = O(c^n)$ para $c > 1$
- Enumerar todos los subconjuntos de *n* números
- Se vuelven poco útiles rapidamente.

#### - Funciones factoriales $f(n) = O(n!)$
- Generar todas las permutaciones de un conjunto de elementos

## Operaciones con notación asintótica

#### Suma de funciones
Manda al término dominante:

- $O(f(n)) + O(g(n)) \; \Rightarrow \; O(max(f(n),g(n)))$
- $\Omega(f(n)) + \Omega(g(n)) \; \Rightarrow \; \Omega(max(f(n),g(n)))$
- $\Theta(f(n)) + \Theta(g(n)) \; \Rightarrow \; \Theta(max(f(n),g(n)))$

#### Multiplicación de funciones
Multiplicar  por una constante noa fecta el comportamiento asintótico.

- $O(c \cdot f(n)) = O(f(n))$
- $\Omega(c \cdot f(n)) = \Omega(f(n))$
- $\Theta(c \cdot f(n)) = \Theta(f(n))$

Sin embargo, si 2 funciones crecientes se multiplican, ambas importan:

- $O(f(n)) \cdot O(g(n)) = O(f(n) \cdot g(n))$
- $\Omega(f(n)) \cdot \Omega(g(n)) = \Omega(f(n) \cdot g(n))$
- $\Theta(f(n)) \cdot \Theta(g(n)) = \Theta(f(n) \cdot g(n))$

# Enlaces

[<- Anterior](AA_Introduccion.md) | [Siguiente ->](AA_AlgoritmosIterativos.md)