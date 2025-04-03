# Práctica 1
## Organización y Arquitectura

> Fernando Romero Cruz - *319314256*

#### Ejercicio 1

> Para la resolución del ejercicio 1, empecé por implementar **3 compuertas básicas**, el *NOT*, *AND* y *OR*, ya que el resto de las compuertas podían expresarse más **comodamente** a partir de estas.

- Para el *NOT* utilicé una constante **1** a parte de la entrada del *NAND* de modo que la entrada siempre resultara **negada**.
- Para el *AND*, utilicé un *NAND* normal seguido del *NOT* que acababa de guiar.
- Para el *OR*, utilicé mi *NOT* para negar las entradas, y redirigí estos resultados a un *NAND*.
- Con estas compuertas implementadas, pude crear *XOR* recreando la expresión *(a AND NOT b) OR (NOT a AND b)*, a partir de las compuertas que había implementado hasta el momento.
- *NOR* y *XNOR* simplemente fue combinar las respectivas compuertas con un *NOT*.

#### Ejercicio 2

> Para este ejercicio, simplemente definí un subcircuito *CHECK* idéntico a *XNOR* con el que me apoyé para revisar los resultados de mis compuertas con los de las compuertas reales. Todos los resultados los dirigí a un *AND* para asegurarnos que todos los *CHECK* se cumplan.