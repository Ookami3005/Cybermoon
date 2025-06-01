# Traducción dirigida por sintaxis

> *Fernando Romero Cruz* - 319314256

### Producciones originales:

```txt
F -> id( P )                     => Guardar(P.lista)
P -> K                          => P.lista = K.lista
P -> ε                          => P.lista = nulo
K -> K_1 , J                     => agregar(K1.lista, J.id, J.tipo)
                                   K.lista = K1.lista
K -> J                          => K.lista = nuevaLista()
                                   agregar(K.lista, J.id, J.tipo)
J -> B id                       => J.id = id
                                   J.tipo = B.tipo
```

1. **Eliminacion de recursividad izquierda**. Transformar la gramática para remover cualquier recursividad por la izquierda presente en las producciones.


La única regla de producción con **recursividad izquierda** es:

```txt
K -> K_1 , J | J
```

Por lo que la transformamos a:

```
K  → J K'
K' → , J K' | ε
```

Incorporando esta nueva regla en su lugar la gramática ya no presenta recursividad izquierda.

2. **Ajuste de atributos sintetizados y heredados**. Reorganizar los atributos de la gramatica para garantizar su correcto funcionamiento despues de eliminar la recursividad izquierda, manteniendo la semántica original.

### Producciones modificadas:

```txt
F → id ( P )                     => Guardar(P.lista)
P → K                          => P.lista = K.lista
P → ε                          => P.lista = nulo
K → J K'                       => K.lista = K'.lista
K' → , J K'1                   => agregar(K'1.lista, J.id, J.tipo)
                                   K'.lista = K'1.lista
K' → ε                         => K'.lista = lista_actual
J → B id                       => J.id = id
                                   J.tipo = B.tipo
```

3. **Esquema de traduccion para análisis descendente**. Desarrollar el esquema de traducción correspondiente para el analisis sintáctico descendente (top-down) con la gramática modificada.

Utilizando la gramática transformada, desarrollamos la **estructura** de las funciones necesarias para llevar a cabo satisfactoriamente este **análisis descendente**.

Cada símbolo **no-terminal** es representado por una función, además se definen las funciones `consumir`, `nuevaLista` y `agregar` que modelan las **reglas semánticas** y la aceptación de símbolos **terminales**.

```txt
funcion F():
    consumir(id)
    consumir('(')
    P()
    consumir(')')
    Guardar(P.lista)

funcion P():
    if token_actual != ')':
        K()
        P.lista = K.lista
    else:
        P.lista = nulo

funcion K():
    J()
    lista_actual = nuevaLista()
    agregar(lista_actual, J.id, J.tipo)
    K_prima()
    K.lista = lista_actual

funcion K_prime():
    if token_actual == ',':
        consumir(',')
        J()
        agregar(lista_actual, J.id, J.tipo)
        K_prima()

funcion J():
    B()
    consumir(id)
    J.id = id
    J.tipo = B.tipo
```

4. **Esquema de traduccion para análisis ascendente**. Utilizando la definición gramatical original (antes de eliminar la recursividad), construir el esquema de traduccion apropiado para el análisis sintáctico ascendente (bottom-up).

Partiendo de la gramática original, debemos considerar la administración de la **pila** que podemos representar mediante la notación **\$i** para referirnos al **i-esimo** símbolo de la producción y **\$\$** para el atributo del símbolo a la **izquierda** de la producción.

Entonces, la nueva gramática se vería así:

```txt
F → id ( P )                  { Guardar($3.lista) }

P → K                        { $$ = {}; $$.lista = $1.lista }

P → ε                        { $$ = {}; $$.lista = nulo }

K → K1 , J                   { agregar($1.lista, $3.id, $3.tipo); $$ = {}; $$.lista = $1.lista }

K → J                        {
                                $$ = {};
                                $$.lista = nuevaLista();
                                agregar($$.lista, $1.id, $1.tipo)
                            }

J → B id                     {
                                $$ = {};
                                $$.id = $2;   // asume que $2 es el nombre del identificador
                                $$.tipo = $1.tipo
                            }

```