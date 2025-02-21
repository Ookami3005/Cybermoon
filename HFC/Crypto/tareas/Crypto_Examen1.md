# Examen 1

> Fernando Romero Cruz - 319314256

#### Ejercicio 10

Dado el siguiente texto cifrado con un sistema de sustitución polialfabética (*Vigenère*), encuentre el mensaje en claro utilizando la prueba de *Kasiski*. Utilice las frecuencias para el idioma en el que fué escrito, éstas se listan en la tabla 2 de la hoja del examen. Describa el desarrollo del proceso utilizado.

```
VFBTY SCKQT VITZR NFSCJ SGXGC EYIMO EBCGB BVISS XFQIQ DVWCC
CCZWP VCGFI UKDXG RCNYN MCFVA TBJGR RGNGV VBJLS TZLIH FGMCP
EWPXG CYCTD YIBCP FPIKW EBYCN BULQE XQVZL VFOTB GCMCR IMKBR
KEEWB AYZRW TQEVQ HPVGE RGNGV VBTFD NFWTX GCICS XQGZT TWWPW
JJXBE VBDKA CEGEN ZCKCS BBVFP TOSCC GCZGG EQXMW XVGCY CTDYI
BCPFP EXFHF PBBBI RAIBC PJRWT HEICP MSCJC RNFKK WWHZG WMGMV
GRRIT QMVPI HGNZN IAFQL EWGCV VAWGC NFENB BVYCL HFNUA PGDTF
RTVHC SSHBB GJQYN GVRQR KMRKY CTZAJ RHTFG JMBXH KDCHT PNVRD
KSXVY AMVGG JPBBV VVIHT CTMSX ROVQH TUGSW UBBFZ LVTKG RICXG
UKFPM ZGKQI ASOSW ETGUK FTXBE IWEMW QERTV VPFJD ZMUFA XTZGE
EXGSG IQJLS FVATI HKFLE KOEKG RXRQE WDNFG DNAHM GVQIH PAGYH
LGGTS GBHAK CRABQ CMVRY GMGCF WVEGR D
```

##### Procedimiento

A lo largo de este ejercicio, me apoyé con un *Script* en el lenguaje *Python* para los cálculos pesados y repetitivos requeridos durante la solución.
Con eso dicho, empecé por limpiar un poco los espacios y saltos de línea del criptograma y definí una función para descomponer un número en sus factores primos:

```python
criptograma="""VFBTY SCKQT VITZR NFSCJ SGXGC EYIMO EBCGB BVISS XFQIQ DVWCC
CCZWP VCGFI UKDXG RCNYN MCFVA TBJGR RGNGV VBJLS TZLIH FGMCP
EWPXG CYCTD YIBCP FPIKW EBYCN BULQE XQVZL VFOTB GCMCR IMKBR
KEEWB AYZRW TQEVQ HPVGE RGNGV VBTFD NFWTX GCICS XQGZT TWWPW
JJXBE VBDKA CEGEN ZCKCS BBVFP TOSCC GCZGG EQXMW XVGCY CTDYI
BCPFP EXFHF PBBBI RAIBC PJRWT HEICP MSCJC RNFKK WWHZG WMGMV
GRRIT QMVPI HGNZN IAFQL EWGCV VAWGC NFENB BVYCL HFNUA PGDTF
RTVHC SSHBB GJQYN GVRQR KMRKY CTZAJ RHTFG JMBXH KDCHT PNVRD
KSXVY AMVGG JPBBV VVIHT CTMSX ROVQH TUGSW UBBFZ LVTKG RICXG
UKFPM ZGKQI ASOSW ETGUK FTXBE IWEMW QERTV VPFJD ZMUFA XTZGE
EXGSG IQJLS FVATI HKFLE KOEKG RXRQE WDNFG DNAHM GVQIH PAGYH
LGGTS GBHAK CRABQ CMVRY GMGCF WVEGR D"""

# Limpieza del criptograma
criptograma = criptograma.replace('\n','')
criptograma = criptograma.replace(' ','')

print(f'Criptograma inicial:\n\n{criptograma}\n')

# Descoposición de factores primos
def factores_primos(n):
    i = 2
    factors = []
    while i * i <= n:
        if n % i:
            i += 1
        else:
            n //= i
            factors.append(i)
    if n > 1:
        factors.append(n)
    return factors
```

Posteriormente, analicé las frecuencias de bloques de caracteres de longitudes desde 2 hasta 10, específicamente aquellas mayores a 1 y descompuse las distancias entre ellas en factores primos con la función antes mencionada:

```python
# Analisis de frecuencias
secciones = []
for tam in range(2,10):
    secciones = [criptograma[i:i+tam] for i in range(0,len(criptograma),tam)]
    for sub in secciones:
        cuenta = secciones.count(sub)
        apariciones = [index for index in range(len(criptograma)) if criptograma[index:index+len(sub)] == sub]
        if cuenta != 1:
            print(f'{sub} : {cuenta} : {[factores_primos(apariciones[i+1] - apariciones[i]) for i in range(len(apariciones)-1)]}')

```

Con esta sección de código obtuve algunos resultados similares a:

```
...
FVA : 2 : [[2, 3, 73]]
GRR : 2 : [[2, 3, 37]]
GNG : 2 : [[2, 3, 3, 5]]
VVB : 2 : [[2, 3, 3, 5]]
JLS : 2 : [[2, 2, 3, 5, 7]]
CYC : 2 : [[2, 3, 23]]
TDY : 2 : [[2, 3, 23]]
IBC : 3 : [[2, 3, 23], [2, 3, 3]]
PFP : 2 : [[2, 3, 23]]
GNG : 2 : [[2, 3, 3, 5]]
VVB : 2 : [[2, 3, 3, 5]]
CYC : 2 : [[2, 3, 23]]
TDY : 2 : [[2, 3, 23]]
IBC : 3 : [[2, 3, 23], [2, 3, 3]]
PFP : 2 : [[2, 3, 23]]
IBC : 3 : [[2, 3, 23], [2, 3, 3]]
GRR : 2 : [[2, 3, 37]]
WGC : 2 : [[2, 3]]
WGC : 2 : [[2, 3]]
UKF : 2 : [[2, 3, 3]]
UKF : 2 : [[2, 3, 3]]
JLS : 2 : [[2, 2, 3, 5, 7]]
FVA : 2 : [[2, 3, 73]]
RGNGV : 2 : [[2, 3, 3, 5]]
RGNGV : 2 : [[2, 3, 3, 5]]
TDYIBC : 2 : [[2, 3, 23]]
TDYIBC : 2 : [[2, 3, 23]]
```

Se nota que todas las ditancias entre bloques comparten un factor de *6*, lo que plantea la posibilidad de que la llave sea de esta misma longitud.
Sabiendo esto, separe el criptograma en bloques de 6 caracteres de la siguiente manera:

```python
# Factor común obtenido: 6

# Separación adecuada
separado=''
bloques = [criptograma[i:i+6] for i in range(0,len(criptograma),6)]
for bloque in  bloques:
        separado += bloque + ' '
separado = separado[:len(separado)-1]

print()
print(f'Mensaje separado adecuadamente:\n\n{separado}')
print()
```

```
VFBTYS CKQTVI TZRNFS CJSGXG CEYIMO EBCGBB VISSXF QIQDVW CCCCZW PVCGFI UKDXGR CNYNMC FVATBJ GRRGNG VVBJLS TZLIHF GMCPEW PXGCYC TDYIBC PFPIKW EBYCNB ULQEXQ VZLVFO TBGCMC RIMKBR KEEWBA YZRWTQ EVQHPV GERGNG VVBTFD NFWTXG CICSXQ GZTTWW PWJJXB EVBDKA CEGENZ CKCSBB VFPTOS CCGCZG GEQXMW XVGCYC TDYIBC PFPEXF HFPBBB IRAIBC PJRWTH EICPMS CJCRNF KKWWHZ GWMGMV GRRITQ MVPIHG NZNIAF QLEWGC VVAWGC NFENBB VYCLHF NUAPGD TFRTVH CSSHBB GJQYNG VRQRKM RKYCTZ AJRHTF GJMBXH KDCHTP NVRDKS XVYAMV GGJPBB VVVIHT CTMSXR OVQHTU GSWUBB FZLVTK GRICXG UKFPMZ GKQIAS OSWETG UKFTXB EIWEMW QERTVV PFJDZM UFAXTZ GEEXGS GIQJLS FVATIH KFLEKO EKGRXR QEWDNF GDNAHM GVQIHP AGYHLG GTSGBH AKCRAB QCMVRY GMGCFW VEGRD
```

Ya contando con estos bloques, analice las frecuencias de las letras en las primeras posiciones, después en las segundas, terceras y así hasta las sextas posiciones, con esta sección del código:

```python
# Agrupación por posición de caracteres en los bloques
caracteres = ['']*6
caracteres[0] = ''.join([bloque[0] for bloque in bloques])
caracteres[1] = ''.join([bloque[1] for bloque in bloques])
caracteres[2] = ''.join([bloque[2] for bloque in bloques])
caracteres[3] = ''.join([bloque[3] for bloque in bloques])
caracteres[4] = ''.join([bloque[4] for bloque in bloques])
caracteres[5] = ''.join([bloque[5] for bloque in bloques[:len(bloques)-1]])

# Frecuencias
for i in range(6):
    frecuencias = {}
    for c in caracteres[i]:
        frecuencias[c] = caracteres[i].count(c)
    print(f'Caracteres en posicion {i+1}:\n{dict(sorted(frecuencias.items(), key=lambda item: item[1], reverse=True))}\n')
```

De esa manera, obtuve los siguientes resultados:

```
Caracteres en posicion 1:
{'G': 19, 'C': 12, 'V': 11, 'E': 7, 'P': 7, 'T': 6, 'Q': 5, 'U': 5, 'N': 5, 'K': 4, 'F': 3, 'A': 3, 'R': 2, 'X': 2, 'O': 2, 'Y': 1, 'H': 1, 'I': 1, 'M': 1}

Caracteres en posicion 2:
{'V': 15, 'F': 11, 'K': 10, 'E': 9, 'Z': 7, 'I': 7, 'J': 6, 'R': 5, 'D': 4, 'B': 3, 'C': 3, 'S': 3, 'M': 2, 'L': 2, 'W': 2, 'G': 2, 'T': 2, 'N': 1, 'X': 1, 'Y': 1, 'U': 1}

Caracteres en posicion 3:
{'Q': 11, 'C': 11, 'R': 10, 'Y': 8, 'G': 8, 'A': 6, 'W': 6, 'P': 5, 'M': 5, 'B': 4, 'S': 4, 'L': 4, 'E': 4, 'J': 3, 'N': 2, 'F': 2, 'D': 1, 'T': 1, 'V': 1, 'I': 1}

Caracteres en posicion 4:
{'I': 12, 'T': 11, 'C': 9, 'G': 7, 'E': 6, 'W': 6, 'H': 6, 'D': 5, 'P': 5, 'R': 5, 'S': 4, 'X': 4, 'N': 3, 'J': 3, 'V': 3, 'B': 2, 'A': 2, 'K': 1, 'L': 1, 'Y': 1, 'U': 1}

Caracteres en posicion 5:
{'B': 14, 'X': 12, 'T': 10, 'M': 9, 'N': 7, 'H': 7, 'F': 5, 'G': 5, 'K': 5, 'V': 4, 'Y': 3, 'Z': 3, 'L': 3, 'A': 3, 'E': 1, 'P': 1, 'W': 1, 'O': 1, 'I': 1, 'R': 1, 'D': 1}

Caracteres en posicion 6:
{'B': 11, 'G': 10, 'S': 9, 'C': 9, 'F': 8, 'W': 8, 'Z': 5, 'H': 5, 'R': 4, 'Q': 4, 'V': 4, 'O': 3, 'M': 3, 'I': 2, 'A': 2, 'D': 2, 'P': 2, 'J': 1, 'T': 1, 'U': 1, 'K': 1, 'Y': 1}
```

Según la tabla de frecuencias, la letra *E* es la más frecuente en este idioma, de modo que podemos seleccionar las letras más frecuentes y evaluar la distancias entre *E* y dichas letras para obtener los caracteres que componen la llave.

```python
print(f'1 : {alfabeto[alfabeto.index('G')-alfabeto.index('E')]}')
print(f'2 : {alfabeto[alfabeto.index('V')-alfabeto.index('E')]}')
print(f'3 : {alfabeto[alfabeto.index('Q')-alfabeto.index('E')]}')
print(f'4 : {alfabeto[alfabeto.index('I')-alfabeto.index('E')]}')
print(f'5 : {alfabeto[alfabeto.index('B')-alfabeto.index('E')]}')
print(f'6 : {alfabeto[alfabeto.index('B')-alfabeto.index('E')]}')
print()
```

```
1 : C
2 : R
3 : M
4 : E
5 : X
6 : X
```

Esta llave la verdad, no tiene mucho sentido. Esto puede ser por la asunción errónea de que estos caracteres equivalen a la letra *E*.
Lo que podemos hacer en donde sea necesario y **probable**, por ejemplo donde las frecuencias más altas son similares, es seleccionar los siguientes caracteres con más frecuencia y suponer que representan a la letra *E*, esperando que la llave tenga más sentido.

Despues de muchas pruebas, llegué a esta selección de caracteres:

```python
print(f'1 : {alfabeto[alfabeto.index('G')-alfabeto.index('E')]}')
print(f'2 : {alfabeto[alfabeto.index('V')-alfabeto.index('E')]}')
# print(f'3 : {alfabeto[alfabeto.index('Q')-alfabeto.index('E')]}')
print(f'3 : {alfabeto[alfabeto.index('C')-alfabeto.index('E')]}')
# print(f'4 : {alfabeto[alfabeto.index('I')-alfabeto.index('E')]}')
print(f'4 : {alfabeto[alfabeto.index('T')-alfabeto.index('E')]}')
# print(f'5 : {alfabeto[alfabeto.index('B')-alfabeto.index('E')]}')
print(f'5 : {alfabeto[alfabeto.index('X')-alfabeto.index('E')]}')
# print(f'6 : {alfabeto[alfabeto.index('B')-alfabeto.index('E')]}')
# print(f'6 : {alfabeto[alfabeto.index('G')-alfabeto.index('E')]}')
print(f'6 : {alfabeto[alfabeto.index('S')-alfabeto.index('E')]}')
print()
```

De este modo se obtiene la llave *CRYPTO*:

```
1 : C
2 : R
3 : Y
4 : P
5 : T
6 : O
```

De este modo, ya podemos descifrar el mensaje revirtiendo la sustitución de la siguiente manera:

```python
# Llave obtenida CRYPTO
llave='CRYPTO'

# Descifrado
pos = 0
descifrado = ''
for c in criptograma:
    descifrado += alfabeto[(alfabeto.index(c)-alfabeto.index(llave[pos])) % len(alfabeto)]
    pos = (pos + 1) % 6

print(f'Mensaje en claro descifrado:\n\n{descifrado}')
```

Y así, finalmente, obtenemos el mensaje en claro:

```
TODEFEATSECURITYMEASURESANATTACKERINTRUDERORSOCIALENGINEERMUSTFINDAWAYTODECEIVEATRUSTEDUSERINTOREVEALINGINFORMATIONORTRICKANUNSUSPECTINGMARKINTOPROVIDINGHIMWITHACCESSWHENTRUSTEDEMPLOYEESAREDECEIVEDINFLUENCEDORMANIPULATEDINTOREVEALINGSENSITIVEINFORMATIONORPERFORMINGACTIONSTHATCREATEASECURITYHOLEFORTHEATTACKERTOSLIPTHROUGHNOTECHNOLOGYINTHEWORLDCANPROTECTABUSINESSJUSTASCRYPTANALYSTSARESOMETIMESABLETOREVEALTHEPLAINTEXTOFACODEDMESSAGEBYFINDINGAWEAKNESSTHATLETSTHEMBYPASSTHEENCRYPTIONTECHNOLOGYSOCIALENGINEERSUSEDECEPTIONPRACTICEDONYOUREMPLOYEESTOBYPASSSECURITYTECHNOLOGYKEVINMITNICK
```