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

##### Phishlets

# Enlaces

[<- Fundamentos del Phishing](RedTeam_PhishingTeoria.md) |