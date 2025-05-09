# Orale Kernelito

## Antecedentes

La **explotación de binarios** típica tiene objetivos como la redirección de ejecución, corrupción de memoria y demás.

#### Corrupción de memoria

> Entre los ataques más comunes de corrupción de memoria se destacan:

- *Buffer Overflow*
	- Control de datos despues de un buffer controlado
	- Llega a primitivos tal como escritura, lectura o ejecución.
	- Implementar protecciones no es tan dificil
	
- Ataques de la **heap**
	- *Use-after-free*, *double free*, ataques a los asignadores de memoria
	- Principalmente primitivos de lectura y escritura
	
- *Race conditions*
	- Aleatorios y/o dificiles
	- No tan peligrosos

Todos estos ataques tienen un claro objetivo **ideal** en común:

> Su ***objetivo principal*** es la ==ejecución de código arbitrario==.

#### En 2025

Hay que tener en cuenta el contexto actual de la **explotación de binarios** típica.

- Primitivos debiles privilegiados, es decir vulnerabilidades límitadas pero con ciertos privilegios.
- Protecciones del compilador (+ bibliotecas locales), del nucleo y del *CPU*
- El logro máximo que se puede obtener es la *shell root*

## Explotación del Kernel

En el **modo de ejecución** del *Kernel* nos encontramos en un modo de ejecución totalmente distinto a aquel del *userland*.

En este poseemos características como:

- Acceso a todas las funciones del *CPU*
	- Registros específicos
	- Instrucciones *CPU* privilegiadas
- Un espacio de memoria virtual exclusivo
	- No comprendida (*translatable*) desde los procesos *userland*

Recordemos que el rol del *Kernel* es:

- Orquestar la buena ejecución de las tareas *userland*
- Organizar propiamente la memoria (via MMU)
- Mantener una coherencia de seguridad en el contexto de ejecución.

### ¿Porqué atacar al Kernel?

- Posee pocas mitigaciones y contramedidas
- Privilegios altos: fuera de vigilancia típica
- Disfrutar el reto y desafío que representa

### Etapas principales de la explotación

- Preparación del sistema - *feng shui*, etc

- Explotación del primitivo

- Reparación de los daños causados

- Escalada de privilegios del proceso atacante
	- Cuando un programa se ejecuta, el kernel tiene una estructura en memoria donde almacena todos los detalles de ejecución del programa. Este suele ser el objetivo típico del escalamiento
	
- Regreso tranquilo al userland

## Ejemplo

#### sys_calc

Imaginemos la siguiente implementación de una **llamada al sistema**.

```c
int sys_calc(int a, unsigned char o, int b, int *r)
{
	switch ( o )
	{
		case '%':
			*r = a % b;
			break;
		case '&':
			*r = a & b;
			break;
		case '*':
		case 'x':
			*r = b * a;
			break;
		case '+':
			*r = a + b;
			break;
		// Resto de los casos...
	}
}
```

Esta puede apŕovecharse debido a que no tiene ninguna restricción sobre el **puntero** de retorno, de modo que podemos hacer que el *Kernel* escriba en cualquier dirección arbitraria de memoria.

Primero claro, deberíamos saber donde escribir, por lo que una idea podría ser el archivo `/proc/kallsyms`, aquel donde se almacenan las direcciones de las funciones exportadas por el propio *Kernel*.

De esta manera, podemos remplazar la dirección de memoria de una llamada al sistema para que la siguiente vez que se invoque, en realidad el *Kernel* ejecute nuestra nueva dirección de memoria.
Es un proceso similar a la manipulación de la tabla *GOT* de la explotación típica.

Por tanto, una vez identificada la dirección donde se almacena la diferencia a la `sys_calc`, podemos reescribirla por medio de la propia `sys_calc` y escribir el puntero de alguna función creada por nosotros que, por ejemplo, altere la estructura con los detalles y permisos de la ejecución de nuestro programa y le brinde **permisos de administrador**.

Una vez elevado nuestros privilegios podríamos invocar directamente una *root shell* para establecer nuestro control sobre el sistema.