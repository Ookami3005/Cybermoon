# Tarea 5

> Fernando Romero Cruz - 319314256

## Investigación

#### 1. Paginación de un nivel

> La paginación de un solo nivel es un mecanismo de administración de memoria que divide tanto la memoria virtual como la memoria principal en bloques de tamaño uniforme. Estos bloques se llaman **páginas** en la memoria virtual y **marcos** en la memoria física. Cada página se asigna a un marco mediante una **única tabla de páginas**, lo que facilita la traducción de direcciones lógicas a físicas. Aunque es fácil de implementar, puede no ser eficiente en sistemas con espacios de direcciones muy amplios debido al tamaño de la tabla.

#### 2. Componentes involucrados

- **Dirección virtual**: Se parte en dos segmentos:
  - *Número de página virtual* (parte alta de los bits).
  - *Desplazamiento* (parte baja de los bits).

- **Dirección física**:
  - *Número de marco físico* asignado por la tabla de páginas.
  - *Offset* que se traslada sin cambios.

- **Tabla de páginas**: Arreglo que permite identificar en qué marco físico se encuentra una página virtual específica.

- **MMU (Unidad de Gestión de Memoria)**: Hardware especializado encargado de realizar la conversión de direcciones.

- **Marco**: Sección de la RAM del mismo tamaño que una página virtual.

---

## Ejercicio

Dado un sistema con las siguientes características:

- Tamaño de la memoria virtual: 64 KB
- Tamaño de la memoria física: 8 KB
- Tamaño de página: 1 KB

### Cálculo de cantidades

- **Cantidad de páginas**:  
  ```
  64 KB / 1 KB = 64 páginas
  ```

- **Cantidad de marcos físicos**:  
  ```
  8 KB / 1 KB = 8 marcos
  ```

---

### Proceso de conversión de direcciones

1. Se convierte la dirección lógica en hexadecimal a decimal.
2. Se identifica la página:  
   `número_página = dirección / tamaño_página`
3. Se calcula el desplazamiento dentro de la página:  
   `offset = dirección % tamaño_página`
4. Con la tabla de páginas se determina el marco correspondiente.
5. La dirección física resultante se calcula con:  
   `marco * tamaño_página + offset`

---

### Ejemplos de traducción

| Dirección Virtual | Dirección Física | Descripción                                                                              |
| ----------------- | ---------------- | ---------------------------------------------------------------------------------------- |
| `0x0C0A`          | `0x140A`         | - Decimal: 3082<br>- Página 3 → Marco 5<br>- Dirección física: `5 * 1024 + 10 = 5130`    |
| `0x1E0F`          | `0x0A0F`         | - Decimal: 7695<br>- Página 7 → Marco 2<br>- Dirección física: `2 * 1024 + 527 = 2575`   |
| `0x320F`          | `0x1A0F`         | - Decimal: 12815<br>- Página 12 → Marco 6<br>- Dirección física: `6 * 1024 + 527 = 6671` |
| `0x2F00`          | **Page Fault**   | - Página 11 no tiene asignación, se produce un fallo.                                    |
