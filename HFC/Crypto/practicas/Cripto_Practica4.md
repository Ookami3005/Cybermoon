# Práctica 4
## Criptografía y Seguridad - 2025-2

> Fernando Romero Cruz - 319314256

### Reporte

Para esta práctica, investigué el procedimiento estándar de un **ataque de oráculo de padding** y pude entender que debía hacer lo siguiente:

1. Alterar el último *byte* del **penúltimo bloque** ($C_{n-1}$) hasta que el oráculo indique un *padding* correcto
2. Esto indica que nuestro *byte* $b$ al operarlo mediante **xor** con un *byte* secreto $b'$ se obtiene ==en el texto plano== un *padding* de $0x1$, por eso el oráculo indica un *padding* correcto.
3. Podemos recuperar el *byte* secreto $b'$ gracias a la propiedad inversa del **xor**, operando: $b \; \oplus \; 0x1$. Este *byte* secreto no es más que el último *byte* resultante de aplicar la función de descifrado al **último bloque** ($D(C_{n})$).
4. Podemos repetir este proceso para descubrir todos los *bytes* del descifrado ($D(C_{n})$), pero tenemos que adecuar correspondientemente los *bytes* que ya conocemos para que al realizar un **xor**, resulten en el *byte* adecuado, ya sea $0x2$, $0x3$, etc. Esto lo podemos realizar una vez más gracias a la propiedad inversa del **xor**.
5. Ya que tengamos todo el **descifrado** del último bloque ($D(C_{n})$), recordemos que este no es el texto plano que buscamos, aun debemos revertir el *xor* aplicado sobre este con el bloque **cifrado** anterior ($C_{n-1}$), es decir, $C_{n-1} \oplus D(C_{n})$.

Con esta idea en mente, puse manos a la obra para recrear este procedimiento en un *script* de *Python*, me dispuse a pulirlo, agregar verbosidad para que imprima más información en pantalla cuando se le indique y optimicé las solicitudes lo más que pude:

```python
#!/usr/bin/env python3
from signal import signal,SIGINT
from requests import Session
from pwn import xor
from argparse import ArgumentParser

parser = ArgumentParser()
parser.add_argument('-v','--verbosity',dest='verbosity',action='store_true',help='Activa la verbosidad')
args = parser.parse_args()

url = 'http://64.227.29.191/validador.cbc'
hex_cipher = '82a6f4f7a60f9798f167bf61232d7754824d8538ded42f7e4a53915327c07456'
hex_blocks = [hex_cipher[i:i+32] for i in range(0,len(hex_cipher),32)]
raw_blocks = [bytes.fromhex(b) for b in hex_blocks]

def handler(sig,frame):
    print()
    print("\n[!] Ataque detenido")
    quit()

def reassemble_ciphertext(blocks):
    return bytes.hex(b''.join(blocks))

def bruteforce_padding(blocks,fake_iv):
    penultimo,ultimo = blocks
    current_index = len(fake_iv)+1
    response = None
    with Session() as session:
        print('\r[*] Realizando ataque de fuerza bruta sobre el padding...', end='')
        for i in range(256):
            if i % 2 == 0:
                print('\r[+] Realizando ataque de fuerza bruta sobre el padding...',flush=True,end='')
            elif i % 3 == 1:
                print('\r[-] Realizando ataque de fuerza bruta sobre el padding...',flush=True,end='')
            else:
                print('\r[*] Realizando ataque de fuerza bruta sobre el padding...',flush=True,end='')
            payload = penultimo[:len(penultimo)-len(fake_iv)-1]+bytes([i])+b''.join(bytes([b ^ current_index]) for b in fake_iv)
            cipher = reassemble_ciphertext([payload,ultimo])
            if args.verbosity:
                print(f'\rProbando con el texto cifrado: {cipher}',end='',flush=True)
            response = session.get(url,
                                    params={'ciphertext':cipher})
            if 'OK' in response.text:
                new_byte = i ^ current_index
                if args.verbosity:
                    print()
                    print(f'Nuevo byte descubierto: {hex(new_byte)}')
                    print()
                fake_iv = [new_byte] + fake_iv
                return fake_iv

def main():
    signal(SIGINT,handler)
    iv = []
    print(f'Oraculo seleccionado => {url}')
    print()
    for _ in range(16):
        iv = bruteforce_padding((raw_blocks[0],raw_blocks[1]),iv)
    original_block = bytes.fromhex(hex_cipher[:32])
    plaintext = b''
    print()
    if iv is not None:
        if args.verbosity:
            print('Operando el bloque cifrado anterior y el bloque descifrado obtenido con xor... ')
            print(f'{hex_cipher[:32]} ^ {bytes.hex(b''.join([bytes([n]) for n in iv]))}')
        plaintext = xor(original_block,b''.join([bytes([b]) for b in iv]))
    print()
    print(f'Fragmento de texto plano recuperado: {plaintext.decode()}')

if __name__ == "__main__":
    main()
```

Aunque mi idea procedimiento si era adecuado, presente un ***gran problema***, olvidé que un bloque de *AES-128* eran 16 caracteres regulares (o *bytes*) **o 32 caracteres hexadecimales**, ya que en la representación hexadecimal, 2 dígitos hexadecimales representan un *bytes*.
Entonces mucho tiempo estuve batallando y depurando la lógica de mi programa sin darme cuenta que el error residia desde el inicio en la segmentación del criptograma en la línea `hex_blocks = [hex_cipher[i:i+32] for i in range(0,len(hex_cipher),32)]`, donde en lugar de 32, tenía un 16.

En fin, aprendí a tomar en cuenta esos detalles iniciales desde el diseño y depuración de mi código.

### Cuestionario

1. ¿Cuál es el mensaje parcial que pudo recuperar al realizar el ataque?

> Se recupera el fragmento del mensaje `rc3_be_w1th_y0u\x01`, donde el último caracter es el *padding* del mensaje.

2. ¿Qué información adicional puede deducir sobre el mensaje original?

> Se puede deducir que el mensaje es una variante de la frase `may_the_force_be_with_you` con algunos caracteres sustituidos por números de apariencia similar.

3. ¿Cómo puede evitarse este tipo de ataques?

> Primeramente, evitar a toda costa la retroalimentación a un usuario acerca del *padding* de su mensaje o demás detalles. Sin embargo, si asi se requiere, podríamos implementar algún mecanismo de autenticación sobre el remitente de los criptogramas o algun otro control de integridad sobre el mensaje, como un *checksum*.