[<- Índice](../LenguajesProgramacion.md)
# Sintaxis concreta

La especificación fromal de los lenguajes de programación permite asegurar la precisión, la correctud y la eficiencia tanto en el diseño como en la implementación y evolución de los lenguajes de programación.

Recordemos que la sintaxis de un lenguaje de programación se refiere a las reglas que definen cómo se deben estructurar y organizar los símbolos y palabras reservadas del lenguaje para formar programas correctos.

En otras palabras, estas reglas determinan qué combinaciones de caracteres y estructuras son aceptables y cuáles no.

La **sintaxis concreta** se refiere a la estructura específica de un lenguaje de programación que **exáctamente** cómo se deben escribir los programas.

Matemáticamente, esto se describe mediante una gramática formal que especifíca las reglas de formación para las secuencias válidas de símbolos en el lenguaje.
Esta especificación formal se divide normalmente en 2 niveles: La **sintaxis léxica** y la **sintaxis libre de contexto**.

Ejemplificaremos estos conceptos sobre un subconjunto del lenguaje de programación *Lisp* reducido al que llamaremos *MiniLips*.

## MiniLisp

**Lips** (*List Processing*) es un lenguaje de programación de alto nivel diseñado inicialmente por *John McCarthy* en 1958 para el procesamiento de datos simbólicos.

*Lisp* se caracteriza por el uso extensivo de *s-expressions* (expresiones simbólicas), su enfoque en la manipulación de listas como estructuras de datos formales, su flexibilidad y su uso en Inteligencia Artificial.

Es considerado como el primer lenguaje de programación funcional de la historia.

Utilizaremos específicamente el subconjunto *MiniLisp* por varias razones pedagógicas y técnicas:

- **Simplicidad y uniformidad en su sintaxis** : Nos ayudara a enfocarnos unicamente en los conceptos de diseño y programación.

- **Introducción a la programación funcional**

- **Historia y contexto** : Proporcionara comprensión del desarrollo de los distintos lenguajes de programació y como las ideas de este lenguaje han permeado otros lenguajes.

- **Árboles de sintaxis y compiladores** : Es ideal para enseñar sobre ==árboles de sintaxis abstracta== y otros conceptos introductorios de Compiladores. Podras escribir intérpretes y compiladores para *MiniLisp* de forma relativamente sencilla.

- **Exploración de estilos** : Soporta múltiples estilos (funcional, estructurado y orientado a objetos) lo que permite experimentar y comparar distintos estilos de programación dentro del mismo lenguaje.

### MiniLisp : Expresiones aritméticas

Primero, empecemos por la definición de *s-expressions*:

> Definimos una ***s-expression** S de forma recursiva como sigue:
> 
> - Si $a \in ATOM$, entonces $a \in S$
> - Si $e_1,e_2,e_3,\cdots,e_n \in S$  para  $n \geq 1$ entonces $(e_1,e_2,e_3,\cdots,e_n) \in S$
> - Son todas

Una *s-expression* es entonces una notación utilizada en *MiniLisp* para representar datos y código de manera uniforme y estructurada.

Prácticamente, una *s-expression* puede ser un átomo o una lista de *s-expressions* entre parentesis, delimitados por comas.

Un átomo puede ser:

- Un **símbolo** : Una secuencia de caracteres que representa una variable, función, etc.

- Un **número** : Un entero, número de punto flotante, etc.

- Una **cadena de texto** : Una secuencia de caracteres entre comillas.

Para esta primera versión de *MiniLisp* nuestros átomos serán números enteros y los operadores de suma (+) y resta (-), permitiendonos generar *s-expressions* de la forma:

- 1729
- (+ 3 4)
- (- (+ 10 2) -8)

Hay que considerar que *MiniLisp* se utiliza la ==notación prefija==, es decir, los operadores a la izquierda de los operandos.

## Sintaxis léxica

La sintaxis léxica se refiere a la estructura de los *tokens* (*lexemas*) básicos de un lenguaje de programación que son las unidades mínimas de significado, tales como palabras reservadas, identificadores, literales, operadores y delimitadores.

Formalmente, la sintaxis léxica se define utilizando **expresiones regulares** y **automatas finitos**.

### Definición formal

Sean los siguientes componentes:

- **Alfabeto ($\Sigma$)** : Un conjunto finito de símbolos o caracteres que se pueden utilizar en el lenguaje, incluyendo letras, dígitos y caracteres especiales.

- **Tokens** : Secuencias de caracteres que forman unidades léxicas básicas del lenguaje. Cada tipo de *token* puede ser descrito por una ==expresión regular==.

> Una **expresión regular** E sobre un alfabeto $\Sigma$ se define recursivamente como:
> 
> - $\varepsilon$ (la cadena vacía) es una expresión regular
> - Para cualquier símbolo $a \in Sigma$, $a$ es una expresión regular que representa la cadena que contiene solo $a$.
> - Si $E_1$ y $E_2$ son expresiones regulares, entonces $E_1E_2$ (concatenación) es una expresión regular
> - SI $E_1$ y $E_2$ son expresiones regulares, entonces $E_1 + E_2$ (unión) es una expresión regular
> - Si $E$ es una expresión regular, entonces $E^*$ (cerradura de Kleene) es una expresión regular, representando a cero o más repeticiones de $E$.

### Ejemplo: Sintaxis léxica de MiniLisp

Para las expresiones mencionadas antes en *MiniLisp* podemos proponer las siguientes expresiones regulares:

Por supuesto, el alfabeto propuesto es:

$\Sigma = \{0,1,2,3,4,5,6,7,8,9,-,+,(,)\}$

Para los **paréntesis**: Unicamente son 2 posibilidades, el paréntesis que abre y el que cierra

> $( \; + \; )$

Para los **operadores aritméticos**: Únicamente son 2 posibilidades, el operador de suma y el de resta

> $'+' \quad + \quad '-'$

Para los **números enteros**: Para generar números enteros, necesitamos establecer algunos conjuntos de dígitos primero

> $D = \{0..9\}$
> $Z = D - \{0\}$
> $ZD^* + -ZD^*$

Asi nos aseguramos que los números inicien con un dígito distinto de 0, excluyendo números como "01" del lenguaje.

---

Por supuesto, al tratarse de expresiones regulares, pueden ser convertidas en **autómatas finitos deterministas** para su procesamiento.

Decimos entonces, que la sintaxis léxica se define formalmente con expresiones regulares que describen a los *tokens*.
Esta estructura formal permite que los ==*analizadores léxicos*== (*lexers*) procesen el código fuente y lo dividan en *tokens* que luego pueden ser procesador por el ==*analizador sintáctico*== (*parser*).

## Sintaxis libre de contexto

La sintaxis libre de contexto se refiere a la estructura de un lenguaje de programación en la que las reglas de formación de sus ==sentencias== se pueden describir mediante una ==gramática libre de contexto== (*GLC* o *CFG*).

> Una **gramática libre de contexto** se define como una *4-tupla* $G=(N,\Sigma,P,S)$, donde:
>
> - $N$ es un conjunto finito de **simbolos no terminales** (o **variables**). Estos símbolos representan categorías sintácticas y pueden ser descompuestos en otras categorías o en símbolos terminales.
>
> - $\Sigma$ es un conjunto finito de **símbolos terminales** (o **constantes**). Estos símbolos básicos del lenguaje, que aparecen en las sentencias del lenguaje (por ejemplo, palabras reservadas, operadores, identificadores, etc).
>
> - $P$ es un conjunto finito de **reglas de producción**. Cada regla de producción tiene la forma $A \rightarrow \alpha$, donde $A \in N$ y $\alpha \in (N \cup \Sigma)^*$
>
> - $S$ es el **símbolo inicial**, tal que $S \in N$ desde el cual se empieza la derivación de las cadenas del lenguaje.

### Sintaxis libre de contexto de MiniLisp

La regla de escritura para combinar los tokens en esta versión de nuestro lenguaje son simples:

- Toda expresión debe ir delimitada por paréntesis
- Se usa notación prefija
- Las operaciones son binarias

Con esto podemos definir entonces la gramática como sigue:

> $S \rightarrow E$

> $E \rightarrow N$
> $E \rightarrow (+ \quad E \quad E)$
> $E \rightarrow (- \quad E \quad E)$

> $N \rightarrow 0$
> $N \rightarrow 1$
> $N \rightarrow 2$
> $\cdots$

> $N \rightarrow -1$
> $N \rightarrow -2$
> $\cdots$

> $N \rightarrow 1N$
> $N \rightarrow 2N$
> $\cdots$

> $N \rightarrow -1N$
> $N \rightarrow -2N$
> $\cdots$

Aunque esta forma de describir la gramática es totalmente formal y correcta, evitando las ambigüedades, se considera dificil de leer y tediosa de escribir.
Esto se debe a que es una notación de naturaleza matemática.

Debido a esto, entre 1950 y 1960, John Backus y Peter Naur inventan la notación *BNF* (*Backus Naur Form*) la cual surgió como una solución a la necesidad de una manera más clara y precisa de definir la sintaxis de los lenguajes de programación.

### Notación BNF

La notación *BNF* es una notación formal para describir las sintaxis de lenguajes formales.
Destaca por su capacidad de definir alternativas mediante el uso del operador |.

#### Componentes básicos de BNF

- **Variables**: Se escriben generalmente entre diamantes (<>).
- **Constantes**: Se representan igual que en la notación clásica.
- **Reglas de producción**: Se representan igual que en la notación clásica pero usando los símbolos $::=$ en lugar de $\rightarrow$.
- **Uso del operador |** :  El operador | se utiliza para especificar alternativas en las reglas de produciión, evitando escribir un renglón nuevo para cada regla. Esto significa que la variable del lado izquierdo puede expandirse en cualquiera de las alternativas proporcionadas.

#### Ejemplo: Sintaxis libre de contexto de MiniLisp en BNF

> \<S\> := \<Expr\> 
> \<Expr\> := \<Int\> |  (+ \<Expr\> \<Expr\>) | (- \<Expr\> \<Expr\>) 
> \<Int\> := \<N\> | -\<M\>
> \<D\> := 1 | 2 | ... | 9
> \<N\> := 0 |  \<D\>\<N\> | $\varepsilon$
> \<M\> := \<D\>\<N\>

Esta notación redujo significativamente la forma en que denotamos los lenguajes de programación, sin embargo, en la práctica es propensa a ambigüedades.

### Notación EBNF

Despues de la introducción de *BNF* en 1960, se reconoció la necesidad de una notación aún más poderosa que pudiera describir la sintaxis de los lenguajes de programación de una manera más concisa y legible.

Entre 1970 y 1980, varios investigadores y desarrolladores de lenguajes de programación comenzaron a proponer y utilizar extensiones de BNF.

Una de las primeras formalizaciones de *EBNF* (*Extended Backus Naur Form*) fue realizada por Niklaus Wirth.

#### Componentes básicos de EBNF

##### Repetición
En *EBNF* las secuencias que pueden repetirse cero o más veces se indican utilizando llaves ({}).
##### Opcionalidad
Las secuencias opcionales se indican utilizando corchetes (\[\]).
##### Agrupaciones
Las agrupaciones de elementos se indican usando paréntesis ().
##### Alternativas
Las alternativas se heredan de BNF mediante el operador |.

## Sintaxis concreta

La sintaxis concreta de un lenguaje de programación  puede formalizarse matemáticamente utilizando las definiciones de la sintaxis léxica y la sintaxis libre de contexto.

> La **sintaxis concreta** se puede definir como un par $(L, G)$ donde:
>
> - $L$ es ka defunición léxica representada por el conjunto de expresiones regulares $R$.
> - $G$ es la gramática libre de contexto $(N, \Sigma, P, S)$

Podemos pensar la sintaxis concreta como las secuencias de caracteres del alfabeto $\Sigma$ que se convierten en programas válidos del lenguaje. Esto se logra mediante los pasos siguientes:

- **Análisis léxico**: Definimos una función léxica $lexer : \Sigma^* \rightarrow [Token]$ que toma una cadena de caracteres y produce una secuencia de *tokens* según las expresiones regulares $R$.

- **Análisis sintáctico**: Definimos una función sintáctica $parser:[Token] \rightarrow AST$ que toma una secuencia de *tokens* y produce un ==árbol de sintaxis abstracta== (AST) según la gramática libre de contexto $G$[^1].

[^1]: SI el programa no respeta las reglas de sintaxis, este árbol no puede ser construido.

# Enlaces

[<- Anterior](LPNotaClase03.md) | [Siguiente ->](LPNotaClase05.md)