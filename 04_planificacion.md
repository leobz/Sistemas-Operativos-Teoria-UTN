# Capítulo 4: Planificación

## Objetivo de la planificaion del CPU

* Asignar procesos al procesador
* Rendimiento/ Productividad
* Optimizar algun aspecto del comportamiento del sistema

## Ciclo de rafagas

* CPU -> E/S -> CPU (las mas comunes)
* Limitados por la CPU (CPU bound) Mucho procesamiento
* Limitados por E/S(IO bound) Mucha entrada y salida

## Tipos de Planificacion

### De largo plazo

Los estados New y Exit definiran el grado de multiprogramacion del SO. Ejemplo, Windows Vista Starter solo dejaba crear 3 procesos de usuario.

### De Medio Plazo

* Ready/Suspended
* Bloqued/Suspended

Para que muchos procesos no usen tantos recursos de memoria u otros recursos se suspenden los procesos pasandolos a un alamacenamiento intermedio (Swap)

### De corto plazo

Estados implicados:  

* Ready
* Running
* Bloqued

Es la planificacion mas frecuente que utiliza el SO.

### Tipos planificacion

* Decide cual es el proceso que se debe ejecutar
* Decide donde ubicar el PCB del proceso que estaba ejecutando.

## Criterios de Planificacion

### Orientados al usuario/proceso

Cuantitativos:

* Tiempo de ejecucion
* Tiempo de respuesta (cuando hago un clic al mouse, abir un archivo, etc)
* Tiempo de espera

Cualitativos:

* Previsibilidad

### Orientados al sistema

Cuantitativos:

* Tasa de procesamiento (rendimiento)
* Utilizacion del CPU %

Cualitativos:

* Equidad
* Imposicion de prioridades
* Equlibrado de recursos

### Algoritmos de planificacion

#### Por prioridad

* A cada proceso se le asigna una prioridad
* La prioridad de un proceso puede variar
* Desicion del proximo proceso a ejecutar sera el de maxima prioridad

#### FIFO

* El primero que llega es el primero en ejecutarse
* Se ejecuta por orden de llegada a la cola de Ready
* Los procesos solo salen de Running cuando hacen una syscall o finaliza

#### Short Job First (SJF sin desalojo o SPN)

* El proceso con rafaga mas corta se ejecutara primero
* Se fija cual proceso tiene la duracion mas corta(menos ciclos del cpu)

Nota: si hay dos procesos con la misma rafaga, se elige el primero que llegue a la lista, o por el numero del proces id (esto se hace desde la catedra)

Desventajas:

* **Inanicion**: Situacion en la que a un proceso se le niega la posibilidad de utilizar un recurso (en este caso seria el cpu).
* Si hay procesos con rafaga mas corta, puede que un proceso nunca se ejecute.

### Como estimar la rafaga de un proceso

* Por estadistica de usos de un proceso
* Por **Formula del promedio exponencial**

Notas:

* Para el uso del estimador de rafagas, se necesitaran guardar todas las rafagas anteriores para hacer los calculos.
* La estimacion inicial, sera un numero que el SO asigne por defecto para todos los procesos.
* Si ya vengo ejecutando tomare los datos que ya tengo como referencia

## Categorias de algoritmos

* Sin desalojo (Sin expulsion) : FIFO y SJF
* Con desalojo (Con expulsion) : SRT (SFJ con desalojo), Round Robin

### SJF con desalojo

* Mientras se ejecuta un proceso 1, si la rafaga restante que le queda por ejecutar es menor a un proceso en Ready con una rafaga mas corta, se sacara al proceso 1 de ejecucion y se pondra en la cola de Ready.

Notas: si lo que queda del proceso actual es igual a una rafaga en espera, se seguira ejecutando el proceso en CPU (Esto es decicion de la catedra)

### Round Robin

* Colas de procesos Ready es FIFO
* Cuanto o rodaja de tiempo(quantum)
* Hay un tiempo determinado en el que el proceso se interrumpira (se le acaba el quantum de tiempo), y pasara al estado de listo. (Interrupcion por clock o quantum)

Desventajas segun Quantums usados:

* q=1: A pesar de ser justo, produce mucho OverHead.
* q<=4: Termina con tiempos muy largos y resulta casi lo mismo que hacer fifo(no se aprovecha el desalojo)

### Virtual Round Robin (VVR)

* Tratan de beneficiar a los IO bound
VER (copiar los que faltan)

* Los procesos que salen de bloqueados van a la cola Auxiliar
* Los procesos de la cola Auxiliar tienen mas prioiridad que los de la cola de Listos
* Si bien los que van a la cola Auxiliar tienen mas prioridad, su quantum sera igual a lo que le quedaba de quantum antes de ser interrumpidos(Quantum mas chico, es decir, mas prioridad, pero tampoco para que se pasen de vivos)
* Si un proceso pasa a bloqueado y su quantum restante es cero, entonces pasara a estado de Ready y no a Auxiliar.
* Si un proceso de Auxiliar pasa a CPU y luego a bloqueado nuevamente (de manera seguida), su quantum sera el quantum anterior que le quedaba menos lo que ejecuto en el CPU.

### Simultaneidad de eventos en Ready (aplica en varios algoritmos)

Los procesos se ordenaran por la siguiente prioridad

1) Interrupcion por Clock pasa a Listo  
2) Interrupcion por finalizacion de evento pasa a Listo  
3) Pasar de estado Nuevo a Listo

### HRRN (Primero el de mayor tasa de respuesta)

* Sin desalojo
* Aging (envejecimientos): Mecanismo que aumenta la prioridad del proceso con el paso del tiempo en ready, previene la inanicion.
* La tasa de respuesta es un calculo que implica el tiempo en Ready y el tiempo de rafaga estimado
* Prioriza a los que estan mayor tiempo en Ready y con tiempo de rafaga mas chico.

>> Formula aqui

### Colas multinivel

Se clasifican los procesos por tipos en diferentes colas.

* Hay colas que tendran mas prioridad que otras
* Cada cola usa su propio algoritmo de planificacion
* Las colas pueden tener distintos criterios

### Colas de Priodidades

(Copiar) No esta en el

### Colas Multinivel Retroalimentado(Feedback multinivel)

Parecido al de colas multinivel solo que

* Un proceso que entra desde Nuevo, pasa a la cola de maxima prioridad
* Pueden existir reglas para pasar los procesos a otras colas segun algun criterio que se quiera.

### Hilos

Se lanza el hilo con TID mas corto antes? Porque en el ejemplo no se basaban en las rafagas de los hilos.

* por convencion de la catedra:
* Preguntar por los proceso o hilos por defecto

* Syscall pasan o no por la biblioteca?
* Hay Jacketing?
Consultar en la proxima clase

Nota: Recordar que si  un proceso se corta por clock va a Ready, no a bloqueados

### Recomendaciones de la catedra

* Resolver toda la guia de ____
* Ver parciales anteriores para ver el nivel que se espera
