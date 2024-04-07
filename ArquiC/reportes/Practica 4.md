# Práctica 4
## Integrantes

- Márquez Corona Danna Lizette 320279991
- Romero Cruz Fernando 319314256

## Expresiones

1. $\overline{AB}C\overline{D} + \overline{A}B\overline{CD} + \overline{A}BC\overline{D}+A\overline{BCD} + A\overline{B}C\overline{D} + AB\overline{CD} + ABC\overline{D}$

- Tabla de verdad

| A   | B   | C   | D   | ***F*** |
| --- | --- | --- | --- | ------- |
| 0   | 0   | 0   | 0   | 0       |
| 0   | 0   | 0   | 1   | 0       |
| 0   | 0   | 1   | 0   | 1       |
| 0   | 0   | 1   | 1   | 0       |
| 0   | 1   | 0   | 0   | 1       |
| 0   | 1   | 0   | 1   | 0       |
| 0   | 1   | 1   | 0   | 0       |
| 0   | 1   | 1   | 1   | 1       |
| 1   | 0   | 0   | 0   | 1       |
| 1   | 0   | 0   | 1   | 0       |
| 1   | 0   | 1   | 0   | 1       |
| 1   | 0   | 1   | 1   | 0       |
| 1   | 1   | 0   | 0   | 1       |
| 1   | 1   | 0   | 1   | 0       |
| 1   | 1   | 1   | 0   | 1       |
| 1   | 1   | 1   | 1   | 0       |

- Minimización por mapas de Karnaugh

| AB/CD | 00      | 01  | 11   | 10      |
| ----- | ------- | --- | ---- | ------- |
| 00    | 0       | 0   | 0    | 1(b)    |
| 01    | 1(d)    | 0   | 1(c) | 0       |
| 11    | 1(a)(d) | 0   | 0    | 1(a)    |
| 10    | 1(a)    | 0   | 0    | 1(a)(b) |

Entonces obtenemos la expresión minimizada:
$$A\overline{D} + \overline{B}C\overline{D} + \overline{A}BCD + B\overline{CD}$$

- Minimización por equivalencias lógicas

$$\equiv A\overline{D}(B\overline{C} + \overline{BC} + BC + \overline{B}C) + \overline{B}C\overline{D}(A+\overline{A}) + B\overline{CD}(A+\overline{A}) + \overline{A}BCD$$
$$\equiv A\overline{D}(B(C+\overline{C}) + \overline{B}(C+\overline{C})) + \overline{B}C\overline{D}(1) + B\overline{CD}(1) + \overline{A}BCD$$
$$\equiv A\overline{D} + \overline{B}C\overline{D} + \overline{A}BCD + B\overline{CD}$$

2. $\overline{AB}C + \overline{A}BC + A\overline{B}C + AB\overline{C} + ABC$

- Tabla de verdad

| A   | B   | C   | ***F*** |
| --- | --- | --- | ------- |
| 0   | 0   | 0   | 0       |
| 0   | 0   | 1   | 1       |
| 0   | 1   | 0   | 0       |
| 0   | 1   | 1   | 1       |
| 1   | 0   | 0   | 0       |
| 1   | 0   | 1   | 1       |
| 1   | 1   | 0   | 1       |
| 1   | 1   | 1   | 1       |

- Minimización por mapa de Karnaugh

| A/BC | 00  | 01   | 11      | 10   |
| ---- | --- | ---- | ------- | ---- |
| 0    | 0   | 1(a) | 1(a)    | 0    |
| 1    | 0   | 1(a) | 1(a)(b) | 1(b) |

Entonces obtenemos la expresión minimizada:
$$C + AB$$

- Minimización por equivalencias lógicas

$$\equiv C(\overline{AB} + \overline{A}B + A\overline{B} + AB) + AB(C + \overline{C})$$
$$\equiv C(\overline{A}(B + \overline{B}) + A(B + \overline{B})) + AB(1)$$
$$C(A + \overline{A}) + AB \equiv C + AB$$

3. $\overline{A}B\overline{C} + A\overline{BC} + A\overline{B}C + AB\overline{C}$

- Tabla de verdad

| A   | B   | C   | ***F*** |
| --- | --- | --- | ------- |
| 0   | 0   | 0   | 0       |
| 0   | 0   | 1   | 0       |
| 0   | 1   | 0   | 1       |
| 0   | 1   | 1   | 0       |
| 1   | 0   | 0   | 1       |
| 1   | 0   | 1   | 1       |
| 1   | 1   | 0   | 1       |
| 1   | 1   | 1   | 0       |

- Minimización por mapa de Karnaugh

| A/BC | 00   | 01   | 11  | 10   |
| ---- | ---- | ---- | --- | ---- |
| 0    | 0    | 0    | 0   | 1(b) |
| 1    | 1(a) | 1(a) | 0   | 1(b) |

Entonces obtenemos la expresión minimizada:
$$A\overline{B} + B\overline{C}$$

- Minimización por equivalencias lógicas

$$\equiv A\overline{B}(C + \overline{C}) + B\overline{C}(A + \overline{A})$$
$$A\overline{B} + B\overline{C}$$

4. $\overline{A}BC + A\overline{B}C + AB\overline{C} + ABC$

| A   | B   | C   | ***F*** |
| --- | --- | --- | ------- |
| 0   | 0   | 0   | 0       |
| 0   | 0   | 1   | 0       |
| 0   | 1   | 0   | 0       |
| 0   | 1   | 1   | 1       |
| 1   | 0   | 0   | 0       |
| 1   | 0   | 1   | 1       |
| 1   | 1   | 0   | 1       |
| 1   | 1   | 1   | 1       |

- Minimización por mapa de Karnaugh

| A/BC | 00  | 01   | 11         | 10   |
| ---- | --- | ---- | ---------- | ---- |
| 0    | 0   | 0    | 1(b)       | 0    |
| 1    | 0   | 1(a) | 1(a)(b)(c) | 1(c) |

Entonces obtenemos la expresión minimizada:
$$AC + BC + AB$$

- Minimización por equivalencias lógicas

$$\equiv AC(B + \overline{B}) + BC(A + \overline{A}) + AB(C + \overline{C})$$
$$\equiv AC + BC + AB$$

5. $ABCDE + ABCD\overline{E} + \overline{A}BCDE + \overline{A}BCD\overline{E} + A\overline{BC}DE + A\overline{BC}D\overline{E} + A\overline{B}CDE + A\overline{B}CD\overline{E}$

- Tabla de verdad

| A   | B   | C   | D   | E   | ***F*** |
| --- | --- | --- | --- | --- | ------- |
| 1   | 1   | 1   | 1   | 1   | 1       |
| 1   | 1   | 1   | 1   | 0   | 1       |
| 0   | 1   | 1   | 1   | 1   | 1       |
| 0   | 1   | 1   | 1   | 0   | 1       |
| 1   | 0   | 0   | 1   | 1   | 1       |
| 1   | 0   | 0   | 1   | 0   | 1       |
| 1   | 0   | 1   | 1   | 1   | 1       |
| 1   | 0   | 1   | 1   | 0   | 1       |
| x   | x   | x   | x   | x   | 0       |

- Minimización por mapa de Karnaugh

| AB/CDE | 000 | 001 | 011  | 010  | 110  | 111  | 101 | 100 |
| ------ | --- | --- | ---- | ---- | ---- | ---- | --- | --- |
| 00     | 0   | 0   | 0    | 0    | 0    | 0    | 0   | 0   |
| 01     | 0   | 0   | 0    | 0    | 1(a) | 1(a) | 0   | 0   |
| 11     | 0   | 0   | 0    | 0    | 1(a) | 1(a) | 0   | 0   |
| 10     | 0   | 0   | 1(b) | 1(b) | 1(b) | 1(b) | 0   | 0   |

Entonces obtenemos la minimización:
$$BCD + A\overline{B}D$$

- Minimización por equivalencias lógicas

$$\equiv BCD(A(E + \overline{E}) + \overline{A}(E + \overline{E})) + A\overline{B}D(C(E + \overline{E}) + \overline{C}(E + \overline{E}))$$
$$\equiv BCD(A + \overline{A}) + A\overline{B}D(C + \overline{C})$$
$$\equiv BCD + A\overline{B}D$$
