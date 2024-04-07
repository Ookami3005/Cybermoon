[<- Índice](../Internet%20of%20Things%20(IoT).md)

## HTTP
#### Devolviendo texto plano a las peticiones del cliente

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
  
//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.println("Dirección IP: ");
  Serial.println(WiFi.localIP());
  delay(2000);
}
 
void setup(){
  Serial.begin(115200);
  setup_wifi();
  
  //Comenzar el servidor-----------------------
  server.begin();

  //GET texto plano----------------------------
  /*
   * Elementos de la función server.on:
      server.on(Ruta, Método, [](AsyncWebServerRequest *request){
        request->send(Código de estado, tipo, mensaje);
      });
  */

  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/plain", "Hola, grupo de IoT");
  }); 

  server.on("/pageA", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/plain", "Web A");
  });
  
  server.on("/pageB", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/plain", "Web B");
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest *request){
    request->send(404, "text/plain", "Error 404");
  });
}
 
void loop(){

}
```

#### Devolviendo texto en formato HTML a las peticiones del cliente

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
/*
 * Decargar las bibliotecas de:
 * https://github.com/me-no-dev/ESPAsyncWebServer/archive/master.zip}
 * https://github.com/me-no-dev/AsyncTCP/archive/master.zip
*/

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Código HTML----------------------------------
const char index_html[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML><html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Clase IoT</title>
  </head>
  <body>
    <h2>Hola, grupo de IoT</h2>
    <p>Este es un ejemplo de texto en HTML.</p>
    <p>Tiene buena pinta.</p>
    <a href="/pageA" >Web A</a>
    <a href="/pageB" >Web B</a>
  </body>
</html>)rawliteral";

const char pageA[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML><html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Clase IoT</title>
  </head>
  <body>
    <h2>Web A</h2>
    <p>Vamos bien?</p>
    <p>Alguna pregunta?</p>
  </body>
</html>)rawliteral";

const char pageB[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML><html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Clase IoT</title>
  </head>
  <body>
    <h2>Web B</h2>
    <p>Vamos bien?</p>
    <p>Alguna pregunta?</p>
  </body>
</html>)rawliteral";


//Conexión WiFi--------------------------------
void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.println("Dirección IP: ");
  Serial.println(WiFi.localIP());
  delay(2000);
}
 
void setup(){
  Serial.begin(115200);
  setup_wifi();
  
  //Comenzar el servidor-----------------------
  server.begin();

  //GET HTML-----------------------------------
  /*
   * Elementos de la función server.on:
      server.on(Ruta, Método, [](AsyncWebServerRequest *request){
        request->send(Status Code, tipo, mensaje);
      });
  */
  
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/html", index_html);
  }); 

  server.on("/pageA", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/html", pageA);
  });

  server.on("/pageB", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/plain", pageB);
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest *request){
    request->send(404, "text/plain", "Error 404");
  });
}
 
void loop(){

}
```

#### Devolviendo texto HTML con formato CSS a las peticiones del cliente

En este ejemplo, el CSS se escribe adentro del mismo *HTML* usando las etiquetas `<style></style>`

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
/*
   Decargar las bibliotecas de:
   https://github.com/me-no-dev/ESPAsyncWebServer/archive/master.zip}
   https://github.com/me-no-dev/AsyncTCP/archive/master.zip
*/

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Código HTML----------------------------------
const char index_html[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML><html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Clase IoT</title>
    <style>
      html {
        font-family: Arial;
        display: inline-block;
        margin: 0px auto;
        text-align: center;
      }
      h2 { font-size: 4.0rem;
           color: #059e8a;
           }
      p { font-size: 2.5rem; }
    </style>  
    </head>
  <body>
    <h2>ESP32 IoT</h2>
    <p>Este es un ejemplo de texto en HTML con un poco de formato</p>
    <p>Tiene buena pinta</p>
    <p><a href="/pageA">Web A</a></p>
  </body>
</html>)rawliteral";

const char pageA[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML><html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Clase IoT</title>
    <style>
      html {
        font-family: Arial;
        display: inline-block;
        margin: 0px auto;
        text-align: center;
      }
      h2 { font-size: 6.0rem;
           color: #030e8a;
           }
      p { font-size: 3rem; }
    </style> 
  </head>
  <body>
    <h2>Web A</h2>
    <p>Vamos bien?</p>
    <p>Alguna pregunta?</p>
    <p><a href="/">Index</a></p>
  </body>
</html>)rawliteral";

//Conexión WiFi--------------------------------
void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.println("Dirección IP: ");
  Serial.println(WiFi.localIP());
  delay(2000);
}

void setup() {
  Serial.begin(115200);
  setup_wifi();

  //Comenzar el servidor-----------------------
  server.begin();

  //GET HTML-----------------------------------
  /*
     Elementos de la función server.on:
      server.on(Ruta, Método, [](AsyncWebServerRequest *request){
        request->send(Status Code, tipo, mensaje);
      });
  */
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(200, "text/html", index_html);
  });

  server.on("/pageA", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(200, "text/html", pageA);
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest * request) {
    request->send(404, "text/plain", "Error 404");
  });
}

void loop() {

}
```

---

## SPIFFS
#### Ejemplo básico utilizando SPIFFS

Para este ejemplo, se utilizo SPIFFS para forzar un sistema de archivos en la memoria del *ESP32* y poder almacenar nuestros archivos *.html, .css, .js*.

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
#include "SPIFFS.h" 

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
  delay(10);
  Serial.print("Conectando a: ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi conectado");
  Serial.println("Dirección IP: ");
  Serial.println(WiFi.localIP());
  delay(2000);
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  
  //Inicializar SPIFFS-------------------------
  if(!SPIFFS.begin(true)){
    Serial.println("An Error has occurred while mounting SPIFFS");
    return;
  }

  //Comenzar el servidor-----------------------
  server.begin();

  //GET HTML-----------------------------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/index.html", String(), false);
  });

  server.on("/pageA", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/pageA.html", String(), false);
  });
  
  //Cargar style.css---------------------------
  server.on("/style.css", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(SPIFFS, "/style.css", "text/css");
  });

  //Archivos-----------------------------------
  server.on("/meme.webp", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/meme.webp", String(), true);
  });

  server.on("/gato.webp", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/gato.webp", String(), false);
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest *request){
    request->send(404, "text/plain", "Error 404");
  });
}

void loop() {

}
```

#### Listar los archivos almacenados por SPIFFS

```ino
//Bibliotecas----------------------------------
#include "SPIFFS.h"

int count = 0; //Contador

//Función para listar archivos
void listarArchivos() {
  File root = SPIFFS.open("/"); //Se declara "/" como ubicación root
  File file = root.openNextFile(); //En file se asigna el primer archivo

  //Mientras haya archivos, se imprimen los nombres
  while (file) {
    count++;
    Serial.print("Archivo ");
    Serial.print(count);
    Serial.print(": ");
    Serial.println(file.name()); //Imprime nombre de archivo
    file = root.openNextFile();  //Pasa al siguiente archivo
  }
}

void setup() {
  Serial.begin(115200); //Inicia puerto serial

  //Inicializa SPIFFS e imprime mensaje de error si hay algún problema
  if (!SPIFFS.begin(true)) {
    Serial.println("Ocurrió un error al ejecutar SPIFFS.");
    return;
  }

  //Llama a la función para listar los archivos
  Serial.println("Listado de archivos:");
  listarArchivos();
}

void loop() {}
```
# Enlaces

[<- Anterior](SPIFFS.md) | [Siguiente ->](Codigos%20avanzados%20HTTP.md)