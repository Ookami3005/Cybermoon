[<- Índice](../LenguajesProgramacion.md)
# Combinadores de Punto FIjo

> Un ==**combinador**== es una expresión cerrada, es decir, no tiene variables libres.

Por otro lado, se dice que $x$ es un ==punto fijo== de $f$ si $x = f(x)$.

De esta forma, en el Cálculo $\lambda$ existen una serie de combinadores que reciben como parámetros una función y devuelven como resultado el punto fijo de la misma.

A estos combinadores se les da el nombre de ***combinadores de punto fijo***.

Son herramientas fundamentales en la teoría de lenguajes de programación pues, ==permiten definir funciones recursivas== sin necesidad de referirse ellas directamente por medio de un nombre.

Es una especie de **truco** matemático que permite que una función se siga llamando a sí misma, pero sin tener un nombre explícito para hacerlo.

### Suma de los primeros n números naturales

Podemos definir dentro del cálculo $\lambda$ una función que sume los primeros números naturales como sigue:

$$
\lambda n. \; if0 \; n \; 0 \; (S \; n (f \; (Pn)))
$$

Al tratar de usar esta definición, notamos que f es una variable libre, lo cual representa un problema pues bloquea las derivaciones sin llegar al resultado esperado.

Por ejemplo:

$$
(\lambda n. \; if0 \; n \; 0 \; (S \; n (f \; (Pn))))3 \quad \rightarrow_{\beta} \quad if0 \; 3 \; 0 \; (S \; 3 \; (f (P 3)))
$$
$$
\rightarrow_{\beta} \quad S \; 3 (f \; (2)) \quad \rightarrow_{\beta} \quad ?
$$

Para ligar la variable $f$, podemos redefinir la función de forma que $f$ sea pasada como parámetro (esta es la única forma que tenemos de ligar variables en el Cálculo $\lambda$)

$$
A \; =_{def} \; \lambda f. \lambda n. \; if0 \; n \; 0 \; (S \; n \; ((f f) \; (Pn)))
$$

Ahora podemos observar que la reducción de esta expresión se comporta de la manera que esperamos:

![combinadorPuntoFijo.png](imagenes/combinadorPuntoFijo.png)

Entonces podemos aplicar estos mismos pasos para lograr la recursión de cualquier función, es decir:

1. Ligar la función que queremos volver recursiva añadiendo un parámetro adicional a la función.
2. Modificar todas las llamadas de la función de tal forma que se autoapliquen.

Sin embargo, en un lenguaje de programación real (y en general) ==el paso 2 puede ser complicado== pues dependemos de analizar la estructura del cuerpo de la función, lo cual ==complica, entre otras cosas, el análisis sintáctico== pues hay que detectar todos los puntos donde se manda a llamar la función.

Es aqui