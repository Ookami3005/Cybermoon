[<- Índice](InternetOfThings.md)
#### Código que cifra un texto por *AES*

```ino
//Biblioteca
#include "mbedtls/aes.h" //Seeed_Arduino_mbedtls - Gestor de Librerías Arduino

void setup() {
  Serial.begin(115200);

  //1 char = 1 byte
  char *key = "Ábrete Sésamo!!!"; //Clave de 128 bits (16 bytes)  
  char *input = "rVmiP7fF71A     "; //Texto a cifrar (16 bytes)
  unsigned char chipherText[16]; //Búfer de salida (16 bytes)

  //Estructura para cifrar
  mbedtls_aes_context aes; //Variable de tipo estructura
  mbedtls_aes_init(&aes); //Incicializamos el método
  
  //Establecemos la clave de cifrado (tipo, clave, tamaño)
  mbedtls_aes_setkey_enc(&aes, (const unsigned char*)key, 128);
  
  //Establecemos tipo de cifrado (tipo, operación, entrada, salida)
  mbedtls_aes_crypt_ecb(&aes, MBEDTLS_AES_ENCRYPT, (const unsigned char*)input, chipherText);
  
  mbedtls_aes_free(&aes); //Terminamos el método

  Serial.println("\nTexto cifrado:"); //Imprime el texto cifrado
  for (int i = 0; i < 16; i++) {
    Serial.print(chipherText[i]);
    Serial.print(","); //Se imprimen con comas por facilidad de identificación
  }
  for (int i = 0; i < 16; i++) {
    Serial.print((char)chipherText[i]);
    //Serial.print(","); //Se imprimen con comas por facilidad de identificación
  }
}

void loop() {}
```

#### Código que descrifa un texto por *AES*

```ino
//Biblioteca
#include "mbedtls/aes.h" //Seeed_Arduino_mbedtls - Gestor de Librerías Arduino

void setup() {
  Serial.begin(115200);

  char *key = "Ábrete Sésamo!!!"; //Clave de 128 bits (16 bytes)
  unsigned char chipherText[16] = {86,106,154,43,74,206,188,15,140,222,141,154,14,46,204,179}; //Declaro variable de texto cifrado
  unsigned char outputBuffer[16]; //Búfer de salida (16 bytes)

  //Estructura para cifrar
  mbedtls_aes_context aes; //Variable de tipo estructura
  mbedtls_aes_init(&aes); //Incicializamos el método
  
  //Establecemos la clave de decifrado (tipo, clave, tamaño)
  mbedtls_aes_setkey_dec(&aes, (const unsigned char*) key, 128); 
  
  //Establecemos tipo de decifrado (tipo, operación, entrada, salida)
  mbedtls_aes_crypt_ecb(&aes, MBEDTLS_AES_DECRYPT, (const unsigned char*)chipherText, outputBuffer); 
  
  mbedtls_aes_free(&aes); //Terminamos el método

  Serial.println("\nTexto descifrado:"); //Imprime el texto decifrado
  for (int i = 0; i < 16; i++) {
    Serial.print((char)outputBuffer[i]);
  }
}

void loop() {}
```

# Enlaces

[<- Anterior](Ciberseguridad.md) | [Siguiente ->](Alarmas%20y%20Control.md)