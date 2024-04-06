[<- Índice](../Internet%20of%20Things%20(IoT).md)

## Señales

#### ¿Qué tipo de señales se manejan en el Internet de las Cosas?

Tanto en el Internet de las cosas (IoT) como en la electrónica en general se utilizan 2 tipos de señales: ==Digital== y ==Analógica==

![digitalVsAnalogica.png](imagenes/digitalVsAnalogica.png)

##### Señales analógicas

Señal contínua representada por una frecuencia, longitud de onda, periodo y amplitud.

- Periodo (T)
- Frecuencia (f, $\nu$)
- Longitud de onda ($\lambda$)
- Amplitud (A)

![analogica.png](imagenes/analogica.png)

##### Señales digitales

Señal discreta cuantificada en ==bits==, por tanto, solo puede tener 2 estados posibles.

- 0, 1
- False, True
- LOW, HIGH

***Nota:***
- 0, 3.3 *V* (*ESP32*)
- 0, 5 *V* (*Arduino*)

##### Comparaciones y relaciones

| Analógicas                                                                           | Digitales                                                                                               |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| ***Ventajas:***<br>- Señales de variables físicas<br>- Señales puras (mayor calidad) | ***Ventajas:***<br>- Transmisión de datos eficiente<br>- Inmunes al ruido<br>- Almacenamiento eficiente |
| ***Desventajas:***<br>- Fallas en la transmisión de información<br>- Degradación     | ***Desventajas:***<br>- Más inseguro<br>- Programación                                                  |

##### Conversiones

¿Puedo convertir una señal digital a una analógica y viceversa?

###### ADC (Analogue to Digital Converter)

Convertir señales analógicas a señales digitales

- Bits
*Ejemplo:*
3 bits -> $2^3$ -> 8
16 bits -> $2^{16}$ -> 65536

![ADC.png](imagenes/ADC.png)

###### DAC (Digital to Analogue Converter)

Convertir señales digitales a señales analógicas

![DAC.png](imagenes/DAC.png)

###### PWM (Pulse Width Modulation)

Simular una señal analógica con el ancho de banda

![PWM.png](imagenes/PWM.png)

## Dispositivos

Los dispositivos básicos que arman la primer capa de la arquitecutra IoT son 2:

- Transductores
- Controladores

![capa1ArqIoT.png](imagenes/capa1ArqIoT.png)

#### Transductores

En su forma más básica, un transductor es un dispositivo que convierte un tipo de energía en otra. Existen 2 tipos de transuctores:

##### Sensores

Es un transductor que detecta cambios en su entorno generando fluctuaciones proporcionales a estos de la energía electrica inducida al dispositivo.

![sensores.png](imagenes/sensores.png)

##### Actuadores

Al contrario de los sensores, estos son transuctores que en función de una señal electrica de entrada, generan una acción de alguna forma física de salida.

![actuadores.png](imagenes/actuadores.png)

#### Controladores

Dispositivos que procesan las señales de entrada y ejecutan acciones en función de estas.

##### System on Chip (SoC)

Todos los componentes de un sistema electrónico (o de cómputo) está integrado en un chip.

![SoC.png](imagenes/SoC.png)

##### Embedded system

Es un sistema de computación diseñado para realizar funciones específicas, y cuyos componentes se encuentran integrados en una placa base.

![embeddedS.png](imagenes/embeddedS.png)

##### ¿Qué microcontroladores hay en IoT?

- Bits
- RAM
- Flash
- GPIO
- Interfaz de comunicación
- Consumo energético
- Costo
- Documentación

###### Node MCU ESP32 Dev Kit V1

![ESP32 Dev Kit.png](imagenes/ESP32%20Dev%20Kit.png)

###### Diferentes módulos

![diferentesModulos.png](imagenes/diferentesModulos.png)

# Enlaces
[<- Anterior](Introduccion%20al%20Internet%20de%20las%20Cosas.md) | [Siguiente ->](Codigos%20de%20se%C3%B1ales%20digitales%20y%20analogicas.md)