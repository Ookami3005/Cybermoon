[[Arquitectura de Computadoras|<- Índice]]
## Sistema numérico binario

El sistema utilizado en las computadoras para representar y procesar datos e información es el ==sistema numérico binario==, o base 2.

Cualquier número puede representarse usando dos simbolos: *0* y *1*, mediante utilizar la misma notación posicional como en el sistema decimal.

Un dígito binario se conoce como ==bit== (**b**inary dig**it**)

**¿Cuantos bits se necesitan?** La regla general es con *n* bits se pueden representar los números del 0 al $2^{n-1}$.

 En un sistema de cómputo, un grupo de 8 bits se conoce como ==byte== y puede representar números del 0 al 255.

---
## Sistemas octal y hexadecimal

Los sistemas numéricos ==octal== (base 8) y ==hexadecimal== (base 16) prooven de una representación más corta y clara para números con múltiples bits en un sistema digital, dado que sus bases son potencias de 2.

El sistema octal utiliza los dígitos del **0 al 7** del sistema decimal, dado que solo requiere 8 dígitos para representar números.

El sistema hexadecimal utiliza los dígitos del **0 al 9** y las letras **A, B, C, D, E
y F**, dado que requiere de 16 símbolos para representar cualquier número.

La utilidad de los sistemas octal y decimal radica en que representan una forma más compacta de los números binarios y es relativamente fácil convertirlos a binario.

Para convertir un número binario a octal, se procede en los siguientes pasos:

1. Comenzando por el extremo derecho de la cadena de bits binarios, y hacia la izquierda.

2. Se añaden ceros a la izquierda hasta hacer el número total de bits un múltiplo de 3.

3. Se separan en grupos de 3.

4. Se remplaza cada grupo de bits con el correspondiente dígito octal.

El procedimiento para convertir de binario a hexadecimal es similar, excepto que se forman y remplazan grupos de 4 bits.

---

##  Parte fraccional

El punto que separa la parte entera de la parte fraccionaria se llama punto decimal para la base 10.

En el caso general, se llama punto a la base, en particular, en base 2 se llama punto binario, en base 8 se llama punto octal y en base 16, punto hexadecimal.

Las fracciones binarias se presentan en un número binario, como por ejemplo $0.101_2$ tiene:

> 1 mitad + 0 cuartos + 1 octavo

Y por lo tanto, se tiene que:

$0.101_2 = 1 \times 2^{-1} + 0 \times 2^{-2} + 1 \times 2^{-3} = 0.5 + 0 + 0.125 = 0.625_{10}$

---

## Cambiando de una base a otra

#### Decimal a binario

Para esto se hace necesario obtener cuántos dígitos binarios se encuentran en la primera posición del número entero, es decir, el dígito que se multiplica por $2^0$. Para hacer esto, se divide el número entre 2, por ejemplo para $53_{10}$:

$$53/2 = 26 + 1/2$$

El residuo de la división (en este caso 1) representa el dígito binario del número multiplicado por $2^0$. Este proceso se repite obteniendo $2^1$, $2^2$, etc.

A continuación, se examina cómo una fracción expresada en base 10, puede
convertirse en binario:

En este caso, el proceso involucra la multiplicación con la base en lugar de la división entre la base, como en el caso de los números enteros.

Por ejemplo, considerese el equivalente binario de $0.125_{10}$:

$$0.125 \times 2 = 0.25$$

La parte entera del resultado es 0, por lo que el digito binario que multiplica a $2^{-1}$ es 0. Repitiendo este proceso obtenemos el número binario $0.001_2$.

La conversión de algunas fracciones decimales a su equivalente binario puede resultar en algunas inexactitudes, ya que ni las personas ni las computadoras pueden trabajar con un número infinito de términos.

Tal inexactitud se conoce como *error de redondeo* (**round-off error**). El uso de suficientes términos puede hacer que la inexactitud sea despreciable.

Tambien es importante recalcar que cuando se convierte un número que consta tanto de parte entera como fraccional, la conversión se hace por separado.

***Nota:***
Si se desea convertir un número decimal a su equivalente octal o hexadecimal, se utiliza el mismo procedimiento, excepto que en el caso octal se divide/multiplica por 8 y en el hexadecimal, se utiliza el 16.

---

## Aritmética binaria elemental

[[¿Qué es una computadora?|<- Anterior]]