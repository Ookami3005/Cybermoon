# Universidad Nacional Autónoma de México

## Facultad de Ciencias
## Lenguajes de programación
### Romero Cruz Fernando 319314256

### Tarea 01

1. **Sea la función `mist`definida como:**

> `mist([], ys) = ys`
> `mist((x : xs), ys) = mist(xs, (x:ys))`

**Nota**: Recuerda que la definición de lista es:

> La lista vacía `[]` es una lista
> Si `x` es un elemento y `xs` una lista, `(x:xs)` es una lista

Y la definición de la reversa es la que sigue:

> `rev([]) = []`
> `rev((x:xs)) = rev(xs)` $\cup$ `(x:[])`

#### a) ¿Qué hace `mist`?

*Concatena la reversa de la lista `xs` a la izquierda de la lista `ys`*

#### b) Demostrar que `rev(xs) = mist(xs, [])`
---
##### *Demostración auxiliar*
Primero, voy a probar con una breve inducción sobre la longitud `n` de `xs` que `mist(xs, ys) = mist(xs, [])` $\cup$ `ys` para cualquier lista `ys`.

**Caso base** `n=0` :
Por un lado `mist(xs, ys) = mist([], ys) = ys`.
Por otro, `mist(xs, [])` $\cup$ `ys = mist([], [])` $\cup$ `ys = []` $\cup$ `ys = ys`.
Por tanto, `mist([], ys) = mist([], [])` $\cup$ `ys`.

**Paso inductivo** :

*Hipótesis*: Supongamos que se cumple la igualdad para una lista `xs` de longitud `n` $\neq$ `0`.

*Por demostrar*: Que se cumple tambien para una lista `(x:xs)` de longitud `n+1`.

- Sabemos que `mist((x:xs), ys) = mist(xs, (x:ys))` por definición de `mist`.
- Por hipótesis,  `mist(xs, (x:ys)) = mist(xs, [])` $\cup$ `(x:ys)`.
- Podemos seguir desarrollando, `mist(xs, [])` $\cup$ `(x:[])` $\cup$ `ys` por propiedades de concatenación.
- Asociando, `(mist(xs, [])` $\cup$ `(x:[]))` $\cup$ `ys = mist(xs, (x:[]))` $\cup$ `ys`
- Por definición de mist, podemos simplificar como: `mist((x:xs), [])` $\cup$ `ys`

Por tanto, concluimos que `mist((x:xs), ys) = mist((x:xs), [])` $\cup$ `ys`

---
##### Demostración principal

Para la **demostración principal**, nuevamente haré inducción sobre la longitud `n` de la lista `xs`.

###### *Caso base*

Para `n=0`, implica que `xs = []`, entonces por una parte `rev(xs) = rev([]) = []`.
Analizando entonces `mist(xs, ys) = mist([], ys) = []` para cualquier lista `ys`.
Por tanto `rev(xs) = [] = mist(xs)`.

###### *Paso inductivo*

**Hipótesis inductiva**: Supondremos que `rev(xs) = mist(xs, [])` para cualquier lista `xs` de longitud `n`.

**Por demostrar**: Que se cumple tambien para una lista `(x:xs)` de longitud `n+1`.

- Sabemos que `rev((x:xs)) = rev(xs)` $\cup$ `(x:[])`.
- Entonces `= mist(xs, [])` $\cup$ `(x:[])`
- Esto último es equivalente, como probé anteriormente, a `mist(xs, (x:[]))`
- Por definición de `mist`, `mist(xs, (x:[])) = mist((x:xs), [])`

Así, podemos concluir que `rev((x:xs)) = mist((x:xs), [])`.

---

2. **Diseña una expresión regular que pueda reconocer el lenguaje de todos los números válidos en un lenguaje tipo *C*. Las reglas que cumplen los números en este lenguaje son las siguientes:**

a) Todos los números pueden ser precedidos por un $+$ o $-$.

b) Los enteros pueden tener como prefijo $0$ si está en base octal, $0x$ o $0X$ si está en base hexadecimal.

c) Los enteros en base octal solo pueden contener dígitos del 0 al 7.

d) Los enteros en base decimal solo pueden tener dígitos del 0 al 9.

e) Los enteros en base hexadecimal solo pueden tener dígitos del 0 al $F$ y las letras deben ser mayúsculas.

f) Los enteros (de cualquier base) pueden tener como sufijo **u**, **U**, **l**, **L**, **ul**, **Ul**, **UL**, **uL**, **lu**, **Lu**, **lU** o **LU**.

g) Los números con punto flotante solo pueden tener un **.** y debe haber un número antes y después de él.

h) Los números con punto flotante pueden tener notación científica; esto se denota con una **E** despues de la base, y seguida por el exponente.

i) En caso de tener una notación científica, la potencia debe ser un número entero en base decimal, puede contener (o no) signos $+$, $-$.

j) Los números de punto flotante (tengan o no notación científica), pueden tener los sufijos: **l**, **L**, **f**, **F**.

#### Definiciones

Aqui voy a definir una serie de conjuntos que utilizaré en la expresión regular de modo que sea más legible.

- $SIGN = \{+, -, \varepsilon\}$
- $N = \{0,1,2,3,4,5,6,7,8,9\}$
- $N' = N - \{0\}$
- $O = \{0,1,2,3,4,5,6,7\}$
- $O' = O-\{0\}$
- $HX = \{0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F\}$
- $HX' = HX - \{0\}$
- $U' = \{u, U, \epsilon\}$
- $L' = \{l, L, \epsilon\}$
- $F' = \{f, F, \varepsilon\}$

De igual manera propongo las siguientes expresiones regulares para los respectivos casos:

- **Enteros**: $INT = (N'\cdot N^*) + 0$
- **Base octal**: $OCT = (0 + \varepsilon)(O'\cdot O^* + 0)$
- **Base hexadecimal**: $HEX= (0x+0X+\varepsilon)(HX' \cdot HX^* + 0)$
- **Punto flotante**: $FLOAT= (INT)(\varepsilon + .N^*N')(\varepsilon + ((e + E) \cdot SIGN \cdot INT))$

---
#### Expresión

Entonces la expresión completa que define a los números del lenguaje *C* es:

$NUM = SIGN((INT + OCT+ HEX)(U'L'+L'U') + FLOAT(L' + F'))$