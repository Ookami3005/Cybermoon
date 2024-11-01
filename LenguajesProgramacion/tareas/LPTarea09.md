[<- Volver](../LenguajesProgramacion.md)
# Semanal 9

#### Lenguajes de Programación

#### - Fernando Romero Cruz 319314256

## 🚀 Ejercicios

El objetivo de esta Evaluación Semanal es comprender la definición de máquina abstracta en el contexto de la teoría de lenguajes de programación y el funcionamiento específico de la máquina *CEK*, así como su relación con el concepto de continuaciones.

1. [x] Lee la introducción y el capítulo 4 de la tesis "Una implementación de máquinas de reducción" de Rodríguez Hernández, Alexis Arturo, disponible en TESIUNAM. Presta atención a los conceptos relacionados con las máquinas abstractas y la máquina CEK, especialmente en el contexto de lenguajes de programación y continuaciones.

2. Responde las siguientes preguntas basándote en tu lectura:
   
   - ¿Qué es una máquina abstracta? Describe sus características principales y su relevancia en la modelación de la ejecución de lenguajes de programación.

> Las máquinas abstractas son conceptos teóricos que buscan proveer un estado de abstracción intermedio para la comprensión de un lenguaje de programación, estan pensadas para cerrar la brecha entre el alto nivel de un lenguaje de programación y el bajo nivel de una máquina real.

   - Explica los componentes de la máquina CEK (Control, Entorno y Continuación) y su función en la ejecución de programas. ¿Cómo gestionan las continuaciones durante el proceso de reducción?

> La máquina *CEK* tiene 3 componentes:
> 
> - **Control**: Es el encargado de almacenar el término que se está evaluando actualmente además del símbolo $\updownarrow$ que se utiliza para indicar a la máquina cuando tiene que procesar la continuación.
>
> - **Ambiente**: Guarda el ambiente representado como una lista de pares ordenados de la forma (variable, *valor semántico*).
>   
> - **Continuación**: Código de continuación que representa el *"resto"* del cálculo, es decir, lo que la máquina tendrá que hacer posteriormente a evaluar el término actual.

> Al reducir una expresión, la invocación de una continuación remueve la continuación actual y comenzar una nueva con su argumento como punto de partida. En cada reducción el operador va quitando las subexpresiones externas.
   
   - Describe brevemente el proceso de reducción en la máquina CEK. ¿Qué aspectos clave se discuten en el capítulo 4 sobre cómo se evalúan las expresiones en lenguajes de programación?

> Los primeros pasos de la evaluación definen el comportamiento típico de una máquina *SECD*. Despues se preparan las operaciones necesarias para procesar una $\mathcal{C}$-aplicación, después removemos la continuación actual por la continuación inicial y finalmente realizamos la evaluación utilizando el $\mathcal{A}$-argumento para terminar el cálculo restante.

3. Escribe una breve reflexión (150-200 palabras) sobre la relevancia de comprender las máquinas abstractas y la máquina CEK en el estudio de lenguajes de programación. ¿Cómo crees que esta comprensión influirá en tu desarrollo como programador o investigador en el campo de la computación?

> ***Resumen:***

Entender las máquinas abstractas y la máquina CEK, es importante en el estudio de los lenguajes de programación porque ayuda a comprender cómo se comportan internamente los programas. Las máquinas abstractas son modelos que simulan la ejecución del código para que podamos analizar el procesamiento de las instrucciones.

Además la estructura particular de las máquinas *CEK* ayuda a entender mejor cómo se manejan las variables, los cálculos y el flujo del programa desde un punto de vista funcional, tan necesario para los lenguajes comprendidos por este paradigma.

Al conocer la máquina CEK, los programadores o investigadores podriamos mejorar nuestra capacidad tanto de entender como de optimizar código funcional complejo, lo que es esencial para crear programas eficientes y adecuados para el lenguaje.