[<- Índice](../WebHacking.md)
# cURL

> *Client URL* (***cURL***) es una utilidad de línea de comandos y biblioteca enfocada en *HTTP* principalmente.

Es una opción muy importante al realizar *scripts* que automaticen interacciones *HTTP* además de ser esencial durante una prueba de penetración sobre servicios *web*.

Podemos enviar una solicitud básica simplemente indicandole a *cURL* la URL deseada como argumento de la siguiente forma:

```bash
curl http://inlanefreight.com/
# o curl inlanefreight.com

#<!DOCTYPE HTML PUBLIC>
#<html><head>
#...
```

### Salida

Por supuesto, *cURL* no renderiza la respuesta obtenida como un servidor web sino que la imprime en formato **crudo**.

Alternativamente, se puede indicar la bandera `-O` para que la salida se almacene en un archivo con el mismo nombre que el archivo remoto o la bandera `-o` seguido del nombre que deseamos que tenga el archivo donde se almacena la salida.

Además, existe la bandera `-s` para silenciar la información de descarga que despliega por defecto.

```bash
curl inlanefreight.com/index.html -o default.html
# Guarda la salida en default.html e imprime información de la descarga

curl inlanefreight.com/index.html -s -O
# Guarda la salida en un archivo local index.html y no imprime nada
```

### Verbosidad

> Por defecto, *cURL* unicamente recupera el cuerpo de la respuesta *HTTP* a la solicitud realizada. Si deseamos visualizar la solicitud realizada por completo y la respuesta completa debemos indicar la bandera de **verbosidad** `-v`.

```bash
curl inlanefreight.com -v

# *   Trying SERVER_IP:80...
# * TCP_NODELAY set
# * Connected to inlanefreight.com (SERVER_IP) port 80 (#0)
# > GET / HTTP/1.1
# > Host: inlanefreight.com
# > User-Agent: curl/7.65.3
# > Accept: */*
# > Connection: close
# > 
# * Mark bundle as not supporting multiuse
# < HTTP/1.1 401 Unauthorized
# < Date: Tue, 21 Jul 2020 05:20:15 GMT
# < Server: Apache/X.Y.ZZ (Ubuntu)
# < WWW-Authenticate: Basic realm="Restricted Content"
# < Content-Length: 464
# < Content-Type: text/html; charset=iso-8859-1
# < 
# <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
# <html><head>
# ...
```

### Encabezados de respuesta

Por otra parte, si únicamente estamos interesados en los encabezados de respuesta, podemos indicar la bandera `-I` para realizar una petición **HEAD** al servidor, recibiendo únicamente los encabezados de respuesta.

De requerirse el cuerpo, la bandera `-i` realizara una petición convencional, pero desplegará los encabezados de respuesta ==junto con el recurso obtenido==.

```bash
curl -I https://www.inlanefreight.com

# Host: www.inlanefreight.com
# User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko)
# Cookie: cookie1=298zf09hf012fh2; cookie2=u32t4o3tb3gg4
# Accept: text/plain
# Referer: https://www.inlanefreight.com/
# Authorization: BASIC cGFzc3dvcmQK
```

```bash
curl -i https://www.inlanefreight.com

# Host: www.inlanefreight.com
# User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko)
# Cookie: cookie1=298zf09hf012fh2; cookie2=u32t4o3tb3gg4
# Accept: text/plain
# Referer: https://www.inlanefreight.com/
# Authorization: BASIC cGFzc3dvcmQK
#
# <!DOCTYPE HTML PUBLIC>
# <html><head>
# ...
```

### Modificación de encabezados

> Además, podemos modificar los encabezados de la solicitud con la bandera `-H` seguida del encabezado en la forma `Encabezado: Valor`, aunque algunos encabezados destacados poseen una bandera propia, como `User-Agent` con la bandera `-A`.

```bash
curl -A ookami http://nmap.org/ --v

# *   Trying [2600:3c01:e000:3e6::6d4e:7061]:80...
# * Connected to nmap.org (2600:3c01:e000:3e6::6d4e:7061) port 80 (#0)
# > GET / HTTP/1.1
# > Host: nmap.org
# > User-Agent: ookami
# > Accept: */*
# > 
# < HTTP/1.1 301 Moved Permanently
# < Date: Mon, 06 Jan 2025 16:20:52 GMT
# < Server: Apache/2.4.6 (CentOS)
# < Location: https://nmap.org/
# < Content-Length: 298
# < Content-Type: text/html; charset=iso-8859-1
# < 
# <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
# <html><head>
# ...
```

### Credenciales Basic

Podemos especificar credenciales de autenticación de *Basic* con la bandera `-u`.

```bash
curl -I http://inlanefreight.com/

# HTTP/1.1 401 Authorization Required
# Date: Mon, 21 Feb 2022 13:11:46 GMT
# Server: Apache/2.4.41 (Ubuntu)
# Cache-Control: no-cache, must-revalidate, max-age=0
# WWW-Authenticate: Basic realm="Access denied"
# Content-Length: 13
# Content-Type: text/html; charset=UTF-8
```

```bash
curl -u admin:admin http://inlanefreight.com/

# <!DOCTYPE html>
# <html lang="en">
# <head>
```
## HTTPS

**cURL** gestiona automáticamente todos los pasos necesarios en una comunicación *HTTPS*. SIn embargo, cabe recalcar que, de encontrarse con un sitio web sin un certificado *TLS/SSL* válido. imposibilitará toda la comunicación con este, por temas de seguridad.

```bash
curl https://inlanefreight.com
# curl: (60) SSL certificate problem: Invalid certificate chain
# More details here: https://curl.haxx.se/docs/sslcerts.html
```

Los navegadores web modernos, por supuesto, realizan un bloqueo similar, pero nos permiten continuar con la conexión aceptando el riesgo.

Para omitir esta verificación del certificado con **cURL**, podemos utilizar la bandera `-k`.

```bash
curl https://inlanefreight.com
# <!DOCTYPE HTML PUBLIC>
# <html><head>
# ...
```

**¿Porqué querriamos omitirla?** Muchas veces, durante una prueba de penetración o simplemente durante la prueba de un sitio web en desarrollo, el sitio no posee un certificado válido aún, de modo que nos vemos en la necesidad de omitir la verificación para proseguir con la interacción.

Esto solo debe realizarse cuando se tiene absoluta confianza en el sitio web objetivo.

# Enlaces