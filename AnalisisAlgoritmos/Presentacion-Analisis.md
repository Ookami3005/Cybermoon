# Análisis de Algoritmos

En este curso aprenderemos a analizar pero sobre todo a diseñar algoritmos. El enfoque particular del curso se centra en 2 aspectos:

1. Nos enfocaremos en los ==paradigmas de diseño==, en lugar de la alternativa usual que suele ser agrupar los algoritmos por objeto de aplicación (gráficas, cadenas, números). En particular ilustraremos, los paradigmas resolviendo problemas sobre ==objetos discretos==.

2. Haremos ==énfasis en el papel de la recursión== como técnica fundamental de diseño.
---
## Bibliografía

El libro pricipal será

> Algorith Design. John Kleinberg y Eva Tardos

También utilizaremos los libros de algoritmos de Skienna y de Cormen et. al.

---

## Evaluación

- #### 70 % - Tareas individuales
- #### 30 % - 2 o 3 Exámenes parciales

## Temario

---
1. ==Introducción==
	1. Que es el análisis y que es el diseño de algoritmos
	
	2. Primeros problemas de ejemplo: recurrencias lineales; máximo común divisor.
	
	3. Propiedad de corrección
		1. Recursión e inducción
		
		2. Algoritmos secuenciales e invariantes
	
	4. Propiedad de complejidad
		1. Árboles de recursión
		
		2. Acumulación de iteraciones
		
		3. Notación asintótica
---
2. ==Divide y vencerás: reduce un problema grande a problemas más chicos==
	1. Definición intuitiva
	
	2. Caso de estudio 1. MergeSort
	
	3. Caso de estudio 2. QuickSort
	
	4. Caso de estudio 3. Pareja de puntos más cercana
	
	5. Caso de estudio 4. Dominación vectorial
	
	6. Caso de estudio 5. Transformada rápida de Fourier
		1. FFT y convolución: aplicaciones
	
	7. Paralelismo inherente en algoritmos divide y vencerás
---
3. ==Algoritmos *greedy*: argumento de intercambio==
	1. Definición intuitiva
	
	2. Caso de estudio 1. Calendarización de intervalos
	
	3. Caso de estudio 2. Rutas más cortas en gráficas
	
	4. Caso de estudio 3. Árboles generadores de peso mínimo
	
	5. Caso de estudio 4. Ordenación de enteros en tiempo lineal
	
	6. Notas sobre problemas de optimización en gráficas perfectas.
	
	7. Matroides y algoritmos *greedy*
---
4. ==Programación dinámica: recursión optimizada==
	1. Árboles de recursión con tareas repetidas
	
	2. Memorización como primera optimización
	
	3. Programación dinámica como segunda optimización
	
	4. Caso de estudio 1. Multiplicación de cadenas de matrices
	
	5. Caso de estudio 2. Rutas más cortas en gráficas con pesos negativos
	
	6. Caso de estudio 3. Distancia de edición, alineación de secuencias y problemas similares
	
	7. Caso de estudio 4. Subsucesión común más larga
	
	8. Paralelismo inherente en la programación dinámica
---
5. ==Flujo en redes: flujo máximo = corte mínimo==
	1. Definición
	
	2. Lema de flujo máximo/corte mínimo
	
	3. Algoritmo de Ford/Fulkenson
	
	4. Caso de estudio 1. Emparejamiento bipartito
	
	5. Caso de estudio 2. Conectividad de gráficas y trayectorias disjuntas
	
	6. Caso de estudio 3. Segmentación de imágenes
	
	7. Caso de estudio 4. Selección de proyectos
---
6. ==Más allá de algoritmos secuenciales (opcional)==
	1. NP-Dureza y reducciones
	
	2. Heurísticas de aproximación
	
	3. Algoritmos paralelos
	
	4. Algoritmos probabilistas
---