# Tarea de Cálculo de Predicados

1. Un número primo es un número entero que no es divisible por otros números, excepto por si mismo y por 1. Formaliza el concepto de ser primo por medio de un predicado, suponiendo que ya tienes otro predicado que define divisibilidad.

	> Sea $D(x,y)$ el predicado de divisibilidad que representa *$x$ divide a $y$* , podemos definir el predicado de ser primo $P(x)$ como:

	> $P(x) \iff \neg(\exists y. \; D(y,x))$ con $x \in (\mathbb{N} - \{0,1\}), \quad y \in [2,x-1]$

2. Señala las variables libres y ligadas de las siguientes fórmulas:

	1. $(\forall x. \; (\exists y. \; P_2^2(x,z)) \lor P_2^1(y));$
	 
	 >*Las variable $z$ es libre y las variables $x$, $y$ son ligadas.*
	
	2. $(\exists x. \; (\forall z. \; (\exists y. \; P_1^3(x,y,z)))) \iff P_2^3(x_1,y,z_2);$

	> *Las variables $x_1, \; z_2$ son libres, las variables $x, \; y, \; z$ son ligadas.*

	3. $(\exists x. \; P_1^3(x,y,z)) \Rightarrow P_1^1(x);$

	> *Las variables $y$, $z$ son libres y la variable $x$ es ligada.*
	
	4. $(\forall y. \; P_1^3(x,y,z)) \land (\exists z. \; P_2^3(x,y,z)) \land (\forall x. \; P_3^3(x,y,z)).$

	> *No hay variables libres, las variables $x$, $y$, $z$ son ligadas*
	

3. Realiza las siguientes sustituciones:

	1. $((\exists x. \; P_1^3(x,y,z))) \Rightarrow P_1^1(x))_{[z \: := \: f_1^2(x,y)]}$

	> $\equiv ((\exists x. \; P_1^3(x,y,f_1^2(x,y)))) \Rightarrow P_1^1(x))$
	
	2. $(((\forall x. \; P_1^3(x,y,z)) \land (\forall y. \; P_1^3(x,y,z)) \land (\exists z. \; P_1^3(x,y,z)))_{[x \: := \: f_1^2(y,z)]})_{[z \: := \: f_1^2(y,x)]}$	

	> $(((\forall x. \; P_1^3(x,y,f_1^2(y,x))) \land (\forall y. \; P_1^3(f_1^2(y,f_1^2(y,x)),y,f_1^2(y,x))) \land (\exists z. \; P_1^3(f(y,z),y,z)))$


4. Dada la siguiente interpretación, dí si las fórmulas *(a)-(e)* son satisfechas, verdaderas, válidas (o ninguna de las anteriores):

	 Universo de números racionales $\mathbb{Q}$ 

	 $\psi(c)=0 \quad \quad \psi(x)=1 \quad \quad \psi(y)=-1$

	 $\Phi(f_1^1) = \; sucesor \quad \quad \Phi(f_2^1) = \; predecesor \quad \quad \Phi(f_1^2) = \div \quad \quad \Pi(P_2^2) = \leq$

- $\forall x. \; \forall y. \neg(y = 0) \Rightarrow \exists z. \; f_1^2(x,y) = z;$

	> Esta formula es verdadera

- $\forall x. \; \exists y. \; P_1^2(x,y) \Rightarrow P_1^2(x,y);$

	> Esta fórmula es válida

- $\forall x. \; \forall y. \; \exists z. \; P_1^2(x,y) \Rightarrow P_1^2(x,z) \land P_1^2(z,y);$

	> Esta fórmula es válida 

- $c = f_2^1(y) \land c=f_1^1(x);$

	> Esta fórmula es satisfecha

- $P_1^2(f_2^1(y), c) \land P_1^2(c, f_1^1(x)).$

	> Esta fórmula es satisfecha


5. Encuentra un modelo para las siguientes fórmulas:

	1. $\forall x. \; (\exists y. \; P_1^2(y,x))$

		> $U = \mathbb{Z}$, $\psi(x) = a$, $\psi(y)=b$, $\Pi = \{(P_1^2,\leq)\}$
	
	2. $\exists x. \; \neg P_2^2(x,c) \land P_1^2(x,c)$

		> $U = \mathbb{Z}$, $\psi = \{(x,a),(c,0)\}$, $\Pi = \{(P_1^2,>),(P_2^2,<)\}$
	
	3. $\neg \forall x. \; P_2^2(c,f_1^1(x))$

		> $U = \mathbb{Z}, \psi = \{(x,a),(c,10)\}$, $\Phi(f_1^1) = \; sucesor$ y $\Pi(P_2^2) = \; \geq$
	
	4. $\neg \exists x. \; P_2^2(x,f_1^1(x))$

	> 	$U = \mathbb{N}, \psi = \{(x,a),(c,0)\}$, $\Phi(f_1^1) = \; sucesor$ y $\Pi(P_2^2) = \; \geq$
	

6. Demuestra los siguientes teoremas de deducción natural:

	1. $\vdash_N (\exists x. \; P_1^1(x) \lor P_2^1(x)) \iff (\exists x. \; P_1^1(x)) \lor (\exists x. \; P_2^1(x));$

	
	
	2. $\forall x. \; (\exists y. \; P_1^1(x) \Rightarrow P_2^1(y)) \quad \vdash_N \quad \neg (\exists x. \; (\forall y. \; P_1^1(x) \land \neg P_2^1(y))).$