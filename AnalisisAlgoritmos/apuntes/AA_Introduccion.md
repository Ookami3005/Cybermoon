[<- Índice](../AnalisisAlgoritmos.md)
# Introducción

## Problemas y algoritmos

Empecemos por definir una serie de conceptos útiles:

**Algoritmo**: Es un ==procedimiento== para llevar a cabo determinada tarea.

Un **procedimiento computacional**  definido de forma precisa el cuál toma como **entrada** un valor o conjunto de valores y produce como **salida** un valor o conjunto de valores.

- Secuencia de instrucciones "elementales"
- Ejecutable mecánicamente
- Explícito, sin ambigüedad

Una definición alternativa de algoritmo sería:

**Algoritmo**: Herramienta para resolver un **problema computacional/algoritmico**.

**Problema**: Tarea a resolver especificada por el conjunto de instancias que recibe como entrada y la salida que debe producir al resolverse en una de dichas instancias.

- El problema a resolver especifica la relación **entrada/salida** en su enunciado.

---
##### Ejemplo: Problema algoritmico del ordenamiento

**Problema**: Ordenamiento

**Entrada**: Una secuencia de *n* valores $<a_1,a_2,\cdots,a_n>$

**Salida**: Una permutación (reordenamiento) $<a_1',a_2',\cdots,a_n'>$ de los valores de entrada tal que $a_{1}'\leq a_{2}' \leq a_{3}' \cdots \leq a_n'$

---

**Instancia**: Caso particular de la entrada de un problema

Por ejemplo, instancias del problema del ordenamiento:

- $\{3,5,28,1,4\}$
- $\{LXT, ASW, KNO, CER\}$

Otra definición alternativa de algoritmo:

**Algoritmo**: Procedimiento que toma cualquiera de las instancias de un problema y lo transforma en la salida deseada para la especificación del problema.

Un algoritmo es ==correcto== si, para cada instancia de entrada regresa la salida correcta.
Se dice que un algoritmo correcto **resuelve** el problema computacional.

> Pueden existir muchos algoritmos para resolver un mismo problema.

---

Los objetivos de este curso son:

> Tener un algoritmo correcto, eficiente en cuanto a complejidad computacional o espacial y fácil de implementar en un lenguaje de programación.

> La corrección y la complejidad de un algoritmo se establecen mediante una demostración (prueba)

> Los programas son ==representaciones particulares== de algoritmos, pero los algoritmos no son programas, sino técnicas independientes de cualquier lenguaje de programación.

## Tipos de problemas algorítmicos

La siguiente forma de clasificar los problemas nos ayuda a determinar que estrategia utilizar

- **Optimización**. Encontrar un mínimo o máximo valor (valor óptimo) para alguna función.
- **Decisión**: Responder preguntas de si o no acerca de que se cumpla cierta propiedad.
- **Enumeración**: Encontrar todos los elementos que cumplan cierta propiedad

> No todos los problemas caen en uno de estos 3 tipos principales. Por ejemplo: agregar un elemento a un conjunto es un **problema de salida general**.

### Problemas de optimización

Queremos encontrar los argumentos o parámetros de entrada de una función que resulten en una salida máxima o mínima de la función.

Por ejemplo:

- Encontrar una trayectoria hamiltoniana de longitud mínima.
- 2-suma máxima: Dado un conjunto de *n* numeros encontrar el par cuya suma sea máxima.
- Mínimo conjunto dominante: Dada una gráfica $G=(V,E)$ encontrar un subconjunto de tamaño mínimo de $V$, $V'$ tal que todo elemento de $V$ o esté en $V'$ o tenga un vecino en $V'$

### Problemas de decisión

Queremos responder sí/no sobre los valores de entrada. Podría tratarse de responder si un objeto cumple con cierta propiedad o no.

Un derivado de esta categoría son los problemas de existencia:
*¿Hay algún objeto que cumple cierta propiedad?*

Como respuesta podemos obtener un **testigo** (ejemplo que cumple la propiedad) o que el algoritmo nos diga que no existe.

- Problema del árbol: Dada una gráfica $G$ decidir si es o no un árbol.

- Dado un número decidir si es primo

### Problemas de enumeración

En este tipo de problemas nos interesa encontrar todos aquellos objetos que cumplan con cierta propiedad. Ya sea que únicamente nos interese saber cuántos son o que queramos enlistarlos

- Dado un conjunto de puntos en el plano, determinar cúantos están contenidos en su cierre convexo.
- Dado un conjunto parcialmente ordenado determinar todos sus elementos maximales.

## Complejidad

**Complejidad computacional o tiempo de ejecución**: Número de instrucciones

La complejidad computacional es una medida de la ==cantidad de trabajo== que necesitamos para resolver un problema.

También se utiliza el concepto para describir la máxima cantidad de operaciones que un algoritmo necesita realizar para resolver un problema (que tan "óptimo" es).

Nos ayuda a determinar que tan "fácil" o "difícil" es un problema, es decir, si existe una solución algorítmica que dé una respuesta en una cantidad razonable de tiempo.

Cabe recalcar que una medida como *"0.35 ms"* no tiene mucho sentido ya que el tiempo depende del *hardware*, el *software* y el tamaño de la lista.

Así, la respuesta debería ser una función del tamaño de la instancia particular a resolver.

**Complejidad espacial o memoria para almacenamiento de datos**: Generalmente medida como la cantidad de variables elementales.

---

Las distinciones principales que hacemos entre los problemas es si toman tiempo polinomial o no: si el número de pasos requeridos puede ser descrito como un $O(n^k)$ para algún $k$.

Podemos realizar entonces las siguientes distinciones entre problemas desde el punto de vista de la complejidad computacional.

- **Problemas tratables**: Pueden ser resueltos usando una máquina de Turing determinista en un número de pasos proporcional a una función polinomial del tamaño de su entrada.

- **Problemas intratables**: No existe un algoritmo en tiempo polinomial que lo resuelva.

### Reducciones

Un problema $A$ puede ser reducido a otro problema $B$ si cualquier instancia de $A$ puede ser convertida en una instancia de $B$ tal que su solución es una solución a la instancia de $A$.

> Se denota como $A \leq_{p}B$

En este caso decimos que $A$ se reduce a $B$ o que $B$ resuelve a $A$.

Si $A$ se reduce a $B$ en tiempo polinomial entonces decimos que $A$ no es más difícil que $B$.

## Clases de problemas de decisión

#### P (Polinomial)

Problemas para los que se conocen algorítmos deterministas en tiempo polinomial.

- **Algoritmo determinista**: Siempre encuentran la respuesta correcta (esencialmente).

#### NP (No determinista, polinomial)

Pueden ser resueltos en tiempo polinomial por una máquina de Turing no determinista si permites "adivinar con algo de suerte" los problemas se pueden solucionar en tiempo polinomial.

De forma más intuitiva, el conjunto $NP$ es el conjunto de problemas para los cuales las instancias donde la respuesta es "si" se pueden certificar con cálculos determinísticos en tiempo polinómico.

> Es decir, dada una posible solución a una instancia del problema, podemos verificar usando un algoritmo de tiempo polinómico si dicha solución es o no válida.

#### NP-Completos

Un problema de decisión $L_1$ es NP-completo si:

1. $L_{1} \in NP$
2. Todo problema $L \in NP$ puede ser transformado en $L_1$ en tiempo polinomial (*Reducción*).

> Son problemas más dificiles en $NP$.

Por ejemplo, **coloración de gráficas**.

#### NP-difíciles (NP-hard)

> Problemas tan difíciles de resolver como el problema más difícil en $NP$.

No necesariamente se conoce un algoritmo polinomial para saber, dada una solución si es válida o no.

Un problema $L$ es $NP$-hard si existe un problema $L'$ que sea $NP$-completo y se pueda reducir a $L$ en tiempo polinomial.

Esta definición **no aplica exclusivamente** a un problema de decisión. Uno de optimización puede ser $NP$-hard.

> Un problema $NP$-completo está al mismo tiempo en $NP$ y en $NP$-hard.

## Clases de problemas de decisión considerando la complejidad espacial

- **Clase L (D Log Space)**: Pueden ser resueltos por una *Maquina de Turing* determinista utilizando espacio logarítmicamente proporcional al tamaño de la entrada. Existe solución única.

- **Clase NL (Non-deterministic Logarithmic Space)**: Problemas de decisión que pueden resolverse en espacio $log(n)$ por una *Máquina de Turing* no determinista. Si tienen solución es única.

Por último los problemas pueden clasificarse en:

- **Problemas indecidibles**: Son aquellos que no pueden resolverse mediante un algoritmo. (Por ejemplo *Halting Problem*)

- **Problemas decidibles**: Son aquellos para los que existe (o puede existir) un algoritmo para su cómputo.

# Enlaces

[Siguiente ->](AA_Complejidad.md)