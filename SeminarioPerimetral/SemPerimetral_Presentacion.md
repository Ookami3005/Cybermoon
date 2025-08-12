# Presentación

## Objetivo 

> El estudiante comprenderá y aplicará conceptos, herramientas y técnicas fundamentales para proteger la infraestructura perimetral de una red mediante *firewalls*, reglas *iptables*, *scripting* en *shell* y otras herramientas de seguridad, con el fin de implementar una defensa efectiva contra amenazas externas en un entorno digital.

## Temario

1. **Introducción a la Seguridad Perimetral**
	1. Concepto de perimetro de red
	2. Amenazas comunes: escaneo, intrusiones, denegación de servicio
	3. Arquitecturas seguras y zonas de red (*DMZ*, zonas desmilitarizadas)
	4. Modelos de defensa: *"Defensa en profundidad"*

2. **Fundamentos y Tipos de Firewalls**
	1. Tipos de *firewalls*,: *packet filtering*, *inspection*, *application-level*
	2. *Firewalls* multiplataforma
	3. Políticas de seguridad perimetral
	4. Métodos de autenticación en el perimetro

3. **Administración de firewall locales**
	1. Introducción a *NetFilter* e *iptables*
	2. Reglas y políticas por defecto
	3. Casos de uso (puertos, *NAT*, redirección, filtrado por IP y etc)

4. **Scripting para Automatización de Seguridad**
	1. Fundamentos de *shell scripting*
	2. Condicionales, ciclos, funciones
	3. Automatización de reglas de *firewall*
	4. Registro y notificación de eventos sospechosos

5. **Firewalls de Nueva Generación**
	1. Funcionalidades adicionales: inspección profunda, control de aplicaciones, geobloqueo
	2. Dispositivos y *software* comerciales (ej. PfSense, Fortinet, Palo Alto)
	3. Integración con *IDS/IPS* (Snort, Suricata, etc)

6. **Gestión de Registros y Monitoreo**
	1. Registro de eventos (`/var/log/`, `syslog`, `journald`)
	2. Herramientas de monitoreo: *logwatch*, *logrotate*
	3. Análisis de tráfico: *tcpdump*, *Wireshark*

7. **Segmentación y Políticas de red**
	1. *VLANs* y segmentación lógica
	2. Control de acceso por zonas
	3. Listas de control de acceso (*ACL*)
	4. Buenas prácticas de administración de reglas