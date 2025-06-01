# Práctica 4: Analizador Sintáctico de Descenso Recursivo

## Compiladores 2025-2

<p  align="center">
  <img  width="200"  src="https://media.licdn.com/dms/image/v2/D4E0BAQErtaHuzN8ehw/company-logo_200_200/company-logo_200_200/0/1719282098472?e=2147483647&v=beta&t=RXd3JJN8OVl21DW-MFoiXMUfpUsX18inBRQ7MknrthY"  alt="">
</p>

> Fernando Romero Cruz

---
### Gramática

> P = {
> 
> > S -> num S'  
> > S' -> + num S' | ε  
>
> }

---

1. **Determinar los conjuntos _N_, _Σ_ y el símbolo inicial _S_.** (0.5 pts.)

> **S** -> *num* S'
> **N** -> {**S**, **S'**}
> $\Sigma$ -> {*num*, *+*}

2. **Mostrar la construcción de la tabla de análisis sintáctico predictivo para _G_.** (1 pt.)

Sean los **FIRST** y **FOLLOW**:

> **FIRST(S)**: { *num* }
> **FIRST(S')**: { **+**, $\varepsilon$ }

> **FOLLOW(S)**: { **$** }
> **FOLLOW(S')**: { **$** }

Derivamos la siguiente tabla de análisis predictivo:

| Símbolo no-terminal | num         | +             | $                  |
| ------------------- | ----------- | ------------- | ------------------ |
| **S**               | S -> num S' |               |                    |
| **S'**              |             | S' → + num S' | S' → $\varepsilon$ |

3. [x] **Modificar el main.py para que nuestro programa sea capaz de recibir archivos y no sólo cadenas**. (2 pts.)

4. [x] **Agregar las reglas necesarias para que el Analizador Léxico (lexico.py) reconozca los terminales de _G_**. (0.5 pts.)

5. [x] **Definir _Σ_ en el _enum_ de _ClaseLexica_ en _clase_lexica.py_**. (0.10 pts.)

6. [x] **Definir _N_ en un _enum_ de _NoTerminales_ en _clase_lexica.py_**. (0.10 pts.)

7. [x] **Cargar _N ∪ Σ_ en _sintactico.py_**. (0.25 pts.)

8. [x] **Cargar _P_ en _sintactico.py_**. (0.25 pts.)

9. [x] **Cargar la tabla de análisis sintáctico predictivo en _sintactico.py_**. (0.25 pts.)

10. [x] **Implementar el algoritmo de análisis sintáctico de descenso predictivo en _sintactico.py_ de modo que el programa acepte el archivo _tst/prueba.txt_ e imprima las producciones necesarias para llegar de S a w; S el símbolo inicial, w la cadena de entrada**. (4 pts.)

11. [x] **Sobrescribir la función _str_ de Producción para hacer más legible la impresión de las producciones necesaria para la derivación de S →\* w**. (1.05 pts.)