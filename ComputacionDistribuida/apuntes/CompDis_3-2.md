[<- Índice](../ComputacionDistribuida.md)
# Elección distribuida

> Consiste en la elección de un proceso para que tome un rol específico o realice una función en particular.

Por ejemplo...

1. Elegir el nodo menos ocupado.
2. Elegir un nuevo coordinador.
3. Elegir un nodo para remitir un mensaje.

### Consideraciones

1. Cada nodo $p_i$ tiene una variable $e_i$ con el identificador del nodo electo.
2. Cada nodo (no caido) debe estar en uno de 2 estados respecto al algoritmo de elección, **participante** o **no-participante**.
3. Cuando un nodo realiza una acción que desencadena un algoritmo de elección, se le denomina ***nodo convocante*** o se dice que **convoco** la elección.
4. Cuando no se ha elegido a un nodo, se fija $e_i$ con un valor especial que indique que no hay nodo electo.
5. Al finalizar el algoritmo, todos los nodos tienen el mismo nodo electo.
6. Es posible tener más de un algoritmo de elección ejecutandose a la vez, lo que de denomina **elecciones concurrentes**.

### Algoritmo de elección basado en anillo

Se establece una estructura virtual de anillo en la red y se envía hacia *"adelante"* el identificador mayor.
Cuando un nodo recibe su propia *id*, comienza a propagar una señal de *"ganador"* donde le indica a los demás que él es el seleccionado para que puedan actualizar sus identificadores.

### Algoritmo de abusón

Es un algoritmo que tolera la caida de nodos durante su ejecución.

- Requiere que cada noda conozca toda la red y sepa que nodos son superiores a él (poseen un identificador mayor) y cuales no.

Utiliza 3 tipos de mensajes:

- Elección (E)
- Respuesta (R)
- Ganador o Electo (G)

# Enlaces

[<- Anterior](CompDis_3-1.md) |