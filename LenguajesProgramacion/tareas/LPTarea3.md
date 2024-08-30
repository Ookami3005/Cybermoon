[<- Índice](../LenguajesProgramacion.md)
# Evaluación semanal 3

## Integrantes

- Castro Rendón Diego
- Márquez Corona Danna Lizette
- Romero Cruz Fernando

## Ejercicio 1: Evaluaciones

### a. `(-(+ 20 3)(- -18 (+ 50 20)))`

#### Sintaxis abstracta: 

> $ASA$: `Sub(Add(Num(20), Num(3)), Sub(Num(-18), Add(Num(50), Num(20))))`

#### Evaluación mediante Semántica natural:
Utilizando las reglas de inferencia dadas en clase para la semántica natural, proponemos las siguientes denotaciones para hacer más legible la evaluación:

Sea $e$ la expresión:  $Sub(Add(Num(20), Num(3)), Sub(Num(-18), Add(Num(50), Num(20))))$

Sea $p$ las derivaciones: 

$$
\begin{prooftree}
\AxiomC{$Num(20) \Rightarrow Num(20)$}
\AxiomC{$Num(3) \Rightarrow Num(3)$}
\BinaryInfC{$Add(Num(20), Num(3)) \Rightarrow Num(23)$}
\end{prooftree}
$$

Y sea $q$ las siguientes derivaciones:

$$
\begin{prooftree}
\AxiomC{$Num(-18) \Rightarrow Num(-18)$}
\AxiomC{$Num(50) \Rightarrow Num(50)$}
\AxiomC{$Num(20) \Rightarrow Num(20)$}
\BinaryInfC{$Add(Num(50), Num(20)) \Rightarrow Num(70)$}
\BinaryInfC{$Sub(Num(-18),Add(Num(50), Num(20))) \Rightarrow Num(-88)$}
\end{prooftree}
$$

Podemos concluir entonces:

$$
\begin{prooftree}
\AxiomC{p}
\AxiomC{q}
\BinaryInfC{$e \Rightarrow Num(111)$}
\end{prooftree}
$$

#### Evaluación mediante Semántica Estructural:
Especificamos los pasos de la evaluación, con las reglas vistas en clase, de la siguiente manera:

$Sub(Add(Num(20), Num(3)), Sub(Num(-18), Add(Num(50), Num(20))))$
$\rightarrow Sub(Num(23), Sub(Num(-18), Add(Num(50), Num(20))))$
$\rightarrow Sub(Num(23), Sub(Num(-18), Num(70)))$
$\rightarrow Sub(Num(23), Num(-88))$
$\rightarrow Num(111)$

---

### b. `(not (+ 1 (- 3 (+ -8 1))))`

#### Sintaxis Abstracta:

> $ASA$: `Not(Add(Num(1), Sub(Num(3), Add(Num(-8), Num(1)))))`

#### Evaluación mediante Semántica Natural:
Sea $e$ la expresión:
$Not(Add(Num(1), Sub(Num(3), Add(Num(-8), Num(1)))))$

Sea $p$ las derivaciones:

$$
\begin{prooftree}
\AxiomC{$Num(3) \Rightarrow Num(3)$}
\AxiomC{$Num(-8) \Rightarrow Num(-8)$}
\AxiomC{$Num(1) \Rightarrow Num(1)$}
\BinaryInfC{$Add(Num(-8), Num(1)) \Rightarrow Num(-7)$}
\BinaryInfC{$Sub(Num(3), Add(Num(-8), Num(1))) \Rightarrow Num(10)$}
\end{prooftree}
$$

Y sea $q$ las derivaciones:

$$
\begin{prooftree}
\AxiomC{$Num(1) \Rightarrow Num(1)$}
\AxiomC{p}
\BinaryInfC{$Add(Num(1), Sub(Num(3), Add(Num(-8), Num(1)))) \Rightarrow Num(11)$}
\end{prooftree}
$$

Entonces concluimos:

$$
\begin{prooftree}
\AxiomC{$q$}
\UnaryInfC{$e \Rightarrow Boolean(False)$}
\end{prooftree}
$$

#### Evaluación mediante Semántica Estructural:
La evaluación por semántica estructural se da de la siguiente manera:

$Not(Add(Num(1), Sub(Num(3), Add(Num(-8), Num(1)))))$
$\rightarrow Not(Add(Num(1), Sub(Num(3), Num(-7))))$
$\rightarrow Not(Add(Num(1), Num(10)))$
$\rightarrow Not(Num(11))$
$\rightarrow Boolean(False)$

---
### c. `(not (not (+ 3 5)))`

#### Sintaxis abstracta:

> $ASA$: `Not(Not(Add(Num(3), Num(5))))`

#### Evaluación mediante Semántica Natural:
$$
\begin{prooftree}
\AxiomC{$Num(3) \Rightarrow Num(3)$}
\AxiomC{$Num(5) \Rightarrow Num(5)$}
\BinaryInfC{$Add(Num(3), Num(5)) \Rightarrow Num(8)$}
\UnaryInfC{$Not(Add(Num(3), Num(5))) \Rightarrow Boolean(False)$}
\UnaryInfC{$Not(Not(Add(Num(3), Num(5)))) \Rightarrow Boolean(True)$}
\end{prooftree}
$$

#### Evaluación mediante Semántica Estructural:

$Not(Not(Add(Num(3), Num(5))))$
$\rightarrow Not(Not(Num(8)))$
$\rightarrow Not(Boolean(False))$
$\rightarrow Boolean(True)$

---
## Ejercicio 2. Extensión de batería de operaciones de *MiniLisp*

### a) Gramática libre de contexto en EBNF

`<programa>` $::=$ `<expresión>`

`<expresión>` $::=$ `<número>`
            | `<suma>`
            | `<resta>`
            | `<multiplicación>`
            | `<división>`
            | `<incremento>`
            | `<decremento>`
            | `<raíz>`

`<suma>` $::=$ "(+ " `<expresión>` `<expresión>` ")"
`<resta>` $::=$ "(- " `<expresión>` `<expresión>` ")"
`<multiplicación>` $::=$ "(* " `<expresión>` `<expresión>` ")"
`<división>` $::=$ "(/ " `<expresión>` `<expresión>` ")"
`<incremento>` $::=$ "(add1 " `<expresión>` ")"
`<decremento>` $::=$ "(sub " `<expresión>` ")"
`<raíz>` $::=$ "(sqrt " `<expresión>` ")"

`<número>` $::=$ `<N>`{`<D>`}
`<N>` $::=$ "1" | "2" | $\cdots$ | "9"
`<D>` $::=$ "0" | "1" | "2" | $\cdots$ | "9"

---
### b) Sintaxis abstracta

A continuación, describimos las nuevas construcciones de lenguaje que se incluyen en la sintaxis abstracta, correspondientes a las nuevas funciones del lenguaje.

##### - Multiplicación

$$
\begin{prooftree}
\AxiomC{$i \; ASA$}
\AxiomC{$d \; ASA$}
\BinaryInfC{$Mult(i, d) \; ASA$}
\end{prooftree}
$$
##### - División

$$
\begin{prooftree}
\AxiomC{$i \; ASA$}
\AxiomC{$d \; ASA$}
\BinaryInfC{$Div(i, d) \; ASA$}
\end{prooftree}
$$
##### - Incremento

$$
\begin{prooftree}
\AxiomC{$i \; ASA$}
\UnaryInfC{$Incr(i) \; ASA$}
\end{prooftree}
$$
##### - Decremento

$$
\begin{prooftree}
\AxiomC{$i \; ASA$}
\UnaryInfC{$Decr(i) \; ASA$}
\end{prooftree}
$$

##### - Raíz cuadrada

$$
\begin{prooftree}
\AxiomC{$i \; ASA$}
\UnaryInfC{$Sqrt(i) \; ASA$}
\end{prooftree}
$$

---
### c) Reglas de semántica
#### Semántica natural:

##### - Multiplicación

$$
\begin{prooftree}
\AxiomC{$i \Rightarrow Num(i')$}
\AxiomC{$d \Rightarrow Num(d')$}
\BinaryInfC{$Mult(i,d) \Rightarrow Num(i' \times d')$}
\end{prooftree}
$$

##### - División

$$
\begin{prooftree}
\AxiomC{$i \Rightarrow Num(i')$}
\AxiomC{$d \Rightarrow Num(d')$}
\BinaryInfC{$Div(i,d) \Rightarrow Num((\lfloor i' \div d' \rfloor)$}
\end{prooftree}
$$

##### - Incremento


$$
\begin{prooftree}
\AxiomC{$i \Rightarrow Num(i')$}
\UnaryInfC{$Incr(i) \Rightarrow Num(i'+1)$}
\end{prooftree}
$$

##### - Decremento

$$
\begin{prooftree}
\AxiomC{$i \Rightarrow Num(i')$}
\UnaryInfC{$Decr(i) \Rightarrow Num(i'-1)$}
\end{prooftree}
$$

##### - Raíz cuadrada

$$
\begin{prooftree}
\AxiomC{$i \Rightarrow Num(i')$}
\UnaryInfC{$Sqrt(i) \Rightarrow Num(\sqrt{i'})$}
\end{prooftree}
$$

---
#### Semántica estructural:

##### - Multiplicación

$$
\begin{prooftree}
\AxiomC{$i \rightarrow i'$}
\UnaryInfC{$Mult(i,d) \rightarrow Mult(i',d)$}
\end{prooftree}
$$

$$
\begin{prooftree}
\AxiomC{$d \rightarrow d'$}
\UnaryInfC{$Mult(Num(n),d) \rightarrow Mult(Num(n),d')$}
\end{prooftree}
$$

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Mult(Num(n),Num(m)) \rightarrow Num(n \times m)$}
\end{prooftree}
$$

##### - División

$$
\begin{prooftree}
\AxiomC{$i \rightarrow i'$}
\UnaryInfC{$Div(i,d) \rightarrow Div(i',d)$}
\end{prooftree}
$$

$$
\begin{prooftree}
\AxiomC{$d \rightarrow d'$}
\UnaryInfC{$Div(Num(n),d) \rightarrow Div(Num(n),d')$}
\end{prooftree}
$$

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Div(Num(n),Num(m)) \rightarrow Num(\lfloor n \div m \rfloor)$}
\end{prooftree}
$$

##### - Incremento

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Incr(Num(n)) \rightarrow Num(n+1)$}
\end{prooftree}
$$


$$
\begin{prooftree}
\AxiomC{$i \rightarrow i'$}
\UnaryInfC{$Incr(i) \rightarrow Incr(i')$}
\end{prooftree}
$$


##### - Decremento

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Decr(Num(n)) \rightarrow Num(n-1)$}
\end{prooftree}
$$

$$\begin{prooftree}
\AxiomC{$i \rightarrow i'$}
\UnaryInfC{$Decr(i) \rightarrow Decr(i')$}
\end{prooftree}
$$



##### - Raíz cuadrada

$$
\begin{prooftree}
\AxiomC{}
\UnaryInfC{$Sqrt(Num(n)) \rightarrow Num(\sqrt{n})$}
\end{prooftree}
$$

$$\begin{prooftree}
\AxiomC{$i \rightarrow i'$}
\UnaryInfC{$Sqrt(i) \rightarrow Sqrt(i')$}
\end{prooftree}
$$