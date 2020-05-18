# Capítulo 4: Planificación de la CPU

La Planificación de la CPU es un **conjunto de técnicas y algoritmos** de los Sistemas Operativos **Multiprogramados**, que sirven para asignarle a los procesos tiempo de ejecución en el CPU.

## Objetivos de la planificación del CPU

* **Asignar procesos** al procesador
* Rendimiento/ Productividad
* Optimizar algún aspecto del comportamiento del sistema (VER)
  * **Equidad**
  * Evitar **inanición**

**Inanición**: Situación en la que a un proceso se le **niega la posibilidad de utilizar un recurso** (en este caso sería el cpu). Provoca que ante la falta del recurso, el proceso **nunca pueda finalizar**.

## Ciclo de ráfagas

**Ráfagas de CPU**: es el ciclo en donde se realizan las instrucciones del proceso.  
**Ráfagas de E/S**: es el ciclo en donde se utilizan o esperan los dispositivos de Entrada y Salida.  
**Ciclos de ráfagas más comunes**: CPU -> E/S -> CPU

Tipos de procesos, según su uso del **tiempo de ejecución**:

* Limitados por la CPU (**CPU bound**): Mucho procesamiento y poca E/S
* Limitados por E/S(**IO Bound**) Mucha entrada y salida

## Tipos de Planificación

Dependiendo de la importancia que se le de a ciertos estados de los procesos, podemos distinguir entre distintos tipos de planificaciones.

### De largo plazo

Los estados **New y Exit definirán el grado de multiprogramación** del S.O.

Ejemplo: Windows Vista Starter solo dejaba crear 3 procesos de usuario. (El impacto de los estados New y Exit era muy grande)

### De Medio Plazo

Principal foco en los estados:

* **Ready Suspended**
* **Bloqued Suspended**

Se **supenden los procesos** para que no usen tanta memoria u otros recursos, pasándolos a un almacenamiento de intercambio (**swap**).

### De corto plazo

Principal foco en los estados:  

* **Ready**
* **Running**
* **Bloqued**

Es la planificación **más frecuente** que utiliza el SO. De aquí en adelante **nos centraremos en este tipo de planificación**.

## Criterios de Planificación (Corto plazo)

* Deciden cuál es el **siguiente proceso** que se debe ejecutar
* Deciden **donde ubicar el PCB** del proceso que **deja de ejecutarse**.

## Orientados al usuario (Proceso)

**Prestaciones** cuantitativas:

* Tiempo de ejecución
* Tiempo de respuesta (Ej: tiempo que tarda en abrir una carpeta desde que di click con el mouse)
* Tiempo de espera

**Prestaciones** cualitativas:

* Previsibilidad

## Orientados al Sistema

**Prestaciones** cuantitativas:

* Tasa de procesamiento (rendimiento)
* Utilización del CPU (%)

**Prestaciones** cualitativas:

* **Equidad** de tiempo de ejecución
* Imposición de **prioridades**
* Equlibrar **recursos**

## Algoritmos de planificación

Algoritmos que utliiza el módulo de planificación del S.O para tomar desiciones de planificación. Existen varios y **se pueden alternar** dependiendo de los requerimientos del planificador.

Funcionamiento:

* A cada proceso se le asigna una **prioridad**
* La prioridad de un proceso **puede variar**
* El próximo proceso a ejecutar sera el de **máxima prioridad**

## Categorías de algoritmos de planificación

**Sin desalojo** (Sin expulsión de estado Running) : **FIFO** y **SJF**

**Con desalojo** (Con expulsión) : SRT (**SFJ** con desalojo), **Round Robin**, etc.

### FIFO (First In First Out)

* Los procesos se ejecutan por **orden de llegada** a la cola de Ready
* El primero que llega es el primero en ejecutarse
* Los procesos **solo salen de Running cuando hacen una syscall o finaliza**

### Short Job First (SJF sin desalojo o SPN)

* Se **ejecutará primero** el proceso con **ráfaga mas corta** (VER)
* Se fija cuál proceso tiene la **duracion más corta**(menos ciclos del cpu).

**Nota**: si hay dos procesos con la **misma ráfaga**, se elige el primero que llegó a la lista, o por el número del PID (éstos son criterios de la cátedra de la UTN, no es una tautología universal)

#### Desventajas

* **Inanición** (Si siempre hay procesos con ráfaga más corta, puede que un proceso nunca se ejecute)

#### ¿Cómo estimar la ráfaga de un proceso?

* Por estadistíca de usos de un proceso
* Por **Fórmula del promedio exponencial**

TODO: INSERTAR IMAGEN DEL PROMEDIO EXPONENCIAL

#### Notas

* Para el uso del **estimador de ráfagas**, se necesitarán guardar todas las ráfagas anteriores para usarlas como referencias.
* La **estimación inicial**, será un numero que el S.O. asigne **por defecto** para **todos los procesos**.

### SJF con desalojo

Si mientras ejecuta un proceso **A**, su **ráfaga restante** es mayor a la ráfaga de un proceso **B** en Ready, se pone a ejecutar B y A se pone en Ready.

**Nota**: si lo que queda del proceso actual **es igual a una rafaga en espera**, se seguirá ejecutando el proceso en CPU (Ésto es decicion de la cátedra)

### Round Robin

* La cola de **Ready** se comporta cómo **FIFO**
* Existe **Quantum**: pequeño intervalo de tiempo que se asigna a un proceso para usar el CPU
* Terminado el **quantum** de tiempo, el proceso **pasará al estado de listo**. (Se produce una **interrupción por clock**)

**Desventajas según tamaño** del Quantum:

* **q = 1**: A pesar de ser justo, produce **mucho overhead**.
* **q <= 4**: Tiempos muy **largos** y **no se aprovecha el desalojo** (resulta casi lo mismo que hacer FIFO)

### Virtual Round Robin (VRR)

Algoritmo **con desalojo**. Trata de **beneficiar** a los procesos **IO bound**.  

* Existe **Quantum** (interrupción por clock)
* Existen **dos colas de procesos Listos** (se incluye cola **"Auxiliar"**)

Funcionamiento:

* Los procesos que tienen **interrupción por Quantum** van a **Ready**
* Los procesos que **salen de Bloqueados**  van a la cola **Auxiliar**
* Los **procesos** de la cola **Auxiliar** tienen **más prioridad** que los de la cola de **Listos**
* El **quantum** de los procesos de la cola **Auxiliar** será igual su **quantum restante** (q* = q - tiempo en CPU)

Notas:

* Si un proceso **sale de bloqueado** y su quantum restante **es cero**, entonces pasará a estado de **Ready y no a Auxiliar**.
* Si un proceso pasa de **Auxiliar** -> **Running** -> **Bloqueado nuevamente** -> **Auxiliar nuevamente**, su quantum será el quantum anterior que le quedaba menos lo que ejecutó en el CPU.

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

- - -
  
Referencias:

* Sistemas Operativos 5ta Edición (Parte 4) - Willian Stallings

* https://en.wikipedia.org/wiki/CPU-bound
* https://www.udg.co.cu/cmap/sistemas_operativos/planificacion_cpu/sjf/sjf.html