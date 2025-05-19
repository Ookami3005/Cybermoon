# Pentesting Android 101

### Android Studio

> Es el **IDE** oficial para **desarrollo Android** y como auditores de seguridad nos sirve, entre otras cosas, para:

- **Compilar maldad**: Inserción de *payloads* y creación de *apps* maliciosas.
- **Permite realizar pentest sobre aplicaciones**: Pruebas a aplicaciones vulnerables (*Flujos*, *Endpoints*, *API keys*, etc)
- **Reversing**: Permite analizar funcionalidades integradas en las apps y encontrar defectos o debilidades en su implementación.

### Android Virtual Device

> Comunmente llamado **AVD**, es básicamente una máquina virtual o emulador de dispositivos *Android*, sirve para:

- **Ejecutar apps**: Permite instalar nuestros archivos *APK*

Usualmente, los dispositivos virtuales utilizan 10.0.2.1 como **Gateway** y las demas direcciones se asignan a partir de 10.0.2.2.

### RootAVD

> Es un **proyecto multiplataforma** que permite rootear de manera sencilla *AVDs* utilizando determinadas *API* mediante la aplicación maliciosa **Magisk**.

### Frida

> Es un *Framework* utilizado para depuración y *pentest* de aplicaciones móviles. Destaca principalmente por:

- **Automatización**: Cuenta con *scripts* diseñados para facilitar el análisis y explotación
- **Versatilidad**: Permite interceptar y modificar funciones en tiempo real con nuestro código.

### `adb`

> Es una herramienta **CLI** que permite comunicarse con dispositivos *Android*, similar a *SSH*

## Ejercicio 1

- Abrir **Android Studio**
- Abrir *Virtual Device Manager*
- Seleccionar Pixel 4 con *Android Q*
- Iniciar el **AVD** recien creado.
- Descargar [**rootAVD**](https://gitlab.com/newbit/rootAVD)
- Utilizar **rootAVD** para listar los **AVD** presentes en el dispositivo
- Utilizar **rootAVD** para insertar la aplicación **Magisk** en el **AVD**.
- El **AVD** debería reiniciarse automáticamente
- Una vez encendido, entrar a **Magisk** y aceptar las configuraciones faltantes y que se reinicie de nuevo
- El dispositivo ya debería estar configurado, de modo que podemos utilizar el comando `adb`, parte del paquete **android-tools**.

### Burpsuite x Android

- Obtienes un certificado de tu aplicación *Burpsuite*
- `adb push ./cert.cacert /sdcard/Download`
- Ajustes, seguridad, *Encryption & Credentials*, *Install from SD Card*

#### Plugin AlwaysTrustUserCerts

```bash
adb push plugin.zip /sdcard/
adb shell
```

```adb
su
magisk --install-module /sdcard/plugin.zip
```

### SSL Pinning