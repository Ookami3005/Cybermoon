[<- Índice](../LenguajesProgramacion.md)
# Expresiones `letrec`

Añadiremos ahora una nueva construcción a nuestro *MiniLisp* la cual nos permitira definir las expresiones recursivas.

Este constructor se llama `letrec` y se añade a la gramática de nuestro lenguaje como sigue:

![minilisp-letrec.png](imágenes/minilisp-letrec.png)

Este nuevo constructor es una extensión de nuestro ya conocido `let` que se incluye en lenguajes como *Lisp*, *Scheme* o *Racket* y tiene como objetivo definir variables o funciones de forma recursiva.

Será util cuando las definiciones de las variables dependan unas de otras.

Por ejemplo, la siguiente expresión permitirá obtener la suma de los primeros 3 naturales.

$\texttt{(letrec (sum (lambda (n) (if0 n 0 (+ n (sum (- n 1)))))) (sum 3))}$

## Eliminando azucar sintáctica

Al igual que `let` podemos eliminar el azúcar sintáctica de este tipo de expresiones transformandolas en aplicaciones de función de la siguiente manera:

$$
\texttt{((lambda (sum) (sum 3))(lambda (n) (if0 n 0 (+ n (sum (- n 1))))))}
$$

Sin embargo, nos enfrentamos a un problema, ¿qué hacemos con las variables ligadas?

Para explicar esto mejor, veamos como se evalua con nuestro intérprete actual:

1. Como se ve arriba, tenemos una aplicación de función, entonces debemos evaluar el cuerpo $\texttt{(sum 3)}$ en el ambiente formado por el parametro $\texttt{sum}$ y la *lambda* correspondiente.

**Subambiente `lambda`**:

| Identificador | Valor                                         |
| ------------- | --------------------------------------------- |
| *sum*         | *(lambda (n) (if 0 n 0 (+ n (sum (- n 1)))))* |

**Resultado**:

$\texttt{((lambda (n) (if 0 n 0 (+ n (sum (- n 1))))) 3)}$

2. Ahora con esta nueva aplicación de función, debemos indagar el subambiente de esta expresión con la nueva asignación $[n \leftarrow 3]$ y evaluar el cuerpo de esta función $\texttt{(if0 n 0 (+ n (sum (- n 1))))}$

**Subambiente `if0`**

| Identificador | Valor |
| ------------- | ----- |
| *n*           | *3*   |

**Resultado**

$\texttt{if 3 0 (+ 3 (sum (- 3 1)))} \Rightarrow \texttt{(+ 3 (sum (- 3 1)))}$

Sin embargo al  querer evaluar esta última función, nos damos cuenta que no existe una definición de $\texttt{sum}$ en este subambiente, por lo que al querer buscarlo en el subambiente y no encontrarlo recibiriamos un ==**error** de variable libre==.

Este error ocurrio pues al ser funciones anónimas, nuestras expresiones *lambda* dejan libres todas las variables que define el constructor `letrec` en un inicio *sum*.

Para corregir esto, tendremos que ahondar aun más en el estudio del *Calculo $\lambda$*...

# Enlaces

[<- Anterior](LPNota20.md) |