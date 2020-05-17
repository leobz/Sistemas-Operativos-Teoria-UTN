- - -
- - -

## Introducción

### Tipos de Sistemas Operativos
**Monoprogramados**: Solo permiten que un proceso se ejecute en el tiempo.

**Multiprogramados**: Se ejecutan varios procesos en un **intervalo de tiempo**. El procesador solo puede ejecutar un proceso a la vez.

Los sistemas **multiprogramados** permiten la **ejecución concurrente**, que aprovecha que cuando un proceso se pone en espera, deja de usar el CPU, y entonces pone algun proceso que estaba en la cola de listos mientras tanto.

**NOTA:** **Multiprogramación** NO ES LO MISMO que **multiprocesamiento**.


#### Multiprocesamiento, distinto a multiprogramación
El **multiprocesamiento** a diferencia de la multiṕrogramación, ocurre **cuando múltiples procesadores** ejecutan múltiples procesos en el **mismo instante de tiempo**.

## Procesos
### Diferencia entre Programa y Proceso
* **Programa** :Secuencia de instruccines compiladas a código maquina. **No siempre está en ejecucion**.
* **Procesos**: Programa que **está en ejecución**, en conjunto con sus **recursos asignados** (memoria, variables de estado, etc)


### Estructuras de un Proceso

* **Código**:
	Espacio asignado para almacenar la **secuencia de instrucciones del programa**.
* **Datos**:
	Espacio asignado para almacenar **variables GLOBALES**.
* **Pila(Stack)**:
	Espacio usado para **llamadas a funcion**, **parametros** y **variables LOCALES**.
	Se **borran los datos** despues de salir del contexto de la función
* **Heap**:
	Espacio asignado para el uso de **memoria dinamica**. Nota: La memoria dinámica es el lugar donde reserva el malloc(), en el lenguaje C hay que liberar el espacio reservado manualmente .


En multiprogramación, cuando se cambia de un programa a otro, se debe guardar el **contexto de ejecución del procesador** en memoria para luego, al retomarse, buscar el contexto y **restaurlo**.
- - -


### Bloque de control de proceso (PCB)

Es una **estructura de datos** que contiene **toda la información relacionada al proceso**, hay uno **por cada proceso**.

Se guarda **del lado del kernel**, por lo que el **usuario no puede acceder a los TCBs** (aunque si puede solicitar ciertos atributos).

Cada TCB contiene:
* La dirección de las **estructuras del proceso** (código, pila, datos, etc).
* Guarda el **contexto de ejecución** cuando el proceso no está ejecutando.
* **Atributos**
    * **Identificador**: PID
    * Identificador del proceso padre: PPID
    * Informacion de gestion de memoria (Ver)
    * Informacion de **Planificación**
    * Informacion de **E/S**
    * Informacion **contable** (Información extra, ejemplo tiempo usado, etc)


### Imagen del proceso
* PCB
* Código
* Datos
* Heap
* Pila


## Ciclo de vida de un proceso
Es el tiempo que transcurre entre su creación y su finalización. Durante el ciclo de vida, el proceso pasa por diferentes **estados**.

### Creación de un Proceso
- - -
Un proceso puede ser creado por:
* El **sistema operativo** para dar **algún servicio**.
* Un **proceso padre** para llevar a cabo algún requerimiento que este tenga (Ejemplo: Firefox al abrir nuevas pestañas las crea como procesos hijos)


#### Pasos para la creación de un proceso
* Asignación de **PID**
* **Reservar espacio** para estructuras(código, datos, pila y heap)
* Inicializar **PCB**
* Ubicar PCB en **lista de planificación**

#### Creación de un proceso en C

* fork(): Llamada del sistema. Permite a un proceso padre **crear un proceso hijo**. El proceso hijo será una **duplicación del proceso padre**, solo que tendra un **id** distinto y recorrerá una **sección de código** distinta al padre.

### Finalizacion de un proceso
- - -
Un proceso puede tener distintos tipos de terminación:
* **Normal**: exit(int exit_status)/ wait(int status)
* **Por falla**
* **Por otro proceso**: kill() envia una señal un proceso para finalizarlo.



### Tipos de E/S (bloqueante y no bloqueante)
- - -
**No bloqueante**: Pido un recurso pero mientras espero, **sigo ejecutando**.  
**Nota**: El proceso **tiene que preguntar** todo el tiempo si ya esta listo el recurso. De esto no se encarga el S.O. y **lo tiene que crear el programador de la aplicación**. 
Ejemplo: [send()](http://man7.org/linux/man-pages/man2/send.2.html)

**Bloqueante**: Recurso que es necesario para la continuacion del proceso, mientras espero **me bloqueo** 
Ejemplo: [receive()](http://man7.org/linux/man-pages/man2/recv.2.html).





### Estados de los procesos
- - -
Podemos separar un mismo ciclo de vida, en diferente cantidades de **estados**, según el nivel y la profundidad del análisis:


#### Diagrama de 2 estados
Los programas cambian de estado **"En ejecución"** a **"No ejecutado"** y viceversa.


#### Diagrama de 3 estados
Los estados posibles son:

* **Listo** (Ready)
* **Ejecutando** (Exec - Running)
* **Bloqueado** (Blocked): Ocurre cuando el proceso está **esperando un recurso** del sistema operativo. El sistema operativo mediante una **interrupción** se encargara de **avisarle** que el recurso **está listo** y lo pasa a estado de **Listo**.


#### Diagrama de 5 estados

* **Nuevo**
* **Listo**
* **Ejecutando**
* **Bloqueado**
* **Finalizado** (Exit): Sólo se conserva el PCB, que guarda el **Valor de Retorno**. Se borran todos los demás recursos de memoria.
	Cuando se toma el valor de Retorno, se borra el PCB.


### Diagrama de 7 estados
Al diagrama de 5 estados le agregamos los siguietes estados:

* **Listo Suspendido**
* **Bloqueado Supendido**

Estos dos estados ocurren cuando los procesos pasan de la memoria **al disco rígido** a travez de un **área de intercamio** **(swapping)**.

### Depuración (Debuggin)
- - -
Además podríamos incluir un estado más, que es la depuración, que consiste en **detener el proceso en un determinado punto de ejecución** para poder inspeccionar las variables que tiene en uso en ese momento. Esto es particularmente útil para poder encontrar posibles errores del software.



## Cambio de un proceso
Cuando el procesador deja de ejecutar un proceso y pone a ejecutar otro, en el medio el **sistema operativo interviene**.  
Mediante **syscalls** ó **interrupciones**, el proceso **deja de ejecutarse**, y el S.O realizará **tareas administrativas (overhead)**, que dependerán de la planificación y política del mismo.  
Luego de realizar estas tareas, el sistema operativo determinará **cuál será el siguiente proceso a ejecutar**.

#### Overhead
Son tareas burocráticas del sisteama opeartivo que determinan que proceso tendrá el control el CPU.

#### Secuencia de cambio de procesos:

**Ocurre interrupción**
* Ejecutando proceso A
* Se produce **interrupción**
* Se guarda el **PC y PSW** del proceso A
* Se actualiza el PC para atender la interrupción.
* Se **cambia de modo**, de usuario a **kernel**.
* Se guarda el resto del contexto del proceso A.


**Desalojo**:
* **Se decide cambiar de proceso**
* Se guarda el contexto del proceso A en su **PCB**
* Se cambia el estado del proceso A **de Ejecutando a Listo** (u otro).
* El **PCB** del proceso A se **ubica en la lista de procesos Listos** (u otra).

**Sustitución**:
* Se **selecciona otro proceso** para ser ejecutado (el proceso B).
* Se cambia el estado del proceso B a **Ejecutando**.
* Se **actualizan los registros** de administración de memoria del procesador
con los del proceso B.
* Se **actualizan los registros** del procesador **con los del PCB** de B y se cambia
**el modo de ejecución** (modo kernel **a usuario**).



