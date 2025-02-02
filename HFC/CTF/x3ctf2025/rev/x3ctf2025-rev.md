[<- Volver](../x3ctf2025.md)
# Desafios de categoría `rev`

### `notcrypto`

> Este reto consistió de realizar **ingeniería inversa** a un binario `spn`, que recibe un cadena y de alguna manera verifica si esta es la bandera.

***Binario***: https://github.com/x3ctf/challenges-2025/raw/refs/heads/main/rev/notcrypto/challenge-src/spn

Por ejemplo, al introducir la cadena `Hola`.

```shell-session
$ ./spn

Hola
Wronk flag methinks.
```

Para empezar, en mi caso, realicé un desensamblado mediante `radare2`, un decompilado mediante `ghidra` y una depuración de algunas secciones mediante `gdb` junto con la extensión `pwndbg`.

##### Decompilado

Bajo un riguroso análisis y renombrado de variables sobre el código decompilado, el siguiente fragmento de código destaca sobre los demás.

```c
/* Checks that password is at least 38 hex long */
if (bVar == 0x38) {
    local_38 = user_flag;
    octet_start = 0;
    while( true ) {
    /* Retrieve the password octet */
		bVar13 = user_flag[octet_start];
	    bVar10 = user_flag[octet_start + 1];
	    bVar8 = user_flag[octet_start + 2];
	    bVar4 = user_flag[octet_start + 3];
	    bVar7 = user_flag[octet_start + 4];
	    bVar9 = user_flag[octet_start + 5];
	    bVar14 = user_flag[octet_start + 6];
	    bVar5 = user_flag[octet_start + 7];
	    i = 0;
    do {
        /* Starts the encryption of the octet */
        uVar12 = (ulong)bVar13;
        bVar = (ulong)bVar10;
        uVar6 = (ulong)bVar8;
        uVar3 = (ulong)bVar7;
        iterator_byte = (byte)i;
        /* Shuffle bytes order and xor it */
        bVar13 = (&byte_array)[bVar14] ^ iterator_byte;
        bVar10 = (&byte_array)[uVar12] ^ iterator_byte;
        bVar8 = (&byte_array)[bVar5] ^ iterator_byte;
        bVar4 = (&byte_array)[bVar4] ^ iterator_byte;
        bVar7 = (&byte_array)[bVar] ^ iterator_byte;
        bVar9 = (&byte_array)[bVar9] ^ iterator_byte;
        bVar14 = (&byte_array)[uVar6] ^ iterator_byte;
        bVar5 = (&byte_array)[uVar3] ^ iterator_byte;
        i = i + 1;
        /* Repeats 1000 hex times this process */
	} while (i != 0x1000);


    /* Checks for expected octet */
    if (((((bVar13 != (&DAT_00102010)[octet_start]) || (bVar10 != (&DAT_00102011)[octet_start]))
	    || (bVar8 != (&DAT_00102012)[octet_start])) ||
	    ((bVar4 != (&DAT_00102013)[octet_start] || (bVar7 != (&DAT_00102014)[octet_start])))) ||
	    ((bVar9 != (&DAT_00102015)[octet_start] ||
	    ((bVar14 != (&DAT_00102016)[octet_start] || (bVar5 != (&DAT_00102017)[octet_start]))))))
	    break;

		/* Sets next octet start */
		octet_start = octet_start + 8;
		if (0x37 < octet_start) {
			puts("Correct flag methinks.");
	        exit(0);
	    }
	}
}
puts("Wronk flag methinks.");
exit(1);
```

##### Desensamblado

La correspondiente sección del desensamblado es:

```arm-asm
0x0000000000001484:	cmp    QWORD PTR [rsp+0x8],0x38 ; Checks length
0x000000000000148a:	jne    0x15b8
0x0000000000001490:	mov    rax,QWORD PTR [rsp]
0x0000000000001494:	mov    QWORD PTR [rsp+0x20],rax
0x0000000000001499:	xor    ecx,ecx
0x000000000000149b:	lea    rdx,[rip+0x2bae]
0x00000000000014a2:	data16 data16 data16 data16 cs nop WORD PTR [rax+rax*1+0x0]
0x00000000000014b0:	mov    rax,QWORD PTR [rsp+0x20]
0x00000000000014b5:	movzx  r14d,BYTE PTR [rax+rcx*1]
0x00000000000014ba:	movzx  r10d,BYTE PTR [rax+rcx*1+0x1]
0x00000000000014c0:	movzx  r8d,BYTE PTR [rax+rcx*1+0x2]
0x00000000000014c6:	movzx  ebx,BYTE PTR [rax+rcx*1+0x3]
0x00000000000014cb:	movzx  edi,BYTE PTR [rax+rcx*1+0x4]
0x00000000000014d0:	movzx  r9d,BYTE PTR [rax+rcx*1+0x5]
0x00000000000014d6:	movzx  r15d,BYTE PTR [rax+rcx*1+0x6]
0x00000000000014dc:	movzx  ebp,BYTE PTR [rax+rcx*1+0x7]
0x00000000000014e1:	xor    r11d,r11d
0x00000000000014e4:	data16 data16 cs nop WORD PTR [rax+rax*1+0x0]
0x00000000000014f0:	movzx  r15d,r15b
0x00000000000014f4:	movzx  r12d,r14b
0x00000000000014f8:	movzx  r13d,bpl
0x00000000000014fc:	movzx  ebx,bl
0x00000000000014ff:	movzx  ebp,r10b
0x0000000000001503:	movzx  r9d,r9b
0x0000000000001507:	movzx  esi,r8b
0x000000000000150b:	movzx  eax,dil
0x000000000000150f:	movzx  r14d,BYTE PTR [r15+rdx*1]
0x0000000000001514:	xor    r14b,r11b
0x0000000000001517:	movzx  r10d,BYTE PTR [r12+rdx*1]
0x000000000000151c:	xor    r10b,r11b
0x000000000000151f:	movzx  r8d,BYTE PTR [r13+rdx*1+0x0]
0x0000000000001525:	xor    r8b,r11b
0x0000000000001528:	movzx  ebx,BYTE PTR [rbx+rdx*1]
0x000000000000152c:	xor    bl,r11b
0x000000000000152f:	movzx  edi,BYTE PTR [rbp+rdx*1+0x0]
0x0000000000001534:	xor    dil,r11b
0x0000000000001537:	movzx  r9d,BYTE PTR [r9+rdx*1]
0x000000000000153c:	xor    r9b,r11b
0x000000000000153f:	movzx  r15d,BYTE PTR [rsi+rdx*1]
0x0000000000001544:	xor    r15b,r11b
0x0000000000001547:	movzx  ebp,BYTE PTR [rax+rdx*1]
0x000000000000154b:	xor    bpl,r11b
0x000000000000154e:	inc    r11d
0x0000000000001551:	cmp    r11d,0x1000
0x0000000000001558:	jne    0x14f0
0x000000000000155a:	lea    rax,[rip+0xaaf]
0x0000000000001561:	cmp    r14b,BYTE PTR [rcx+rax*1]
0x0000000000001565:	jne    0x15b8
0x0000000000001567:	cmp    r10b,BYTE PTR [rcx+rax*1+0x1]
0x000000000000156c:	jne    0x15b8
0x000000000000156e:	cmp    r8b,BYTE PTR [rcx+rax*1+0x2]
0x0000000000001573:	jne    0x15b8
0x0000000000001575:	cmp    bl,BYTE PTR [rcx+rax*1+0x3]
0x0000000000001579:	jne    0x15b8
0x000000000000157b:	cmp    dil,BYTE PTR [rcx+rax*1+0x4]
0x0000000000001580:	jne    0x15b8
0x0000000000001582:	cmp    r9b,BYTE PTR [rcx+rax*1+0x5]
0x0000000000001587:	jne    0x15b8
0x0000000000001589:	cmp    r15b,BYTE PTR [rcx+rax*1+0x6]
0x000000000000158e:	jne    0x15b8
0x0000000000001590:	cmp    bpl,BYTE PTR [rcx+rax*1+0x7]
0x0000000000001595:	jne    0x15b8
0x0000000000001597:	add    rcx,0x8
0x000000000000159b:	cmp    rcx,0x38
0x000000000000159f:	jb     0x14b0
0x00000000000015a5:	lea    rdi,[rip+0xab1]
0x00000000000015ac:	call   0x1070 <puts@plt>
0x00000000000015b1:	xor    edi,edi
0x00000000000015b3:	call   0x1050 <exit@plt>
0x00000000000015b8:	lea    rdi,[rip+0xa89]
0x00000000000015bf:	call   0x1070 <puts@plt>
0x00000000000015c4:	mov    edi,0x1
0x00000000000015c9:	call   0x1050 <exit@plt>
```

##### Solución

Con el correspondiente **análisis estático** y **dinámico** del binario, se deduce que el binario repite un proceso de "encriptación" que consta de recuperar de un arreglo de *bytes* el índice correspondiente al ==valor hexadecimal de un caracter de la contraseña==, realizar una operación *XOR* con él y los últimos 2 digitos del iterador, y almacenar este resultado como el siguiente índice a consultar.
Esto repetido 1000 veces por cada caracter de un octeto de *bytes* de la cadena recibida, para posteriormente, revisar que cada caracter del octeto encriptado sea el esperado y continuar este proceso con los demás octetos hasta revisar toda la contraseña.

Adicionalmente, en alguna parte de este cifrado se alteran las posiciones de los caracteres originales.

Con esto en mente, con el arreglo de *bytes* y con todos los *bytes* que se esperan de la contraseña encriptada, podemos revertir este proceso de cifrado para obtener los caracteres originales de cada octeto, y reordenar estos a sus posiciones originales.

```python
#!//usr/bin/env python

# x3CTF 2025
# NotCrypto: https://github.com/x3ctf/challenges-2025/tree/main/rev/notcrypto

"""
My solution
"""

# ------------------
# Ookami
# Hackers Fight Club
# ------------------

# Expected bytes
encrypt_flag=b"\x16\x2d\x79\xca\x56\xc6\x65\xe9\xe9\x16\x66\x23\x09\x2d\x1b\x09\x1c\x09\xc6\x1c\x1f\xad\xe9\xda\xa0\xc6\x1a\x66\x09\xad\x81\x1c\x80\x39\xa0\x21\x09\x65\x2d\x30\xf6\x57\xf6\xa2\x65\x65\x21\xa2"

# Byte array for encryption
byte_array=b'c|w{\xf2ko\xc50\x01g+\xfe\xd7\xabv\xca\x82\xc9}\xfaYG\xf0\xad\xd4\xa2\xaf\x9c\xa4r\xc0\xb7\xfd\x93&6?\xf7\xcc4\xa5\xe5\xf1q\xd81\x15\x04\xc7#\xc3\x18\x96\x05\x9a\x07\x12\x80\xe2\xeb\'\xb2u\t\x83,\x1a\x1bnZ\xa0R;\xd6\xb3)\xe3/\x84S\xd1\x00\xed \xfc\xb1[j\xcb\xbe9JLX\xcf\xd0\xef\xaa\xfbCM3\x85E\xf9\x02\x7fP<\x9f\xa8Q\xa3@\x8f\x92\x9d8\xf5\xbc\xb6\xda!\x10\xff\xf3\xd2\xcd\x0c\x13\xec_\x97D\x17\xc4\xa7~=d]\x19s`\x81O\xdc"*\x90\x88F\xee\xb8\x14\xde^\x0b\xdb\xe02:\nI\x06$\\\xc2\xd3\xacb\x91\x95\xe4y\xe7\xc87m\x8d\xd5N\xa9lV\xf4\xeaez\xae\x08\xbax%.\x1c\xa6\xb4\xc6\xe8\xddt\x1fK\xbd\x8b\x8ap>\xb5fH\x03\xf6\x0ea5W\xb9\x86\xc1\x1d\x9e\xe1\xf8\x98\x11i\xd9\x8e\x94\x9b\x1e\x87\xe9\xceU(\xdf\x8c\xa1\x89\r\xbf\xe6BhA\x99-\x0f\xb0T\xbb\x16'

# Reverse the cipher process
def decipher_char(c):
    global byte_array
    index=ord(c)
    for i in range(0xfff,-1,-1):
        index = byte_array.index(index ^ (i & 0xff))
    return chr(index)

# Reorder the retrieved flag
def reorder_flag(disordered_flag):
    flag=['' for _ in disordered_flag]
    for i in range(0,len(encrypt_flag),8):
        flag[i]=disordered_flag[i+2]
        flag[i+1]=disordered_flag[i+6]
        flag[i+2]=disordered_flag[i+4]
        flag[i+3]=disordered_flag[i+3]
        flag[i+4]=disordered_flag[i]
        flag[i+5]=disordered_flag[i+5]
        flag[i+6]=disordered_flag[i+7]
        flag[i+7]=disordered_flag[i+1]
    return flag

# Output the decrypted flag
print('Flag: '+''.join([decipher_char(chr(c)) for c in reorder_flag(encrypt_flag)]))
```

### `pickle-season`

> En este reto, se nos brindaba un *Script* en *Python* que reconstruye un archivo binario comprensible por el módulo `pickle` de este mismo lenguaje.
> Este binario, una vez ejecutado, espera una entrada y revisa de alguna manera si es la bandera de este reto.
> Claramente, el desafío es realizar **ingeniería inversa** sobre este binario tan atípico y determinar la bandera.

***Script inicial***: https://raw.githubusercontent.com/x3ctf/challenges-2025/refs/heads/main/rev/pickle-season/challenge-src/challenge.py

```python
import pickle

data = "8004637379730a6d6f64756c65730a8c017494636275696c74696e730a747970650a8c00297d87529473304e9430636275696c74696e730a7072696e740a9470320a68014e7d8c0162946802738662303063740a622e5f5f73656c665f5f0a70320a68014e7d680468027386623030284b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004b004ad6ffffff6c70320a68014e7d8c0169946802738662303063740a692e657874656e640a63740a622e6d61700a63740a622e6f72640a63740a622e696e7075740a8c06466c61673f20855286528552307d4da2027d4dcc027d4dfc027d4d8f027d4dbb027d4d88027d4dfb027d4da4027d4d97027d4dfb027d4d90027d4df3027d4dc2027d4d92027d4dcd027d4da3027d4d92027d4dcd027d4da0027d4d90027d4df4027d4dc7027d4db5027d4d85027d4dc7027d4dbc027d4df1027d4da7027d4dea027d4e8c08436f72726563742173737373737373737373737373737373737373737373737373737373737370320a68014e7d8c0164946802738662303068008c05642e676574948c05692e706f70948c09782e5f5f786f725f5f94303030304ddf0270320a68014e7d8c017894680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d6806680273866230306800680793680068099368006808932952855270320a68014e7d680a6802738662307d865270320a68014e7d680668027386623030680368006807934e8c0b57726f6e672e2e2e203a28865285522e"
pickle.loads(bytes.fromhex(data))
```

Podemos ver que el binario esta representado mediante una cadena de dígitos **hexadecimales**, que posteriormente son interpretados por la función `bytes.fromhex()` para reconstruir el archivo y cargarlo a `pickle`.

Podemos extraer esta cadena del *Script* ya sea manualmente o de cualquier otra forma elegante y despues interpretarla como los *bytes* de un archivo con la herramienta de tu preferencia.

Por ejemplo, con la terminal:

```bash
grep -oE '".*"' challenge.py | cut -d \" -f 2 | xxd -p -r > pickle.bin
```

Ahora con el archivo binario, podemos decompilarlo con apoyo de algún decompilador de `pickle`, el más útil (y pareciera que el único) es `fickling`.
En mi caso, tuve algunas complicaciones que pude solucionar mediante los siguientes comandos en la misma carpeta del desafío.

```bash
pipenv install setuptools
pipenv install fickling
pipenv shell
```

Esto crea un archivo Pipfile y abre un ambiente virtual, del que nos podemos salir mediante `ctrl+d`, donde ya podemos utilizar `fickling` sin problema sobre nuestro archivo extraído anteriormente.

```bash
fickling pickle.bin

# from sys import modules
# _var0 = type('', (), {})
# _var1 = modules
# _var1['t'] = _var0
# _var2 = _var0
# _var2.__setstate__((None, {'b': print}))
# from t import b.__self__
# _var3 = _var0
# _var3.__setstate__((None, {'b': b.__self__}))
# _var4 = _var0
# _var4.__setstate__((None, {'i': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, -42]}))
# from t import i.extend
# from t import b.map
# from t import b.ord
# from t import b.input
# _var5 = b.input('Flag? ')
# _var6 = b.map(b.ord, _var5)
# _var7 = i.extend(_var6)
# _var8 = _var0
# _var8.__setstate__((None, {'d': {674: {716: {764: {655: {699: {648: {763: {676: {663: {763: {656: {755: {706: {658: {717: {675: {658: {717: {672: {656: {756: {711: {693: {645: {711: {700: {753: {679: {746: {None: 'Correct!'}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}))
# _var9 = _var0
# _var9.__setstate__((None, {'x': 735}))
# from t import d.get
# from t import x.__xor__
# from t import i.pop
# ...
```

Esto nos brinda un código ofuscado en *Python*, que con apoyo de tu *IA* favorita, se puede desofuscar a un *script* similar a:

```python
from sys import modules

def check_flag(flag):
    key = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, -42]
    key.extend(map(ord, flag))
    
    tree = {
        674: {716: {764: {655: {699: {648: {763: {676: {663: {763: {656: {755: {706: {658: {717: {675: {658: {717: {672: {656: {756: {711: {693: {645: {711: {700: {753: {679: {746: {None: 'Correct!'}}}}}}}}}}}}}}}}}}}}}}}}}}}}}
    }
    
    x = 735
    for _ in range(len(flag)):
        last = key.pop()
        x ^= last
        tree = tree.get(x, {})
    
    print(tree.get(None, 'Wrong... :('))

if __name__ == "__main__":
    user_input = input("Flag? ")
    check_flag(user_input)
```

Ahora si conocemos el funcionamiento del binario `pickle`, en pocas palabras, revisa los caracteres de la bandera en **orden inverso** mediante una operación *XOR*.
Es decir, el valor númerico del último caracter y el número 735 se operan con un *XOR* donde se espera que el resultado sea 674 para poder recuperar el siguiente diccionario anidado, y así repetidamente hasta llegar al mensaje `Correct!`.

Si en algún momento las llaves no coinciden, se comienza a iterar sobre diccionarios vacíos de modo que nunca llegaríamos al mensaje esperado.

##### Solución

Para reconstruir la bandera, basta con encontrar los números que al operar la llave actual con ellos resulta en la llave siguiente.
Esto es fácil por las propiedades invertibles de la operación *XOR*, de modo que solo debemos operar indices consecutivos e ir almacenando los resultados en orden inverso.

```python
#!/usr/bin/env python

# x3CTF 2025
# Pickle-Season: https://github.com/x3ctf/challenges-2025/tree/main/rev/pickle-season

"""
Solution
"""

# ------------------
# Ookami
# Hackers Fight Club
# ------------------

# Dictionary keys to access
keys=[735, 674, 716, 764, 655, 699, 648, 763, 676, 663, 763, 656, 755, 706, 658, 717, 675, 658, 717, 672, 656, 756, 711, 693, 645, 711, 700, 753, 679, 746]

# Flag recreation
flag=''
for i in range(0,len(keys)-1):
    flag = chr(keys[i] ^ keys[i+1]) + flag

# Output flag
print(flag)
```

# Enlaces

[<- web](x3ctf2025-web.md) | [crypto ->](x3ctf2025-crypto.md)