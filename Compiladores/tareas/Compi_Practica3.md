Para la gramática G = ( N, Σ, P, S), descrita por las siguientes producciones: 

> P = {
>> programa → declaraciones sentencias <br>
>> declaraciones → declaraciones declaracion | declaracion <br>
>> declaracion → tipo lista-var **;** <br>
>> tipo → **int** | **float** <br>
>> lista_var → lista_var **,** _**identificador**_ | _**identificador**_ <br>
>> sentencias → sentencias sentencia | sentencia <br>
>> sentencia → _**identificador**_ **=** expresion **;** | **if** **(** expresion **)** sentencias **else** sentencias | **while** **(** expresión **)** sentencias <br>
>> expresion → expresion **+** expresion | expresion **-** expresion | expresion __\*__ expresion | expresion **/** expresión | _**identificador**_ | **_numero_** <br>
>> expresion → **(** expresion **)** <br>
 }

1. Determinar los conjuntos _N_, _Σ_ y el símbolo inicial _S_.  (0.5 pts.)

El conjunto *N* de símbolos no terminales está dado por: _{declaraciones,declaración,tipo,lista-var,sentencias,sentencia,expresión}_.

El conjunto _Σ_ esta dado por: _{int,float, `,` ,identificador,=,if,else,while,(,), ; ,+,-,*,/,0-9}_.

El símbolo inicial _S_ es el no terminal: _programa_.

2. Mostrar el proceso de eliminación de ambigüedad o justificar, en caso de no ser necesario. (1 pts.).

Para la eliminación de la ambigüedad, tenemos que enfocarnos principalmente en la derivación de las expresiones, pues éstas pueden crear expresiones iguales, por ejemplo, _a+b*c_.
Para esto, podemos ajustar un poco las reglas introduciendo 2 nuevos símbolos no-terminales, _expresion-suma_ y _expresion-producto_. Estas las definimos de la siguiente manera:

_expresión_: _expresión-suma_ | _expresión-producto_ 

_expresion-suma_: _expresión-suma_ **+** _expresión-suma_ | _expresión-suma_ **+** _expresión-producto_ | _expresión-producto_ **+** _expresión-suma_ | _expresión-producto_ **+** _expresión-producto_ | _expresión-suma_ **-** _expresión-suma_ | _expresión-suma_ **-** _expresión-producto_ | _expresión-producto_ **-** _expresión-suma_ | _expresión-producto_ **-** _expresión-producto_

_expresión-producto_: _expresión-producto_ **\*** _expresión-producto_ | _expresión-producto_ **/** _expresión-producto_ | **identificador** | **número**

3. Mostrar el proceso de eliminación de la recursividad izquierda o justificar, en caso de no ser necesario. (1 pts.)

La definición del símbolo _declaraciones_, _sentencias_ y _lista\_var_ son definiciones recursivas izquierdas, de modo que para eliminar estas recursividad debemos introducir un nuevo símbolo terminal _lista\_var'_ de modo que podamos replantear las siguientes reglas:

_lista\_var_: **identificador** _lista\_var'_
_lista\_var'_: **, identificador** _lista\_var'_ | _ε_

De manera análoga:

_declaraciones_: _declaracion_ _declaraciones'_
_declaraciones'_: _declaracion_ _declaraciones'_ | _ε_

_sentencias_: _sentencia_ _sentencias'_
_sentencias'_: _sentencia_ _sentencias'_ | _ε_

4. Mostrar el proceso de factorización izquierda o justificar, en caso de no ser necesario. (1 pts.)

En este caso, las reglas de _expresión_ son aquellas que nos dan problemas. Hay que adecuarlas de manera similar a la **eliminación de la recursividad izquierda** para evitar las ambigüedades en las reglas. Partiendo de mi planteación anterior podemos mejorarlas a:

expresion → expresion-suma
expresion-suma → expresion-producto expresion-suma'
expresion-suma' → **+** expresion-producto expresion-suma' | **-** expresion-producto expresion-suma' | ε
expresion-producto → **(** expresión **)** | **identificador** | **numero** | expresion-producto **\*** expresion-producto | expresion-producto **/** expresion-producto

De este modo, factorizamos por la izquierda todos los símbolos en común que podían causar ambigüedades en la derivación del lenguaje.

5. Mostrar los nuevos conjuntos _N_ y _P_. (0.5 pts.)

> P' ={
>> programa → declaraciones sentencias <br>
>> _declaraciones_: _declaracion_ _declaraciones'_
>> _declaraciones'_: _declaracion_ _declaraciones'_ | _ε_
>>
>> declaracion → tipo lista-var **;** <br>
>> tipo → **int** | **float** <br>
>> _lista\_var_: **identificador** _lista\_var'_
>> _lista\_var'_: **, identificador** _lista\_var'_ | _ε_
>>
>> _sentencias_: _sentencia_ _sentencias'_
>> _sentencias'_: _sentencia_ _sentencias'_ | _ε_
>>
>> sentencia → _**identificador**_ **=** expresion **;** | **if** **(** expresion **)** sentencias **else** sentencias | **while** **(** expresión **)** sentencias <br>
>> expresion → expresion-suma
>> expresion-suma → expresion-producto expresion-suma'
>> expresion-suma' → **+** expresion-producto expresion-suma' | **-** expresion-producto expresion-suma' | ε
>> expresion-producto → | **(** expresión **)** | **identificador** | **numero** | expresion-producto **\*** expresion-producto | expresion-producto **/** expresion-producto
 }

Entonces, el conjunto _N_ ahora es: _{declaraciones,declaraciones',declaración,tipo,lista\_var, lista\_var',sentencias,sentencias',sentencia,expresión,expresión-suma,expresión-suma',expresión-producto}_.

6. Modificar el main.py para que nuestro programa sea capaz de recibir archivos y no sólo cadenas. (2 pts.)

Listo

7. Implementar el Analizador Sintáctico (_analisis/sintactico.py_) de descenso recursivo, documentando las funciones de cada No-Terminal, de forma que el programa descrito en el archivo _tst/prueba.txt_ sea reconocido y aceptado por el analizador resultante. (4 pts.)

Listo