[<- Índice de Módulos](../HackingFightClub.md)
# Introducción a Sistemas Unix

![bonfire2.jpg](../../imagenes/bonfire2.jpg)

## Over The Wire

[Contraseñas](tareas/overthewire.md)

## Notas

- [Historia de UNIX y comandos básicos](apuntes/HFC06_08_2024.md)
- [Gestión de Software, Usuarios y grupos y más comandos utiles](apuntes/HFC07_08_2024.md)
- [sudo, Permisos, Variables de Entorno y Redes](apuntes/HFC08_08_2024.md)
- [Herramientas de busqueda y Bash Scripting](apuntes/HFC09_08_2024.md)
- [Procesos y Cron](apuntes/HFC12_08_2024.md)

## Tareas

- [Tareas 1](tareas/tarea1.md)
- [Tarea 2](tareas/tarea2/tarea2.md)
- [Tareas 3](tareas/tarea3.md)

---
# Instructor

### *Temario*

1. *¿Qué es Unix y cual es su situación?*
	1. **Sistema Operativo**
	2. **Historia**. UNIX, Linus Torvalds, Richard Stallman/GNU
	3. **Software Libre**. Fortaleza y punto débil
	4. **Distros**. Gestor de paquetes, Familias e Ideas
	5. **Instalación**. Particiones
	6. **Ajustes iniciales**. Usuarios, Grupos, Sudo, Actualización
	7. ==Ejercicio TryHackMe y dudas==

2. *Comandos esenciales e Instalaciones*
	1. **Tipos de archivos básicos**
	2. **Comandos básicos (Navegación y manipulación)**. `cd`, `ls`, `mv`, `cp`, `rm`, `mkdir`, `touch`. 
	3. ==Ejercicio de navegación y creación==
	4. **Gestor de paquetes**. Instalación, Busqueda y Desinstalación
	5. **Paquetes alternativos**. Flatpak, AppImage, Snap
	6. **Github, Scripts**
	7. **Instalaciones de fuente**
	8. ==Ejercicio de algunas instalaciones importantes==
	9. **Carpetas del sistema**. `/etc`, `/bin`, `/sbin`, `/usr`...

3. *Shell a fondo*
	1. **Diferencia entre Shell y Terminal**. *Bash*, *Zsh*, *Fish y otras*
	2. **Más comandos (Archivos)** `cat`, `more/less`, `file`, `head`, `tail`, `nl`, `wc`.
	3. **Banderas**. Banderas clásicas, Manuales, Ayuda
	5. **Permisos**: `ugoa`, `rwx`, `chmod`, `chown`, `chgrp`
	6. ==Ejercicio ???==
	7. **Sudo**, **SUID/SGID**
	8. **Variables (de entorno)**. Variables, `PATH`, `"`, `'`.
	9. **Más comandos**: `grep`, `tr`, `cut`, `sed`, `awk`
	10. **Editores de texto** `nano`, `vim`
	11. ==Ejercicio ???==

4. *Administración de Usuarios y Grupos* | *Operadores*
	1. **Grupos**
	2. **Archivos importantes 1**. `/etc/passwd`, `/etc/group`
	3. **Función hash**
	4. **Contraseñas**: `/etc/shadow`
	5. `useradd`, `usermod`, `userdel`
	6. `groupadd`, `groupmod`, `groupdel`
	7. ==Ejercicio de usuarios y *su*==
	8. **Más comandos**. `find`, `whereis`, `which`, `locate`
	9. **Salida Estándar, Salida de Error, Entrada Estándar**
	10. **Redirecciones**, **Operadores** y **Sustitución**
	11. ==Ejercicio operadores==

5. *Procesos, Redes, Sistema*

6. *Bash Scripting*

7. *Miscelaneo y Ejercicios*