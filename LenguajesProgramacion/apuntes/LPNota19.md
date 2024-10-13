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

# Enlaces
 
 [<- Anterior](LPNota18.md) | [Siguiente ->](LPNota20.md)