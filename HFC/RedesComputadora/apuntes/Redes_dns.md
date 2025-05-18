[<- Índice](../RedesComputadora.md)
# Domain Name System

> El protocolo ***DNS*** actua como el **GPS** del *Internet* traduciendo los reconocibles **nombres de dominio** que utilizamos hoy en día a la direcciones *IP* precisas para poder comunicarnos con el servidor.

### ¿Cómo funciona?

El proceso completo de una consulta *DNS* para identificar un servidor se compone de varios procedimientos que permiten que la conexión entre el servidor y tu computadora suceda.

1. ***La computadora realiza formalmente una consulta DNS***: Cuando deseas iniciar una conexión e indicas un nombre de dominio la computadora primera verifica en su memoria **caché** si ha solicitado recientemente esta consulta. De no ser el caso, realiza la consulta a un servidor **DNS Resolver**, típicamente indicado por el provedor de internet.

2. ***El DNS Resolver inicia una consulta recursiva del nombre***: Este tipo de servidores también tienen un **caché** propio que pueden consultar, pero de no encontrar la dirección solicitada, inicia una **consulta DNS recursiva** desde un servidor *DNS* **raíz**, los servidores con **mayor** jerarquía.

3. ***El servidor DNS raiz indica el servidor TLD correcto***: Aunque estos servidores de mayor jerarquía no conocen directamente la respuesta, si saben que servidor **TLD** (*Top Level Domain*) es el encargado del dominio consultado de modo que le indican dicho servidor al **Resolver**.

4. ***El servidor DNS TLD indica el servidor que si conoce la respuesta***: Nuevamente, aunque el servidor **TLD** no conce la respuesta, si determina cual es el servidor **autoritativo** del dominio, es decir, el servidor encargado oficialmente de la información del nombre consultado y envía dicho servidor al **Resolver**.

5. ***El servidor autoritativo brinda la respuesta definitiva***: Finalmente, el servidor **autoritativo** indica la dirección IP correcta al **Resolver** para que este pueda comunicarselo a la computadora.

6. ***La computadora se conecta al servidor***: Ahora que la computadora conoce la dirección IP, inicia una conexión formal con el servidor indicado.

### El archivo `hosts`

> El archivo `hosts` es un archivo simple de texto que se utiliza para mapear nombres de dominio a direcciones *IP* de forma manual y local, por encima de los procesos *DNS*.

Es como si permitiera definir o **sobreescribir** localmente nombres de dominio, especialmente útil para pruebas de desarrollo y depuración, o incluso para el bloqueo de sitios web.

Este archivo `hosts` se encuentra en la ruta `C:\Windows\System32\drivers\etc\hosts` en sistemas *Windows* o en `/etc/hosts` en sistemas *Linux* y *macOS*.

El formato del archivo es:

- `<IP> <Dominio> [<Alias> ...]`

Por ejemplo:

```txt
127.0.0.1       localhost
192.168.1.10    devserver.local
```