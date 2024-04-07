[<- Índice](../Internet%20of%20Things%20(IoT).md)

## LEDS

#### Prendiendo un LED con peticiones HTTP

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//LED GPIO-------------------------------------
const int ledPin = 25;
String ledState;

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
    <h1>Clase IoT</h1>
  <h2>Control de un LED</h2>
  <p>Estado LED: <strong> %STATE%</strong></p>
  <p><a href="/on"><button>ON</button></a></p>
  <p><a href="/off"><button>OFF</button></a></p>
  </body>
</html>)rawliteral";

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
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

//Acciones a realizar--------------------------
String processor(const String& var){
  if(var == "STATE"){
    if(digitalRead(ledPin)){
      ledState = "ON";
    }
    else{
      ledState = "OFF";
    }
    Serial.print("Estado LED: ");
    Serial.println(ledState);
    return ledState;
  }
  return String();
}
 
void setup(){
  Serial.begin(115200);
  setup_wifi();
  pinMode(ledPin, OUTPUT);
  pinMode(14, OUTPUT);
  digitalWrite(ledPin, LOW);
  digitalWrite(14, LOW);

  //Comenzar el servidor-----------------------
  server.begin();

  //GET HTML-----------------------------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(200, "text/html", index_html);
  }); 

  //STATUS LED------------------------------------
  server.on("/on", HTTP_GET, [](AsyncWebServerRequest *request){
    digitalWrite(ledPin, HIGH);
    request->send_P(200, "text/html", index_html, processor);
  });

  server.on("/off", HTTP_GET, [](AsyncWebServerRequest *request){
    digitalWrite(ledPin, LOW);
    request->send_P(200, "text/html", index_html, processor);
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest *request){
    request->send(404, "text/plain", "Error 404");
  });
}
 
void loop(){
  
}
```

#### Prendiendo led mediante HTTP con archivos HTML y CSS

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
#include "SPIFFS.h"

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//LED GPIO-------------------------------------
const int ledPin = 25;
String ledState;

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
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

//Acciones a realizar--------------------------
String processor(const String& var) {
  Serial.println(var);
  if (var == "STATE") {
    if (digitalRead(ledPin)) {
      ledState = "ON";
    }
    else {
      ledState = "OFF";
    }
    Serial.print("Estado LED: ");
    Serial.println(ledState);
    return ledState;
  }
  return String();
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  pinMode(ledPin, OUTPUT);
  pinMode(14, OUTPUT);
  digitalWrite(ledPin, LOW);
  digitalWrite(14, LOW);

  //Comenzar el servidor-----------------------
  server.begin();

  //Revisar archivos subidos a la memoria flash---
  if (!SPIFFS.begin(true)) {
    Serial.println("Error al leer SPIFFS");
    return;
  }

  //GET HTML----------------------------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });

  //Archivo CSS--------------------------------
  server.on("/style.css", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/style.css", "text/css");
  });

  //Ruta donde el LED estará en estado ON------
  server.on("/on", HTTP_GET, [](AsyncWebServerRequest * request) {
    digitalWrite(ledPin, HIGH);
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });

  //Ruta donde el LED estará en estado OFF-----
  server.on("/off", HTTP_GET, [](AsyncWebServerRequest * request) {
    digitalWrite(ledPin, LOW);
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest * request) {
    request->send(404, "text/plain", "Error 404");
  });
}

void loop() {

}
```

```html
<!DOCTYPE html>
<html>
<head>
  <title>Control Led</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <h1>Clase IoT</h1>
  <h2>Control de un LED</h2>
  <p>Estado LED: <strong> %STATE%</strong></p>
  <p><a href="/on"><button class="button">ON</button></a></p>
  <p><a href="/off"><button class="button button2">OFF</button></a></p>
</body>
</html>
```

```css
html {
  font-family: Helvetica;
  display: inline-block;
  margin: 0px auto;
  text-align: center;
}
h1{
  color: #0d2a61;
  padding: 2vh;
}
h2{
  color: #10bcf0;
  padding: 1vh;
}
p{
  font-size: 1.5rem;
}
.button {
  display: inline-block;
  background-color: #008CBA;
  border: none;
  border-radius: 12px;
  color: white;
  padding: 16px 40px;
  text-decoration: none;
  font-size: 30px;
  margin: 2px;
  cursor: pointer;
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
}
.button2 {
  background-color: #990000;
}
```

#### Prendiendo 2 leds mediante peticiones HTTP con parametros

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
#include "SPIFFS.h"

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//LEDs GPIO-------------------------------------
#define ledPinBlue 25
#define ledPinRed 26
const char* PARAM_INPUT_1 = "output"; //Variable de led
const char* PARAM_INPUT_2 = "state"; //Variable de estado

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
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

//Acciones a realizar--------------------------
String processor(const String& var) {
  //Encender o apagar los leds, usando HTML
  if (var == "BUTTONPLACEHOLDER") {
    String buttons = "";
    buttons += "<h4>LED 4</h4><label class=\"switch\"><input type=\"checkbox\" onchange=\"toggleCheckbox(this)\" id=\"25\" " + outputState(ledPinBlue) + "><span class=\"slider1\"></span></label>";
    buttons += "<h4>LED 3</h4><label class=\"switch\"><input type=\"checkbox\" onchange=\"toggleCheckbox(this)\" id=\"26\" " + outputState(ledPinRed) + "><span class=\"slider2\"></span></label>";
    return buttons;
  }
  return String();
}

//Estado del led-------------------------------
String outputState(int output) {
  if (digitalRead(output)) {
    return "checked";
  }
  else {
    return "";
  }
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  //Leds---------------------------------------
  pinMode(ledPinBlue, OUTPUT);
  digitalWrite(ledPinBlue, LOW);
  pinMode(ledPinRed, OUTPUT);
  digitalWrite(ledPinRed, LOW);

  //Comenzar el servidor-----------------------
  server.begin();

  //Revisar archivos subidos a la memoria flash---
  if (!SPIFFS.begin(true)) {
    Serial.println("An Error has occurred while mounting SPIFFS");
    return;
  }

  //Ruta raíz del archivo HTML-----------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });

  //Archivo CSS--------------------------------
  server.on("/style.css", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/style.css", "text/css");
  });

  //Petición a "/update" con el método GET
  server.on("/update", HTTP_GET, [] (AsyncWebServerRequest * request) {
    String inputMessage1;
    String inputMessage2;
    
    //Se obtienen los parámetros de <ESP_IP>/update?output=<inputMessage1>&state=<inputMessage2>
    if (request->hasParam(PARAM_INPUT_1) && request->hasParam(PARAM_INPUT_2)) {
      inputMessage1 = request->getParam(PARAM_INPUT_1)->value();
      inputMessage2 = request->getParam(PARAM_INPUT_2)->value();
      //Convertir los valores a enteros y utilizarlos, para controlar un pin GPIO
      digitalWrite(inputMessage1.toInt(), inputMessage2.toInt());
    }
    else {
      inputMessage1 = "Mensaje no enviado";
      inputMessage2 = "Mensaje no enviado";
    }
    //Imprime el pin accionado y el estado
    Serial.print("GPIO: ");
    Serial.print(inputMessage1);
    Serial.print(" - estado: ");
    Serial.println(inputMessage2);

    //Responder a la solicitud con un código de estado 200 (OK) y un mensaje en texto plano
    request->send(200, "text/plain", "OK");
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest * request) {
    request->send(404, "text/plain", "Error 404");
  });
}

void loop() {

}
```

```html
<!DOCTYPE HTML><html>
<head>
  <title>Switch Button</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" href="data:,">
  <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  <h2>Switch Button</h2>
  %BUTTONPLACEHOLDER%
  <script>
    function toggleCheckbox(element) {
      // Crear un objeto XMLHttpRequest
      var xhr = new XMLHttpRequest();
  
      // Verificar si el checkbox está marcado
      if (element.checked) {
        // Si está marcado, abrir una solicitud GET con el estado 1
        xhr.open("GET", "/update?output=" + element.id + "&state=1", true);
      } else {
        // Si no está marcado, abrir una solicitud GET con el estado 0
        xhr.open("GET", "/update?output=" + element.id + "&state=0", true);
      }
  
      // Enviar la solicitud al servidor
      xhr.send();
    }
  </script>
</body>
</html>
```

```css
html {font-family: Arial; 
    display: inline-block; 
    text-align: center;}
h2 {font-size: 4.0rem;
    color: rgb(5, 11, 58);}
p {font-size: 3.0rem;}
body {max-width: 600px; 
    margin:0px auto; 
    padding-bottom: 25px;}
.switch {position: relative; 
    display: inline-block; 
    width: 120px; 
    height: 68px} 
.switch input {display: none}
.slider1 {position: absolute; 
    top: 0; 
    left: 0; 
    right: 0; 
    bottom: 0; 
    background-color: #ccc; 
    border-radius: 6px}
.slider1:before {position: absolute; 
    content: ""; 
    height: 52px; 
    width: 52px; 
    left: 8px; 
    bottom: 8px; 
    background-color: #fff; 
    -webkit-transition: .4s; 
    transition: .4s; 
    border-radius: 3px}
.slider2 {position: absolute; 
    top: 0; 
    left: 0; 
    right: 0; 
    bottom: 0; 
    background-color: #ccc; 
    border-radius: 6px}
.slider2:before {position: absolute; 
    content: ""; 
    height: 52px; 
    width: 52px; 
    left: 8px; 
    bottom: 8px; 
    background-color: #fff; 
    -webkit-transition: .4s; 
    transition: .4s; 
    border-radius: 3px}
input:checked+.slider1 {background-color: #2521d8}
input:checked+.slider2 {background-color: #da1425}
input:checked+.slider1:before {-webkit-transform: translateX(52px); -ms-transform: translateX(52px); transform: translateX(52px)}
input:checked+.slider2:before {-webkit-transform: translateX(52px); -ms-transform: translateX(52px); transform: translateX(52px)}
```

#### Modificando el brillo del LED por PWM mediante peticiones HTTP

Para este ejemplo, se incluyo un archivo *.js*, para modificar el comportamiento de la página web.

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
#include "SPIFFS.h"

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//LED GPIO-------------------------------------
const int ledPin = 25;
String sliderValue = "0";

//PWM propiedades------------------------------
const int freq = 1000;
const int ledChannel = 0;
const int resolution = 8;
const char* PARAM_INPUT = "value";

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
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

// Replaces placeholder with LED state value---
String processor(const String& var){
  if (var == "SLIDERVALUE"){
    return sliderValue;
  }
  return String();
}
 
void setup(){
  Serial.begin(115200);
  setup_wifi();
  pinMode(ledPin, OUTPUT);
  pinMode(14, OUTPUT);
  digitalWrite(14, LOW);

  //Activar PWM--------------------------------
  ledcSetup(ledChannel, freq, resolution);
  ledcAttachPin(ledPin, ledChannel);
  ledcWrite(ledChannel, sliderValue.toInt());

  //Confirmar archivos subidos
  //a la memoria flash-------------------------
  if(!SPIFFS.begin(true)){
    Serial.println("Error al leer SPIFFS");
    return;
  }

  //Comenzar el servidor-----------------------
  server.begin();

  //GET HTML-----------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });
  
  //Archivo CSS--------------------------------
  server.on("/style.css", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(SPIFFS, "/style.css", "text/css");
  });

  //Archivo JS---------------------------------
  server.on("/script.js", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(SPIFFS, "/script.js", "text/js");
  });

  server.on("/slider", HTTP_GET, [] (AsyncWebServerRequest *request) {
    String inputMessage;
    if (request->hasParam(PARAM_INPUT)) {
      inputMessage = request->getParam(PARAM_INPUT)->value();
      sliderValue = inputMessage;
      ledcWrite(ledChannel, sliderValue.toInt());
    }
    else {
      inputMessage = "Mensaje no enviado";
    }
    Serial.println(inputMessage);
    request->send(200, "text/plain", "OK");
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest *request){
    request->send(404, "text/plain", "Error 404");
  });
}
 
void loop(){
  
}
```

```html
<!DOCTYPE HTML><html>
<head>
	<title>PWM Led</title>
  	<meta name="viewport" content="width=device-width, initial-scale=1">
 	<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
  	<h2>PWM Led</h2>
 	<p><span id="textSliderValue">%SLIDERVALUE%</span></p>
	<p><input type="range" onchange="updateSliderPWM(this)" id="pwmSlider" min="0" max="255" value="%SLIDERVALUE%" step="1" class="slider"></p>
	
	<script src="script.js"></script>
</body>
</html>
```

```css
html{
	font-family: Helvetica; 
	display: inline-block; 
	margin: 0px auto; 
	text-align: center;
}

h2{
	color: #0F3376; 
	padding: 2vh; 
	font-size: 2.3rem;
}

p{
	font-size: 1.9rem;
}

body{
	max-width: 400px; 
	margin:0px auto; 
	padding-bottom: 25px;
}
.slider{
	-webkit-appearance: none; 
	margin: 14px; 
	width: 360px; 
	height: 25px; 
	background: #07061a;
      	outline: none; 
	-webkit-transition: .2s; 
	transition: opacity .2s;
}
    
.slider::-webkit-slider-thumb{
	-webkit-appearance: none; 
	appearance: none; 
	width: 35px; 
	height: 35px; 
	background: #389bc9; 
	cursor: pointer;
}

.slider::-moz-range-thumb{ 
	width: 35px; 
	height: 35px; 
	background: #389bc9; 
	cursor: pointer;
}
```

```js
function updateSliderPWM(element) {
    //Obtiene el valor actual del slider
      var sliderValue = document.getElementById("pwmSlider").value;

    //Actualiza el contenido del span con el ID "textSliderValue" con el valor actual del slider
      document.getElementById("textSliderValue").innerHTML = sliderValue;

    console.log(sliderValue);

    //Crea objeto para peticiones desde el navegador
      var xhr = new XMLHttpRequest();

    //Golicitud GET con la URL "/slider" y un parámetro "value" que lleva el valor actual del slider
    //La solicitud será asincrónica (true).
      xhr.open("GET", "/slider?value="+sliderValue, true);

    //Envía la solicitud al servidor.
      xhr.send();
}
```

## Sensores

#### Leyendo temperatura y humedad del ESP32 mediante peticiones HTTP

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
#include "SPIFFS.h"
//#include <Adafruit_Sensor.h>
#include <DHT.h>

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//Sensor---------------------------------------
#define DHTPIN 32
//Uncomment the type of sensor in use:
#define DHTTYPE    DHT11     // DHT 11
//#define DHTTYPE    DHT22     // DHT 22 (AM2302)
//#define DHTTYPE    DHT21     // DHT 21 (AM2301)
DHT dht(DHTPIN, DHTTYPE);
float temp = 0;
float hum = 0;
long timeDHT = 0;

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
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

String readDHTTemperature() {
  String t = String(temp);
  return t;
}

String readDHTHumidity() {
  String h = String(hum);
  return h;
}

// Replaces placeholder with DHT values
String processor(const String& var) {
  if (var == "TEMPERATURE") {
    return readDHTTemperature();
  }
  else if (var == "HUMIDITY") {
    return readDHTHumidity();
  }
  return String();
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  dht.begin();
  pinMode(14, OUTPUT);
  digitalWrite(14, LOW);

  //Confirmar archivos subidos
  //a la memoria flash-------------------------
  if (!SPIFFS.begin(true)) {
    Serial.println("Error al leer SPIFFS");
    return;
  }

  // Start server
  server.begin();

  //GET HTML-----------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });
  //Archivo CSS--------------------------------
  server.on("/style.css", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/style.css", "text/css");
  });
  //Archivo JS--------------------------------
  server.on("/script.js", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/script.js", "text/js");
  });
  //Enviar dato sensor-------------------------
  server.on("/temperature", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send_P(200, "text/plain", readDHTTemperature().c_str());
  });
  server.on("/humidity", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send_P(200, "text/plain", readDHTHumidity().c_str());
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest * request) {
    request->send(404, "text/plain", "Error 404");
  });
}

void loop() {
  if (millis() - timeDHT > 2000){
    hum = dht.readHumidity();
    temp = dht.readTemperature();
    Serial.print(hum);
    Serial.print(", ");
    Serial.println(temp);
  
    if (isnan(hum) || isnan(temp)) {
      Serial.println("Failed to read from DHT sensor!");
      return;
    }
    timeDHT = millis();
  }
}
```

###### HTML

```html
<!DOCTYPE HTML>
<html>

<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.2/css/all.css" integrity="sha384-fnmOCqbTlWIlj8LyTjo7mOUStjsKC4pOpQbqyi7RrhN7udi9RwhKkMHpvLbHG9Sr" crossorigin="anonymous">
  <link rel="stylesheet" type="text/css" href="style.css">
  <title>Clase IoT</title>
</head>

<body>
  <h2>DHT Server</h2>
  <p>
    <i class="fas fa-thermometer-half" style="color:#cd1824;"></i> 
    <span class="dht-labels">Temperatura</span> 
    <span id="temperature">%TEMPERATURE%</span>
    <sup class="units">&deg;C</sup>
  </p>
  <p>
    <i class="fas fa-tint" style="color:#00add6;"></i> 
    <span class="dht-labels">Humedad</span>
    <span id="humidity">%HUMIDITY%</span>
    <sup class="units">&percnt;</sup>
  </p>
</body>
<script src="script.js"></script>

</html>
```

###### CSS

```css
html {
    font-family: Arial;
    display: inline-block;
    margin: 0px auto;
    text-align: center;
}

h2 { 
    font-size: 4.0rem;
    color: darkblue;
}

p { 
    font-size: 3.0rem; 
}

.units { 
    font-size: 1.2rem; 
}
   
.dht-labels{
    font-size: 1.5rem;
    vertical-align:middle;
    padding-bottom: 15px;
}
```

###### Javascript

```js
setInterval(function ( ) {
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
      if (this.readyState == 4 && this.status == 200) {
        document.getElementById("temperature").innerHTML = this.responseText;
      }
    };
    xhttp.open("GET", "/temperature", true);
    xhttp.send();
  }, 5000 ) ;

  
setInterval(function ( ) {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById("humidity").innerHTML = this.responseText;
    }
  };
  xhttp.open("GET", "/humidity", true);
  xhttp.send();
}, 5000 ) ;
```

#### Leyendo el sensor mediante peticiones HTTP

```ino
//Bibliotecas----------------------------------
#include "WiFi.h"
#include "ESPAsyncWebServer.h"
#include "SPIFFS.h"

//Credenciales Red-----------------------------
const char* ssid = "Clase_IoT";
const char* password = "0123456789";

//SENSOR GPIO----------------------------------
const int sensorPin = 34; //LDR

//Objeto AsyncWebServer, puerto 80-------------
AsyncWebServer server(80);

//Conexión WiFi--------------------------------
void setup_wifi() {
  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
  
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

//Leer el sensor-------------------------------
String ReadSensor() {
  float sensorVal = analogRead(sensorPin)*(3.3/4096.0);
  Serial.println(sensorVal);
  return String(sensorVal);
}

//Acciones a realizar--------------------------
String processor(const String& var){
  if (var == "SENSOR"){
    return ReadSensor();
  }
  return String();
}
 
void setup(){
  Serial.begin(115200);
  setup_wifi();
  pinMode(sensorPin, INPUT);
  pinMode(14, OUTPUT);
  digitalWrite(14, LOW);

  //Confirmar archivos subidos
  //a la memoria flash-------------------------
  if(!SPIFFS.begin(true)){
    Serial.println("Error al leer SPIFFS");
    return;
  }

  //Comenzar el servidor-----------------------
  server.begin();

  //GET HTML-----------------
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(SPIFFS, "/index.html", String(), false, processor);
  });
  
  //Archivo CSS--------------------------------
  server.on("/style.css", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(SPIFFS, "/style.css", "text/css");
  });

  //Archivo JS--------------------------------
  server.on("/script.js", HTTP_GET, [](AsyncWebServerRequest * request) {
    request->send(SPIFFS, "/script.js", "text/js");
  });

  //Enviar dato sensor-------------------------
  server.on("/sensor", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send_P(200, "text/plain", ReadSensor().c_str());
  });

  //Función para error 404
  server.onNotFound([](AsyncWebServerRequest *request){
    request->send(404, "text/plain", "Error 404");
  });
}
 
void loop(){

}
```

###### HTML

```html
<!DOCTYPE HTML>
<html>

<head>
  <title>Clase IoT</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" type="text/css" href="style.css">
</head>

<body>
  <h2>Voltaje LDR</h2>
  <p>
    <span class="sensor">Voltaje:</span>
    <span id="SENSOR_ID">%SENSOR%</span>
    <sup class="units">V</sup>
  </p>
</body>
<script src="script.js"></script>

</html>
```

###### CSS

```css
html{
	font-family: Arial;
	display: inline-block;
	margin: 0px auto;
	text-align: center;
}

h2{
	font-size: 3.5rem;
	color: 	#2c33af;
}

p{
	font-size: 3.0rem;
}

.units{
	font-size: 1.2rem;
}

.sensor{
	font-size: 1.5rem;
	vertical-align:middle;
	padding-bottom: 15px;
}
```

###### JavaScript

```js
//setInterval se usa para ejecutar una función cada X tiempo
setInterval(function () {
    //Objeto que se utiliza para interactuar con los servidores de forma asíncrona.
    var xhttp = new XMLHttpRequest();

    xhttp.onreadystatechange = function () {
      //Si la condición se ha completado (readyState == 4)
      //y si el estado de la respuesta es OK (status == 200)
      if (this.readyState == 4 && this.status == 200) {
        //Actualiza el contenido del HTML del id:"SENSOR_ID"
        document.getElementById("SENSOR_ID").innerHTML = this.responseText;
      }
    };
    //Método que inicializa la solicitud
    xhttp.open("GET", "/sensor", true);
    //Se envía la solicitud
    xhttp.send();
  },
  //Tiempo de espera entre ejecución y ejecución (ms)
  100);
```

# Enlace

[<- Anterior](Codigos%20b%C3%A1sicos%20HTTP.md) | [Siguiente ->](Protocolos%20de%20mensajeria%20(MQTT).md)