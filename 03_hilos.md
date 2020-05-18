# Capítulo 3: Hilos

## Hilo

Es un componente de un **proceso**, un **subproceso**. Tiene los mismos **estados** que un proceso, aunque la conmutación entre hilos de un mismo proceso es **menos costosa** que la  conmutación de procesos.  

Un proceso puede ser **monohilo** (es decir, tiene una sola linea de ejecución en un intervalo de tiempo) o **multihilo** (múltiples lineas de ejecución en un intervalo de tiempo)

Al hablar de hilos, haremos referencia a los procesos **multihilos**.

## Estructura de un proceso con hilos

* Se **comparte** el **código**, los **datos** y el **heap**.
* La **pila no se comparte**
* El **PCB** crea un campo **TCB** por cada hilo, que tiene la información particular de cada hilo.


### Ventajas de usar hilos

* Capacidad de respuesta
* Economia (No se tiene que crear otro proceso mediante el OverHead del sistema operativo a cada rato)
* **Compartición** de recursos
* Comunicación **eficiente**
* Permite **multiprocesamiento** (Solo [KLT](https://es.wikipedia.org/wiki/Hilo_(inform%C3%A1tica)#Hilos_a_nivel_de_n%C3%BAcleo_(KLT)))
* Procesamiento **asincrónico**

### Hilos a nivel de usuario (ULT)

En una implementación ULT, todo el trabajo de gestion de hilos se realiza en la aplicación, y el núcleo o kernel no es conciente de la existencia de estos hilos.

Los ULT necesitan una biblioteca de hilos que se encarge de la planificacion de los mismos.

Ventajas:

* Bajo overhead (ya que se evita pasar a modo kernel)
* Permiten **portabilidad** ya que no tienen que llamar al sistema operativo para que los maneje.
* La planificación es manejada por el programador

Desventajas:

* Una Syscall bloqueante **bloquea todo el proceso**. El uso de un **recurso bloqueante** va a hacer que el sistema operativo **bloquee todos los hilos**. Ya que del lado del sistema operativo el PCB va a tener un unico TCB y lo va a reconocer como un solo hilo.
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
