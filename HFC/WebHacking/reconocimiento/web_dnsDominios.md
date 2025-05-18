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

#### Salida

# Subdominios

# Enlaces

[<- WHOIS](web_whois.md) | [Crawling ->](web_crawling.md)