[<- Índice](Internet%20of%20Things%20(IoT).md)

## Salidas digitales

#### Alternando 2 LEDs

```ino
//Defino pines
#define LED1 14
#define LED2 27

void setup() {
  //Declaro los pines como salida
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
}

void loop() {
  //Enciendo LED1 y apago LED2 durante 200 ms
  digitalWrite(LED1, HIGH);
  digitalWrite(LED2, LOW);
  delay(200);

  //Apago LED1 y enciendo LED2 durante 200 ms
  digitalWrite(LED1, LOW);
  digitalWrite(LED2, HIGH);
  delay(200);
}
```

#### Encendiendo los 5 LEDs

```ino
//Defino los leds de uso en un arreglo
const int leds[5] = {14, 27, 26, 25, 33};

void setup() {
  //Defino mis pines como salida
  for (int i = 4; i >= 0 ; i--) {
    pinMode(leds[i], OUTPUT);
  }
}

void loop() {
  //Ciclo for para encender 5 leds y esperar 200 ms
  for (int i = 0; i < 5; i++) {
    digitalWrite(leds[i], HIGH);
  }
  delay(200);

  //Ciclo for para apagar 5 leds y esperar 200 ms
  for (int i = 0; i < 5; i++) {
    digitalWrite(leds[i], LOW);
  }
  delay(200);
}
```

---

## Entradas digitales

#### Leyendo la señal digital de los botones SW

```ino
#define PinBotton_PullUp 4
#define PinBotton_PullDown 15

void setup() {
  pinMode(PinBotton_PullUp, INPUT);
  pinMode(PinBotton_PullDown, INPUT);
}

void loop() {
  digitalRead(PinBotton_PullUp);
  digitalRead(PinBotton_PullDown);
}
```

#### Prendiendo LEDs cuando se presiona un boton SW

```ino
#define PinBotton_PullUp 4
#define PinBotton_PullDown 15

#define PinLed_1 14
#define PinLed_2 27

void setup() {
  pinMode(PinLed_1, OUTPUT);
  pinMode(PinLed_2, OUTPUT);
  pinMode(PinBotton_PullUp, INPUT);
  pinMode(PinBotton_PullDown, INPUT);
}

void loop() {
  int x = digitalRead(PinBotton_PullUp);
  int y = digitalRead(PinBotton_PullDown);
  digitalWrite(PinLed_1, x);
  digitalWrite(PinLed_2, y);
}
```

#### Imprimiendo por el monitor serial la lectura del boton SW

```ino
#define PinBotton_PullUp 4
#define PinBotton_PullDown 15

void setup() {
  Serial.begin(115200);
  pinMode(PinBotton_PullUp, INPUT);
  pinMode(PinBotton_PullDown, INPUT);
}

void loop() {
  int in_up = digitalRead(PinBotton_PullUp);
  int in_down = digitalRead(PinBotton_PullDown);
  Serial.print(in_up);
  Serial.print(", ");
  Serial.println(in_down);
}
```

#### Contador implementando un boton SW

Para éste código se implemento un sistema Threshold para el correcto funcionamiento del boton, pues corrige la sensibilidad y apoya al correcto detectamiento del pulso del boton
```ino
#define PinBotton_PullUp 4

int count = 0;
long timeCounter = 0;
const int timeThreshold = 250;

void setup() {
  Serial.begin(115200);
  pinMode(PinBotton_PullUp, INPUT);
}

void loop() {
  int in_up = digitalRead(PinBotton_PullUp);
  if (in_up == LOW) {
    if (millis() > timeCounter + timeThreshold) {
      count++;
      Serial.println(count);
      timeCounter = millis();
    }
  }
}
```

---

## Entradas analógicas (ADC)

#### Leyendo la lectura de la fotoresistencia

```ino
#define PinADC 34 //Defino pin (LDR)

void setup() {
  Serial.begin(115200); //Inicializo comunicación serial
  pinMode(PinADC, INPUT); //Declaro al pin como entrada
}

void loop() {
  int sensor = analogRead(PinADC); //Guardo en una variable la lectura del pin
  Serial.println(sensor); //Imprimo el valor de la variable "sensor"
}
```

#### Imprimiendo la conversión a voltaje de la lectura de la fotoresistencia

```ino
#define PinADC 34 //Defino pin

void setup() {
  Serial.begin(115200); //Inicializo comunicación serial
  pinMode(PinADC, INPUT); //Declaro al pin como entrada
}

void loop() {
  int sensor = analogRead(PinADC); //Guardo en una variable la lectura del pin
  float voltage = 3.3/4096 * analogRead(PinADC);
  Serial.print(voltage); //Imprimo el valor de la variable "sensor"
  Serial.println(" V"); //Imprimo las unidades
  delay(20);
  }
  ```

#### Obteniendo el promedio de lecturas de la fotorresistencia

```ino
#define PinADC 34 //Defino pin

void setup() {
  Serial.begin(115200);     //Inicializo comunicación serial
  pinMode(PinADC, INPUT);   //Declaro al pin como entrada
}

void loop() {
  int sensor = analogRead(PinADC);  //Guardo en una variable la lectura del pin
  int prom = media(100, PinADC);     //Guardo en una variable el resultado de la función media
  Serial.print("Prom:");
  Serial.print(prom);               //Imprimo el valor de la variable "prom"
  Serial.print(",");
  Serial.print("Sensor:");
  Serial.println(sensor);           //Imprimo el valor de la variable "sensor"
}

//Función media(número de datos, pin de entrada)
int media(int data_number, int pin) {
  int value = 0;
  for (int i = 0; i < data_number; i++) {
    value = analogRead(pin) + value;
  }
  value /= data_number;
  return value;
}
```

---

## Salidas analógicas (DAC)

#### Emitiendo una señal analógica por medio del DAC

```ino
#define PinDAC 26

void setup() {
  pinMode(PinDAC, OUTPUT);
}

void loop() {
  for(int i = 0; i < 256; i++){
    dacWrite(PinDAC, i);
    delay(10);
  }
  for(int i = 255; i > -1; i--){
    dacWrite(PinDAC, i);
    delay(10);
  }
}
```

#### Emitiendo señal con el DAC y leyendola con el ADC para imprimiral por el Serial

```ino
//Se debe conectar un cable entre el PinADC y el PinDAC.
//Definir pines
#define PinDAC 26
#define PinADC 12

void setup() {
  Serial.begin(115200); //Inicializar comunicación serial
  pinMode(PinDAC, OUTPUT); //Declaro pin como salida
  pinMode(PinADC, INPUT); //Declaro pin como entrada
}

void loop() {
  //Escribo un contínuo incremental de valore y los leo con el ADC
  for(int i = 0; i <= 255; i++){
    dacWrite(PinDAC, i);
    float val = analogRead(PinADC);
    Serial.println(val);
    delayMicroseconds(5000);
  }
  //Escribo un contínuo decremental de valore y los leo con el ADC
  for(int i = 255; i >= 0; i--){
    dacWrite(PinDAC, i);
    float val = analogRead(PinADC);
    Serial.println(val);
    delayMicroseconds(5000);
  }
}
```

#### Emitiendo señal analógica conforme la función Seno e imprimiendola por el Serial

```ino
//Se debe conectar un cable entre el PinADC y el PinDAC.
//Definir pines
#define PinDAC 26
#define PinADC 12

void setup() {
  Serial.begin(115200); //Inicializar comunicación serial
  pinMode(PinDAC, OUTPUT); //Declaro pin como salida
  pinMode(PinADC, INPUT); //Declaro pin como entrada
}

void loop() {
  //Escribo un contínuo incremental de valore y los leo con el ADC
  for (int i = 0; i < 360; i++) {
    float seno = 126 * sin(i * M_PI / 180) + 127; //Estas conversiones se realizaron para ajustar los valores entre 0 y 255, el rango del DAC
    dacWrite(PinDAC, seno);
    Serial.println(analogRead(PinADC));
    delay(5);
  }
}
```

---

## Salidas analógicas (PWM)

#### Emitiendo una señal analógica mediante PWM

Es importante recalcar, que el PWM simula una señal analógica manipulando el ancho de onda de una señal digital

```ino
//Definir pines
#define PinPWM 26

//Variables para definir el PWM
const int freq = 1000; //Frecuencia en Hz
const int ChanelPWM = 0; //Canal 0 - 15
const int resolution = 8; //Bits de resolución, hasta 8

void setup() {
  pinMode(PinPWM, OUTPUT); //Declarar pin como salida
  ledcSetup(ChanelPWM, freq, resolution); //Configuración del PWM
  ledcAttachPin(PinPWM, ChanelPWM); //Enlazar pin a canal
}

void loop() {
  //Escribo un contínuo incremental
  for(int i = 0; i <= 255; i++){
    ledcWrite(ChanelPWM, i);
    delayMicroseconds(10000);
  }
  //Escribo un contínuo decremental
  for(int i = 255; i >= 0; i--){
    ledcWrite(ChanelPWM, i);
    delayMicroseconds(10000);
  }
}
```

#### Emitiendo señal analógica mediante PWM y leyendola con el ADC para imprimirla por Serial

```ino
//Conectar PinPWM a PinADC

#define PinPWM 26
#define PinADC 13

//Variables para definir el PWM
const int freq = 5000; //Frecuencia en Hz
const int ChanelPWM = 0; //Canal 0 - 15
const int resolution = 8; //Bits de resolución, hasta 8

void setup() {
  Serial.begin(115200); //Inicializar comunicación serial
  pinMode(PinADC, INPUT); //Declarar pin como entrada
  pinMode(PinPWM, OUTPUT); //Declarar pin como salida
  ledcSetup(ChanelPWM, freq, resolution); //Configuración del PWM
  ledcAttachPin(PinPWM, ChanelPWM); //Enlazar pin a canal
}

void loop() {
  //Escribo un contínuo incremental de valore y los leo con el ADC
  for(int i = 0; i <= 255; i++){
    ledcWrite(ChanelPWM, i);
    float v = analogRead(PinADC)*(3.3/4096.0);  
    Serial.println(v);
    delayMicroseconds(10000);
  }
  //Escribo un contínuo decremental de valore y los leo con el ADC
  for(int i = 255; i >= 0; i--){
    ledcWrite(ChanelPWM, i);
    float v = analogRead(PinADC)*(3.3/4096.0);  
    Serial.println(v);
    delayMicroseconds(10000);
  }
}
```

#### Emitiendo 2 señales PWM y leyendo ambos con 2 ADC distintos

```ino
//Conectar PinPWM1 a PinADC1 y PinPWM2 a PinADC2

#define PinPWM1 26
#define PinPWM2 25
#define PinADC1 13
#define PinADC2 12

//Variables para definir el PWM
const int freq1 = 2000; //Frecuencia en Hz
const int ChanelPWM1 = 0; //Canal 0
const int resolution1 = 8; //Bits de resolución, hasta 8

const int freq2 = 500; //Frecuencia en Hz
const int ChanelPWM2 = 1; //Canal 1
const int resolution2 = 8; //Bits de resolución, hasta 8

void setup() {
  Serial.begin(115200);
  pinMode(PinPWM1, OUTPUT);
  pinMode(PinPWM2, OUTPUT);
  pinMode(PinADC1, INPUT);
  pinMode(PinADC2, INPUT);

  ledcSetup(ChanelPWM1, freq1, resolution1);
  ledcSetup(ChanelPWM2, freq2, resolution2);
  ledcAttachPin(PinPWM1, ChanelPWM1);
  ledcAttachPin(PinPWM2, ChanelPWM2);
}

void loop() {
  for(int i = 0; i <= 255; i++){
    ledcWrite(ChanelPWM1, i);
    ledcWrite(ChanelPWM2, i);
    float v1 = analogRead(PinADC1)*(3.3/4096.0);
    float v2 = analogRead(PinADC2)*(3.3/4096.0);   
    delayMicroseconds(10000);
    
    Serial.print("S1:");
    Serial.print(v1);
    Serial.print(",");
    Serial.print("S2:");
    Serial.println(v2);    
  }
}
```

#### Comparación de señales PWM vs DAC

```ino
//Conectar PinPWM a PinADC y PinADCPWM a PinADCDAC

#define PinPWM 26
#define PinDAC 25
#define PinADCPWM 13
#define PinADCDAC 12

//Variables para definir el PWM
const int freq = 1000; //Frecuencia en Hz
const int ChanelPWM = 0; //Canal 0 - 15
const int resolution = 8; //Bits de resolución, hasta 8
const int timedelay = 10000; //delay microseconds
bool state = true; //Variable de estado

void setup() {
  Serial.begin(115200);
  pinMode(PinPWM, OUTPUT);
  pinMode(PinDAC, OUTPUT);
  pinMode(PinADCPWM, INPUT);
  pinMode(PinADCDAC, INPUT);
  ledcSetup(ChanelPWM, freq, resolution);
  ledcAttachPin(PinPWM, ChanelPWM);
}

void loop() {
  int val = state ? 255 : 0;
  for (int i = 0; i < 256; i++) {
    dacWrite(PinDAC, abs(val - i));
    ledcWrite(ChanelPWM, abs(val - i));
    Serial.print("PWM:");
    Serial.print(analogRead(PinADCPWM));
    Serial.print(",");
    Serial.print("DAC:");
    Serial.println(analogRead(PinADCDAC));
    delayMicroseconds(timedelay);
  }
  state = !state;
}
```

---

# Enlaces

[<- Anterior](Dispositivos%20del%20Internet%20de%20las%20Cosas.md) | [Siguiente ->](Conectividad.md)