[[Logica Computacional|<- Índice]]
# Conjuntos inductivos

### Definiciones inductivas

##### Definiciones "circulares"

Es común definir un conjunto de manera "circular". Por ejemplo, las fórmulas del cálculo de proposiciones:

> Si $\varphi$ y $\psi$ son proposiciones, entonces $\; \neg \varphi \;$ y $\; \varphi \lor \psi \;$ son proposiciones

Si la ==definición== anterior fuera realmente circular, sería incorrecta. En realidad, el conjunto anterior está ==definido de manera ***inductiva***==. Una definición inductiva parte de un conjunto básico y de un conjunto de funciones para generar nuevos elementos.

***Nota:***
En algunos textos se llama ***recursivas*** a estas definiciones. Esto es un error terminológico.

##### Conjuntos inductivos

Sea $A \subseteq U$ un conjunto y sea $F = \{f_i^n : U^n \rightarrow U \}$ un conjunto de funciones. Diremos que **A es cerrado bajo F** si y solo si:

> $\forall f_k^n \in F$ y $\forall a_1,a_2,\cdots,a_n \in A$ resulta que $f_k^n(a_1,a_2,\cdots,a_n) \in A$

Sea $X \subseteq U$, entonces un conjunto $Y \subseteq U$ es **inductivo en X bajo F** si y solo si:

> $X \subseteq Y$ y Y es cerrado bajo F

### Cerradura inductiva

Sea $X \subseteq U$ y sea $F=\{f_i^{n} : U^{n} \rightarrow U\}$ un conjunto de funciones.

La ***cerradura inductiva de X bajo F***, construida de abajo hacia arriba, es:

> $$X_0 = X$$
> $$X_{i+1} = X_i \cup \{f_k^{n}(x_1,x_2, \cdots, x_n)\} \; | \; f_k^n \in F \land x_1,x_2,\cdots,x_n \in X_i$$
> $$X_{+} = \bigcup_{i \in \mathbb{N}} X_i$$

Sea $\chi$ la familia de conjuntos inductivos en X bajo F. La cerradura inductiva de X bajo F, construida de arriba hacia abajo es el conjunto:

> $$X^{+} = \bigcap_{Y \in \chi} Y$$

##### Equivalencia

Ambas definiciones son equivalentes, es decir $X_+$ = $X^+$

**Demostración**. Es claro que $X_+$ es inductivo en $X$ y como $X^+$ es la intersección de los conjuntos inductivos en $X$, se tiene que $X^+ \subseteq X_+$.

La otra contención se demostrara por inducción matemática:

- Caso base
  $X_0 = X \subseteq X^+$

- Hipótesis inductiva
  $X_i \subseteq X^+$

- Por demostrar que $X_{i+1} \subseteq X^+$
  Pero esto último se sigue del hecho de que X⁺ es cerrado.

Por inducción, $\forall n \in \mathbb{N} \quad X_n \subseteq X^+$. Entonces $X_+ \subseteq X^+$

###### Ejemplo

Ahora podemos definir el cálculo de proposiciones inductivamente.
Sea $\Sigma = \{p,q,r,p_1,\cdots\} \cup \{(,)\} \cup \{\lor,\neg\}$ un alfabeto y sean $\neg \varsigma : \Sigma^* \rightarrow \Sigma^*$ y  $\lor \varsigma : \Sigma^* \times \Sigma^* \rightarrow \Sigma^*$
las funciones definidas asi:

> $\neg \varsigma (\alpha) = \neg \alpha$

> $\lor \varsigma (\alpha , \beta) = (\alpha \lor \beta)$

Sea $P_a = \{ p, q, r, p_1, \cdots \}$ el conjunto básico. El conjunto de
fórmulas del cálculo de proposiciones *P*, se define usando los dós métodos:

> $$P_0 = P_a$$
> $$F = \{\neg \varsigma, \lor \varsigma \} \quad n \in \{1,2\}$$
> $$P_{i+1} = P_i \cup \{f_i^n(x_1, \cdots, x_n)\} \; | \; f_i^n \in F, \; x_1, \cdots, x_n \in P_i$$
> $$P = \bigcup_{i \in \mathbb{N}} P_i$$

---
# Relaciones bien fundadas

### Relaciones binarias

Si 2 elementos a, b de un conjunto están en una relación R se escribirá "$(a,b) \in R$",      "aRb" o " R(a,b) ".

Sea A un conjunto y sea $R \subseteq A \times A$. Se dice que R es:

- **Reflexiva** sii $\forall a \in A \; R(a,a)$

- **Simétrica** sii $\forall a,b \in A \; R(a,b) \Rightarrow R(b,a)$

- **Antisimétrica** sii $\forall a,b \in A \; R(a,b) \land R(b,a) \Rightarrow a=b$

- **Transitiva** sii $\forall a,b,c \in A \quad R(a,b) \land R(b,c) \Rightarrow R(a,c)$

- **Total** sii $\forall a,b \in A \quad a \neq b \Rightarrow R(a,b) \lor R(b,a)$

##### Cerraduras reflexivas y transitivas

Dada una relación R en A x A se puede construir sus cerraduras **transitiva** y **reflexiva y transitiva**, denotadas por R⁺ y R^*, respectivamente:

> $$R^0 = \{(a,a) \; | \; a \in A\}$$
> $$R_1 = R$$
> $$R^{n+1} = R^n \cup \{(a,c) | \exists b \quad (a,b) \in R^n \land (b,c) \in R^n\}$$
> $$R^+ = \bigcup_{n \in \mathbb{N}} R^{n+1}$$
> $$R^* = \bigcup_{n \in \mathbb{N}} R^n$$

##### Órdenes

En matemáticas hay un tipo de relaciones muy comunes llamadas órdenes. Sin embargo, los órdenes vienen en distintos sabores.

- Una relación R es un ==**pre-orden**== (o **cuasi-orden**) sii es **reflexiva** y **transitiva**

- Un ==**orden parcial**== (o *poset*) es una relación **reflexiva**, **transitiva** y **antisimétrica**

- Un ==**orden total**== o *lineal* es simplemente una **relación total**.
	Observese cómo no se supone que tenga ninguna otra propiedad

###### Ejemplos

- El orden parcial más conocido es $\leq$ en $\mathbb{N}$

- En el mismo conjunto, < es un orden total

- Un ejemplo de pre-orden en $P(\mathbb{N})$ es el ==**preorden inferior o de Hoare**==:

  $$A \sqsubseteq_L B \quad \iff \quad \forall a \in A. \; \exists b \in B. \; a \leq b$$

Se usaran con frecuencia los simbolos $\prec$ y $\succ$ para denotar relaciones de orden arbitrarias.

##### Órdenes en n-adas

Una relación de orden en $A$ se puede extender a $A^n$ en dos formas:

###### Órden lexicográfico

La relación $\prec_{lex} \; \subseteq (A \times A) \times (A \times A)$ se define asi:

> $(a,b) \prec_{lex} (a',b') \quad$ sii $\quad a \prec a'$ y $a \neq a' \quad$ o $\quad a=a'$ y $b \prec b'$

La definición anterior se extiende a $A^n$ de la misma forma en que se extiende el
concepto de par ordenado a n-adas arbitrarias.

###### Órden por coordenadas

$\prec_{coor}$ es la siguiente relación:

$(a,b) \prec_{coor} (a',b') \quad$ sii $\quad a \prec a'$ y $b \prec b'$,

que se extiende del miso modo a n-adas arbitrarias.

##### Máximos, mínimos, etc

Conviene dar nombre a algunos elementos especiales de un conjunto ordenado:

Sea $\prec$ una relación en AxA y sea $X\subseteq A$. Se dice que $x \in X$ es un:

- mínimo sii $\quad \forall y \in X \quad x \neq y \Rightarrow x \prec y$

- máximo sii $\quad \forall y \in X \quad x \neq y \Rightarrow y \prec x$

- minimal sii $\quad \neg \exists y \in X \quad x \neq y \land y \prec x$

- maximal sii $\quad \neg \exists y \in X \quad x \neq y \land x \prec y$

###### Un caso extraño

En un orden parcial, un mínimo es un minimal, pero esto no es necesariamente el caso en un preorden. Más adelante se verán otros órdenes en los que los mínimos son siempre minimales.

### Relaciones bien fundadas

##### Cadenas

Sea $\prec$ un orden en $U \times U$ y sea $C \subseteq U$

Decimos que C es una cadena sii $\forall x,y \in C$ es el caso que $x \prec y$ o $y \prec x$

Una cadena descendente infinita es una cadena C tal que $\forall x \in C$, $\exists y \in C$ tal que $y \prec x$

De manera dual, una cadena infinita ascendente C tiene la propiedad de que $\forall x \in C$, $\exists y \in C$ tal que $x \prec y$

Obsérvese cómo una cadena $C \subseteq U$ es un subconjunto de U que visto de manera aislada forma un orden total.

##### Órdenes bien fundados

Las cadenas descendentes nos permiten caracterizar los órdenes bien fundados.

Sea $\prec \; \subseteq U \times U$ una relación. Decimos que $\prec$ es **bien fundado** sii no existen cadenas infinitas descendentes en U.

Por ejemplo, < en los naturales.

Sea $\prec$ una relación bien fundada. Entonces:

- $\prec$ no es reflexiva ni simétrica

- Su cerradura transitiva es bien fundada

- Los órdenes lexicográficos y por coordenadas basados en $\prec$ son bien fundadas

- $\prec^*$ es un orden parcial

###### Relaciones bien fundadas alternativo

Una relación $\prec$ es bien fundada en $A \times A$ sii todo $X \subseteq A \; (X \neq \varnothing)$ tiene un minimal.

### Principio de inducción

Sea *P* una propiedad de elementos de A y sea $\prec \; \subseteq A \times A$. La siguiente afirmación se conoce como el principio de inducción:

$(\forall a \in A \quad P(a)) \iff (\forall a \in A (\forall b \in A \quad b \prec a \Rightarrow P(b)) \Rightarrow P(a))$

Este principio se aplica en conjuntos donde $\prec$ es un orden bien fundado:

Sea $\prec \; \subseteq A \times A$ una relación bien fundada y sea *P* : $A \Rightarrow \{V,F\}$ un predicado. Entonces el principio de inducción vale

##### Demostraciones por inducción

El principio de inducción nos da una estrategia para demostrar una propiedad *P* en un conjunto A con una relación bien fundada $\prec$

1. Se demuestra que todos los elementos mínimos cumplen *P*

2. Se asume una hipótesis inductiva
   > $$\forall b \in A \quad b \prec a \Rightarrow P(b)$$

3. Se demuestra que entonces $P(a)$
---
# Recursión

### Funciones recursivas

##### Definiciones de funciones

Una forma muy común de definir funciones en conjuntos es por enumeración. Por ejemplo, la función identidad en el conjunto {1,2} se puede expresar como un conjunto de pares ordenados: {(1,1),(2,2)}.

No obstante, con conjuntos de gran tamaño esto es poco práctico.

Cuando contamos con una regla que nos permite calcular el valor de una función en un elemento dado es mejor usar una ecuación, por ejemplo:

$$f(x) = x^2$$

o varias ecuaciones

$$0! = 1$$
$$(n+1)! = n! \times (n+1)$$

##### Funciones recursivas

El último ejemplo ilustra además una posibilidad más: las ==definiciones recursivas==, donde la función definida aparece del lado izquierdo y derecho de la ecuación.
Sin embargo, es muy fácil cometer "errores" en las definiciones recursivas: $g(x) = g(x+1)$ es una "función" que nunca nos da un resultado.

Este ejemplo, es muy obvio, pero en general no es posible decir si una función recursiva con dominio y contradominio en los naturales da siempre un resultado o no.

Un problema adicional es cuando se puede obtener más de un resultado, dependiendo del orden en que se apliquen las ecuaciones que definen la función.

###### Definiciones correctas

Aunque no hay condiciones necesarias para saber si una función recursiva está bien definida, la definición y el teorema siguientes nos dan condiciones suficientes.

El concepto clave es la generación libre de un conjunto inductivo.

### Generación libre

Sean A un conjunto, $X \subseteq A$ y $F = \{ f_i^n : A^n \rightarrow A \}$ un conjunto de funciones. Se dice que $X_+$ (o $X^+$) es generado libremente por $X$ y $F$ sii se cumplen las siguientes condiciones:

1. La restricción de toda $f^n \in F$ a $X_+^n$ es inyectiva

2. Para todas $f_k^m, f_i^n \in F$, si $f_k^m \neq f_i^n$ entonces $f_k^m(X_+) \cap f_i^n(X_+) = \varnothing$

3. Los elementos de X son realmente básicos, es decir, para toda $f_i^n \in F$ y para todas $x_1,...,x_n \in X_+$ se tiene que $f_i^n(x_1,...,x_n) \notin X$

##### Generación libre y homomorfismos

Este teorema permite definir funciones recursivas en conjuntos generados libre e inductivamente:

Sea $X_+$ un conjunto generado libremente por $X$ y $F$ y sea $B \neq \varnothing$ un conjunto acompañado por las funciones $G = \{ g_i^n: B^n \rightarrow B \}$.

Sea $\delta : F \rightarrow G$ una función tal que $\delta (f_i^n) = g_i^m$ implica que $n=m$.

Finalmente, sea $h : X \rightarrow B$ una función. Entonces, existe una única $h': X_+ \rightarrow B$ tal que:

1. Para todo $x \in X$. $\quad h'(x) = h(x)$

2. $h'(f_i^n (x_1, \cdots, x_n)) = g_k^m(h'(x_1), \cdots, h'(x_n))$

###### Un lema útil

Sea $X_+$ un conjunto generado libre e inductivamente por $X$ y $F$. Para toda $f_k^n \in F$, si $x_1,...,x_n \in X_i$ , entonces $f_k^n (x_1, ... , x_n) \notin X_i$

[[Cálculo de proposiciones|Siguiente ->]]