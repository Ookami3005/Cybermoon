[<- Índice](../SistemasWindows.md)
# MSConfig (*Configuración del Sistema*)

> La utilidad de **configuración del sistema**, oficialmente denominada ***MSConfig***, es una herramienta avanzada con el propósito de **rastrear y solucionar problemas en el sistema** ademas de **monitorear y obtener información** sobre este.

Cabe recalcar que para utilizar esta herramienta se necesitan de permisos privilegiados, ya sea una cuenta de **administrador** o **administrador local** (*UAC*).

Esta herramienta posee 5 herramientas principales:

1. **General**: Permite indicar cuantos *drivers* y servicios se cargan al iniciar el dispositivo seleccionando 1 de 3 modos, **Normal**, **Diagnóstico** o **Selectivo**.
2. **Arranque**: Permite definir algunas opciones de arranque del sistema
3. **Servicios**: Lista todos los servicios configurados en el sistema, independientemente de si estan activos o no
4. **Al iniciar (Startup)**: Te redirige al *Gestor de Tareas* pues ahi es donde se configuran los procesos u aplicaciones que se lanzan al arrancar.
5. **Utilidades**: Una lista de herramientas útiles para distintas tareas y los comandos que las ejecutan y que explicare a continuación

## Utilidades

Algunas herramientas destacadas, que se encuentran listadas en la configuración del sistema son:

#### User Account Control Settings
Permite modificar algunos ajustes sobre el *UAC* e incluso deshabilitarlo por completo.

#### Computer Management
Aporta valiosa información y configuración sobre el sistema, categorizada de la siguiente manera:

- **Herramientas del sistema**
	- Gestor de tareas
	- Eventos (*Logs*)
	- Carpetas compartidas
	- Usuarios y Grupos locales
	- Desempeño
	- Gestor de dispositivos
- **Almacenamiento**
	- Respaldo de *Windows Server*
	- Gestor de discos
- **Servicios y Aplicaciones**

#### System Information
Recopila información básica y avanzada de toda la computadora y la presenta de manera entendible al usuario. Por ejemplo, muestra desde detalles del *hardware* hasta variables de entorno u información de las redes.

#### Resource Monitor
Muestra información acerca del uso de **CPU**, **Memoria**, **Disco** y **Red** de cada proceso del sistema. Permite analizar detenidamente cuantos recursos utiliza cada proceso, provee valiosa información sobre estos e incluso permite administrarlos (detenerlos, continuarlos).

También incluye gráficas en cafa categoría para visualizar mejor el uso de recursos sobre el tiempo.

#### Command Prompt
La primera línea de comandos de *Windows*, permite interactuar con el sistema operativo y realizar tareas con este. Es el famosísimo ***cmd***.

#### Registry Editor
Permite modificar el **WIndows Registry**, una base de datos jerárquica utilizada para almacenar información necesaria para la configuración del sistema, aplicaciones y *hardware*. Por supuesto es una herramienta altamente avanzada pues la modificación al **Windows Registry** podría afectar gravemente el comportamiento esperado de la computadora.

# Enlaces

[<- Introducción](IntroduccionWindows.md) | [Seguridad ->](Seguridad.md)