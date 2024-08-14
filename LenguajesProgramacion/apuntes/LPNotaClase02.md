[<- Volver](../LenguajesProgramacion.md)
# Componentes de los Lenguajes de Programación

En el estudio de la teoría de lenguajes de programación, es esencial comprender los componentes fundamentales que los constituyen:

- **Sintaxis** : La sintaxis define las reglas y estructuras que permiten escribir código de manera correcta y coherente estableciendo el esqueleto del lenguaje.

- **Semántica** : La semántica por su parte, se enfoca en el significado y la lógica detrás de esas estructuras, asegurando que las instrucciones se *interpreten* y ejecuten correctamente.

- **Pragmática** : La pragmática aborda el uso práctico del lenguaje, incluyendo los *idioms* o patrones de uso común que optimizan la eficiencia y claridad del código, y las bibliotecas que proporcionan soluciones preconstruidas para problemas frecuentes.

## Sintaxis

#### Definición 1 (Sintaxis de un lenguaje de programación)

> Definimos de manera informal, la **sintaxis** de un lenguaje de programación como el conjunto de reglas y estructuras que especifican cómo deben escribirse los símbolos y palabras reservadas de un programa para que sea considera válido por el traductor del lenguaje.

La sintaxis se asegura que el código fuente siga un formato estructuralmente coherente que el sistema pueda procesar y ejecutar.

Debido a esto, necesitamos ==mecanismos formales== que nos permitan especificar la sintaxis eliminando ambigüedades.
Para especificar formalmente la sintaxis de un lenguaje de programación, se utilizan algunas herramientas y notaciones como:

- **Gramáticas formales**
- **Árboles sintácticos**
- **Analizadores léxicos y sintácticos**

#### Ejemplo

Veamos un ejemplo de sintáxis en un lenguaje de programación real.
Tenemos el siguiente programa escrito en el lenguaje de programación *Python*:

```python
def saludo(nombre):
	print("¡Hola, " + nombre + "!")
```

En este código, la sintaxis de *Python* especifica las siguientes reglas:

- Al definir funciones se usa la palabra reservada `def` seguida del nombre de la función y un par de paréntesis que pueden o no contener parámetros. La línea termina con 2 puntos

- El cuerpo de la función debe estar identado con un nivela dicional de sangría para indicar que las líneas de código pertenecen a la función. En *Python*, la indentación es crucial y forma parte de la sintaxis.

- Dentro del cuerpo de la función se llama a otra función `print` que toma un argumento.

Estos elementos y su correcta organización son parte de la sintaxis que *Python* requiere para interpretar y ejecutar el código sin errores.

## Semántica

#### Definición 2 (Semántica de un lenguaje de programación)

> Definimos, informalmente, la **semántica** de un lenguaje de programación como el significado de las construcciones sintácticas definidas por el lenguaje.

Mientras que la sintaxis se ocupa de las reglas y la estructura correcta del código, la semántica se enfoca en lo que ese código debe hacer cuando se ejecuta.
Es decir, ==determina el comportamiento del programa==: cómo se interpretan y que efectos tienen las instrucciones y expresiones del lenguaje sobre el estado del programa y sus resultados.

Para especificar formalmente la semántica de un lenguaje de programación, se utilizan varias herramientas y enfoques como:

- **Semántica operacional**
- **Semántica denotativa**
- **Semántica axiomática**

La formalización de la semántica tiene grandes beneficios tales como:

- **Precisión y claridad** : Una especificación formal, elimina ambigüedades y proporciona descripciones claras  del comportamiento del lenguaje.

- **Verificación y validación** : Permite demostrar formalmente que un programa cumple con su especificación.

- **Portabilidad y consistencia** : Asegura que el comportamiento del lenguaje sea consistente en diferentes plataformas y entornos de implementación.

- **Optimización y mejora** : Proporciona una base teórica para optimizaciones y mejoras del lenguaje y sus implementaciones.

- **Desarrollo de herramientas** : Pemite el desarrollo de herramientas automáticas, como analizadores estáticoss, verificadores de tipos y generadores de código, que dependen de una comprensión formal del comportamiento del lenguaje.

#### Ejemplo

Veamos un ejemplo para obtener intuición.
Tenemos ahora el programa tambien escrito en *Python*:

```python
x = 5
y = 10
z = x + y
print(z)
```

En este código la semántica de cada línea de código es:

- `x = 5` Asigna el valor entero 5 a la localidad de memoria representada por la variable *x*
- `y = 10` Asigna el valor entero 10 a la localidad de memoria representada por la variable y
- `z = x + y` Suma los valores almacenados en *"x"* y *"y"* y asigna el resultado a la localidad de memoria representada por la variable *"z"*.
- `print(z)`Envía el valor almacenado en *"z"* a la sálida estándar.

## Pragmática

#### Definición 3 (Pragmática de un lenguaje de programación)

> Definimos de manera informal, la **pragmática** de un lenguaje de programación como la manera en que éstos se usan para resolver problemas del mundo real

Incluye el conocimiento sobre las convenciones, los patrones de uso común (*idioms*) y las bibliotecas y herramientas disponibles que facilitan la programación eficiente y efectiva. Otros ejemplos de pragmática incluyen:

- **Patrones de diseño**
- **Convenciones de nombres**
- **Control de versiones**
- **Pruebas unitarias**
- **Documentación**
- **Gestión de dependencias**
- **Refactorización**
- **Optimización del rendimiento**
- **Uso de bibliotecas y *frameworks***
- **Prácticas de desarrollo ágil**

Aunque son un componente clave de los lenguajes de progrmación, su estudio recae mayoritariamente en los curso de Ingeniería del Software, no en este curso.

#### Ejemplo

Supongamos que estamos desarrollando una aplicación web en *Python*. En lugar de construir todo desde 0, decidimos usar *Django*, un *framework* de alto nivel. Esta decisión es un ejemplo de pragmática donde *Django* ofrece un conjunto de herramientas y convenciones que facilitan y aceleran el desarrollo

# Enlaces

[<- Anterior](LPNotaClase01.md) |