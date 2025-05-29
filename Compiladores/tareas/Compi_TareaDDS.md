# Traducción dirigida por sintaxis

### Producciones:

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


La única producción con recursividad izquierda es:

```txt
K -> K_1 , J | J
```

Transformamos a:

```
K  → J K'
K' → , J K' | ε
```

2. **Ajuste de atributos sintetizados y heredados**. Reorganizar los atributos de la gramatica para garantizar su correcto funcionamiento despues de eliminar la recursividad izquierda, manteniendo la semántica original.

### Producciones modificadas:

```
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

Utilizando la gramática transformada:

```markdown
funcion F():
    emparejar(id)
    emparejar('(')
    P()
    emparejar(')')
    Guardar(P.lista)

funcion P():
    si token_actual != ')':
        K()
        P.lista = K.lista
    sino:
        P.lista = nulo

funcion K():
    J()
    lista_actual = nuevaLista()
    agregar(lista_actual, J.id, J.tipo)
    K_prime()
    K.lista = lista_actual

funcion K_prime():
    si token_actual == ',':
        emparejar(',')
        J()
        agregar(lista_actual, J.id, J.tipo)
        K_prime()

funcion J():
    B()
    emparejar(id)
    J.id = id
    J.tipo = B.tipo
```

4. **Esquema de traduccion para análisis ascendente**. Utilizando la definición gramatical original (antes de eliminar la recursividad), construir el esquema de traduccion apropiado para el análisis sintáctico ascendente (bottom-up).

Usamos la gramática original:

Producciones + Acciones:

```
F → id ( P )                     { Guardar(P.lista) }
P → K                          { P.lista = K.lista }
P → ε                          { P.lista = nulo }
K → K1 , J                     { agregar(K1.lista, J.id, J.tipo); K.lista = K1.lista }
K → J                          { K.lista = nuevaLista(); agregar(K.lista, J.id, J.tipo) }
J → B id                       { J.id = id; J.tipo = B.tipo }
```