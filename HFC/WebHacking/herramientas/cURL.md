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