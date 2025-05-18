[<- Índice](../RedesComputadora.md)
# Domain Name System

> El protocolo ***DNS*** actua como el **GPS** del *Internet* traduciendo los reconocibles **nombres de dominio** que utilizamos hoy en día a la direcciones *IP* precisas para poder comunicarnos con el servidor.

Algunos conceptos clave que es bueno conocer son:

| Concepto                                   | Descripción                                                                                        | Ejemplo                                                                                                                                                  |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ***Nombre de dominio***                    | Un nombre o etiqueta reconocible que identifica un sitio web, servidor o recurso en el *Internet*. | `www.google.com`                                                                                                                                         |
| ***Dirección IP***                         | Un identificador númerico que se le asigna a cualquier dispositivo conectado a *Internet*.         | `192.0.2.1`                                                                                                                                              |
| ***Servidores DNS Resolver o Recursivos*** | Un servidor *DNS* encargado de determinar las direcciones IP de los nombres de dominio.            | Por ejemplo, el *Resolver* oficial de *Google*, `8.8.8.8`                                                                                                |
| ***Servidores DNS Raíz***                  | Los servidores *DNS* de mayor jerarquía aunque su función no es determinar la dirección IP.        | Existen **13** servidores *DNS* raíz actualmente en todo el mundo nombrados a partir de las letras de la `a` a la `m`, por ejemplo: `a.root-servers.net` |
| ***Servidores DNS TLD***                   | Servidores a cargo de todo un dominio de alto nivel o **TLD** (*Top Level Domain*)                 | Por ejemplo, els ervidor **Verisign** que se encarga del dominio *TLD* `.com` y **PIR** de `.org`.                                                       |
| ***Servidores DNS autoritativos***         | El servidor que realmente conoce la dirección IP del dominio solicitado.                           | Por ejemplo, servidores oficiales de la organización que registró el dominio.                                                                            |
| ***Registros DNS***                        | Distintos tipos de información que almacena un *DNS*                                               | Se identifican mediante claves alfabéticas como `A`, `AAAA`, `CNAME`, `MX`, etc.                                                                         |

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

### Zonas

> Para **DNS**, una ***zona*** es todo un conjunto **administrado** de nombres de dominio pertinentes a una organización. Podemos pensarlas como contenedores virtuales de varios nombres de dominio.

La definición y configuración de una **zona** se realiza en un archivo que define los todos los **registros** de esta.
Por ejemplo, un archivo de configuración de **zona** se ve así:

```txt
$TTL 3600 ; Default Time-To-Live (1 hour)
@       IN SOA   ns1.example.com. admin.example.com. (
                2024060401 ; Serial number (YYYYMMDDNN)
                3600       ; Refresh interval
                900        ; Retry interval
                604800     ; Expire time
                86400 )    ; Minimum TTL

@       IN NS    ns1.example.com.
@       IN NS    ns2.example.com.
@       IN MX 10 mail.example.com.
www     IN A     192.0.2.1
mail    IN A     198.51.100.1
ftp     IN CNAME www.example.com.
```

Podemos ver que se definen los **servidores DNS** de la zona como registros **NS**, servidores de correo en registros **MX** y direcciones IP en registros **A** para varios servidores que forman parte de esta zona, con subdominios asignados.

#### Registros

> Ya sabemos que los registros son una forma de clasificar los distintos tipos de información que puede almacenar un servidor *DNS*, típicamente en una sintaxis:

- `<Dominio> IN <Registro> <Dato>`

| Registro    | Descripción                                                                                                                                    | Ejemplo                                                                                            |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| ***A***     | Almacena las direcciones IPv4 de cierto nombre de dominio.                                                                                     | `www.example.com.` IN **A** `192.0.2.1`                                                            |
| ***AAAA***  | Almacenan las direcciones IPv6 de un nombre de dominio.                                                                                        | `www.example.com.` IN **AAAA** `2001:db8:85a3::8a2e:370:7334`                                      |
| ***CNAME*** | Almacena nombres de dominio **alias** para otro nombre de dominio.                                                                             | `blog.example.com.` IN **CNAME** `webserver.example.net.`                                          |
| ***MX***    | Especifica los servidores de correo responsables de la zona, además de un número que representa su **prioridad**.                              | `example.com.` IN **MX** 10 `mail.example.com.`                                                    |
| ***NS***    | Especifica los servidores *DNS* autoritativos de esta zona.                                                                                    | `example.com.` IN **NS** `ns1.example.com.`                                                        |
| ***TXT***   | Almacena cualquier información arbitraria de la zona, es comunmente utilizada para la verificación del dominio y demás políticas de seguridad. | `example.com.` IN **TXT** `"v=spf1 mx -all"` (*SPF record*)                                        |
| ***SOA***   | Indica información administrativa de la zona, como el servidor *DNS* principal, el correo electrónico de la persona responsable, etc.          | `example.com.` IN **SOA** `ns1.example.com. admin.example.com. 2024060301 10800 3600 604800 86400` |
| ***SRV***   | Define el nombre de dominio y puerto de un servicio especifico, identificado con un nombre de dominio.                                         | `_sip._udp.example.com.` IN **SRV** 10 5 5060 `sipserver.example.com.`                             |
| ***PTR***   | Utilizado para consultas *DNS* inversas, donde deseamos consultar el nombre de dominio relacionado a una *IP* específica.                      | `1.2.0.192.in-addr.arpa.` IN **PTR** `www.example.com.`                                            |
