[<- Índice](LenguajesProgramacion.md)
# Estrategias de Evaluación y Alcance

Recordemos un poco nuestra regla de semántica:

$$
\begin{prooftree}
\AxiomC{$f,\varepsilon \Rightarrow \; <p,c,\varepsilon'>$}
\AxiomC{$a \Rightarrow a_v$}
\AxiomC{$c, \varepsilon'[p \leftarrow a_{v]}\Rightarrow c_v$}
\TrinaryInfC{$App(f,a) \Rightarrow c_v$}
\end{prooftree}
$$

La naturaleza glotona de nuestro lenguaje reside en la regla:

$$
a \Rightarrow a_v
$$
Hemos manejado esta estrategia de evaluación hasta el momento, de modo que ya tendrás una idea de como se evalua.

Entonces pasemos a lo novedoso

## Volviendo a *MiniLisp* perezoso

Para adecuar *MiniLisp* a una estrategia de evaluación perezosa, debemos adecuar nuestra regla de semántica de la siguiente manera:

$$
\begin{prooftree}
\AxiomC{$f,\varepsilon \Rightarrow \; <p,c,\varepsilon'>$}
\AxiomC{$c, \varepsilon'[p \leftarrow a_{v]}\Rightarrow c_v$}
\BinaryInfC{$App(f,a) \Rightarrow c_v$}
\end{prooftree}
$$

Evaluemos por ejemplo:

$\texttt{(let (x (+ 2 2))}$
$\hspace{1cm}\texttt{(let (y (+ x 2))}$
$\hspace{2cm}\texttt{(let (x (+ 3 3))}$
$\hspace{3cm}\texttt{y)))}$

**Ambiente inicial**:

| Identificador | Valor     |
| ------------- | --------- |
| *x*           | *(+ 3 3)* |
| *y*           | *(+ x 2)* |
| *x*           | *(+ 2 2)* |

Evaluando entonces la expresión *y* sustituimos como:

*(+ x 2)*

Y sustituyendo la primera aparición de *x* en la pila, obtenemos la expresión

*(+ (+ 3 3) 2)*

Una vez nos vemos forzados a evaluar, obtenemos entonces el resultado $8$, contrario al esperado $6$ ya que hemos vuelto a encontrarnos con un **alcance dinámico**.

### Cerraduras de expresión

El alcance cambia debido a que estamos evaluando las expresiones sobre el ambiente actual y no sobre el ambiente donde éstas fueron definidas.

Entonces si deseamos recuperar el **alcance estático**, nos vemos forzados a aplicar la misma técnica que la última vez.
Haremos que las expresiones carguen con el ambiente donde fueropn definidas, definiendo así las ==cerraduras de expresión==.

Por ejemplo, para el ejercicio realizado anteriormente:

$\texttt{(let (x (+ 2 2))}$
$\hspace{1cm}\texttt{(let (y (+ x 2))}$
$\hspace{2cm}\texttt{(let (x (+ 3 3))}$
$\hspace{3cm}\texttt{y)))}$

Definimos igualmente el **ambiente inicial** como:

| Identificador | Valor     |
| ------------- | --------- |
| *x*           | *(+ 3 3)* |
| *y*           | *(+ x 2)* |
| *x*           | *(+ 2 2)* |

Sin embargo, no olvidemos que cada una de las expresiones carga con el subambiente en el que fueron definidas, por ejemplo, para la expresión asignada a *y*, *(+ x 2)*, poseemos la cerradura:

| Identificador | Valor     |
| ------------- | --------- |
| *x*           | *(+ 2 2)* |

Pues cuando definimos *y*, solo dicha variable habia ingresado al ambiente.

Este subambiente es el único que nos interesa para este ejercicio, pero recalco energéticamente que ==todas las expresiónes en el ambiente cargan con su respectivo subambiente de cuando fueron definidas==.

Entonces evaluando el cuerpo del let más anidado, *y*

Debemos sustituir por *(+ x 2)*, y para la sustitución de *x* debemos consultar el subambiente de esta expresión definido unos momentos antes, quedando entonces la expresión:

*(+ (+ 2 2) 2)*

Cuando nos vemos forzados a evaluarla podemos ver que obtenemos por fin, el esperado $6$.

## Combinaciones de Alcance y Estrategias de Evaluación

Cada combinación de alcance y estrategia ofrece una serie de puntos fuertes y puntos débiles en términos de eficiencia, facilidad de uso y comodidad.

Por supuesto, en los lenguajes modernos el favorito es el ==alcance estático== mientras que las combinaciones con alcance dinámico son menos comunes debido a su complejidad y dificultad para razonar sobre el código.

### Alcance Estático y Evaluación Glotona

| Ventajas                                                                                               | Desventajas                                                                                                | Lenguajes Asociados    |
| ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- | ---------------------- |
| - Simplicidad y predictibilidad en el código<br><br>- El orden de la evaluación es claro y consistente | - Puede realizar evaluaciones innecesarias<br><br>- No es eficiente con grandes cálculos que no son usados | *C*, *Python* y *Java* |

### Alcance Estático y Evaluación Perezosa

| Ventajas                                                                                                                                                                      | Desventajas                                                                                                                                                                                  | Lenguajes Asociados |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| - Alta eficiencia en programas donde muchas expresiones no se usan<br><br>- Capacidad de trabajar con estructuras infinitas<br><br>- Ahorra memoria y tiempo de procesamiento | - Mayor complejidad en la implementación y depuración del lenguaje<br><br>- El orden de evaluación no es inmediato y el momento en que se evaluan las expresiones puede ser menos intuitivo. | *Haskell*           |

### Alcance Dinámico y Evaluación Glotona

| Ventajas                                                                                                   | Desventajas                                                                                                                                  | Lenguajes Asociados           |
| ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| - Rápida ejecución inicial al evaluar todo de inmediato<br><br>- Fácil de implementar en sistemas pequeños | - Dificil de razonar sobre las variables ya que su valor dependen del lugar de ejecución<br><br>- Propenso a errores de captura de variables | Algunos "dialectos" de *Lisp* |


### Alcance Dinámico y Evaluación Perezosa

| Ventajas                                                                                                                                | Desventajas                                                                                                                                    | Lenguajes Asociados |
| --------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| - Evita calculos innecesarios y podria ser util cuando se requiere que las invocaciones dependan de la ejecución (O sea realmente nada) | - Gran complejidad inherente al alcance dinámico<br><br>- El valor de las variables es ampliamente impredecible, lo que complica la depuración | Muy poco común      |

# Enlaces
 
 [<- Anterior](LPNota18.md) | [Siguiente ->](LPNota20.md)