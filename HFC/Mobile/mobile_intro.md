# Pentesting Android 101

## Conceptos clave

#### Android Studio

> Es el **IDE** oficial para **desarrollo Android** y como auditores de seguridad nos sirve, entre otras cosas, para:

- **Compilar maldad**: Inserción de *payloads* y creación de *apps* maliciosas.
- **Permite realizar pentest sobre aplicaciones**: Pruebas a aplicaciones vulnerables (*Flujos*, *Endpoints*, *API keys*, etc)
- **Reversing**: Permite analizar funcionalidades integradas en las apps y encontrar defectos o debilidades en su implementación.

#### Android Virtual Device

> Comunmente llamado **AVD**, es básicamente una máquina virtual o emulador de dispositivos *Android*, sirve para:

- **Ejecutar apps**: Permite instalar nuestros archivos *APK*
- **Depurar aplicaciones**

Usualmente, los dispositivos virtuales utilizan 10.0.2.1 como **Gateway** y las demas direcciones se asignan a partir de 10.0.2.3, pues la dirección 10.0.2.2 identifica a nuestra máquina *host*.

#### rootAVD

> Es un **proyecto multiplataforma** que permite rootear de manera sencilla *AVDs* utilizando determinadas *API* mediante la aplicación maliciosa **Magisk**.

#### Frida

> Es un *Framework* utilizado para depuración y *pentest* de aplicaciones móviles. Destaca principalmente por:

- **Automatización**: Cuenta con *scripts* diseñados para facilitar el análisis y explotación
- **Versatilidad**: Permite interceptar y modificar funciones en tiempo real con nuestro código.

#### `adb`

> Es una herramienta **CLI** que permite comunicarse con dispositivos *Android*, similar a *SSH*

## Rootear android

1. Abrir **Android Studio**
2. Abrir *Virtual Device Manager*
3. Para este ejercicio se seleccionó Pixel 4 con *Android Q* como **AVD**
4. Iniciar el **AVD** recien creado.
5. Descargar [**rootAVD**](https://gitlab.com/newbit/rootAVD)
6. Utilizar **rootAVD** para listar los **AVD** presentes en el dispositivo:

```bash
rootAVD ListAllAVDs

# rootAVD A Script to root AVD by NewBit XDA
#
# Usage:	rootAVD [DIR/ramdisk.img] [OPTIONS] | [EXTRA ARGUMENTS]
# or:	rootAVD [ARGUMENTS]
#
# ...
#
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img FAKEBOOTIMG
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img DEBUG PATCHFSTAB GetUSBHPmodZ
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img restore
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img InstallKernelModules
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img InstallPrebuiltKernelModules
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img InstallPrebuiltKernelModules GetUSBHPmodZ PATCHFSTAB DEBUG
# ./rootAVD.sh system-images/android-36/google_apis_playstore/x86_64/ramdisk.img AddRCscripts
```

Esta herramienta despliega por defecto el menu de ayuda indicando todas las opciones que posee y hasta el final despliega los archivos pertinentes de **AVD**s reconocidos en el equipo.

7. Utilizar **rootAVD** para insertar la aplicación **Magisk** en el **AVD**. Esto se realiza ejecutando uno de los primeros comandos ejemplo con los archivos que reconoció antes, siempre que la API sea compatible con **rootAVD**, por ejemplo:

```bash
./rootAVD.sh system-images/android-29/google_apis_playstore/x86_64/ramdisk.img

# ...
# ...
# ...
# [*] repacking back to ramdisk.img format
# [!] Rename Magisk.zip to Magisk.apk
# [*] Pull ramdiskpatched4AVD.img into ramdisk.img
# [-] 
# [*] Pull Magisk.apk into 
# [-] 
# [*] Pull Magisk.zip into .
# [-] 
# [-] Clean up the ADB working space
# [-] Install all APKs placed in the Apps folder
# [*] Trying to install Apps/Magisk.apk
# [*] Performing Streamed Install
# [*] Success
# [-] Shut-Down & Reboot (Cold Boot Now) the AVD and see if it worked
# [-] Root and Su with Magisk for Android Studio AVDs
# [-] Trying to shut down the AVD
# [!] If the AVD doesn't shut down, try it manually!
# [-] Modded by NewBit XDA - Jan. 2021
# [!] Huge Credits and big Thanks to topjohnwu, shakalaca, vvb2060 and HuskyDG
```

El **AVD** debería **apagarse** automáticamente.

8. Iniciar el **AVD** nuevamente y una vez encendido, al entrar a la nueva app **Magisk** dentro del **AVD** se nos solicitará reiniciar el dispositivo para concluir las configuraciones faltantes.

Tras este último reinicio, el dispositivo ya debería estar correctamente configurado, de modo que podemos utilizar el comando `adb`, parte del paquete **android-tools**, para establecer una conexión.

```bash
adb shell
```

9. En esta nueva interfaz de comandos, utilizamos la instrucción `su` para elevar nuestros privilegios a **root**, pero deberemos aceptar en el **AVD** la solicitud de superusuario que se genere.

![superuser_request.png](imagenes/superuser_request.png)

FInalmente, hemos **rooteado** nuestro **AVD**.

![rooted_android.png](imagenes/rooted_android.png)

## Burpsuite x Android

Lo primero es obtener un certificado de tu aplicación *Burpsuite*, en **Ajustes** -> **Proxy** -> **Importar/Exportar certificado CA**.

Despues subimos este certificado al dispositivo con apoyo de `adb`:

```bash
adb push ./burp.cert /sdcard/Download/.

# Downloads/burp.cer: 1 file pushed, 0 skipped. 15.3 MB/s (939 bytes in 0.000s)
```

Ahora, en el dispositivo debemos instalar este certificado en la app **Configuración** -> **Seguridad** -> **Avanzado** -> **Encryption & Credentials** -> **Install from SD Card** y seleccionamos nuestro certificado.

#### Plugin AlwaysTrustUserCerts

Por defecto, el certificado que acabamos de cargar se considera como un certificado de usuario y se le tiene menor confianza.
Por esto siempre es buena idea disponer del módulo [**Always Trust User Certs**](https://github.com/Benojir/Always-Trust-User-Certs-Magisk) para **Magisk**, que configura el dispositivo para brindar confianza completa a estos certificados.

Para esto, debemos descargar el archivo *AlwaysTrustUserCerts.zip* de la seccion *Releases* del repositorio del plugin.

Después cargamos el plugin al dispositivo con `adb` y desplegar una línea de comandos en el dispositivo.

```bash
adb push plugin.zip /sdcard/
adb shell
```

Dentro de esta *Shell*, ascendemos a **root** e instalamos el plugin con apoyo de la utilidad `magisk`:

```sh
su
magisk --install-module /sdcard/plugin.zip

# - Device is system-as-root
# ***************************************
#  Always Trust User Certificates (2024) 
#  by Benojir Sultana 
# ***************************************
# *******************
#  Powered by Magisk 
# *******************
# - Extracting module files
# - Done
```

---

Ahora, que el certificado esta configurado correctamente, debemos configurar el *proxy HTTP* del dispositivo con apoyo de `adb` de la siguiente forma:

```bash
`adb shell settings put global http_proxy 10.0.2.2:8080`
```

Podemos verificar que el *proxy* haya sido configurado correctamente con el siguiente comando, donde deberiamos ver la IP y puerto que acabamos de indicar:

```bash
adb shell settings get global http_proxy
```

Finalmente, nuestra aplicación **Burpsuite** ya debería poder interceptar el tráfico *HTTP* del dispositivo móvil sin ningún problema.

![burpxandroid.png](imagenes/burpxandroid.png)

### SSL Pinning