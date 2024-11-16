# Sincronización de relojes

> La sincronización de relojes en sistemas distribuidos es de vital importancia por una variedad de propositos como:

1. Registros de bitácoras
2. Fechas de archivos
3. Aplicaciones en tiempo real

Normalmente medida por el estándar *UTC* (*Tiempo Universal Coordinado*)

Ademas debemos de hacer una distinción entre ***relojes físicos*** y ***relojes lógicos***.

- **Reloj físico**: Aquel que mide tiempo y hora del mundo real.
- **Reloj lógico**: Regula el orden en que ocurren los sucesos.

### Algoritmo de Cristian

> Es el algoritmo más básico de sincronización de reloj entre un servidor y **un solo cliente**.

1. **Solicitud**: El cliente envía una solicitud de tiempo al servidor y **registra** el momento en que se envió como $T_{0}$.
2. **Respuesta**: El servidor recibe la solicitud, la procesa y envía una respuesta $r$ con su tiempo actual. El cliente la recibe y **registra** el momento como $T_{1}$.
3. **Estimación de tiempo**: Dada la latencia de la comunicación entre el servidor y el cliente, este último calcula el posible tiempo de propagación y/o retraso con la siguiente fórmula:

$$
d = \frac{T_{1}- T_{0}}{2}
$$

4. **Actualización del reloj**: El cliente actualiza su hora como $r+d$.

Si se requiere una mayor precisión en la hora puede realizarse este mismo procedimiento varias veces para reducir la posibilidad de resultados anómalos.

### Algoritmo de Berkeley

> El ***algoritmo de Berkeley*** es un algoritmo de sincronización de relojes para sistemas distribuidos, se caracteriza por no depender de un servidor de tiempo externo, si no que posee un enfoque descentralizado y cooperativo entre los nodos de la gráfica, permitiendo que se sincronizen entre si.

1. Primero se designa un nodo coordinador.
2. Este nodo coordinador solicita la hora de todos los demás nodos.
3. Los demás nodos responden con su hora y registran el momento en el que la envían.
4. El coordinador descarta valores anómalos y calcula el promedio entre los valores aceptados.
5. El coordinador calcula para cada nodo particular cuanto debe atrasar o adelantar su reloj para concordar con la hora obtenida y la envía a cada uno.
6. Cada nodo realiza el reajuste necesario tomando en cuenta el tiempo de propagación de la misma forma que el *Algoritmo de Cristian*.

# Enlaces

[<- Anterior](CompDis_2-4.md) | [Siguiente ->](CompDis_3-2.md)