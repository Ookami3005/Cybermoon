[<- Índice de Módulos](../HackingFightClub.md)
# Redes de Computadoras

![hogueraDS](../../imagenes/bonfire2.jpg)

1. [Introducción a Redes y Capas físicas](apuntes/HFC15_08_2024.md)
2. [Bases, conversiones y Capa de enlace de red](apuntes/HFC16_08_2024.md)
3. [Capa de red](apuntes/HFC19_08_2024.md)
4. **Capa de aplicación**
	1. 

### Temario

1. **Introducción**
	1. **Que es una red**: Publicas y privadas
	2. **Dispositivos finales, intermedios**
	3. **Medios de comunicación**: Aire, Electricidad, Luz
	4. **Clasificación según su alcance**: PAN, LAN/WLAN, MAN, WAN
	5. **Topologías**
	6. **Unicast, Multicast y Broadcast**
	7. **Opcional**
		1. Binario, Hexadecimal, Decimal
		2. Trailer de modelos
		3. Trailer de paquetes

2. **Modelo OSI**
	1. **Física**: Interfaces de red, Cables, Hardware
	2. **Enlace de datos**: Direcciones *MAC*, *Switch*
	3. **Red**: IP, ICMP, Routers
	4. **Transporte**: Puertos, TCP, UDP
	5. **Sesión**
	6. **Presentación**: Cifrado
	7. **Aplicación**: Protocolos

3. **Modelo TCP/IP**
	1. **Fisica**
	2. **Internet**
	3. **Transporte**
	4. **Aplicación**

4. **Paquetes y encapsulación**
	1. Paquete red
	2. Marco MAC
	3. Cabecera IP
	4. Cabecera TCP/UDP/ICMP
	5. Datos de aplicación
	6. ***Simulaciones***

7. **Subredes**
	1. **Submáscaras**
	2. **CDIR**
	3. ¿Por qué segmentar las redes?
	4. **Segmentación**
		1. *VLSM*
		2. Otra

8. **IP a fondo**
	1. IPv4 vs IPv6
	2. Clases
	3. Direcciones IP privadas
	4. Direcciones IP especiales
	5. NAT
	6. *VPN*
	
9. **TCP/UDP/ICMP**
	1. Porque usar TCP?
	2. Porque usar UDP?
	3. Porque usar ICMP?
	4. Banderas TCP
	5. Acknowledge y Sequence Number
	6. **nping**
	7. **nc o ncat**

11. **Protocolos de aplicación**
	1. ***DNS***
		1. *Dig*, *Nslookup*
	2. *HTTP*
	3. *SSH*
	4. **Otros**: *FTP*, *IMAP/POP3*, *SMB*, etc
	5. Versiones seguras: *HTTPS*, *FTPS*, *IMAPS*, *DoT*
	6. *Wireshark*