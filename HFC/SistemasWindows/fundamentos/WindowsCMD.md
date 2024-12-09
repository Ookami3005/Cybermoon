[<- Índice](../SistemasWindows.md)
# Windows Command Line (CMD)

> Aunque potencialmente obsoleto, el ***CMD*** de *Windows* sigue siendo una forma efectiva y simple de interactuar con el sistema operativo, además de que para dispositivos más antiguos podría ser la única forma de interacción.

Es de alguna manera similar a los *Shells* de Linux y los comandos principales son:

##### Básicos

Los comandos básicos para el uso del ***CMD*** son:

- `cd`: Permite moverte de directorio.
- `dir`: Permite listar el contenido de una carpeta.
- `tree`: Otra manera de listar el contenido de carpetas
- `ver`: Muestra la versión del sistema operativo.
- `systeminfo`: Brinda información sobre el dispositivo, el sistema operativo y demás.
- `type`: Muestra el contenido de un archivo en pantalla
- `copy`: Copia un archivo
- `del`: Borra un archivo
- `mkdir`: Crea una carpeta
- `rmdir`: Borra una carpeta
- `more`: Muestra por partes el contenido de un archivo
- `set`: Lista las variables de entorno de la *shell*
- `where`: Busca archivos por su nombre
- `find`: Similar a `grep`, busca patrones de texto en archivos.
- `echo`: Repite el argumento recibido
- `cls`: Limpia la pantalla.

```batch
type user.txt

dir *.py*

REM Busca flag.txt en todo el disco
where /R C:\ flag.txt

set

echo "Hola"
```

##### Red

Los siguientes comandos pueden brindar valiosa información del estado del dispositivo en la red y demás datos útiles.

- `netstat`: Brinda información sobre las conexiones y sesiones de red activas en el dispositivo.
- `ipconfig`: Brinda información y configuraciones de red
- `nslookup`: Resuelve nombres de dominio a direcciones *IP*

##### Procesos

El principal comando que muestra información sobre los procesos en el sistema es `tasklist`. Su uso puede resultar extraño al inicio, pero toda la información necesaria se encuentra con la bandera `/?`.

Si se necesita matar procesos, existe el comando `taskkill /PID [PID]`. Basta con concer el *PID* del proceso para dar el comando anterior y detener su ejecución.

```batch
tasklist

tasklist /FI "imagename eq sshd.exe"

taskkill /PID 4752
```

---

Sobre todo, el comando `help` es capaz de listar la **mayoría** de los comandos disponibles, y si se le indica uno, despliega las opciones disponibles de tal comando.

```batch
help

help find
```

Al iniciar a aprender sobre una nueva *shell*, sin duda es importante la documentación de las opciones disponibles así que es crucial conocer las banderas u opciones de ayuda.

### Banderas

> En este tipo de terminal, las banderas **mayormente** se indican como **/x** donde **x** varía según la opcion que deseemos.

Por ejemplo, para obtener aun más información de `ipconfig` podemos indicar la bandera `/all`.

La bandera para desplegar información de ayuda, suele ser `/?`. De este modo podemos investigar que banderas nos sirven según nuestras necesidades.

Sin embargo, según el comando, puede variar pues algunos reciben banderas en formato *Unix* como `netstat` o de alguna otra manera.

```batch
REM Información extra sobre la configuración de la red
ipconfig /all

REM Listado de archivos ocultos
dir /a

netstat -ab
```

### Redirecciones

> ***CMD*** también soporta las redirecciones típicas de *Unix*, para salida estandar, entrada estándar, salida de error y el tan valioso **pipe**.

No me entretendre mucho en esta sección pues las redirecciones son completamente iguales, sin embargo puedo mencionar que existe la palabra clave `nul` que se comporta como `/dev/null` de los sistemas *Unix*, es decir, es un vacío de información para los datos que no deseamos.

```batch
echo "Hola Mundo" > saludo.txt
echo "Adios" >> saludo.txt

dir /s C:\Windows > nul

more < flag.txt

type C:\Windows 2> nul

type prueba.txt | find "FLAG"
```

### Variables

En este tipo de terminal, podemos interactuar con variables locales o de entorno con el comando `set`. Además, las variables se representan entre simbolos de porcentaje (*%*), por ejemplo `%var%`.

```batch
set saludo="Hola mundo"
echo %saludo%

echo %windir%

echo %PATH%
```

### Batch Scripting

> En ***CMD***, podemos realizar **scripts** para automatizar una serie de comandos de este tipo de *Shell*. Se almacenan como archivos `.bat` o `.cmd`.

Algunos comando utiles para crear ***Scripts*** son:

- `@echo off`: Por defecto, los **Scripts** poseen cierto grado de verbosidad, anunciando los comandos realizados. Este comando deshabilita dicha funcionalidad y solo muestra los resultados.

- `REM comentario`: Los comentarios se realizan con la palabra clave `REM`

- `pause`: Detiene la ejecución hasta que el usuario presione una tecla

- `set /p nombre=Introduce tu nombre:` : Recibe una entrada del usuario, mostrando en pantalla el texto despues del (=).

#### Lógica de control

> Por supuesto, no pueden faltar las estructuras de control para gestionar el flujo del **Script**.

##### Saltos

Podemos indicar saltos a traves del código mediante el comando `goto` y el uso de *"etiquetas"*.

```batch
@echo off
goto :etiqueta
echo Me van a saltar
echo A mi también
:etiqueta
echo Saltamos hasta aca
```

##### Condicionales

No podían faltar las condicionales `if`, son principalmente utilizadas para comparar cadenas, numeros o para verificar la existencia de archivos.

Las comparaciones se realizan con las palabras clave:

- `EQU` o `==`: Igual
- `NEQ`: Distinto
- `LSS`: Menor que
- `LEQ`: Menor o igual que
- `GTR`: Mayor que
- `GEQ`: Mayor o igual que

Mientras que la existencia de archivos se verifica con el palabra clave `exist`. Este solo busca el archivo en el directorio en el que nos ubiquemos a menos que indiquemos la ruta completa.

Ademas podemos negar cualquier condición de un `if` con la palabra clave `not`.

```batch
REM Informa si encuentra una bandera en el directorio actual
if exist flag.txt echo Encontre una bandera en %CD%!

set cadena1=Hola
set cadena2=Adios
if not %cadena1% EQU %cadena2% (
	echo Esto es parte de un bloque de codigo
	echo Esto tambien
	echo Y esto
)
```

Adicionalmente, podemos agregar, opcionalmente, un `else` para indicar que se debe hacer en caso de no cumplirse la condición pero ahora deberemos encerrar los **comandos** entre paréntesis.

```batch
if exist flag.txt echo (Encontre una bandera en %CD%!) else (echo No encontre nada)

set cadena1=Hola
set cadena2=Adios
if %cadena1% EQU %cadena2% (
	echo Esto es parte de un bloque de codigo
	echo Esto tambien
	echo Y esto
) else (
	echo Esto es parte de la clausula else
)
```

##### Bucles

> Tampoco podian faltar los bucles, brindando la capacidad de repetir varias acciones con una sola estructura de control.

Por ejemplo, el ciclo para listar los archivos en el directorio actual serían:

```batch
REM Cuando el for esta en un Script Batch, se le debe poner doble porcentaje a las variables
for %%i in (*.py,*.java,*.hs) do (
	echo %%i es un archivo de programacion!!
)

REM Sin embargo, en la línea de comandos solo se le debe de colocar un porcentaje
for %i in (*) do echo %i
```

# Enlaces

[<- Seguridad](Seguridad.md) | [PowerShell ->](PowerShell.md)