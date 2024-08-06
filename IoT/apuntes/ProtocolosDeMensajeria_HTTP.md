[<- Índice](../InternetOfThings.md)
## ¿Qué es?

*HTTP*, de sus siglas en inglés: "HyperText Transfer Control", es el nombre de un protocolo el cual nos permite realizar una petición de datos y recursos, como pueden ser los documentos *HTML*.

Es un protocolo basado en el principio de cliente-servidor. Las peticiones son enviadas por una entidad: el agente del usuario (*cliente*).

![http.png](imagenes/http.png)

#### Características

- *HTTP* es sencillo (facilmente interpretado por personas)
- *HTTP* es extensible (apliación por uso de cabeceras)
- *HTTP* es un protocolo con sesiones, pero sin estados (*cookies*)

#### Componentes

![componentesHttp.png](imagenes/componentesHttp.png)

#### Mensajes

###### Peticiones

![peticionesHttp.png](imagenes/peticionesHttp.png)

###### Respuestas

![respuestasHttp.png](imagenes/respuestasHttp.png)

## Implementación ESP32

![httpEsp32.png](imagenes/httpEsp32.png)

#### Tipos de servidores

- Síncronos
- Asíncronos

![tiposDeServidores.png](imagenes/tiposDeServidores.png)

#### Bibliotecas

- ESPAsyncWebServer
- AsyncTCP

#### Archivos necesarios

- *HTML*
- *CSS*
- *JavaScript*

![archivosNecesarios.png](imagenes/archivosNecesarios.png)

###### HTML

*HTML* (HyperText Markup Language) es el bloque de construcción más básico de la Web. Define el significado y la estructura del contenido web. Otras tecnologías además de *HTML* se utilizan generalmente para describir la apariencia/presentación (*CSS*) o la funcionalidad/comportamiento (*JavaScript*) de una página web. 

###### CSS

Las hojas de estilo en cascada (*CSS*) son un lenguaje de hojas de estilo que se utiliza para describir la presentación de un documento escrito en *HTML* o *XML*. *CSS* describe cómo se deben representar los elementos en la pantalla, en el papel, en el habla o en otros medios.

###### JavaScript

*JavaScript* es un lenguaje de programación que te permite implementar cosas complejas en las páginas web.

#### Aplicación

> `request->send(200,"text/plain","Ontas?");`

## WebSockets

##### ¿Qué es?

Es una *API* que nos permite realizar comunicación **bidireccional y abierta** entre 2 dispositivos; en este preciso caso, entre un cliente y un servidor

![webSockets.png](imagenes/webSockets.png)

![httpsVsWebSocket.png](imagenes/httpsVsWebSocket.png)

# Enlaces

[<- Anterior](CodigosConectividad.md) | [Siguiente ->](SPIFFS.md)