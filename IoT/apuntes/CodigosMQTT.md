[<- Índice](../InternetOfThings.md)
#### Configuración básica del MQTT

```ino
//Biblioteca para MQTT
#include "EspMQTTClient.h"

#define pinSensor 34 //LDR

//Variables de configuración del objeto MQTT cliente
const char* ssid = "Clase_IoT";          //Nombre de red
const char* password = "0123456789";   //Contraseña de red
const char* broker = "test.mosquitto.org";  //Dirección del broker
const char* nameClient = "ESP32_Vazzvel";   //Nombre del dispositivo
const int port = 1883;                      //Puerto

//Tópicos
String topicSubA = "ClaseIoT/Vazzvel/SubA";
String topicSubB = "ClaseIoT/Vazzvel/SubB";
String topicPubA = "ClaseIoT/Vazzvel/PubA";
String topicPubB = "ClaseIoT/Vazzvel/PubB";

long timePub = 0; //tiempo auxiliar

//Objeto cliente
EspMQTTClient client(ssid, password, broker, nameClient, port);

void setup() {
  Serial.begin(115200);
  pinMode(pinSensor, INPUT);
  
  if (!client.isConnected()){
    Serial.println("Conectado con el broker!");
  }
  else{
    Serial.println("No Conectado, revise su conexión");
  }
  delay(1000);  
}

//Función que se encarga de suscribirse a los tópicos
void onConnectionEstablished(){
  client.subscribe(topicSubA, [](const String & payloadA) {
    Serial.print("El tópico en subA es: ");
    Serial.println(payloadA);
  });

  client.subscribe(topicSubB, [](const String & payloadB) {
    Serial.print("El tópico en subB es: ");
    Serial.println(payloadB);
  });
}

void loop() {
  client.loop(); //Manejador del cliente MQTT

  if (millis() - timePub > 1000){
    int val = random(0, 25); //valor aleatorio entre 0 y 25
    client.publish(topicPubA, String(val)); //Publica la variable var en tipo String
    Serial.print("El mensaje enviado al tópico PubA, es: ");
    Serial.println(val);

    int sensor = analogRead(pinSensor);
    client.publish(topicPubB, String(sensor)); //Publica la variable var en tipo String
    Serial.print("El mensaje enviado al tópico PubB, es: ");
    Serial.println(sensor);

    timePub = millis();
  }
}
```

#### Leyendo temperatura y humedad mediante MQTT

```ino
//Bibliotecas
#include "EspMQTTClient.h"
#include "DHT.h"

//Definimos pines
#define pinLED 25
#define DHTPIN 32
#define DHTTYPE DHT11

//Variables de configuración del objeto MQTT cliente
const char* ssid = "Clase_IoT";          //Nombre de red
const char* password = "0123456789";   //Contraseña de red
const char* broker = "test.mosquitto.org";  //Dirección del broker
const char* nameClient = "ESP32_Vazzvel";   //Nombre del dispositivo
const int port = 1883;                      //Puerto

//Tópicos
String T_LEDS = "ClaseIoT/Vazzvel/Leds";
String T_TEMP = "ClaseIoT/Vazzvel/DHT/Temp";
String T_HUME = "ClaseIoT/Vazzvel/DHT/Hume";

//Variables globales
long timeDHT = millis();

//Objeto cliente
DHT dht(DHTPIN, DHTTYPE);
EspMQTTClient client(ssid, password, broker, nameClient, port);


void setup() {
  Serial.begin(115200);
  dht.begin();
  pinMode(pinLED, OUTPUT);

  if (!client.isConnected()) {
    Serial.println("Conectado con el broker!");
  }
  else {
    Serial.println("No Conectado, revise su conexión");
  }
  delay(5000);
}


void onConnectionEstablished() {
  //Prender y apagar LEDs
  client.subscribe(T_LEDS, [](const String & payload) {
    if (payload == "1") {
      digitalWrite(pinLED, HIGH);
      Serial.println("Led encendido");
    }
    else if (payload == "0") {
      digitalWrite(pinLED, LOW);
      Serial.println("Led apagado");
    }
    else {
      Serial.println("Dato no válido");
    }
  });
}

void loop() {
  client.loop();
  //Espero 2.5 s para leer el sensor y publicar los datos
  if (millis() - timeDHT >= 2500) { 
    float h = dht.readHumidity();
    float t = dht.readTemperature();
    if (isnan(h) || isnan(t)) {
      Serial.println("Failed to read from DHT sensor!");
      return;
    }

    //Publica la humedad y temperatura
    client.publish(T_TEMP, String(t));
    client.publish(T_HUME, String(h));
    Serial.print("Humedad: ");
    Serial.print(h);
    Serial.print(" %,  Temperatura: ");
    Serial.print(t);
    Serial.println(" °C");

    timeDHT = millis();
  }
}
```

# Enlaces

[<- Anterior](ProtocolosDeMensajeria_MQTT.md) | [Siguiente ->](Ciberseguridad.md)