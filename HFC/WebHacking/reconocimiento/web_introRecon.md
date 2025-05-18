[<- Índice](../WebHacking.md)
# Introducción

> El ***reconocimiento web*** es fundamental en cualquier auditoría de seguridad.
> Este proceso implica una recolección sistemática y meticulosa de información sobre un sitio o aplicación web **objetivo**.

Puede percibirse como la fase de **preparación** antes de ahondar en un análisis profundo y cualquier potencial explotación.

Entre los principales **objetivos** de este tipo de **reconocimiento** se encuentran:

- ***Identificar recursos públicos***: Identificar todos los recursos y componentes públicos de la organización como paginas *web*, subdominios, direcciones IP y tecnologías. Esto nos permite tener una comprensión general de la presencia del objetivo en el **internet**.

- ***Revelar información oculta***: Localizar información sensible que no debería ser accesible al público, como archivos de respaldo, de configuración o documentación interna que nos revelen aspectos importantes o posibles vectores de ataque.

- ***Analizar la superficie de ataque***: Examinar la superficie de ataque que disponemos del equipo es clave para identificar potenciales vulnerabilidades y debilidades. Esto incluye evaluando las tecnologías utilizadas y configuraciones.

- ***Recolección de información***: Recolectar toda la información que pueda indicarnos vías de explotación o de ataques. Por ejemplo, identificar llaves personales, correos electrónicos, patrones de comportamiento, etc

## Tipos de reconocimiento

> El ***reconocimiento web***, como el reconocimiento general, se divide en 2 metodologías fundamentales, reconocimiento **pasivo** y **activo**.

#### Reconocimiento activo

Este tipo de reconocimiento implica la interacción directa con los sistemas objetivo.
Algunas técnicas conocidas, por ejemplo, son:

| Técnica                            | Descripción                                                                                                                                                                                                         | Herramientas                                     | Riesgo de detección                                                                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ***Escaneo de puertos***           | Identificar puertos abiertos y los servicios que alojan.                                                                                                                                                            | *Nmap*, *Masscan*, *Unicornscan*                 | **Alto**: puede ser detectado por sistemas *IDS* y *firewalls*                                                                                                        |
| ***Escaneo de vulnerabilidades***  | Realizando varias pruebas de vulnerabilidades conocidas sobre el objetivo.                                                                                                                                          | *Nessus*, *OpenVAS*, *Nikto*                     | **Alto**: Estos escaneos literalmente envían *payloads* de explotación al objetivo, por lo que son muy facilmente identificados.                                      |
| ***Mapeo de redes***               | Identificar la topología de la red del objetivo, dispositivos conectados a esta y como se relacionan.                                                                                                               | *Traceroute*, *Nmap*                             | **Medio** a **alto**: Generan tráfico de red excesivo e inusual.                                                                                                      |
| ***Banner Grabbing***              | Identificando información presente en los *banners* de los servicios que se despliegan al conectarse.                                                                                                               | *Netcat*, *cURL*                                 | **Bajo**: Típicamente requieren muy poca interacción con el objetivo, básicamente conectarse y ya, pero aun así podría registrarse en bitácoras.                      |
| ***Escaneo de Sistema Operativo*** | Identificar el sistema operativo del servidor objetivo.                                                                                                                                                             | *Nmap*, *Xprobe2*                                | **Bajo**: Usualmente requiere de técnicas muy poco invasivas, pero dependiendo del tipo de escaneo si podría ser detectado.                                           |
| ***Enumeración de servicios***     | Determinar versiones y detalles de los servicios que ofrece el servidor                                                                                                                                             | *Nmap*                                           | **Bajo**: De igual manera, requiere una interacción simple con el objetivo y es poco probable que detone alarmas.                                                     |
| ***Web Spidering***                | Consiste en realizar *crawling*, básicamente identificar todos los recursos **referenciados** en el servidor *web* siguiendo agresivamente los enlaces presentes en este y consultando todos los recursos posibles. | *BurpSuite Spider*, *OWASP ZAP Spider*, *Scrapy* | **Bajo** a **medio**: Puede ser detectado si el comportamiento del *crawler* no se configura correctamente para asimilar un *crawler* legítimo, como los de *Google*. |


En conclusión, aunque el **reconocimiento activo** brinda información más directa y comprensiva del objetivo, conlleva un mayor riesgo de detección pues nuestra interacción siempre podría detonar alarmas o levantar sospechas.

#### Reconocimiento pasivo

En contrase con el reconocimiento activo, el **reconocimiento pasivo** consiste en recolectar toda la información que podamos del objetivo sin ningún contacto o interacción directa con él, por ejemplo utilizando fuentes públicas de información o terceros.

| Técnica                                          | Descripción                                                                                                                                                 | Herramientas                                                               | Riesgo de detección                                                                                                                                  |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| ***Busquedas avanzadas en motores de búsqueda*** | Identificar y descubir recursos del objetivo aprovechandonos de motores de búsqueda.                                                                        | *Google*, *Duck Duck Go*, *Bing*, *Shodan*, etc.                           | **Muy bajo**: Las busquedas web son una práctica común y permitida por lo que no tendrían porque detonar alertas.                                    |
| ***Consultas WHOIS***                            | Consultar las bases de datos *WHOIS* para identificar información acerca del registro de dominios web.                                                      | Comando `whois` o sitios web de consultas *WHOIS*                          | **Muy bajo**: Las consultas *WHOIS* son legítimas y comunes, no deberpian levantar sospechas.                                                        |
| ***Enumeración DNS***                            | Enumeración de **registros** *DNS* con la intención de identificar subdominios, servidores de correo y demás infraestructura.                               | `dig`, `nslookup`, `host`, `dnsenum`, `fierce`, `dnsrecon`                 | **Muy bajas**: Las consultas *DNS* son **fundamentales** para el funcionamiento del *Internet*, por lo que no generan sospechas.                     |
| ***Análisis del archivo web***                   | Examinar copias **históricas** del sitio web para identificar cambios, vulnerabilidades e información oculta mediante paginas tipo *"maquina del tiempo"*.  | *Wayback Machine*                                                          | **Muy baja**: El acceso a versiones archivadas o pasadas de sitios web igualmente es una actividad común y no vigilada.                              |
| ***Analisis de redes sociales***                 | La recolección de información en redes sociales muy utilizadas.                                                                                             | *LinkedIn*, *X*, *Facebook* y demás herramientas especializadas en *OSINT* | **Muy baja**: Las redes sociales poseen una actividad elevada de usuarios de modo que pasamos desapercibidos.                                        |
| ***Enumeración de repositorios de código***      | Analizando código dispuesto públicamente en repositorios que podría filtrar información o revelar una vulnerabilidad en el código fuente de una aplicación. | *Github*, *Gitlab*                                                         | **Muy baja**: Los repositorios de código precisamente permiten que todo el público general acceda y consulte su contenido, lo cual no es sospechoso. |

# Enlaces

[WHOIS ->](web_whois.md)