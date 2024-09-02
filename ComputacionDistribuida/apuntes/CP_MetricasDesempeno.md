[<- Índice](../ComputacionDistribuida.md)
## Métricas de desempeño

Para esta sección, primero aclararé la notación que se utiliza:

$T(n, \; p)$ se refiere al tiempo de cómputo sobre un ejemplar de "tamaño" $n$ usando $p$ nodos o núcleos para resolverlo.

$T(n, 1)$ representa el tiempo de cómputo usando el **mejor** algoritmo secuencial conocido (en un solo computador o nodo).

Por supuesto en un caso ideal $T(n, \; p) = \frac{T(n, \; 1)}{p}$ , pero no siempre es así. 

Ahora si podemos comenzar con las métricas:

### *Speed Up* (aceleración)

Esta métrica nos dice cúantas veces más rápido es un algoritmo distribuido en comparación con el secuencial.

Su valor esta dado por:
$$
S(n,p) = \frac{T(n,1)}{T(n,p)}
$$

Podemos pensar $T(n,p) = \frac{T(n,1)}{x}$ entonces el *Speed Up* sería exactamente $x$ , pues se entiende que la computación es $x$ veces más rápida.

Como mencionamos anteriormente, el caso ideal para *Speed Up* es $S(n, \; p) = p$.

### Eficiencia

Nos dice que tanto se están aprovechando los recursos del sistema para resolver el problema.

La eficiencia se calcula de la siguiente manera:
$$
E(n, \; p) = \frac{S(n, \; p)}{p}
$$
Observando la fórmula podemos notar que la eficiencia se mide entre 0 y 1:
$0 \leq E(n, \; p) \leq 1$

Ademas, del mejor caso de *Speed Up*, se sigue que el mejor caso de la eficiencia es 1.

### Ley de Amdahl

Indica cual es la ganacia global para un sistema cuando se implementa una mejora que afecta solamente una parte del sistema.
(Por mejora, entiendase que se acelero la velocidad de un componente del sistema)

Se calcula con:
$$
T_{m}= T_{a} \times (1 - F_{m} + \frac{F_m}{A})
$$

Donde:

- $T_m$ es el tiempo mejorado del sistema (globalmente)
- $T_a$ es el tiempo anterior a la mejora del sistema (globalmente)
- $F_m$ es la fracción del sistema mejorada (El porcentaje del sistema al que afecta directamente la mejora)
- $A$ es el factor de aceleración de la mejora del sistema

Una fórmula alternativa para cuando la mejora consiste en aumentar el número de nucleos es:
$$
S = \frac{1}{(1-P) + \frac{P}{N}}
$$
Donde:

- $S$ es la mejora global del sistema
- $P$ es la fracción de trabaja que puede paralelizarse (o que se mejora)
- $N$ es el nuevo número de unidades de procesamiento que se utiliza

**Ejemplo Primera fórmula:** Para mejorar una *pipeline* (r-d-e-w) si se duplico la velocidad de la sección *d*.

- $T_{a}= r+d+e+w$
- $F_m=\frac{d}{r+d+e+w}$
- $A=2$

$T_{m}=(r+d+e+w) \times (1 - \frac{d}{r+d+e+w} + \frac{d}{2(r+d+e+w)})$
$T_{m}=(r+d+e+w) \times (1 - \frac{d}{2(r+d+e+w)})$
$T_{m}=(r+d+e+w) - \frac{d}{2}$
$T_m= r+\frac{d}{2}+e+w$

### Fracción serial

La proporción del algoritmo (o del tiempo de ejecución) que es "inherentemente secuencial" (o no paralelizable).

Se denota por $F(n,p)$ y podemos despejarla a partir de la ley de Amdahl, suponiendo que mejoramos un sistema de 1 solo nodo a $p$ nodos.

$T(n,p) = T(n,1)(F(n,p) + \frac{1-F(n,p)}{p})$

$\frac{T(n,p)}{T(n,1)} = F(n,p) + \frac{1-F(n,p)}{p}$

> Ya que $S(n,p) = \frac{T(n,1)}{T(n,p)}$ entonces $\frac{1}{S(n,p)} = \frac{T(n,p)}{T(n,1)}$, por tanto:

$\frac{1}{S(n,p)} = \frac{pF(n,p)+1-F(n,p)}{p}$ \* Pues $F(n,p) = \frac{pF(n,p)}{p}$

$\frac{1}{S(n,p)} - \frac{1}{p} = \frac{F(n,p)(p-1)}{p}$

$$
\displaystyle
F(n,p) = \frac{\frac{1}{S(n,p)} - \frac{1}{p}}{\frac{p-1}{p}}
$$
Finalmente:
$$
\displaystyle
F(n,p) = \frac{\frac{1}{S(n,p)} - \frac{1}{p}}{1-\frac{1}{p}}
$$

O una variante que me interezo a mi:
$$
\displaystyle
F(n,p)=\frac{\frac{p}{S(n,p)}-1}{p-1}
$$
# Enlaces

[<- Anterior](CP_AlgoritmosDistribuidos.md) | [Siguiente ->](CP_BusquedaDistribuida.md)