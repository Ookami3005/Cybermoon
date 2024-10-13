[<- Índice](LenguajesProgramacion.md)
# Puntos Estrictos

Antes de introducir el concepto de **Puntos Estrictos**, extenderemos la gramática de nuestro lenguaje añadiendo el condicional `if0` que verifica si la expresión recibida como primer argumento es cero, en cuyo caso regresa el resultado de evaluar el segundo argumento o en caso contrario, la evaluación del tercer argumento.

Por ejemplo, un `if0` válido sería:

$\texttt{(if0 (- 5 5) 3 2)}$

### Sintaxis Concreta

Sea nuestra sintaxis hasta el momento:

![[minilisp-lambdas.png]]

Añadiremos una nueva regla a $\texttt{<expr>}$ de la siguiente manera

$| \quad \texttt{(if0 <expr> <expr> <expr>)}$

### Sintaxis abstracta

De igual manera añadimos un nuevo contructor a la *Sintaxis Abstracta*:

$$
\begin{prooftree}
\AxiomC{$c$ \texttt{ASA}}
\AxiomC{$t$ \texttt{ASA}}
\AxiomC{$e$ \texttt{ASA}}
\TrinaryInfC{$If0(c,t,e) \;$ \texttt{ASA}}
\end{prooftree}
$$

Donde *c* es por condición, *t* es por '*then*' y *e* es por '*else*'

### Semántica Natural

La siguientes 2 reglas representaran el comportamiento del `if0`, la primera cuando la condición se evalua a 0 y la segunda cuando no.

$$
\begin{prooftree}
	\AxiomC{$c,\varepsilon \Rightarrow NumV(0)$}
	\AxiomC{$t,\varepsilon \Rightarrow t_v$}
	\BinaryInfC{$If0(c,t,e), \varepsilon \Rightarrow t_v$}
\end{prooftree}
$$

$$
\begin{prooftree}
	\AxiomC{$c,\varepsilon \Rightarrow NumV(n)$}
	\AxiomC{$n \neq 0$}
	\AxiomC{$e,\varepsilon \Rightarrow e_v$}
	\TrinaryInfC{$If0(c,t,e), \varepsilon \Rightarrow t_v$}
\end{prooftree}
$$

---

Analicemos entonces la siguiente expresión:

$\texttt{(let (a (+ 4 4))}$
$\hspace{1cm}\texttt{(let (b (+ a a))}$
$\hspace{2cm}\texttt{(let (a (- 4 4))}$
$\hspace{3cm}\texttt{(if0 b 1 2))))}$

1. Ambiente inicial

| Identificador | Valor     |
| ------------- | --------- |
| *a*           | *(- 4 4)* |
| *b*           | *(+ a a)* |
| *a*           | *(+ 4 4)* |

2. Se evalua el cuerpo del *let* más anidado, es decir, $\texttt{(if0 b 1 2)}$

Al ser un identificador, buscamos su valor en el ambiente y sustituimos, obteniendo

$$
\texttt{(if0 (+ a a) 1 2)}
$$

Sustituyendo $a$, obtenemos:

$$
\texttt{(if 0 (+ (+ 4 4) (+ 4 4)) 1 2)}
$$

Sin embargo, para continuar la evaluación necesitamos conocer inmediatamente el resultado de la suma $\texttt{(+ (+ 4 4) (+ 4 4))}$, lo cual no es permitido por nuestra definición de semántica ya que pospone completamente todos los cálculos.

## Puntos estrictos

Al usar evaluación perezosa, existen ciertos puntos dentro de la implementación en los que no puede postergarse la evaluación. Por ejemplo, la condición de la expresión `if` anterior.

> A los puntos donde la semántica de un lenguaje de programación perezoso, fuerza la reducción de una expresión a un valor (si existe) se les llama ==**puntos estrictos**==.

Por ejemplo, en nuestro lenguaje contamos con los siguientes puntos estrictos:

1. **Los operandos de una operación unaria o binaria** $\texttt{(+ (- 2 3) (+ 3 4))}$ por ejemplo.
2. **Las condiciones de las expresiones `if0`**
3. **La función en la aplicación de funciones**: No confundir con el argumento de la función, si no el cuerpo de la función, de modo que podamos continuar con la evaluación.

Definimos entonces una función `strict` que nos permitirá aplicar los **puntos estrictos** de nuestro lenguaj. Esta función reduce la expresión recibida, en caso de ser un punto estricto, hasta llegar a un valor del lenguaje.

`strict` $NumV(n)$ = $NumV(n)$
`strict` $BooleanV(b)$ = $BooleanV(b)$
`strict` $<p,c,\varepsilon>$ = $<p,c,\varepsilon>$
`strict` $<a,\varepsilon>$ = `struct` $e \quad$ si $a,\varepsilon \Rightarrow e$

Con esta definición, debemos modificar nuestras reglas de semántica para que apliquen los puntos estrictos correspondientes:

### Semántica redefinida

**Operaciones unarias/binarias**:

$$
\begin{prooftree}
	\AxiomC{$i,\varepsilon \Rightarrow i'$}
	\AxiomC{$d,\varepsilon \Rightarrow d'$}
	\AxiomC{$strict(i')=NumV(i_v)$}
	\AxiomC{$strict(d')=NumV(d_v)$}
	\QuaternaryInfC{$Add(i,d),\varepsilon \Rightarrow NumV(i_v+d_v)$}
\end{prooftree}
$$

**Condicional `if0`**:

$$
\begin{prooftree}
	\AxiomC{$c,\varepsilon \Rightarrow c'$}
	\AxiomC{$strict(c') = NumV(0)$}
	\AxiomC{$t, \varepsilon \Rightarrow t_v$}
	\TrinaryInfC{$If0(c,t,e),\varepsilon \Rightarrow t_v$}
\end{prooftree}
$$

$$
\begin{prooftree}
	\AxiomC{$c,\varepsilon \Rightarrow c'$}
	\AxiomC{$strict(c') \neq NumV(0)$}
	\AxiomC{$e, \varepsilon \Rightarrow e_v$}
	\TrinaryInfC{$If0(c,t,e),\varepsilon \Rightarrow e_v$}
\end{prooftree}
$$

**Aplicaciones de función**:

$$
\begin{prooftree}
	\AxiomC{$f,\varepsilon \Rightarrow f'$}
	\AxiomC{$strict(f') = <p,c,\varepsilon'>$}
	\AxiomC{$c,\varepsilon [p \leftarrow \; <a,\varepsilon>] \Rightarrow c_v$}
	\TrinaryInfC{$App(f,a), \varepsilon \Rightarrow c_v$}
\end{prooftree}
$$

### Continuando el ejemplo

Nos quedamos en la expresión $\texttt{(if0 (+ (+ 4 4) (+ 4 4)) 1 2)}$, ahora que podemos aplicar puntos estrictos, la condición entonces corresponde a $16$, resultando en la expresión:

$$
\texttt{(if0 16 1 2)}
$$

La cual ya es evaluable, obteniendo el resultado esperado $2$.

# Enlaces

[<- Anterior](LPNota19.md) | [Siguiente ->](LPNota21.md)