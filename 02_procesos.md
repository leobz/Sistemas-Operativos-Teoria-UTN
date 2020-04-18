Existe un modo Kernel y un modo usuario tanto en El procesador y otro en el sistema operativo, como una doble proteccion.


### Programa:

Secuencia de instruccines compiladas a codigo maquina. No siempre esta en ejecucion.

#### Ejecucion Ooncurrente

Dos o mas programas ejecutandose en un intervalo de tiempo


#### Sistemas Monoprogramados

Un solo programa se ejecuta en el tiempo






#### Sistemas Multiprogramados

NNO ES LO mismo que MULTIPROCESAMIENTO
Se ejecutan varios programas en el tiempo (Aunque el procesador solo puede ejecutar uno a la vez).

Cuando uno de los programas esta en espera, uno o mas programas aprovechan esa espera para ejecutarse. Solo se puede ejecutar un solo programa a la vez, pero se pueden ejecutar varios en un intervalo de tiempo.


#### Multiprocesamiento
Multiples procesadores (Es decir en el mismo instante dos o mas programas se pueden ejecutar al mismo tiempo). Diferente a multiprogramacion.


### Procesos
Programa que esta en ejecucion en conjunto con sus recursos asignados (memoria, variables de estado, etc)

#### Estructuras de un Proceso

* Codigo:
	Espacio asignado para almacenar la secuencia de instrucciones del programa.
* Datos:
	Espacio asignado para almacenar variables GLOBALES.
* Pila(Stack):
	Espacio usado para llamadas a funcion, parametros y 
	variables LOCALES.
	Se borran los datos despues de salir del contexto de una funcion
* HEAP (Cumulo):
	Espacio asignado para el uso de memoria dinamica. Nota: Es donde reserva el malloc(), y hay que borrar los datos manualmente en C.

#### Procesos en Multiprogramacion

#### Contexto de Ejecucion de un proceso

Cuando se cambia de un programa a otro, se debe guardar el contexto(estado del cpu) del procesador y guardarse en memoria para luego cuando al retomarse buscar el contexto y restaurlo.


### Atributos de un proceso

* Identificador: PID / PPID (Proceso Padre)/ UID (VER)
* Informacion de gestion de memoria (Ver)
* Informacion de Planificacion
* Informacion de E/S
* Informacion contable (Informacion extra, ejemplo tiempo usado, etc)

### Bloque de control de proceso (PCB)

Es una estructura de datos que contiene toda la informacion relacionada al proceso. (PID, Contexto, las direcciones de memoria de la Pila, de los datos, etc)
Se guarda del lado del kernel con un puntero.

* Hay uno por cada proceso del sistema.
* Contiene la direccion ... (VER)



### Imagen del proceso

* Codigo
* Datos
* HEAP
* PILA
* PCB

### Ciclo de vida de un proceso

* Tiempo que trancurre en un proceso
* ver

### Estado de los procesos: diagrama de 2 estados

Los programas cambian de estado 

* En ejecucion
* No ejecutado

#### Diagrama de 3 estados

* Listos
* Ejecutados
* Bloqueados (Ocurre cuando esta esperando un recurso del sistema operativo, el sistema operativo mediante una interrupcion se encargara de avisarle que el recurso esta listo y lo pasa a estado de listo)


### E/S bloqueante y no bloqueante para el proceso.

* No bloqueante: Pido un recurso pero mientras espero sigo ejecutando (el proceso tiene que preguntar todo el tiempo si ya esta listo el recurso, de esto no se encarga el SO y lo tiene que crear el programador de la aplicacion)
Ejemplo send()

* Bloqueante: Recurso que es necesario para la continuacion del proceso 
. Ejemplo receive()).

### Diagrama de 5 estados

* Nuevo

* Listo
* CPU (En ejecucion)
* Bloqueado

* Exit/Finalizado:
	Solo se conserva el PCB que guarda el valo,r de Retorno y se borran todos recurso de memoria.
	Cuando se toma el valor de Retorno, se borra el PCB.

### Creacion de un Proceso

* Puede ser creado por el sistema operativo para dar algun servicio

* Puede ser creado por un proceso padre para algun requerimiento que este tenga (Ejemplo Firefox y pestanias)

#### Pasos para la creacion de un proceso
* Asignacion de PID
* Reservar espacio para estructuras(codigo, datos, pila y heap)
* Inicializar PCB
* Ubicar PCB en lista de planificacion

### Creacion de un proceso en C

* fork(): Proceso padre crea un proceso hijo. El proceso hijo es una duplicacion del proceso padre, solo que tendra un id distinto y entrara por un camino dentro del codigo distinto al padre.

### Finalizacion de un proceso

* Terminacion normal
	* exit(int exit_status)/ wait(int status)
* Terminado por falla
* Terminado por otro proceso
	* kill(pid, sig) / kill envia una senial para cerrar el programa

### Diagrama de 7 estados

Las de el deiagrama de 5 estados mas

* Ready Suspendido
* Bloqued Supendido

Estas dos ocurren cuando los procesos pasan de la memoria al disco rigido a travez de un area de intercamio (swapping)

### Diagrama d e6 estados

Como el de 6 estados mas 
* Depuracion(Debuugin)



### Cambio de un proceso

Copiar de la presentacion la lista de pasos

### Over head
Tareas burocraticas del sisteama opeartivo que determinan quien tendra el control el procesador (procesos)
