[<- Índice](../WebHacking.md)
# Aplicaciones Web

> Las aplicaciones web son aplicaciones interactivas mediante el protocolo *HTTP* que pretenden ofrecer funcionalidades y soluciones especificas para un grupo de usuarios.

Dado que el protocolo *HTTP* es una comunicación modelo cliente-servidor, tiene 2 componentes principales.

### Client-side (Frontend)

Toda la interfaz del usuario

- *HTML* - Estructura
- *CSS* - Estilo
- *Javascript* - Dinamismo

Clientes más comunes:

- Navegadores web
- Bots/*Scripts*
- Cualquier cosa que pueda realizar peticiones *HTTP*

### Server-side (Backend)

El grueso de la implementación de la aplicación.

Servidores web comunes:
- Apache
- Nginx
- IIS

**Lenguajes comunes**:

- *PHP*
- *Java*
- *Python*
- *Javascript* (Sí, se puede implementar todo el *backend* con *Javascript*)

**Frameworks y CMS**:

- Angular (Enfocado a *Javascript*)
- Django (Enfocado a *Python*)
- Wordpress
- Drupal

### Otras consideraciones de arquitectura

- CDNs: Hacer entrega de contenido estático (que casi nunca cambia)

- Balanceadores de carga (Tienes varios servidores que se encargan de responder a peticiones)

- *Proxies* inversos

- *WAF*: Es *Web Application Firewall*, ayuda a limitar las comunicaciones, trabaja unicamente en la ==capa de aplicación== y detiene comunicaciones malintencionadas.

### ¿Donde implementamos la seguridad *Frontend* o *Backend*?

> Principalmente en el **Backend** pero nunca está de más implementar seguridad también en el *Frontend*.

## Repaso: *HTML*, *CSS*, *JavaScript*

### HTML

- *Hypertext Markup Language*
- Lenguaje de etiquetas o marcas
- Estructura principal de la página
- Utiliza etiquetas para señalizar el contenido
- Normalmente hay un *CSS* y/o un *Javascript* dentro del *HTML*

### CSS

- Lenguaje de hoja de estilos
- *Cascading Style Sheet*
- Definir el estilo de la página
- Utiliza selectores para señalizar el contenido
- Hay veces que hay comentarios útiles, sin embargo no es nuestro principal objetivo de estudio

### Javascript

- Lenguaje de programación
- Define la interactividad y el dinamismo de la página
- Es Turing Completo y tiene sintaxis parecida a *C*

### ¿Porque es importante este repaso?

Las aplicaciones web actualmente están principalmente compuestas por estos 3 tipos de archivos para funcionar correctamente.

Aunque los desarrolladores web utilicen *Typescript*, *Angular*, *Wordpress*, etc; al final del día estas herramientas transpilan sus lenguajes a estos 3 archivos para que funcione correctamente la página web.

## CRUD API

> Existen muchos tipos de *API*'s cuyo objetivo es interactuar con una abse de datos. Una de las más conocidas es la *CRUD*.

| Operación | Método | Descripción                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| Create    | POST   | Crea un nuevo elemento en la base de datos/API.                                 |
| Read      | GET    | Consulta una entrada de la base de datos.                                       |
| Update    | PUT    | Actualiza una entrada en la *API*, es decir, la remplaza con la que se adjunte. |
| Delete    | DELETE | Borra una entrada en la *API*                                                   |

# Pruebas de penetración

## Metodologías

**Ejemplos**:
- *OWASP testing Guides*
- *Penetration Testing Execution standard*
- Metodologías personales
- Metodologías internas

**Etapas**:
- Planeación
- Recolección de inteligencia / ==reconocimiento pasivo==
- Reconocimiento activo
- Análisis de vulnerabilidades
- Explotación
- Post-explotación
- Reportes
## Reconocimiento

#### Reconocimiento pasivo

Se lleva a cabo sin interactuar directamente con el objetivo (*OSINT*):

- Subdominios (Ej. `dnsdumpster`)
- Correos válidos (Ej. Hunter.io)
- Versiones anteriores (Ej. web.archive.org)
- Recursos indexados (Ej. *Google Dorks*)
- Etc

#### Reconocimiento activo

Se lleva a cabo interactuando directamente con el objetivo, lo cual deja huella en las bitácoras.

- Detección de tecnologías (*wappalyzer* : Detecta con que tecnologías se desarrolló un sitio web, *builtwith*)
- Spidering (Ej. Zap, Burpsuite)
- Forced Browsing (Ej. *WFuzz*, *gobuster*, *dirb*)
- Detección de *Virtual hosts* (Ej. *WFuzz*)

# Enlaces

[<- Anterior](HFC25_09_2024.md) | [Siguiente ->](HFC26_09_2024.md)