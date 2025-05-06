# Práctica 2
## Organización y Arquitectura de Computadoras

> Fernando Romero Cruz - 319314256

### Reporte

#### Decodificador

Para poder representar adecuadamente los números solicitados (del 0 al 9), necesitamos poder especificar el número en binario y obtener a cambio la representación visual de cada en dicho display de 7 segmentos.

Para especificar el 9 necesitamos al menos **4** bits, es decir, necesitaremos al menos 4 variables de entrada, que denotaré como `w`, `x`, `y`, `z`.
Mientras que para la salida, requiero exactamente de **7** variables, una por cada segmento del display, que denotaré como `A`, `B`, `C`, `D`, `E`, `F` y `G`.

Con las entradas y salidas ya identificadas, tenemos que plantear el sistema de ecuaciones que, dada una entrada específica, resultará en su representación correspondiente en el *display*.

Haciendo las pruebas y cálculos pertinentes, podemos proponer el siguiente sistema de ecuaciones:

- $A = w + y + xz + \overline{xz}$
- $B = \overline{x}+\overline{yz}+yz$
- $C = x + \overline{y} + z$
- $D = w + \overline{xz}+\overline{x}y+y\overline{z}+x\overline{y}z$
- $E = \overline{xz} + y\overline{z}$
- $F = w + x\overline{y}+x\overline{z}+\overline{yz}$
- $G = w+x\overline{y}+\overline{x}y+y\overline{z}$

Este sistema es el que buscamos implementar como circuito lógico con apoyo de *Logisim* y conformará el *decoder* necesario para que nuestro display funcione.

#### Contador

Para la implementación del contador, opté por utilizar tablas de excitación y transición junto con *flip-flops* **JK**, de modo que el contador pudiera reiniciarse al alcanzar el máximo propuesto por la práctica (9).

Reduciendo el sistema de ecuaciones, propuse este sistema para mi práctica:

- $J_{A} = BC + D$
- $J_{B}=AD$
- $J_{C}=ABD$
- $J_D = ABC$
- $K_A=1$
- $K_{B}=A+D$
- $K_{C} = AB + D$
- $K_{D} = A + B + C$

De igual manera, el circuito lógico implementa dichas ecuaciones para que mi contador tenga el comportamiento deseado.

#### Display

Una vez ensamblador el contador al decodificador, y este último al display, el circuito principal podrá contar del 0 al 9 sin problema y reiniciar la cuenta cuando éste alcance el máximo.