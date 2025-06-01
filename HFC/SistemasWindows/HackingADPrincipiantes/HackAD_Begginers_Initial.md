# Vectores de ataque iniciales contra Active Directory

> En esta sección hablaremos de 2 importantes **vectores de ataque** relacionados a **Active Directory**, que podríamos aprovechar como un acercamiento **inicial** al entorno.

- ***LLMNR Poisoning***
- ***SMB Relay***
- ***IPv6 Attacks***

Ambas técnicas pertecen más a un enfoque **interno** de la prueba de penetración pues se requiere que tengamos acceso completo a la red del **directorio activo**, que típicamente es la *intranet* de la organización.

### LLMNR Poisoning

> ***LLMNR*** es un protocolo de red, mayormente de sistemas *Windows*, utilizado para identificar dispositivos dentro de la red, independientemente del protocolo **DNS**.

Es la evolución del protocolo obsoleto **NBT-NS** y surgió como una alternativa a los **nombres de dominio**.

Sin embargo, este protocolo posee una debilidad pues