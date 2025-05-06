# Práctica 7: Ataque RSA por factorización
## Criptografía y seguridad 2025-2

> Fernando Romero Cruz - 319314256

### Reporte

#### Compilación de Msieve

Una vez descargado el comprimido de **Msieve**, lo extraje con el comando indicado en la práctica:

```bash
tar -xvzf msieve153_src.tar.gz
```

Navegue hacia la carpeta descomprimida, e inicie la compilación pero se me presento un problema de tipos incompatibles con la siguiente descripción:

```
common/savefile.c: In function ‘savefile_open’:
common/savefile.c:155:31: error: assignment to ‘gzFile’ {aka ‘struct gzFile_s *’} from incompatible pointer type ‘struct gzFile_s **’ [-Wincompatible-pointer-types]
  155 |                         s->fp = (gzFile *)fopen(s->name, "a");
      |                               ^
In file included from include/util.h:46,
                 from include/msieve.h:24,
                 from include/common.h:18,
                 from common/savefile.c:15:
/usr/include/zlib.h:1305:26: note: ‘gzFile’ declared here
 1305 | typedef struct gzFile_s *gzFile;    /* semi-opaque gzip file descriptor */
      |                          ^~~~~~
make: *** [Makefile:290: common/savefile.o] Error 1
```

Para solucionarlo, decidi sustituir dicha línea por la siguiente, donde esta nueva función ya devolvía el tipo esperado directamente:

```c
s->fp = gzopen(s->name, "a");
```

Y en cuanto a la instalación de la librería `libgmp-dev`, mi distribución *Linux* ya la incluía desde la instalación bajo el nombre de un paquete `gmp`. De este modo, pude pasar directamente a la compilación del programa provisto, `get_priv_key.c` mediante el comando indicado por la práctica:

```bash
gcc -I/usr/include/openssl/ get_priv_key.c -o get_priv_key -lcrypto
```

#### Obtención de llave privada RSA

Ahora, para la obtención de la llave privada, identifiqué el módulo `n` y el exponente público `e` mediante el comando `openssl` con las banderas especificadas en la práctica y obtuve la siguiente salida:

```bash
openssl rsa -pubin -in rsa_key.pub -text -modulus

# Public-Key: (256 bit)
# Modulus:
#     00:c2:85:c7:8e:2e:8e:68:b8:b5:c9:61:d7:d3:f1:
#     22:a0:d5:7d:57:e2:e6:2c:a1:dd:ea:22:a2:78:6e:
#     21:ef:2d
# Exponent: 65537 (0x10001)
# Modulus=C285C78E2E8E68B8B5C961D7D3F122A0D57D57E2E62CA1DDEA22A2786E21EF2D
# writing RSA key
# -----BEGIN PUBLIC KEY-----
# MDwwDQYJKoZIhvcNAQEBBQADKwAwKAIhAMKFx44ujmi4tclh19PxIqDVfVfi5iyh
# 3eoionhuIe8tAgMBAAE=
# -----END PUBLIC KEY-----
```

Converti con apoyo de *Python* el módulo a decimal, de modo que sabemos que `n` es igual a `87985060565507591627383267321387723045283936983187171043849596928136082222893` y `e` es igual a `65537`.

Con esta información, podemos factorizar `n` con apoyo del recién compilado `msieve`, ejecutando en la misma carpeta donde lo compilamos antes, el siguiente comando provisto:

```bash
./msieve -v 87985060565507591627383267321387723045283936983187171043849596928136082222893
```

Despues de esperar un momento, el programa arroja la siguiente salida:

```
Msieve v. 1.53 (SVN Unversioned directory)
Mon May  5 20:26:06 2025
random seeds: c49c9833 2e9dde9e
factoring 87985060565507591627383267321387723045283936983187171043849596928136082222893 (77 digits)
no P-1/P+1/ECM available, skipping
commencing quadratic sieve (77-digit input)
using multiplier of 3
using generic 32kb sieve core
sieve interval: 12 blocks of size 32768
processing polynomials in batches of 17
using a sieve bound of 920963 (36471 primes)
using large prime bound of 92096300 (26 bits)
using trial factoring cutoff of 26 bits
polynomial 'A' values have 10 factors

sieving in progress (press Ctrl-C to pause)
36664 relations (18838 full + 17826 combined from 192863 partial), need 36567
36664 relations (18838 full + 17826 combined from 192863 partial), need 36567
sieving complete, commencing postprocessing
begin with 211701 relations
reduce to 52276 relations in 2 passes
attempting to read 52276 relations
recovered 52276 relations
recovered 42447 polynomials
attempting to build 36664 cycles
found 36664 cycles in 1 passes
distribution of cycle lengths:
   length 1 : 18838
   length 2 : 17826
largest cycle: 2 relations
matrix is 36471 x 36664 (5.4 MB) with weight 1123350 (30.64/col)
sparse part has weight 1123350 (30.64/col)
filtering completed in 3 passes
matrix is 26093 x 26157 (4.2 MB) with weight 887813 (33.94/col)
sparse part has weight 887813 (33.94/col)
saving the first 48 matrix rows for later
matrix includes 64 packed rows
matrix is 26045 x 26157 (2.8 MB) with weight 646122 (24.70/col)
sparse part has weight 461024 (17.63/col)
commencing Lanczos iteration
memory use: 2.8 MB
lanczos halted after 413 iterations (dim = 26041)
recovered 16 nontrivial dependencies
p39 factor: 283551339098771439139668038887614330931
p39 factor: 310296755589855050966285335755741027103
elapsed time 00:01:25
```

Particularmente, nos interesan los factores obtenidos, `283551339098771439139668038887614330931` y `310296755589855050966285335755741027103`  que representan los primos `p` y `q` a partir de los cuales se obtuvo `n`.
Con esta información, podemos calcular el exponente privado `d` más fácilmente y así generar nuestra propia llave privada en formato *PEM*, con apoyo del programa que compilamos antes.
Esto se realiza ejecutando el binario, indicando los números `p`, `q` y `e` en ese orden:

```bash
./get_priv_key 283551339098771439139668038887614330931 310296755589855050966285335755741027103 65537 > rsa_privkey.pem
```

Para comprobar la integridad de la llave, podemos ejecutar el siguiente comando para obtener los detalles de esta:

```bash
openssl rsa -in rsa_privkey.pem -text -modulus

# Private-Key: (256 bit, 2 primes)
# modulus:
#     00:c2:85:c7:8e:2e:8e:68:b8:b5:c9:61:d7:d3:f1:
#     22:a0:d5:7d:57:e2:e6:2c:a1:dd:ea:22:a2:78:6e:
#     21:ef:2d
# publicExponent: 65537 (0x10001)
# privateExponent:
#     4f:39:b5:61:0f:4d:29:11:b1:d8:67:65:44:08:fe:
#     14:d5:68:e9:95:c5:59:a9:0c:1f:23:92:52:2d:b0:
#     c6:71
# prime1:
#     00:e9:70:fb:38:07:31:6a:51:fd:ae:16:7c:08:41:
#     5b:1f
# prime2:
#     00:d5:52:00:60:9c:01:06:76:66:60:a9:16:4b:12:
#     38:33
# exponent1:
#     12:0c:6d:2f:f0:c6:6e:4c:f6:8b:1e:2b:ea:cb:7a:
#     cb
# exponent2:
#     59:dd:be:cc:f4:4b:b6:46:40:e0:ed:ba:b7:8b:88:
#     63
# coefficient:
#     00:b8:52:6f:48:d6:82:32:ef:f7:a6:17:23:e5:a4:
#     fa:37
# Modulus=C285C78E2E8E68B8B5C961D7D3F122A0D57D57E2E62CA1DDEA22A2786E21EF2D
# writing RSA key
# -----BEGIN PRIVATE KEY-----
# MIHCAgEAMA0GCSqGSIb3DQEBAQUABIGtMIGqAgEAAiEAwoXHji6OaLi1yWHX0/Ei
# oNV9V+LmLKHd6iKieG4h7y0CAwEAAQIgTzm1YQ9NKRGx2GdlRAj+FNVo6ZXFWakM
# HyOSUi2wxnECEQDpcPs4BzFqUf2uFnwIQVsfAhEA1VIAYJwBBnZmYKkWSxI4MwIQ
# EgxtL/DGbkz2ix4r6st6ywIQWd2+zPRLtkZA4O26t4uIYwIRALhSb0jWgjLv96YX
# I+Wk+jc=
# -----END PRIVATE KEY-----
```

Con esta llave, ahora podemos descifrar el archivo cifrado con la llave pública provista, en el archivo `msj_secreto.enc` mediante el siguiente comando para obtener un nuevo archivo en texto plano.

```bash
openssl pkeyutl -decrypt -inkey rsa_privkey.pem -in msj_secreto.enc -out msj_secreto.txt
```

El mensaje descifrado dice: ***Thenemyknwsthesystm.***

#### Generación y uso de llaves RSA