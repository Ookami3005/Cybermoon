[<- Volver](../x3ctf2025.md)
# Desafios de categoría `rev`

### `notcrypto`

> Este reto consistió de realizar **ingeniería inversa** a un binario `spn`, que recibe un cadena y de alguna manera verifica si esta es la bandera.

**Binario**: https://github.com/x3ctf/challenges-2025/raw/refs/heads/main/rev/notcrypto/challenge-src/spn

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

Con esto en mente, con el arreglo de *bytes* y con todos los *bytes* que se esperan de la contraseña encriptada, podemos hacer nuestra propia versión del cifrado del binario y descubrir los caractéres que, encriptados bajo este proceso, resultan en el *byte* que el binario espera.

```python
# Ookami
# HFC

# Expected bytes
encrypt_flag=b"\x16\x2d\x79\xca\x56\xc6\x65\xe9\xe9\x16\x66\x23\x09\x2d\x1b\x09\x1c\x09\xc6\x1c\x1f\xad\xe9\xda\xa0\xc6\x1a\x66\x09\xad\x81\x1c\x80\x39\xa0\x21\x09\x65\x2d\x30\xf6\x57\xf6\xa2\x65\x65\x21\xa2"

# Byte array for encryption
byte_array=b"c|w{\xf2ko\xc50\x01g+\xfe\xd7\xab\x76\xca\x82\xc9}\xfaYG\xf0\xad\xd4\xa2\xaf\x9c\xa4r\xc0\xb7\xfd\x93&6?\xf7\xcc4\xa5\xe5\xf1q\xd81\x15\x04\xc7#\xc3\x18\x96\x05\x9a\x07\x12\x80\xe2\xeb'\xb2u\x09\x83,\x1a\x1bnZ\xa0R;\xd6\xb3)\xe3/\x84S\xd1\x00\xed\x20\xfc\xb1\x5b\x6a\xcb\xbe\x39\x4a\x4c\x58\xcf\xd0\xef\xaa\xfb\x43\x4d\x33\x85\x45\xf9\x02\x7f\x50\x3c\x9f\xa8\x51\xa3\x40\x8f\x92\x9d\x38\xf5\xbc\xb6\xda\x21\x10\xff\xf3\xd2\xcd\x0c\x13\xec\x5f\x97\x44\x17\xc4\xa7\x7e\x3d\x64\x5d\x19\x73\x60\x81\x4f\xdc\x22\x2a\x90\x88\x46\xee\xb8\x14\xde\x5e\x0b\xdb\xe0\x32\x3a\x0a\x49\x06\x24\x5c\xc2\xd3\xac\x62\x91\x95\xe4\x79\xe7\xc8\x37\x6d\x8d\xd5\x4e\xa9\x6c\x56\xf4\xea\x65\x7a\xae\x08\xba\x78\x25\x2e\x1c\xa6\xb4\xc6\xe8\xdd\x74\x1f\x4b\xbd\x8b\x8a\x70\x3e\xb5\x66\x48\x03\xf6\x0e\x61\x35\x57\xb9\x86\xc1\x1d\x9e\xe1\xf8\x98\x11\x69\xd9\x8e\x94\x9b\x1e\x87\xe9\xce\x55\x28\xdf\x8c\xa1\x89\x0d\xbf\xe6\x42\x68\x41\x99\x2d\x0f\xb0\x54\xbb\x16"

# Simulated encryption function
def transforma_caracter(c):
    global byte_array
    index = c
    for i in range(16):
        for j in range(256):
            index = byte_array[index] ^ j
    return index

result=[]

# Brute force on expected chars
for elem in encrypt_flag:
    for i in range(256):
        encrypt_char=transforma_caracter(i)
        if encrypt_char == elem:
            result.append(chr(i))
            break

ordered=['' for _ in result]

# Reorder flag
for i in range(0,len(result),8):
    ordered[i]=result[i+2]
    ordered[i+1]=result[i+6]
    ordered[i+2]=result[i+4]
    ordered[i+3]=result[i+3]
    ordered[i+4]=result[i]
    ordered[i+5]=result[i+5]
    ordered[i+6]=result[i+7]
    ordered[i+7]=result[i+1]


print('Disordered flag: '+''.join(result))
print()
print('Right flag: '+''.join(ordered))
```