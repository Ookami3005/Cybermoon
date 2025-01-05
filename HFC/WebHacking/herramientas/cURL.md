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

Por supuesto, *cURL* no renderiza la respuesta obtenida como un servidor web sino que la imprime en formato **crudo**.

Alternativamente, se puede indicar la bandera `-O` para que la salida se almacene en un archivo con el mismo nombre que el archivo remoto o la bandera `-o` seguido del nombre que deseamos que tenga el archivo donde se almacena la salida.

Además, existe la bandera `-s` para silenciar la información de descarga que despliega por defecto.

```bash
curl inlanefreight.com/index.html -o default.html
# Guarda la salida en default.html e imprime información de la descarga

curl inlanefreight.com/index.html -s -O
# Guarda la salida en un archivo local index.html y no imprime nada
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