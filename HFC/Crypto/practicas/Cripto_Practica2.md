# Práctica 2 - Criptografía y Seguridad

> Fernando Romero Cruz - *319314256*

### Reporte

#### Parte 1

> Para esta sección, se utilizó un *Script* simple de *Python* para abrir el archivo y realizar las iteraciones de descifrado hasta dar con aquella que contuviera la cadena `DOS`. Una vez encontrada, se almacenó la llave y se escribio el contenido descifrado con la llave correcta, retirando la cabecera, en el archivo `virus`.

```python
#!/usr/bin/env python

reader = open('57FD6325.VBN','rb')
cuarentena = reader.read()
llave = 0

for i in range(256):
     temp = bytes([b^i for b in cuarentena])
     if b'DOS' in temp:
        llave = i
        break

print(f'Llave encontrada: {llave}')
desofus = bytes([b^llave for b in cuarentena])
start = desofus.find(b'MZ')
print(f'Numeros magicos encontrados en el byte: {start}')

writer = open('virus','wb')
writer.write(desofus[start:])
```

Este *Script* nos da una salida como la siguiente, ademas de crear el archivo especificado, que podemos comprobar que efectivamente es un **ejecutable** de *Windows*:

![recupera_virus.png](imagenes/recupera_virus.png)

Tambien podemos comprobar que efectivamente es un virus subiendo el archivo a la página *Virus Total*:

![virustotal.png](imagenes/virustotal.png)

#### Parte 2

> Esta segunda sección si se me dificultó un poquito más. Empecé como recomienda el documento, al realizar un *XOR* entre el criptograma que deseamos recuperar con el resto.
> De esta manera sabemos que todos las nuevas cadenas, por la propiedad inversa del *XOR*, son en realidad el resto de **mensajes claros** operados con el mensaje en claro que buscamos.

```python
#!/usr/bin/env python

from pwn import xor
from string import ascii_uppercase,ascii_lowercase,printable
ciphers = [b'']*12

ciphers[0]=bytes.fromhex('72212b29402b66342437255717562d652e5748692f2452206b17153b36364b2236172f2b372b2a2c2c6f2d6e3e3347')
ciphers[1]=bytes.fromhex('5a74202b412f243123743459175c3737394b1224382f5d332244747436364b35365b3426276a3d762e2e206e273f46312a30203b')
ciphers[2]=bytes.fromhex('752122394f23233e39316d555e412c653949472c21295732674537272621572579170b2b742e21762720226e192f582726263527')
ciphers[3]=bytes.fromhex('403b283d412722316d356d4d5952632d2d555b25212456352217313a21235d35345e21296e6a2837633c29203e2854653a74323d23')
ciphers[4]=bytes.fromhex('7e3b213d463a277039213b5717423620784b473a3d2056252245782727734c3725522f67302f643b2c23293c7137542c39')
ciphers[5]=bytes.fromhex('5e353731512166316d253851525d63373d4b422623215d336b17303d38324b7636172226276a3523266f282b333515212226')
ciphers[6]=bytes.fromhex('62212c2b5a6e212224202c4a174a63362d18553b2431576121423d74213c5e3934562a28743a2b246320383c307a572a2035')
ciphers[7]=bytes.fromhex('63313737153a233e292624591b1337243418442c37654d2f675f313e3d735c3377553b223a2b642522212b3c34')
ciphers[8]=bytes.fromhex('13382a2b152d2e393f26245c584063213d1842263f31572f224478252736182532172f25262f2a7a632a206e3b3b51202c')
ciphers[9]=bytes.fromhex('7274353d462f347029316d5d44472c3678515c2a24215d2f33522b743e3c4b76395e2028276a212422216c273f2950352226202a3c5040')
ciphers[10]=bytes.fromhex('603d65365a6e35356d38281854462e35345153276d364d3267543924203a5b3e38446e65382f6432222d2d6e323254332620206a')
ciphers[11]=bytes.fromhex('7b353c78522b282428743c4d52132d2a784c5b2c23201824344337393334577627563c26742f3722266f232838395c2a')
goal=bytes.fromhex('62212078533c2f313e743e5759132f242b185f282324562034173d3a7210512333562a67062f253a')

processed=[xor(ciphers[i],goal) for i in range(len(ciphers))]

```

Sabiendo esto, estuve mucho tiempo intentando figurar como podría apoyarme del dato de los espacios invirtiendo *capitalización* pero sinceramente no me resulto de mucha ayuda.
Investigando un poco, leí que uno de ==los mejores metodos para romper estas encriptaciones *OTP* era intentar descifrar varios segmentos de estas cadenas con palabras que sabemos o esperamos que estén en el mensaje en claro, esperando encontrar palabras con sentido==. Como desconocía cualquier indicio del contenido del mensaje en ese momento, simplemente intente con las palabras más comunes en el español que son adverbios y preposiciones como *"que"*, *"cuando"*, *"pero"*, *"porque"*, *"el"*, y demás palabras similares.

Un problema fue que entre más corta fuera la palabra, más dificil era identificar palabras con sentido en las resultantes. Para esto intente usar las palabras más largas y otra cosa que me fue de mucha ayuda fue agregar espacios al inicio y al final de la palabra, pues regularmente deberían ir separadas todas las palabras, ganando otros 2 caracteres de longitud.

Esta fue la sección de código que utilicé:

```python
palabra = input('Introduce palabra o cadena: ')
palabra = palabra.encode()
for i in range(len(processed)):
    for j in range(len(processed[i])-len(palabra)):
        word = xor(palabra,processed[i][j:j+len(palabra)])
        flag=True
        for c in word:
            # if chr(c) not in printable:
            if chr(c) not in (ascii_uppercase+ascii_lowercase+' '):
                flag=False
                break
        if flag:
            print(word)
```

Recordemos que las cadenas resultantes al inicio, son en realidad el **mensaje en claro** que buscamos, operado con *XOR* a cada uno del resto de mensajes en claro. Entonces si lograbamos descubir alguna palabra en el mensaje que buscamos, **teóricamente** deberiamos recibir mediante este código *11* cadenas con sentido, lo cual sería de mucha ayuda ya que si usamos una palabra en un mensaje en claro restante, solo obtendriamos 1 cadena con sentido (la del mensaje buscado).

En fin, comencé probando varias palabras comunes pero la cadena que más me dio resultados fue ` que `, obteniendo la siguiente salida:

```
Introduce palabra o cadena:  que 
b'scfn '
b'oifws'
b'epwsj'
b'ecnh '
b'muxg '
b' dce,'
b'tuuiJ'
b'edsma'
b'n las'
b'ish, '
b'qwsnn'
b'ad en'
b' iuz '
b' jqqa'
b'en Ci'
b'updzi'
b'ncnws'
b'dsKea'
b' son '
b'ueusa'
```

Particularmente reconocí las siguientes cadenas:

```
b'n las'
b' son '
```

Por ejemplo, utilizar ahora ` son ` no sería muy distinto a la cadena que recién utilizamos, por lo que me fui por `n las ` (agregandole un espacio a la derecha), para ganar un caracter más de visibilidad.

```
Introduce palabra o cadena: n las 
b' en vo'
b'b ha, '
b' otras'
b'iro aq'
b'na hum'
b'hmhlq '
b'l lte '
b'n yws,'
b' que s'
b'en res'
b'kad Re'
b'grbrit'
b' y su '
b', tal '
b'q wega'
b'zawn h'
b'orvjrs'
b'os de '
b'stos i'
b'xn Miu'
b'tufiyo'
b'grve l'
b'cumpli'
b'e no t'
```

Nuevamente, reconoci una serie de cadenas, por ejemplo:

```
b' otras'
b'iro aq'
b' que s'
b'en res'
b' y su '
b', tal '
b'os de '
b'stos i'
b'cumpli'
b'e no t'
```

Como se ve, aumentan un poco las cadenas reconocidas, además ahora podemos extender ` otras` con un espacio a la derecha o incluso aventurarnos a intentar completar palabras como `cumpli` a `cumplio`, etc. También me di cuenta que debia permitir en mi filtro las comas (`,`) porque se perdían repentinamente algunos mensajes y ampliando mi filtro a todos los caracteres imprimibles (`printable`) se solucionaba. Es decir, modifique el filtro a:

```python
# ...
if chr(c) not in (ascii_uppercase+ascii_lowercase+' '+','):
# ...
```

No seguí ningun otro método preciso ni elegante para descifrar los mensajes, simplemente fui ganando terreno caracter por caracter como describí anteriormente hasta tener mensajes grandes que almacené como variables en mi código:

```python
# palabra=b'que frias son las mananas en Ciudad Real!nl6'
# palabra=b'aunque dicho en voz baja, Modesta alcanzo a '
# palabra=b'a tuvo que suspender'
# palabra=b's chirridos de portones que se abren,'
# palabra=b'gente que no tiene estos'
# palabra=b'o se le cumplian sus caprichos'
# palabra=b'a pesar de estos incidentes los ninos eran '
# palabra=b'pero tendria, tal vez un hijo de buena sangr'
# palabra=b'quiso gritar y su grito fue sofocado por otr'
# palabra=b'Marido a quien responder, hijas a las que de'
```

Hay que notar 2 cosas importantes, como dije aquella cadena que imprimía más salidas, teóricamente debería ser nuestro mensaje en claro buscado, y esa resultó ser: `que frias son las mananas en Ciudad Real!nl6`, como se demuestra al introducir esta cadena al algoritmo:

```python
palabra=b'que frias son las mananas en Ciudad Real!nl6'

for i in range(len(processed)):
    for j in range(len(processed[i])-len(palabra)):
        word = xor(palabra,processed[i][j:j+len(palabra)])
        flag=True
        for c in word:
            # if chr(c) not in printable:
            if chr(c) not in (ascii_uppercase+ascii_lowercase+' '+','):
                flag=False
                break
        if flag:
            print(word)
```

En esa ocasión se obtiene la siguiente salida:

```
b'aunque dicho en voz baja, Modesta alcanzo a '
b'I estaban ya otras mujeres, descalzas y mal '
b'modesta tuvo que suspender su tarea de moler'
b'Marido a quien responder, hijas a las que de'
b'quiso gritar y su grito fue sofocado por otr'
b'pero tendria, tal vez un hijo de buena sangr'
b'a pesar de estos incidentes los ninos eran i'
b'hay gente que no tiene estomago para este of'
```

Y en caso de ocupar alguna de estas cadenas, solo se obtiene la cadena antes mencionada.
Otra cuestión fue que dejaba de tener sentido despues del fragmento `Ciudad Real`, cosa algo desconcertante porque los demás mensajes siguen teniendo sentido, entonces se me ocurrió que tal vez ya habia superado la longitud del criptograma inicial que buscabamos romper, por lo revise la longitud del criptograma y efectivamente, el mensaje era de `44` caracteres mientras que el criptograma solo de `40`, entonces recorté el mensaje y realicé un *XOR* con el criptograma para obtener la llave.

Finalmente utilicé esta misma llave para descifrar el resto de criptogramas:

```python
print()
llave=xor(goal,palabra[:len(goal)])
print(f'Llave recuperada: {llave}')

for c in ciphers:
    print(f'Mensaje recuperado: {xor(llave,c[:len(llave)])}')

print(f'Mensaje recuperado: {xor(llave,goal)}')
```

Este código, agrego esta salida al programa:

```
Llave recuperada: b'\x13TEX5NFPMTM873CEX82IME8AG7XTRS8VW7NGTJDV'

Mensaje recuperado: b'aunque dicho en voz baja, Modesta alcanz'
Mensaje recuperado: b'I estaban ya otras mujeres, descalzas y '
Mensaje recuperado: b'fugazmente miro aquellos rostros. El de '
Mensaje recuperado: b'Sometida a una humillante inspeccion: la'
Mensaje recuperado: b'modesta tuvo que suspender su tarea de m'
Mensaje recuperado: b'Marido a quien responder, hijas a las qu'
Mensaje recuperado: b'quiso gritar y su grito fue sofocado por'
Mensaje recuperado: b'pero tendria, tal vez un hijo de buena s'
Mensaje recuperado: b'\x00los chirridos de portones que se abren,'
Mensaje recuperado: b'a pesar de estos incidentes los ninos er'
Mensaje recuperado: b'si no se le cumplian sus caprichos "le d'
Mensaje recuperado: b'hay gente que no tiene estomago para est'
Mensaje recuperado: b'que frias son las mananas en Ciudad Real'
```

Claramente ese primer caracter de la llave no cuadra,  por lo que despues de pensarlo un rato, probé con la misma cadena pero en mayuscula: `Que frias son las mananas en Ciudad Real!nl6`


```
Llave recuperada: b'3TEX5NFPMTM873CEX82IME8AG7XTRS8VW7NGTJDV'

Mensaje recuperado: b'Aunque dicho en voz baja, Modesta alcanz'
Mensaje recuperado: b'i estaban ya otras mujeres, descalzas y '
Mensaje recuperado: b'Fugazmente miro aquellos rostros. El de '
Mensaje recuperado: b'sometida a una humillante inspeccion: la'
Mensaje recuperado: b'Modesta tuvo que suspender su tarea de m'
Mensaje recuperado: b'marido a quien responder, hijas a las qu'
Mensaje recuperado: b'Quiso gritar y su grito fue sofocado por'
Mensaje recuperado: b'Pero tendria, tal vez un hijo de buena s'
Mensaje recuperado: b' los chirridos de portones que se abren,'
Mensaje recuperado: b'A pesar de estos incidentes los ninos er'
Mensaje recuperado: b'Si no se le cumplian sus caprichos "le d'
Mensaje recuperado: b'Hay gente que no tiene estomago para est'
Mensaje recuperado: b'Que frias son las mananas en Ciudad Real'
```

Ahora todo tiene mucho más sentido, comprobando así que la primera sección de la llave es `3TEX5NFPMTM873CEX82IME8AG7XTRS8VW7NGTJDV`, y que el mensaje buscado era `Que frias son las mananas en Ciudad Real`.

### Cuestionario

1. ¿Para qué se usa la herramienta XORsearch? https://blog.didierstevens.com/programs/xorsearch/

> Es una utilidad que permite buscar cadenas en archivos **posiblemente** codificados en *XOR*, *ROL* o *ROT*, precisamente realizando varias de estas desencriptaciones y analizando si la cadena buscada se encuentra en esta nueva información.
> 
> Particularmente para *XOR*, prueba todas las posibles llaves de un solo *byte*, de 0 a 255.

2. ¿De cuántos bytes es la cabecera que le agregó el antivirus al malware?

> De ***13536*** *bytes*, pues los números mágicos comienzan en el **6918**.

3. ¿Qué son los números mágicos? (relacionado con archivos)

> Los números mágicos buscados son `0x4d` y `0x5a`, o tambien representable como la cadena `MZ`.

4. ¿Qué es VirusTotal?

> Una base de datos que almacena las información, características y funcionalidades de *Malware* previamente detectado y cargado a ésta página.
> Este *Malware* es identificado principalmente por sus firmas *hash*.

5. De acuerdo a VirusTotal, ¿qué tipo de malware es?

> *VirusTotal* parece indicar que se trata de un **virus tipo troyano**.

6. En la parte 2 de la práctica, ¿a qué obra y a qué autora pertenecen los textos que logró descifrar?

> Pertenecen a la obra *Modesta Gómez* de la autora *Rosario Castellanos*.

### Conclusiones

> Con el desarrollo de esta práctica, aprendí una manera de poner en cuarentena a un **virus** y como recuperarlo. Me pareció interesante que se haya utilizado la criptografía como una técnica defensiva contra este tipo de virus. Por otra parte, pude comprender la importancia de las reglas de un *OTP*, particularmente aquella que dicta que son de **uso único** pues incluso desde 2 mensajes cifrados con esta podemos realizar diversas técnicas de criptoanálisis a partir de cancelar la llave y utilizar palabras comunes. Me pareció una forma dinámica y entretenida de practicar cifrados de *XOR*, haciendo ligero el aprendizaje y poniendo a prueba los conceptos vistos en clase.