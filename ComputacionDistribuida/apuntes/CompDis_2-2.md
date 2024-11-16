# Protocolo de Bit Alternante (ABT)

> Es un esquema de comunicación que ayuda a detectar y corregir la pérdida de mensajes en la red.

Recordemos que en la ciberseguridad, se busca garantizar 3 propiedades de la información:

- ***Confidencialidad***
- ***Integridad***
- ***Accesibilidad***

El **protocolo de Bit alternante** es una manera simple y confiable de mantener seguimiento e integridad de la conversación y consiste del siguiente procedimiento:

***Envío inicial***:

1. El ***transmisor*** envía un paquete con un bit especial reservado (ya sea 0 o 1) que denominamos ==alternante==.

***Recepción***:

2. El ***receptor*** recibe el paquete y si el bit alternante coincide con el que espera, procesa el paquete y envia un paquete *ACK* de regreso con el mismo bit alternante que se recibió, además actualiza el bit esperado para que coincida con el siguiente paquete.
3. Si no coincide con el valor esperado, significa que es una retransmisión del último paquete, entonces envía un *ACK* **con el valor del paquete anterior** para confirmar nuevamente que lo recibió.
4. Si continua recibiendo el mismo paquete, de igual manera, envia continuamente paquetes *ACK* con el bit adecuado para confirmar que lo recibió

***Retranmisión***:

5. Si el ***transmisor*** no recibe el *ACK* esperado en un tiempo límite, asume que el paquete no llegó y retransmite el paquete.
6. En caso de recibir el *ACK* esperado, actualiza el bit alternante y envía el siguiente paquete.

# Enlaces

[<- Anterior](CompDis_2-1.md) | [Siguiente ->](CompDis_2-3.md)