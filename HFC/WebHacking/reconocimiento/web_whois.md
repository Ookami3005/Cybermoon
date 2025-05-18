[<- Índice](../WebHacking.md)
# WHOIS

> ***WHOIS*** es un protocolo mundialmente utilizado para el acceso y consulta de bases de datos que almacenan información de recursos de *Internet* **registrados**.
> Esta principalmente relacionado con **nombres de dominio** pero tambien almacena detalles de **bloques de direcciones IP** y de otros sistemas autónomos.

Podemos pensar que es un gran directorio telefónico del *Internet*, que te permite conocer quienes son los dueños o responsables de ciertos recursos en línea.


```bash
whois inlanefreight.com

# ...
# Domain Name: inlanefreight.com
# Registry Domain ID: 2420436757_DOMAIN_COM-VRSN
# Registrar WHOIS Server: whois.registrar.amazon
# Registrar URL: https://registrar.amazon.com
# Updated Date: 2023-07-03T01:11:15Z
# Creation Date: 2019-08-05T22:43:09Z
# ...
```

Cada registro *WHOIS* típicamente contiene la siguiente información:

- **Nombre de dominio**: Dominios convencionales, por ejemplo `google.com`
- **Registrar**: La compañia **donde** se registró el dominio (*GoDaddy*, *Namecheap*)
- **Contacto registrante**: La persona u organización que registró el dominio
- **Contacto administrativo**: La persona responsable de administrar el dominio
- **Contacto técnico**: La persona encargada de cualquier problema técnico con el dominio.
- **Fechas de creación y expiración del dominio**
- **Name Servers**: Servidores *DNS* encargados de traducir el nombre de dominio a IP.

## Historia

> En los años 1970, la computóloga *Elizabeth Feinler* y su equipo en el **NIC** (*Network Information Center*) del instituto de investigación *Standford*, identificaron la necesidad de un **sistema de seguimiento** capaz de administrar la creciente cantidad de recursos virtuales en el *ARPANET*, el precursor del internet moderno.

La solución fue la creación del directorio *WHOIS* como una rudimentario pero vital base de datos que almcaenaba todo acerca de usuarios, nombres de dispositivos y nombres de dominio.

### ¿Por qué es importante para el reconocimiento?

Los datos en *WHOIS* son una valiosa fuente de información para auditores de seguridad, pues ofrece un vistazo a la **huella digital** de la organización y potenciales objetivos o vulnerabilidades presentes.

Por ejemplo:

- ***Identificar personal clave***: Los registros *WHOIS* revelan nombres, correos electrónicos y hasta números de telefonos de las personas involucradas con el dominio. Esta información puede aprovecharse para realizar ataques de **ingeniería social** y perfilar posibles campañas de *Phishing*.

- ***Descubir la infraestructura de la red***: Muchos detalles técnicos como servidores *DNS* y direcciones IP que brindan pistas acerca de la infraestructura de red del objetivo.

- ***Análisis de datos históricos***: Accediendo a registros historicos de *WHOIS* mediante aplicaciones web como [WhoisFreaks](https://whoisfreaks.com/) puede revelarnos cualquier cambio en el propietario, información de contacto o detalles técnicos a través del tiempo, que podríamos aprovechar para analizar la evolución de la organización.

## Uso

La manera más sencilla de consultar **nombres de dominio** en registros *WHOIS* es utilizando el comando `whois` e indicarle el dominio que deseas consultar.
Por ejemplo:

```bash
whois facebook.com

#    Domain Name: FACEBOOK.COM
#    Registry Domain ID: 2320948_DOMAIN_COM-VRSN
#    Registrar WHOIS Server: whois.registrarsafe.com
#    Registrar URL: http://www.registrarsafe.com
#    Updated Date: 2024-04-24T19:06:12Z
#    Creation Date: 1997-03-29T05:00:00Z
#    Registry Expiry Date: 2033-03-30T04:00:00Z
#    Registrar: RegistrarSafe, LLC
#    Registrar IANA ID: 3237
#    Registrar Abuse Contact Email: abusecomplaints@registrarsafe.com
#    Registrar Abuse Contact Phone: +1-650-308-7004
#    Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
#    Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
#    Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
#    Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
#    Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
#    Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
#    Name Server: A.NS.FACEBOOK.COM
#    Name Server: B.NS.FACEBOOK.COM
#    Name Server: C.NS.FACEBOOK.COM
#    Name Server: D.NS.FACEBOOK.COM
#    DNSSEC: unsigned
#    URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
# >>> Last update of whois database: 2024-06-01T11:24:10Z <<<
# 
# ...
# Registry Registrant ID:
# Registrant Name: Domain Admin
# Registrant Organization: Meta Platforms, Inc.
# ...
```

# Enlaces

[<- Introducción](web_introRecon.md) | [DNS y dominios ->](web_dnsDominios.md)