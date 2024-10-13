[<- Índice](../LenguajesProgramacion.md)
# Estrategias de Evaluación Clásicas

Una ==estrategia de evaluación== es el conjunto de reglas que un lenguaje de programación utiliza para determinar **cúando** y **como** se evalúan las expresiones en un programa.

Básicamente, define el orden en el que se calculan los valores de las expresiones y subexpresiones (como los argumentos de una función), si antes o después de que se invoque la función.

> Dada una expresión $e$ en un lenguaje de programación, que puede consistir en subexpresiones $e_1,e_2,...,e_n$ y un conjunto de reglas de evaluación $\mathcal{R}$, una **estrategia de evaluación** es un algoritmo que selecciona una subexpresión $e_i$ y aplica una nueva regla de evaluación $r \in \mathcal{R}$ en $e_i$ para obtener una exoresión $e'$.
>
> Esta **estrategia** se aplica iterativamente hasta que alcanza una forma normal, es decir, que no se pueden realizar más reducciones.
>
> Puede ser vista como una funcón de selección $S:E \rightarrow E$  siendo $E$ una expresión.
>
> $S(e) \; = \; e' \quad$ si $\quad e \rightarrow_{\mathcal{R}} e'$

Esta estrategia de evaluación básicamente se refiere a las transiciones entre estados (*Semántica*) que definimos previamente en esta serie de notas.

## Principales Estrategias de Evaluación

### Evaluación glotona (ansiosa)

Selecciona siempre las subexpresiones que están más a la izquierda o mas exteriores para ser evaluadas primero. Tambien se le conoce como ==evaluación ansiosa==.

> Esto significa que las subexpresiones e evalúan antes de que se usen.

Es común en lenguajes como *C*, *Java*, *Python*.

Por ejemplo:

$\texttt{(let (f (lambda (x) (+ x 2))}$
$\hspace{1cm}\texttt{(f (+ 1 3)))}$

En este caso, lo primero que se evalua es la expresión $\texttt{(+ 1 3)}$ a $4$ y ya después se evalúa la función.

### Evaluación perezosa (diferida)

Selecciona las subexpresiones sólo cuando son necesitadas para el cálculo final del programa.
Tambien se le conoce como ==evaluación diferida==.

En esta estrategia las expresiones solo se evaluan cuando ==realmente se necesitan==.

En lugar de evaluar inmediatamente como en su contraparte, el lenguaje espera hasta que se intente utilizar el valor de la expresión.

Es común en lenguajes funcionales como ***Haskell***

En el ejemplo de arriba, sustituiriamos la expresión $\texttt{(+ 1 3)}$, dentro de la función tal cual, sin evaluarse, y continuariamos con la ejecución del programa hasta que nos veamos forzados a evaluarla.

# Enlaces

[<- Anterior](LPNota17.md) | [Siguiente ->](LPNota19.md)