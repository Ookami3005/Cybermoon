[<- Índice](../LenguajesProgramacion.md)
# Alcance Estático y Dinámico

Sea la expresión:

$\texttt{(let (x 3)}$
$\hspace{1cm}\texttt{(let (f (lambda (y) (+ x y)))}$
$\hspace{2cm}\texttt{(let (x 5)}$
$\hspace{3cm}\texttt{(f 4))))}$

Si evaluamos esta expresión utilizando un algoritmo de sustitución, obtendremos la respuesta $7$.

Sin embargo si utilizamos la nueva definición de ambientes de evaluación, con el ambiente:

| Identificador | Valor                  |
| ------------- | ---------------------- |
| *x*           | 5                      |
| *f*           | *(lambda (y) (+ x y))* |
| *x*           | 3                      |

Entonces para la expresión $\texttt{(f 4)}$, primero realizamos la sustitución $\varepsilon (f) = \texttt{(lambda (y) (+ x y))}$ quedando la expresión:

$\texttt{(lambda (y) (+ x y))4}$

Realizando la evaluación correspondiente a la expresión $\texttt{(+ x y)}$ con el ambiente extendido:

| Identificador | Valor                  |
| ------------- | ---------------------- |
| *y*           | 4                      |
| *x*           | 5                      |
| *f*           | *(lambda (y) (+ x y))* |
| *x*           | 3                      |

Obtendriamos, después de 2 sustituciones, la expresión $\texttt{(+ 5 4)}$, debido a la naturaleza de la pila, que nos brindaría la última *x* que entro al ambiente, lo cual resulta en $9$.

**¿Cómo es posible que hayamos obtenido 2 resultados diferentes?

Esto se debe a los distintos alcances que poseen, hasta el momento, las 2 formas de evaluación que hemos visto.

## Alcance estático

El ==alcance estático== es una propiedad de los lenguajes de programación que define como se resuelven las referencias a variables en tiempo de compilación, basandose en la estructura del código fuente.

En un lenguaje con alcance estático, el valor de una variable se determina observando el bloque de código donde la variable fue declarada, sin importar dónde se invoca la función o expresión que hace referencia a dicha variable.

> Dado un programa *P* y una variable *x* referenciada dentro de una función *f*, decimos que *P* utiliza **alcance estático** si el valor de *x* en cualquier punto dentro de *f* está determinado por la declaración más cercana de *x* en el código fuente ==**en el momento de la declaración de *f*** ==, no en el momento de su invocación.

El alcance estático facilita la comprensión y razonamiento sobre el código, ya que el valor de una variable siempre puede determinarse observando el código fuente, independientemente de cómo o desde dónde se invoque la función.

### Ambientes de evaluación

Como vimos en el ejemplo al inicio de la nota, la sustitución nos brinda naturalmente un alcance estático, sin embargo, ¿es posible obtener alcance estático con un ambiente de evaluación?

 La respuesta es **Si**.

Para adecuar los ambientes de evaluación a un alcance estático, debemos incluir subambientes para cada función, al momento de que estas sean declaradas, de modo que al aplicarlas contemos efectivamente con el estado de las variables al momento de la declaración de la función.

Por ejemplo, para el ejercicio al inicio de la nota:

$\texttt{(let (x 3)}$
$\hspace{1cm}\texttt{(let (f (lambda (y) (+ x y)))}$
$\hspace{2cm}\texttt{(let (x 5)}$
$\hspace{3cm}\texttt{(f 4))))}$

Al momento de plantear el **ambiente principal**:

| Identificador | Valor                  |
| ------------- | ---------------------- |
| *x*           | 5                      |
| *f*           | *(lambda (y) (+ x y))* |
| *x*           | 3                      |

Debemos de conservar tambíen el **subambiente** de la expresión $\texttt{(lambda (y) (+ x y))}$ del estado de las variables cuando esta fue declarada (es decir, debajo de donde esta se insertó a la pila)

**Subambiente de** $\texttt{(lambda (y) (+ x y))}$:

| Identificador | Valor |
| ------------- | ----- |
| *x*           | 3     |

Ahora al evaluar la expresión $\texttt{(f 4)}$ y sustituir $f$ por la correspondiente *lambda*, al ambiente que agregamos la asignación $[y \leftarrow 4]$, es al subambiente de la *lambda*, no al principal, ademas de realizar con este mismo la evaluación:

**Subambiente extendido**

| Identificador | Valor |
| ------------- | ----- |
| *y*           | 4     |
| *x*<br>       | 3     |

**Expresión**

$\texttt{(+ x y)}$

> Lo cual nos da el resultado esperado, $7$.

## Alcance Dinámico

El ==alcance dinámico== es una propiedad de algunos lenguajes de programación que define cómo se resuelven las referencias a variables en tiempo de ejecución basándose en el **ambiente de invocación** en lugar de en el ambiente de definición.

Es decir, el valor de la variable se determina buscando la declaración más cercana de esa variable en el ==ambiente principal de evaluación==.

> Dado un programa *P*, una función *f* y una variable *x* referenciada dentro de *f*, decimos que *P* utiliza **alcance dinámico** si el valor de *x* en *f* se determina buscando en el ambiente de evaluación en lugar de en el lugar de definición de *f*.

Precisamente, al inicio de la nota, dimos uso de este tipo de alcance pues buscamos la definición de *x* más cercana en la pila (ambiente), y no en el lugar de definición de la función.

# Enlaces

[<- Anterior](LPNota15.md) | [Siguiente ->](LPNota17.md)