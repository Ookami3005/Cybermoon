[<- Índice](../WebHacking.md)
# DNS

El funcionamiento y conceptos básicos del protocolo **DNS** puede encontrarse a detalle en esta [nota](../../RedesComputadora/apuntes/Redes_dns.md) de la sección de **Redes de Computadora**, de modo que obviaré su explicación en este apartado.

### ¿Por qué nos importa el DNS para reconocimiento web?

Entender y enumerar el *DNS* de una organización puede brindarnos un entendimiento básico de su infraestructura e incluso revelar información sensible que nos guie a descubrir vulnerabilidades o puntos débiles, como:

- ***Descubrir recursos***: Los **registros** *DNS* son una fuente valiosa de información en la que podríamos descubir subdominios, alias, servidores de correos, servidores *DNS* autoritativos e incluso algunos correos de administradores.

- ***Mapear la infraestructura de red***: Podemos identificar varios componentes de la infreastructura en base a los registros o los nombres de dominio que poseen.

- ***Monitoreo de cambios***: Al analizar continuamente la información en un servidor *DNS*, podríamos notar cambios importantes en la infraestructura u organización. De modo que podríamos aprovechar dichos cambios a nuestro favor.

## Herramientas

Para el reconocimiento *DNS*, existen muchas implementaciones de herramientas que ayudan en gran medida a este propósito, mediante consultas *DNS* personalizadas.

Algunas de las más conocidas son:

| Herramienta                           | Descripción                                                                                                                                                                                                                    |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `dig`                                 | Herramienta altamente versatil para realizar consultas *DNS* personalizadas y con obtener información detallada.                                                                                                               |
| `nslookup`                            | Herramienta simple para consultas *DNS* básicas, principalmente para registros **A**, **AAAA** y **MX**.                                                                                                                       |
| `host`                                | Herramienta muy simple preinstalada en sistemas *UNIX* que brinda información concisa, únicamente para registros **A**, **AAAA** y **MX**.                                                                                     |
| `dnsenum`                             | Herramienta de enumeración automática principalmente enfocada en fuerza bruta, ataques de diccionario y transferencias de zona sobre servidores *DNS*.                                                                         |
| `fierce`                              | Herramienta de enumeración de subdominios a partir de consultas recursivas y detección de comodines, conocida tambien por su interfaz amigable.                                                                                |
| `dnsrecon`                            | Herramienta que integra varias técnicas conocidas de enumeración *DNS* y soporta varios formatos de salida, usualmente utilizada para extraer información de los registros y para enumeración de subdominios.                  |
| `theHarvester`                        | Herramienta *OSINT* que recolecta información de muchas fuentes públicas acerca del nombre de dominio, incluyendo por supuesto los registros DNS. Incluso puede descubrir correos electrónicos, información de empleados, etc. |
| ***Páginas de consultas DNS online*** | Páginas con interfaces amigables que permiten realizar consultas *DNS* básicas y avanzadas.                                                                                                                                    |

### Dig

> La utilidad **Dig** es una herramienta poderosa y versatil para realizar consultas *DNS* y recuperar varios tipos de información en los registros. Es **flexible**, **personalizable** y permite filtrar los resultados de forma avanzada.

Es una herramienta con muchisimas opciones pero mayormente estaremos utilizando una sintaxis de la siguiente manera:

- `dig <Registro> <IP> @<ServidorDNS>`

El orden de estos parametros no es obligatorio y podemos indicarlo como con convenga.

Algunas de las formas más comunes de utilizar esta herramienta son:

| Comando                         | Descripción                                                                                                                                                                                      |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `dig domain.com`                | Realiza una consulta de registro *A* para el dominio especificado, recuperando la dirección IP.                                                                                                  |
| `dig aaaa domain.com`           | Realiza una consulta del registro especificado (AAAA) del dominio indicado.                                                                                                                      |
| `dig domain.com CNAME`          | Realiza una consulta de registro CNAME del dominio especificado.                                                                                                                                 |
| `dig @1.1.1.1 domain.com`       | Realiza una consulta por defecto (A) especificamente al servidor `1.1.1.1` del dominio indicado.                                                                                                 |
| `dig +trace domain.com`         | Indica el camino completo que realiza la consulta por defecto del dominio indicado.                                                                                                              |
| `dig -x 8.8.8.8`                | Realiza una consulta inversa de la dirección IP especificada, recuperando su nombre de dominio si es que tiene.                                                                                  |
| `dig +short domain.com`         | Realiza una consulta por defecto del dominio y brinda una respuesta más concisa y breve.                                                                                                         |
| `dig +noall +answer domain.com` | Realiza una consulta por defecto del dominio e imprime únicamente la respuesta en pantalla.                                                                                                      |
| `dig any domain.com`            | Devuelve todos los registros del dominio indicado. Sin embargo, actualmente la gran mayoría de servidores *DNS* ignoran este tipo de consultas para prevenir abusos y reducir el tráfico de red. |

#### Salida

# Subdominios

# Enlaces

[<- WHOIS](web_whois.md) | [Crawling ->](web_crawling.md)