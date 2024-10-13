[<- Índice](../LenguajesProgramacion.md)
# Ambientes de evaluación

Por ejemplo para la expresión:

$\texttt{(let (x 3)}$
$\hspace{1cm} \texttt{(let (y 4)}$
$\hspace{2cm} \texttt{(+ x y))))}$

Para ser evaluada sería necesario llamar varias veces al algoritmo de sustitución.

La primera sustitución se aplica 3 veces, pues se tiene 2 expresiones `let` anidadas y el cuerpo del `let` más interno.
La segunda se aplica 2 veces y la ultima sustitución se aplica una sola vez.

Podemos intuir entonces, de forma general, para un programa con n variables que se necesitarían:

$\displaystyle n+(n-1)+(n-2)+...+1 = \frac{n(n-1)}{2}$ sustituciones

Con lo cual la complejidad del intérprete generado a partir de nuestras reglas tiene una complejidad en tiempo de $O(n^2)$.

Queda claro entonces la necesidad de optimizar nuestro intérprete de manera que pueda almacenar las sustituciones correspondientes y aplicarlas cuando el valor sea solicitado.

Una posible implementación puede ser usando una **pila** (*stack*), donde la búsqueda de variables se da de forma lineal ($O(n)$). A esta pila se le conoce como ==**ambiente de evaluación**==. Por ejemplo, el ambiente para la expresión anterior sería:

| Identificador | Valor |
| ------------- | ----- |
| *z*           | 5     |
| *y*           | 4     |
| *x*           | 5     |

Cada elemento en el ambiente (*pila*) se compone de una asignación (identificador, valor), la cual esta representada mediante el identificador de la variable y el valor asociado a la misma.

El valor puede ingresar al ambiente ==ya evaluado== cuando se usa un régimen de evaluación **glotón**, o ==sin evaluar== cuando se usa un régimen de evaluación **perezoso**.

Por ejemplo, dada la siguiente expresión:

$\texttt{(let (x 3)}$
$\hspace{1cm}\texttt{(let (y (+ x x))}$
$\hspace{2cm}\texttt{(+ x y)))}$

El ambiente de evaluación bajo un régimen de evaluación glotona sería:

| Identificador | Valor |
| ------------- | ----- |
| *y*           | 6     |
| *x*           | 3     |

Y para un régimen de evaluación perezosa sería:

| Identificador | Valor     |
| ------------- | --------- |
| *y*           | *(+ x x)* |
| *x*           | 3         |

Para representar ambientes en nuestra semántica usaremos una notación en forma de lista. Por ejemplo para el ambiente glotón anterior sería:

$[y \leftarrow 6, \; x \leftarrow 3]$

El ambiente vacío se denotara como $\varnothing$.

Y la búsqueda de valores se denotará usando paréntesis, por ejemplo:

$[y \leftarrow 6, \; x \leftarrow 3](x) \; = \; 3$

## Semántica Dinámica

Ahora, nuestras reglas de semántica se definen a partir parejas de la forma $i,\varepsilon$.
Donde $i$ es un *ASA* y $\varepsilon$ es el ambiente de evaluación.

Algunos ejemplos son:

- **Identificadores**

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Id(n),\varepsilon \; \Rightarrow \; \varepsilon(i)$}
\end{prooftree}
$$

- **Números**
$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Num(n),\varepsilon \; \Rightarrow \; Num(n)$}
\end{prooftree}
$$

- **Suma**

$$
\begin{prooftree}
\AxiomC{$i,\varepsilon \Rightarrow Num(i_v)$}
\AxiomC{$d,\varepsilon \Rightarrow Num(d_v)$}
\BinaryInfC{$Add(i,d) \Rightarrow Num(i_v+d_v)$}
\end{prooftree}
$$

El resto de *ASA*s se evalua de una manera similar, sin embargo tenemos un caso especial:

- **Aplicación de función**

$$
\begin{prooftree}
\AxiomC{$f,\varepsilon \Rightarrow Fun(p,c)$}
\AxiomC{$a,\varepsilon \Rightarrow a_v$}
\AxiomC{$c,\varepsilon \cup [p \leftarrow a_v] \Rightarrow c_v$}
\TrinaryInfC{$App(f,a), \varepsilon \Rightarrow c_v$}
\end{prooftree}
$$

# Enlaces

[<- Anterior](LPNota14.md) | [Siguiente ->](LPNota16.md)