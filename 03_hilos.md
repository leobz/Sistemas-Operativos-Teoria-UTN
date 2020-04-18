### Hilo
(Ver)

## Mono hilo
(ver)

## MultiHilo
(Ver))

* pthread_create():
	* Cuando se lanzan hilos dentro de un proceso, esos hilos pertenecen al mismo proceso, no son procesos distintos.
	
### Estructura de un proceso con hilos

*Se comparte el codigo, los datos y el heap.
* La pila no se comparte
* El PCB crea un campo TCB por cada hilo, que tiene la informacion particular de cada hilo.



### Estados de Hilos

* Listo
* Ejecutando
* (ver)

### Ventajas de los hilos

* Capacidad de respuesta
* Economia (No se tiene que crear otro proceso mediante el OverHead del sistema operativo a cada rato)
* Comparticion de recursos
* Comunicacion eficiente.
* Permite multiprocesamiento
* Procesamiento Asincronico

### Hilos de Usuario ULT

* Necesita una biblioteca que simule la planificacion de los hilos
* Ventajas: 
	* Bajo overHead
	* Permiten portabilidad ya que no tienen que llamar al sistema operativo para que los maneje.
	* La planificacion es manejada por el programador
* Desventajas:
	* Syscall bloqueante bloquea todo el proceso. El uso de un recurso bloqueante va a hacer que el sistema operativo bloquee todos los hilos. Ya que del lado del sistema operativo el PCB va a tener un unico TCB y lo va a reconocer como un solo hilo.
	* No permiten multiprocesamiento


### Hilos de Kernel KLT

Implementados por el SO.
Algunos wrapper: p_thread, etc.

Ventajas:

* Cada hilo del kernel tendra su propio TCB.
* El SO los reconoce como subprocesos
* Soporta multiprocesamiento
* Si algun hilo pide una syscall bloqueante
solo se bloqueara ese hilo .

Desventajas:
* Planificacion no personalizable.
* Genera mas OverHead
* Menos portabilidad


### Sistema que utilizan lo ULT para no ser bloqueados (Jacketing)

Se usan wrappers provistos por bibliotecas ULT que previenen que un hilo ULT se bloquee por otro.
Convierten E/S bloqueantes en no bloqueantes

Desventaja: El wrapper tiene que estar preguntando constantemente si le llego el recurso pedido.


### Notas:

Mas informacion: Buscar en google git sisoputnfrba para ver los scripts de hilos y procesos.


Preguntas:
Los hilos que se implementan en el TP son ULT o de kernel?
> Creo que ULT ya que hay que planificar a los jugadores, y la planificacion seria personalizada.
