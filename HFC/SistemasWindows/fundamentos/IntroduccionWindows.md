[<- Índice](SistemasWindows.md)
# Introducción a Windows

> Actualmente, *Windows* es una extensa familia de sistemas operativos que han destacado por su dominio sobre el sector corporativo y personal.

El hecho de que sea tan popular, también lo ha llevado a ser el principal objetivo de *Hackers* y *Malware*.

Se ha destacado por ediciones como:

- *Windows XP*
- *Windows 7*
- *Windows Server 2019*
- *Windows 10*
- *Windows Server 2022*
- *Windows 11*

Mientras que las ediciones *"Server"* son principalmente diseñadas para el ámbito corporativo como parte de un *Active Directory*, el resto de ediciones están pensadas para un uso personal.

### Sistema de Archivos

> El **sistema de archivos** utilizado actualmente por *Windows* es ***NTFS*** (*New Technology File System*) aunque antes utilizaban ***FAT16/FAT32*** y ***HPFS*** (*High Performance File System*).

#### NTFS

***NTFS*** destaca por su capacidad de manejar discos más grandes de manera eficiente además de considerarse más **seguro**, **tolerante a fallos** y mejorar muchas limitaciones de los sistemas de archivos anteriores.

Es así porque tiene funcionalidades como **compresión de archivos**, **cifrado**, **controles de acceso**, pero sobre todo porque posee la capacidad de **reparar automáticamente** archivos o carpetas en caso de alguna falla, utilizando la información en los propios *logs* del sistema.

En volúmnes **NTFS**, podemos administrar los siguientes tipos de permisos sobre los archivos y carpetas del sistema.

- **Control total**: Pemite leer, escribir, mover y borrar contenido en un archivo y/o sobre los archivos de una carpeta.

- **Modificación**: Permite leer, escribir y borrar en contenido en archivos y/o en archivos de una carpeta.

- **Lectura y Ejecución**: Permite leer y ejecutar una rchivo y/o los archivos de una carpeta.

- **Listar el contenido de la carpeta**: Permite ver el contenido de una carpeta, no afecta a los archivos

- **Lectura**: Permite leer el contenido de un archivo y/o carpeta

- **Escritura**: Permite modificar un archivo y/o agregar archivos a una carpeta

En el caso de las carpetas, los permisos se asignan de manera recursiva sobre todo el contenido de la misma.

#### FAT

Por otra parte, aunque las computadoras anteriores si lo utilizan, **FAT** al dia de hoy ha sido relegado a particiones de memorias extraibles *USB*, Micro SD, etc.

## Carpeta Windows

> Una de las carpetas fundamentales de *Windows* es la carpeta `C:\Windows` donde tradicionalmente esta contenido todo el **sistema operativo**. 

Esta carpeta se encuentra en el disco principal (usualmente `C:`) pero puede estar en otro disco si ese es el que se designo como principal e incluso en otra carpeta.

La ***variable de entorno*** asignada a la carpeta del sistema operativo es `%windir%` de modo que aunque se encuentre en otra ruta, esta variable siempre apuntara a la ubicación correcta.

Por supuesto, dentro de la carpeta `C:\Windows`, existen muchisimas más carpetas con todos los datos importantes para el sistema, pero el más importante es sin duda la carpeta `C:\Windows\System32`.

La carpeta ***System32*** contiene todos los datos ==vitales y críticos== para el sistema operativo, de modo que hay que tener mucho cuidado al interactuar con esta carpeta pues puede dejar inoperacional el sistema.

## Usuarios

Las ***cuentas de usuario*** suelen categorizarse como cuentas de **Administrador** o como cuentas de **Usuario estándar**.

Esto ayuda a determinar y gestionar los permisos de un usuario específico.

Por ejemplo, un **Administrador** debe poder realizar cambios críticos al sistema como **creación y borrado de usuarios**, **modificar ajustes globales**, **instalar software**, etc.

Por otra parte un **Usuario Estándar** solo debe poder realizar cambios dentro de las carpetas y archivos atribuidos a este y no a todo el sistema.

Los **perfiles** de los usuarios se almacenan dentro de la carpeta `C:/Users` y son completamente creados hasta que dicho usuario inicie sesión.

#### Grupos

> Un **grupo** es una estructura de *Windows* en la que podemos designar permisos y características a un conjunto de usuarios.

Es muy útil porque evita que tengamos que configurar el mismo permiso manualmente en una gran cantidad de usuarios.
Solo tenemos que designar los permisos deseados en el grupo y agregar a los usuarios requeridos en este.

#### Local User And Group Management

> Una herramienta muy útil para visualizar información sobre **usuarios**y **grupos**, además de administrarlos, es utilizando la herramienta *Local User And Group Management*.

Podemos invocarla desde la barra de busqueda o invocando el ***Run Dialog Box*** que nos pemite llamar programas de manera más rápida.

Solo hace falta presionar la combinación de teclas `Win+R` y en la caja de texto tecleamos `lusrmgr.msc` y damos *Enter*.

### UAC (User Account Control)

> Muchas veces, se necesita que un usuario ejecute ciertas tareas que requieren permisos privilegiados.

Estar todo el tiempo en una sesión de administrador ciertamente es peligroso para el sistema, por lo que no es viable.

Entonces, junto con *Windows Vista*, se introdujo el concepto de **UAC**.
Este consiste en mantener la cuenta del usuario con permisos estándar hasta que este realice una acción que requiera permisos privilegiados.
Entonces, se preguntara en pantalla una confirmación para permitir que esta operación se realice, donde una vez confirmada se elevarán los privilegios **momentáneamente** durante la operación.

Esto por supuesto reduce el impacto de acciones malintencionadas, por ejemplo, detonadas por un *malware* u otra ejecución maliciosa.
Además de permitir permisos más flexibles en el sistema, sin la necesidad de abrir tantas cuentas de administrador.

### Ajustes y Panel de Control

> Durante mucho tiempo, el ***Panel de Control*** fue la utilidad principal para realizar ajustes sobre el sistema, sin embargo, hoy en día ***Ajustes*** ha empezado a remplazarlo para la gestión de ajustes básicos y comunes.

Aún así, cuando requerimos ajustes más sofisticados del dispositivo es altamente probable que nos rediriga al aún vivo ***Panel de Control***, por lo que si no tienes claro en cuál de las 2 buscar, siempre puedes introducir el ajuste buscado en la barra de busqueda y escoger la opción adecuada.

### Gestor de tareas

> El ***gestor de tareas*** es la utilidad que provee información sobre las aplicaciones y procesos activos en el sistema, ademas de detalles de ***desempeño*** como cuanta *CPU* y *RAM* utilizan o los usuarios propietarios de dichos procesos.

---

Con todo esto, estamos listos para adentrarnos más en *Windows* en las siguientes notas. Cabe recalcar que *Windows* puede llegar a ser un sistema mucho más complejo e incluso odioso que *Linux* en algunos aspectos, pues prioriza la ***retrocompatibilidad*** como una propiedad principal.
Esto causa una gran amalgama de programas viejos y nuevos conviviendo en una extraña simbiosis con el fin de que todo funcione como debería (prueba de ello es el zombificado *cmd*).

Sin embargo, esto mismo muchas veces también atribuye a formar un sistema amigable con usuarios nuevos o poco experimentados, característica que lo ha llevado a la posición en la que se encuentra, dominando el mercado de las computadoras personales.

%%Que no se note que no me gusta WIndows%%

# Enlaces

[MSConfig ->](MSConfig.md)