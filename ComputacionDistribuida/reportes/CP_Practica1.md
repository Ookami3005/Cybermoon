# README

### Algoritmo

Para este algoritmo, primero definí unas estructuras de datos auxiliares, una **lista** donde almacenaría el recorrido de los vértices y un **diccionario** donde almacenaría *Booleanos* con los vértices como llaves, representando si el vértice ya ha sido visitado en el transcurso del algoritmo.

Con eso dicho, procedí a introducir el vértice inicial en la cola y marcar su estado como visitado.

Mientras la cola no estuviera vacía, recuperaba los vecinos del vértice en la cabeza de la cola y mientras no hubieran sido previamente visitados (es decir, su estado como visitados fuera *False*), marcaba su estado como visitados en *True* y los almacenaba en la cola.
Estas intrucciones se repetirían hasta recorrer toda la gráfica.

Para el punto extra, decidí recaudar la información por medio de *Strings*, primero pedía un *String* donde cada caracter sería un vértice de la gráfica.

Posteriormente, requeriría los vecinos de cada nodo en la gráfica.
Cabe recalcar, que decidí manejar a los vecinos como un ==*Set*== de *Python* para evitar repiticiones y poder establecer vecindades bilateralmente.

Despues de validar que fueran vecinos válidos en la gráfica, construia la gráfica como un diccionario y pedía un vértice de inicio para partir en el algoritmo `BFS`.

### Ejecución

El programa puede ejecutarse (dentro de la carpeta del archivo *.py*) como:
> `python BFS.py`

Una vez ejecutado, primero desplegará el recorrido bajo `BFS` del diccionario del ayudante, después comenzara a requerir los datos para crear la gráfica presentando las reglas según sea el caso y algunos ejemplos.

Finalmente, si la gráfica recibida por el usuario se pudo construir, presentará el recorrido de esta bajo `BFS`.

**Nota**: La conversión a conjunto de las cadenas recibidas altera el orden de los caracteres en los conjuntos, sin embargo el algoritmo sigue siendo correcto.

---
**Autor**: Romero Cruz Fernando `319314256`