# Tarea 2 - Sistemas Operativos

> Fernando Romero Cruz - *319314256*

### Investigación

- ¿Qué son las llamadas al sistema (*system calls*)?

> Son un mecanismo provisto para **procesos en modo usuario**, de modo que puedan solicitar servicios o funciones al **kernel** del sistema operativo. De este modo pueden interactuar con recursos esenciales como archivos, memoria, dispositivos y demás procesos que están restringidos por defecto.

- ¿Qué son las señales en un sistema operativo?

> Las señales permiten la comunicación **asíncrona** entre procesos, pues permiten que un proceso notifique o interrumpa a otro para que puedan manejar eventos específicos, como lo son interrupciones de usuario, terminación de procesos, errores, etc.

- ¿Las llamadas al sistema y las señales son lo mismo? Justifica tu respuesta

> No, aunque con objetivos similares, son mecanismos distintos. Por ejemplo, una **lamada al sistema** es un mecanismo **síncrono** donde buscamos una respuesta inmediata por parte del *Kernel*, por otra parte, una señal es una llamada enviada por cualquier proceso a cualquier otro proceso de manera **asíncrona**, para que este otro pueda gestionar la señal como y cuando deba.

- ¿Qué es la señal *SIGUSR1*?

> Es un tipo de señal disponible para el usuario envíe y reciba según sus necesidades, aunque por defecto termina el proceso que la reciba si no es manejada adecuadamente.

- ¿Para qué se utiliza?

> Es una señal **multipropósito**, pensada para que el usuario la lance y maneje según la necesite o según defina su utilidad. Sin embargo, algunos usos comunes son para activar eventos en procesos en ejecución, como recargar configuraciones, depuración y comunicación sin *Sockets*.

- ¿Qué otras señales están relacionadas con *SIGUSR1* (por ejemplo, *SIGUSR2*)?

> Me parece que *SIGUSR2* es la única relacionada con *SIGUSR1*, bajo el mismo concepto multipropósito.

### Implementación

- ***Progama en C***

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Manejador de señal
void signal_handler(int signo) {
    if(signo == 10){
        printf("Señal %s recibida.\n", "SIGUSR1");
    }
}

int main() {
    pid_t pid = getpid();
    printf("PID: %d\n", pid);
	
    signal(SIGUSR1, signal_handler);
    printf("Enviando señal SIGUSR1 a mí mismo...\n");
    kill(pid, SIGUSR1);
	
    sleep(1);
    return 0;
}
```

- ***Programa en Python***

```python
#!/usr/bin/env python

import os
import signal
import time

# Manejador de la señal SIGUSR1
def signal_handler(signum, frame):
    if signum == 10:
        print("Señal SIGUSR1 recibida.")

pid = os.getpid()
print(f"PID: {pid}")

signal.signal(signal.SIGUSR1, signal_handler)
print("Enviando señal SIGUSR1 a mí mismo...")
os.kill(pid, signal.SIGUSR1)

time.sleep(1)
```

### Análisis

##### `strace`

***C***:
```c
# ...
getpid()                                = 259162
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
getrandom("\x55\xc0\x52\x48\x0f\x91\x45\xbe", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x5f5be5068000
brk(0x5f5be5089000)                     = 0x5f5be5089000
write(1, "PID: 259162\n", 12PID: 259162
)           = 12
rt_sigaction(SIGUSR1, {sa_handler=0x5f5bd7a8f189, sa_mask=[USR1], sa_flags=SA_RESTORER|SA_RESTART, sa_restorer=0x70709b12bcd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
write(1, "Enviando se\303\261al SIGUSR1 a m\303\255 mi"..., 39Enviando señal SIGUSR1 a mí mismo...
) = 39
kill(259162, SIGUSR1)                   = 0
--- SIGUSR1 {si_signo=SIGUSR1, si_code=SI_USER, si_pid=259162, si_uid=1000} ---
write(1, "Se\303\261al SIGUSR1 recibida.\n", 25Señal SIGUSR1 recibida.
) = 25
rt_sigreturn({mask=[]})                 = 0
clock_nanosleep(CLOCK_REALTIME, 0, {tv_sec=1, tv_nsec=0}, 0x7ffed825ef30) = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

***Python***:
```c
getpid()                                = 261191
write(1, "PID: 261191\n", 12PID: 261191
)           = 12
rt_sigaction(SIGUSR1, {sa_handler=0x78596b4e4590, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK, sa_restorer=0x78596b24bcd0}, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
write(1, "Enviando se\303\261al SIGUSR1 a m\303\255 mi"..., 39Enviando señal SIGUSR1 a mí mismo...
) = 39
kill(261191, SIGUSR1)                   = 0
--- SIGUSR1 {si_signo=SIGUSR1, si_code=SI_USER, si_pid=261191, si_uid=1000} ---
rt_sigreturn({mask=[]})                 = 0
write(1, "Se\303\261al SIGUSR1 recibida.\n", 25Señal SIGUSR1 recibida.
) = 25
clock_nanosleep(CLOCK_MONOTONIC, TIMER_ABSTIME, {tv_sec=7359, tv_nsec=783486236}, NULL) = 0
rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK, sa_restorer=0x78596b24bcd0}, {sa_handler=0x78596b4e4590, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK, sa_restorer=0x78596b24bcd0}, 8) = 0
rt_sigaction(SIGUSR1, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK, sa_restorer=0x78596b24bcd0}, {sa_handler=0x78596b4e4590, sa_mask=[], sa_flags=SA_RESTORER|SA_ONSTACK, sa_restorer=0x78596b24bcd0}, 8) = 0
munmap(0x78596b9dc000, 16384)           = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

- ¿Qué llamadas al sistema se utilizan para manejar señales en *C* y *Python*?

> Tanto el programa en *C* como el *script* en *Python* realizan una llamada `rt_sigaction` para manejar la señal *SIGUSR1* que se envían a si mismos.

- ¿Cómo se relacionan las funciones de alto nivel (como `signal()` en *C* o `signal.signal()` en *Python*) con las llamadas al sistema?

> Ambas funciones, realmente realizan por detrás una **llamada al sistema** `sigaction`, para que el *Kernel* pueda enviar la señal correspondiente al proceso deseado.

- ¿Qué diferencias hay entre las implementaciones en *C* y *Python*?

La implementación en *C*, como un lenguaje de menor nivel que *Python*, permite un control más extenso y directo sobre las señales.
Por parte de *Python*, para interactuar con las señales debemos importar las librerías adecuadas y se nos da un control más simple de estas mismas señales.

En cuanto a las llamadas al sistema, noté que las llamadas del programa compilado a partir del código en *C* realiza unas cuantas llamadas extra sin mucha relación aparente a las señales, mientras que *Python* realiza un poco menos de estas llamadas.