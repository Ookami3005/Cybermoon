[<- Índice](../LenguajesProgramacion.md)
# Cerraduras de Función

Como vimos en la nota anterior, ahora requerimos (para el alcance estático) que las funciones "recuerden" el ambiente de evaluación al momento de su declaración.

> A esto se le conoce formalmente como ==**cerradura de función**== o simplemente ==**cerradura**==.

Una cerradura entonces, se define como una **estructura** la cual almacena el **parámetro**, el **cuerpo** de la función y el ambiente donde ésta fue definida (este último es visto como su *subambiente*).

De esta forma al evaluar una función en lugar de simplemente regresarla, se debe regresar una cerradura que ==encierra o captura== a la función con el ambiente acutal.

Con esto dicho, los nuevos valores finales de nuestro lenguaje son:

1. Cerraduras
2. Números
3. Booleanos.

A partir de ahora separaremos estos estados finales de los demás estados mediante la representación de la relación:

$$
e \; \texttt{ASAValue}
$$

Con lo cual tenemos los siguientes valores:

$$
\begin{prooftree}
\AxiomC{$n \in \mathbb{Z}$}
\UnaryInfC{$NumV(n) \; ASAValue$}
\end{prooftree}
$$

$$
\begin{prooftree}
\AxiomC{$b \in \mathbb{B}$}
\UnaryInfC{$BooleanV(b) \; ASAValue$}
\end{prooftree}
$$

$$
\begin{prooftree}
\AxiomC{p : \texttt{String}}
\AxiomC{c \texttt{ASA}}
\AxiomC{$\varepsilon$ \texttt{Env}}
\TrinaryInfC{$<p,c,\varepsilon>$ \texttt{ASAValue}}
\end{prooftree}
$$

**Nota**: $\varepsilon$ Env representa la relación "$\varepsilon$ es un ambiente de evaluación"

Definimos entonces nuevas reglas de semántica natural para que usen alcance estático mediante cerraduras:

- **Identificadores**

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Id(i),\varepsilon \Rightarrow \varepsilon(i)$}
\end{prooftree}
$$

- **Números**

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Num(n),\varepsilon \Rightarrow NumV(n)$}
\end{prooftree}
$$

- **Suma**

$$
\begin{prooftree}
\AxiomC{$i,\varepsilon \Rightarrow NumV(i')$}
\AxiomC{$d,\varepsilon \Rightarrow NumV(d')$}
\BinaryInfC{$Add(i,d) \Rightarrow NumV(i'+d')$}
\end{prooftree}
$$

- **Funciones**

Ahora generan una cerradura:

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Fun(p,c),\varepsilon \Rightarrow \; <p,c,\varepsilon>$}
\end{prooftree}
$$

- **Aplicaciones de función**

La reducción de la función genera una cerradura donde se evaluará el cuerpo

$$
\begin{prooftree}
\AxiomC{$f,\varepsilon \Rightarrow \; <p,c,\varepsilon'>$}
\AxiomC{$a,\varepsilon \Rightarrow a_v$}
\AxiomC{$c,\varepsilon' \cup [p \leftarrow a_v] \Rightarrow c_v$}
\TrinaryInfC{$App(f,a), \varepsilon \Rightarrow c_v$}
\end{prooftree}
$$

# Enlaces

[<- Anterior](LPNota16.md) | [Siguiente ->](LPNota18.md)