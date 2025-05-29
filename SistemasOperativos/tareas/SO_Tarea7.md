# Tarea 7

> Fernando Romero Cruz - 319314256

##  Secuencia de Referencias
##### Secuencia 1

Considera la siguiente secuencia y **FIFO** con 3 y 4 marcos

```
1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5
```

###### Simulación con 3 marcos:

```
[1] → F
[1,2] → F
[1,2,3] → F
[2,3,4] → F
[2,3,4] (1 ya estaba) → F
[3,4,1] (2 entra, 1 sale) → F
[4,1,2] (5 entra, 2 sale) → F
[1,2,5] (1 ya estaba) → H
[2,5,1] (2 ya estaba) → H
[5,1,3] (3 entra, 4 sale) → F
[1,3,4] (4 entra, 5 sale) → F
[3,4,5] (5 entra, 1 sale) → F
```

> **Fallos de página: 9**

###### Simulación con 4 marcos:

```
[1] → F
[1,2] → F
[1,2,3] → F
[1,2,3,4] → F
[1,2,3,4] (1 ya estaba) → H
[1,2,3,4] (2 ya estaba) → H
[2,3,4,5] (5 entra, 1 sale) → F
[3,4,5,1] (1 entra, 2 sale) → F
[4,5,1,2] (2 entra, 3 sale) → F
[5,1,2,3] (3 entra, 4 sale) → F
[1,2,3,4] (4 entra, 5 sale) → F
[2,3,4,5] (5 entra, 1 sale) → F
```

> **Fallos de página: 10**

---

##### Secuencia 2

Considera la siguiente secuencia y 3 marcos para **LRU**, **Second Chance** y **OPT**

```
2, 3, 2, 1, 5, 2, 4, 5, 3, 2, 5, 2
```

###### Simulación con LRU:

```
[2] → F
[2,3] → F
[2,3] (2 ya estaba) → H
[2,3,1] → F
[3,1,5] (5 entra, 2 LRU) → F
[1,5,2] (2 entra, 3 LRU) → F
[5,2,4] (4 entra, 1 LRU) → F
[2,4,5] (5 ya estaba) → H
[4,5,3] (3 entra, 2 LRU) → F
[5,3,2] (2 entra, 4 LRU) → F
[3,2,5] (5 ya estaba) → H
[3,2,5] (2 ya estaba) → H
```

> **Fallos de página: 8**

###### Simulación con Second Chance:

```
[2] → F
[2,3] → F
[2,3] (2 acceso → bit=1) → H
[2,3,1] → F
[3,1,5] (2 tiene bit=1 → se le da segunda oportunidad, entra 5) → F
[1,5,2] → F
[5,2,4] → F
[2,4,5] → H
[4,5,3] → F
[5,3,2] → F
[3,2,5] → H
[3,2,5] → H
```

> **Fallos de página: 8**

```
[2] → F
[2,3] → F
[2,3] → H
[2,3,1] → F
[3,1,5] (5 entra, 2 se usa más lejos) → F
[1,5,2] (2 entra, 3 nunca más) → F
[5,2,4] → F
[2,4,5] → H
[4,5,3] → F
[5,3,2] → F
[3,2,5] → H
[3,2,5] → H
```

> **Fallos de página: 8**

---

## Preguntas

1. ¿Qué es la **hiperpaginación**?

> La hiperpaginación (thrashing) es una situación donde el sistema operativo pasa más tiempo intercambiando páginas entre la memoria y el almacenamiento secundario que ejecutando procesos. Sucede cuando hay escasa memoria disponible y se producen muchos fallos de página.


2. ¿Qué es la **anomalía de Belady**?

> Es un fenómeno en el que aumentar la cantidad de marcos asignados a un proceso puede provocar un **mayor número de fallos de página**, algo que contradice la intuición. Se observa principalmente con algoritmos como FIFO.


3. ¿Qué es la **stack property**?

> Es una propiedad de los algoritmos de reemplazo como LRU y OPT donde el conjunto de páginas en memoria con `n` marcos es **siempre un subconjunto** de las páginas con `n+1` marcos. Esto asegura que **a mayor número de marcos, no aumentan los fallos de página**.


4. ¿Qué es un **TLB (Translation Lookaside Buffer)**?

> Es una memoria caché especial dentro de la MMU que almacena traducciones recientes de direcciones virtuales a físicas. Su objetivo es **acelerar el proceso de traducción**, evitando consultar la tabla de páginas completa para cada acceso a memoria.