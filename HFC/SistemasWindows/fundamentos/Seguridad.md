[<- Índice](../SistemasWindows.md)
# Seguridad en Windows

> La seguridad en *Windows* esta principalmente basada en el uso de ciertas aplicaciones y funcionalidades incluidas junto con el sistema.

Las más importante son:

### Windows Update

Es un servicio provisto por *Microsoft* que ofrece actualizaciones de seguridad, mejoras y parches para el sistema operativo y demás aplicaciones **periódicamente**.

### Windows Security

Es una sección de los ajustes dedicada a opciones principales de defensa del dispositivo, como son:

- **Protección contra virus y amenazas**
- **Firewall**
- **Control de aplicaciones y navegadores**
- **Reporte de salud y desempeño del equipo**
- Entre otras

#### Firewall

*Windows* incluye su propio *Host Firewall*, es decir, un *firewall* como funcionalidad del mismo sistema operativo en lugar de un dispositivo a parte.

Este puede regular 3 modos pensados para 3 tipos principales de redes:

- ***Redes de Dominio***: Redes donde este dispositivo se autentifica con un **controlador de dominio**.
- ***Redes privadas***: Redes que el usuario designa como seguras y privadas, por ejemplo su propia red en su casa.
- ***Redes públicas***: Cualquier red pública y potencialmente insegura como la red de una cafetería, aeropuerto, etc.

#### Control de aplicaciones

Tambien conocido como ***Microsoft Defender SmartScreen***, es la utilidad encarga de identificar y proteger el dispositivo contra sitios o aplicaciones maliciosas.

Constantemente revisa las aplicaciones que se ejecutan y de considerarlas inseguras, impide que continuen su ejecución y las contiene.

### BitLocker

Es una manera de proteger la información del disco duro mediante la **encriptación** de este, para protegerlo en caso de robo, extravío o cualquier situación que favorezca la manipulación del disco por parte de un actor malicioso.

# Enlaces

[<- MSConfig](MSConfig.md) | [Windows CMD ->](WindowsCMD.md)