[<- Índice](Internet%20of%20Things%20(IoT).md)

## Arquitectura

### ¿Cuáles capas de la arquitectura veremos?

![redes.png](redes.png)

#### Gateway

Gateway IoT es un dispositivo físico o un programa de *software* que sirve como punto de conexión entre la internet y los dispositivos inteligentes.

![gateway.png](gateway.png)

##### Edge Computing

Los datos producidos por los dispositivos IoT se procesen más cerca de donde se crearon en lugar de enviarlos a la nube directamente.

- Velocidad
- Seguridad
- Escalabilidad
- Costos bajos

![edgeComputing.png](edgeComputing.png)

##### Gateway ESP32

![gatewayEsp32.png](gatewayEsp32.png)

#### Redes

¿Cómo interconectaré mis dispositivos IoT?

- Tipos de Conexión
- Topología física
- Accesibilidad a la red

![topologiaDeRed.png](topologiaDeRed.png)

![tipoDered.png](tipoDered.png)

## Protocolos de Conectividad

¿Cómo enlazaré mis dispositivos?

- ZigBee
- NFC
- RFID
- LoRa
- Z-Wave
- **WiFI**
- **Bluetooth**

#### Bluetooth

Bluetooth está definido por el estandar *IEEE 802.15.1* y es una tecnlogía de **comunicación inalámbrica de corto alcance** que funciona a baja potencia para permitir la comunicación entre 2 o más dispositivos.

![arquitecturaBluetooth.png](arquitecturaBluetooth.png)

###### Comparativa

![comparativaBluetooth.png](comparativaBluetooth.png)

###### Perfiles

![perfilesBluetooth.png](perfilesBluetooth.png)

###### ESP32

Para el *Bluetooth* en el ESP32 lo manejaremos de acuerdo a la arquitectura *Bluetooth* como ==Maestro==, ==esclavo== y ==perfil serie==

#### WiFi

Wi-Fi o WiFi se conoce técnicamente por su estándar, IEEE 802.11, y es una tecnología inalámbrica para redes de área local inalámbrica de nodos y dispositivos construidos sobre estandares similares.

##### Comparativa

![comparativaWiFi.png](comparativaWiFi.png)

##### ESP32

Para el WiFi en el ESP32, manejaremos principalmente 3 modos:

![IoT/imagenes/wifiEsp32.png](IoT/imagenes/wifiEsp32.png)


###### Station Mode

![stationMode.png](stationMode.png)

###### Access Point Mode

![accessPointMode.png](accessPointMode.png)

###### Dual Mode

![dualMode.png](dualMode.png)

# Enlaces

[<- Anterior](Codigos%20de%20se%C3%B1ales%20digitales%20y%20analogicas.md) | [Siguiente ->](Codigos%20de%20Conectividad.md)