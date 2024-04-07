[<- Índice](../Internet%20of%20Things%20(IoT).md)
## Bluetooth

Para comunicarse mediante *Bluetooth* con el *ESP32*, utilizamos una aplicación llamada *Serial Bluetooth Terminal*

#### ESP32 en modo *esclavo*

Con este código el *ESP32* prende un LED cuando recibe mediante *Bluetooth* un caracter "1" y lo apaga cuando recibe un "0".

```ino
//Incluir librería de bluetooth
#include "BluetoothSerial.h"

//Condición para habilitar el bluetooth
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

//Definir un pin para el LED
#define LED 26

BluetoothSerial BT; //Objeto Bluetooth

void setup() {
  Serial.begin(115200); //Inicializar la comunicación serial
  pinMode (LED, OUTPUT); //Pin de LED como salida
  BT.begin("ESP32_Vazzvel_Slave"); //Nombre de su dispositivo Bluetooth y en modo esclavo
  //Cambiar ESP32_Vazzvel_Slave por otro nombre.
  Serial.println("El dispositivo Bluetooth está listo para emparejarse");
}

void loop() {
  if (BT.available()) // Si hay datos por recibir vía bluetooth
  {
    int val = BT.read(); // Lee un byte de los datos recibidos
    Serial.print("Recibido: ");
    Serial.println(val);
    if (val == 49) { // 1 en ASCII
      digitalWrite(LED, HIGH);
    }
    if (val == 48) { // 0 en ASCII
      digitalWrite(LED, LOW);
    }
  }
}
```

#### ESP32 en modo *maestro*

El *ESP32* se conecta, como **maestro** a otro dispositivo y envia repetidamente un caracter "1" y un "0".

```ino
//Incluir librería de bluetooth
#include "BluetoothSerial.h"

//Condición para habilitar el bluetooth
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

BluetoothSerial BT; // Objeto Bluetooth

//Variable con el nombre del cliente (dispositivo a conectarse - esclavo)
String clientName = "ESP32_Vazzvel_Slave"; 
bool connected;

void setup() {
  Serial.begin(115200); //Inicializar la comunicación serial
  BT.begin("ESP32_Vazzvel_Master", true); //Nombre de su dispositivo Bluetooth y en modo maestro
  Serial.println("El dispositivo Bluetooth está en modo maestro.\n Conectando con el anfitrión ...");

  connected = BT.connect(clientName); //Inicializar la conexión vía bluetooth
  if (connected) {
    Serial.println("¡Conectado exitosamente!");
  } else {
    while (!BT.connected(10000)) {
      Serial.println("No se pudo conectar. \n Asegúrese de que el dispositivo remoto esté disponible y dentro del alcance, \n luego reinicie la aplicación.");
    }
  }
}

void loop() {
  BT.write(49); // Envía 1 en ASCII
  delay(200);
  BT.write(48); // Envía 0 en ASCII
  delay(200);
}
```

#### Bluetooth Serial Port Profile (SPP)

Mediante este tipo de Bluetooth, podemos relacionar el Monitor Serial con la transferencia de datos por *Bluetooth*

```ino
/*-------------------------------------------------------------------
  Comandos de Bluetooth:

  ESP_SPP_INIT_EVT: Cuando el modo SPP es inicializado
  ESP_SPP_UNINIT_EVT: Cuando el modo SPP es desinicializado
  ESP_SPP_DISCOVERY_COMP_EVT: Cuando se completa el descubrimiento de servicios
  ESP_SPP_OPEN_EVT: Cuando un cliente SPP abre una conexión
  ESP_SPP_CLOSE_EVT: Cuando se cierra una conexión SPP
  ESP_SPP_START_EVT: Cuando se inicializa el servidor SPP
  ESP_SPP_CL_INIT_EVT: Cuando un cliente SPP inicializa una conexión
  ESP_SPP_DATA_IND_EVT: Al recibir datos a través de una conexión SPP
  ESP_SPP_CONG_EVT: Cuando cambia el estado de congestión en una conexión SPP
  ESP_SPP_WRITE_EVT: Al enviar datos a través de SPP.
  ESP_SPP_SRV_OPEN_EVT: Cuando un cliente se conecta al servidor SPP
  ESP_SPP_SRV_STOP_EVT: Cuando el servidor SPP se detiene
  -------------------------------------------------------------------*/

//Incluir librería de bluetooth
#include "BluetoothSerial.h"

//Condición para habilitar el bluetooth
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

//Definir un pin para el LED
#define LED 26

BluetoothSerial BT; //Objeto bluetooth

//Función de llamada haciendo referencia al objeto BT
void callback_function(esp_spp_cb_event_t event, esp_spp_cb_param_t *param) {
  switch (event) {
    case ESP_SPP_START_EVT:
      Serial.println("Inicializado SPP");
      break;
    case ESP_SPP_SRV_OPEN_EVT:
      Serial.println("Cliente conectado");
      break;
    case ESP_SPP_CLOSE_EVT:
      Serial.println("Cliente desconectado");
      break;
    case ESP_SPP_DATA_IND_EVT:
      while (BT.available()) { // Mientras haya datos por recibir
        int incoming = BT.read(); // Lee un byte de los datos recibidos
        if (incoming == 49) { // 1 en ASCII
          digitalWrite(LED, HIGH);
        }
        else if (incoming == 48) { // 0 en ASCII
          digitalWrite(LED, LOW);
        }
      }
      break;
  }
}

void setup() {
  Serial.begin(115200);
  BT.begin("ESP32_Vazzvel_Slave"); // Nombre de tu Dispositivo Bluetooth y en modo esclavo
  Serial.println("El dispositivo Bluetooth está listo para emparejar");
  BT.register_callback(callback_function); //Devolución de la función callback
  pinMode (LED, OUTPUT); // Pin LED como salida
}

void loop() {
  //Imprimo valores aleatorios cada 200 ms
  Serial.println(random(0, 99));
  delay(500);
}
```

#### Bluetooth implementando botones

```ino
//Incluir librería de bluetooth
#include "BluetoothSerial.h"

//Condición para habilitar el bluetooth
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

//Definir pines
#define SW1 15 //Pull Down
#define SW2 4 //Pull Up

BluetoothSerial BT; // Objeto Bluetooth

//Variables para mejorar le funcionamiento del botón
long timeCounter1 = 0;
long timeCounter2 = 0;
int timeThreshold = 250;

void setup() {
  Serial.begin(115200); //Inicializar la comunicación serial
  pinMode(SW1, INPUT); //Pin de entrada
  pinMode(SW2, INPUT); //Pin de entrada

  BT.begin("ESP32_Vazzvel_Slave"); //Nombre de su dispositivo Bluetooth y en modo maestro
  Serial.println("El dispositivo Bluetooth está en modo maestro.\n Conectando con el anfitrión ...");
}

void loop() {
  if (digitalRead(SW1) == 1) {
    On();
  }
  if (digitalRead(SW2) == 0) {
    Off();
  }
}

//Función para enviar "1"
void On() {
  if (millis() > timeCounter1 + timeThreshold) {
    BT.write(49); // Envía 1 en ASCII
    BT.write(13);
    BT.write(10);
    Serial.println("1");
    timeCounter1 = millis();
  }
}

//Función para enviar "0"
void Off() {
  if (millis() > timeCounter2 + timeThreshold) {
    BT.write(48); // Envía 0 en ASCII
    BT.write(13);
    BT.write(10);
    Serial.println("0");
    timeCounter2 = millis();
  }
}
```


---

## WiFi

### ESP32 en modo *station*

#### Ping

El *ESP32* se conecta a internet y manda cierto número de pings a *google.com*

```ino
//Bibliotecas
#include "WiFi.h"
#include <ESP32Ping.h> //https://github.com/marian-craciunescu/ESP32Ping

//Variables
int number = 4;

//Conexión a la red WiFi
void Setup_WiFi(const char* ssid, const char* password) {
  WiFi.mode(WIFI_STA); //Modo de WiFi tipo "station" (dispositivo)
  WiFi.begin(ssid, password); //Credenciales
  Serial.print("Conectado al WiFi... ");
  //Mientras se conecta escribir puntos
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print('.');
    delay(1000);
  }
  Serial.println("\n¡Conexión establecida!");
  Serial.print("Dirección IP: ");
  Serial.println(WiFi.localIP()); //Se obtiene la dirección IP del ESP32
  delay(2000);
}

void setup() {
  Serial.begin(115200);
  Setup_WiFi("Vazzvel", "12345pass");
}

void loop() {
  Serial.print("\nEnviando ");
  Serial.print(number);
  Serial.println(" pings a Google...");
  
  //Envía un número prederteminados de pings
  bool success = Ping.ping("www.google.com", number);
  //Si no hay ping se envía un error
  if (!success) {
    Serial.println("Error, ping no enviado");
    return;
  }
  
  //Tiempo promedio de los pings enviados
  Serial.println("¡Pings enviados!");
  float time_ping = Ping.averageTime();
  Serial.print("Tiempo promedio: ");
  Serial.print(time_ping);
  Serial.println(" ms");
}
```

#### Escaneo de redes

```ino
//Biblioteca WiFi
#include "WiFi.h"

void setup() {
  Serial.begin(115200);

  WiFi.mode(WIFI_STA); //Modo estación
  WiFi.disconnect(); //Desconectar del modo AP (por si estaba conectado)
  delay(100);
  Serial.println("WiFi en modo estación establecido");
}

void loop() {
  Serial.println("Comenzando escaneo de redes WiFi...");
  int n = WiFi.scanNetworks(); //Regresa el número de redes WiFi encontradas
  Serial.print("Escaneo terminado, ");

  if (n == 0) {
    Serial.println("no se encontraron redes WiFi.");
  }
  else {
    Serial.print(n);
    Serial.println(" redes WiFi encontradas:");
    for (int i = 0; i < n; ++i) {
      //Imprimir las redes encontradas
      Serial.print(i + 1);
      Serial.print(": ");
      Serial.print(WiFi.SSID(i)); //Nombre de la red
      Serial.print(" (");
      Serial.print(WiFi.RSSI(i)); //Potencia de la red
      Serial.print(")");
      // Escribir open si la red no tiene credenciales
      Serial.println((WiFi.encryptionType(i) == WIFI_AUTH_OPEN) ? "--open--" : " ");
      delay(10);
    }
  }
  Serial.println("");
  delay(10000);
}
```

### ESP32 en modo *Access Point*

#### Configuración básica del *Access Point*

```ino
//¡¡¡Recuerde que en este ejemplo NO hay conexión a internet!!!

//Biblioteca WiFi
#include "WiFi.h"

const char* ssidESP32 = "WiFi_ESP32_Vazzvel"; //Nombre de red del ESP32
const char* passwordESP32 = "nohayinternet"; //Contraseña de red del ESP32

int channelESP32 = 1; //Número de canal de WiFi 1-13
int ssid_hidden = 0; //Nombre de red oculto, 0 = visible, 1 = oculto
int max_connection = 2; //Número de clientes conectados 1-4


void setup() {
  Serial.begin(115200);

  WiFi.mode(WIFI_AP); //Modo Access Point
  delay(100);
  WiFi.softAP(ssidESP32, passwordESP32, channelESP32, ssid_hidden, max_connection);
  Serial.println("ESP32 en modo Access Point establecido");
}

void loop() {
  Serial.print("Dispositivos conectados: ");
  Serial.println(WiFi.softAPgetStationNum());
  delay(1000);
}
```
#### Imprimiendo la dirección IP y MAC de los dispositivos que se conectan

```ino
//¡¡¡Recuerde que en este ejemplo NO hay conexión a internet!!!

//Biblioteca WiFi
#include "WiFi.h"
#include "esp_wifi.h"

const char* ssidESP32 = "WiFi_ESP32_Vazzvel"; //Nombre de red del ESP32
const char* passwordESP32 = "nohayinternet"; //Contraseña de red del ESP32
int channelESP32 = 1; //Número de canal de WiFi 1-13
int ssid_hidden = 0; //Nombre de red oculto, 0 = visible, 1 = oculto
int max_connection = 2; //Número de clientes conectados 1-4


void setup() {
  Serial.begin(115200);

  WiFi.mode(WIFI_AP); //Modo Access Point
  delay(100);
  WiFi.softAP(ssidESP32, passwordESP32, channelESP32, ssid_hidden, max_connection);
  Serial.println("ESP32 en modo Access Point establecido");
}

void loop() {
  wifi_sta_list_t wifi_sta_list; //Estructura para conocer dispositivos asociados 
  tcpip_adapter_sta_list_t adapter_sta_list; //Estructura para conocer la ip de dispositivos asociados 
 
  //Guarda los valores en un bloque de memoria
  memset(&wifi_sta_list, 0, sizeof(wifi_sta_list));
  memset(&adapter_sta_list, 0, sizeof(adapter_sta_list));

  esp_wifi_ap_get_sta_list(&wifi_sta_list); //Función para obtener la lista de dispositivos asociados
  tcpip_adapter_get_sta_list(&wifi_sta_list, &adapter_sta_list); //Función para obtener la lista de la ip y mac de los dispositivos asociados

  //Imprimir la lista de los dispositivos conectados
  for (int i = 0; i < adapter_sta_list.num; i++) {
    tcpip_adapter_sta_info_t station = adapter_sta_list.sta[i];
    Serial.print("Dispositivo: ");
    Serial.println(i);
    Serial.print("MAC: ");

    //Imprime la dirección mac
    for(int i = 0; i< 6; i++){  
      Serial.printf("%02X", station.mac[i]);  
      if(i<5)Serial.print(":");
    } 
    //Imprime la dirección ip
    Serial.print("\nIP: ");  
    Serial.println(ip4addr_ntoa(&(station.ip)));    
  }
  Serial.println("-----------");
  delay(5000);
}
```
### ESP32 en modo *Dual*

```ino
//¡¡¡Recuerde que en este ejemplo NO hay conexión a internet!!!

//Biblioteca WiFi
#include "WiFi.h"
#include <ESP32Ping.h> //https://github.com/marian-craciunescu/ESP32Ping

const char* ssid_STA = "Clase_IoT";
const char* password_STA = "0123456789";
const char* ssid_AP = "WiFi_ESP32_Vazzvel"; //Nombre de red del ESP32
const char* password_AP = "nohayinternet"; //Contraseña de red del ESP32
//int channelAP = 1; //Número de canal de WiFi 1-13
//int ssid_hidden = 0; //Nombre de red oculto, 0 = visible, 1 = oculto
//int max_connection = 2; //Número de clientes conectados 1-4
int number = 4;


void setup() {
  Serial.begin(115200);
  
  //Modo dual: Access Point y Station
  WiFi.mode(WIFI_AP_STA); 

  //Conexión modo AP
  //WiFi.softAP(ssid_AP, password_AP, channelAP, ssid_hidden, max_connection);
  WiFi.softAP(ssid_AP, password_AP);
  Serial.println("\nESP32 en modo Access Point establecido");
  Serial.print("Dirección IP del modo AP: ");
  Serial.println(WiFi.softAPIP());
  delay(100);

  //Conexión modo STA
  WiFi.begin(ssid_STA, password_STA);
  Serial.print("Conectado al WiFi... ");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print('.');
    delay(1000);
  }
  Serial.println("\n¡Conexión establecida!");
  Serial.print("Dirección IP del modo STA: ");
  Serial.println(WiFi.localIP()); //Se obtiene la dirección IP del ESP32
  delay(1000);
  
}

void loop() {
  Serial.print("\nEnviando ");
  Serial.print(number);
  Serial.print(" pings a Google... ");
  //Envía un número prederteminados de pings
  bool success = Ping.ping("www.google.com", number);
  //Si no hay ping se envía un error
  if (!success) {
    Serial.println("Error, ping no enviado");
    return;
  }
  //Tiempo promedio de los pings enviados
  Serial.println("¡Pings enviados!");
  float time_ping = Ping.averageTime();
  Serial.print("Tiempo promedio: ");
  Serial.print(time_ping);
  Serial.println(" ms");
  delay(1000);

  Serial.print("\nDispositivos conectados: ");
  Serial.println(WiFi.softAPgetStationNum());
  delay(1000);
}
```

# Enlaces

[<- Anterior](Conectividad.md) | [Siguiente ->](Protocolos%20de%20mensajeria%20(HTTP).md)