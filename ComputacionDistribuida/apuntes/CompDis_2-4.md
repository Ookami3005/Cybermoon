[<- Índice](../ComputacionDistribuida.md)
# Algoritmos epidémicos o de rumor

Para este tipo de algoritmos, la información que se quiere diseminar es modelada como un *"chisme"* o *"rumor"*.

Inicialmente, se considera a todos los nodos *"ignorantes"*, pues no conocen ningún rumor.

Cuando un nodo $p$ recibe una actualización, se convierte en un *"rumor candente"* y empieza a diseminarla a todos sus vecinos.
Este proceso se realiza seleccionando periódicamente un vecino al azar para enviarle el rumor *"push"*.

Despues de cierto tiempo, el nodo emisor *"pierde interés"* y deja de esparcir el rumor.

#### Perdida de interés

> Las condiciones para que un nodo pierda interés en un rumor son establecidas durante la implementación del algoritmo y funcionan como un mecanismode **calibración**.

Puede ser un simple
- **Contador**: perder el interés despues de enviar el mensaje *k* veces
- **"Moneda"**: pierde el interés con probablididad $\displaystyle p/k$ después de cada envío
- **"Retroalimentación"**: El nodo emisor solo pierde interés si el nodo vecino ya conocía ese rumor,
- **Ciega**: No se verifica la correcta recepción del mensaje antes de perder interés.

# Protocolos de Agregación

Se denomina ***datos agregados*** a la información sobre la red en general (no sobre un nodo en particular).

Los protocolos de agregación permite el acceso local (por parte de un nodo) a la información global de la red.

Algunos ejemplos de propiedades globales que podrían ser de ayuda:

1. Promedio de carga de trabajo
2. Total de espacio de almacenamiento
3. Número de nodos conectados en una red dinámica

Los protocolos más famosos son:

1. ***Push-Pull*** (*P2P*): Cada nodo selecciona periodicamente un nodo al azar para enviar (*push*) sus datos y recibir (*pull*) los del vecino para que ambos realicen los calculos correspondientes. Como este proceso se repite por varias rondas en todos los nodos, eventualmente todos los nodos llegan a un aproximado.
2. ***Tree-Based***:  Se construye una estructura de arbol de nodos para facilitar la agregación de los datos, donde las hojas envían los datos a sus padres para que realicen los cálculos pertinentes y envíen esos datos a su vez, a sus propios padres hasta que finalmente llegue a la raiz.
3. ***Algoritmos Epidemicos***: Los algoritmos epidémicos son una forma eficiente de propagar los datos necesarios a través de la red.

# Enlaces

[<- Anterior](CompDis_2-3.md) |