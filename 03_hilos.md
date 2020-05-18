# Capítulo 3: Hilos

## Hilo

Es un componente de un **proceso**, un **subproceso**. Tiene los mismos **estados** que un proceso, aunque la conmutación entre hilos de un mismo proceso es **menos costosa** que la  conmutación de procesos.  

Un proceso puede ser **monohilo** (es decir, tiene una sola linea de ejecución en un intervalo de tiempo) o **multihilo** (múltiples lineas de ejecución en un intervalo de tiempo)

Al hablar de hilos, haremos referencia a los procesos **multihilos**.

## Estructura de un proceso con hilos

* Se **comparte** el **código**, los **datos** y el **heap**.
* La **pila no se comparte**
* El **PCB** crea un campo **TCB** por cada hilo, que tiene la información particular de cada hilo.

## Ventajas de usar hilos

* Capacidad de respuesta
* Economia (No se tiene que crear otro proceso mediante el OverHead del sistema operativo a cada rato)
* **Compartición** de recursos
* Comunicación **eficiente**
* Permite **multiprocesamiento** (Solo [KLT](https://es.wikipedia.org/wiki/Hilo_(inform%C3%A1tica)#Hilos_a_nivel_de_n%C3%BAcleo_(KLT)))
* Procesamiento **asincrónico**

## Tipos de hilos

### Hilos a nivel de usuario (ULT)

En una implementación ULT, todo el trabajo de gestion de hilos se realiza en la aplicación, y el núcleo o kernel no es conciente de la existencia de estos hilos.

Los ULT necesitan una biblioteca de hilos que se encarge de la planificacion de los mismos.

Ventajas:

* Bajo **overhead** (ya que se evita pasar a modo kernel)
* Permiten **portabilidad** ya que no tienen que llamar al sistema operativo para que los maneje.
* La **planificación** es manejada por el programador

Desventajas:

* No permiten **multiprocesamiento**
* Una Syscall bloqueante **bloquea todo el proceso**. El uso de un **recurso bloqueante** va a hacer que el sistema operativo **bloquee todos los hilos**. TODO: Ya que del lado del sistema operativo el PCB va a tener un unico TCB y lo va a reconocer como un solo hilo.

### Hilos de Kernel (KLT)

En una implementacion KLT todo el trabajo de gestión de hilos lo realiza el kernel.

Ventajas:

* Cada KLT tendra su propio **TCB**.
* El S.O. los reconoce como **subprocesos**
* Soportan **multiprocesamiento**
* Si algún hilo pide una syscall bloqueante
**sólo** se bloqueará **ese hilo**.

Desventajas:

* Planificación **no personalizable**.
* Genera **más** **overhead**
* **Menos portabilidad**

En [POSIX](https://es.wikipedia.org/wiki/POSIX) podemos crear hilos mediante la syscall p_thread

## Jacketing

Es una técnica que le permite a los hilos ULT, **convertir una llamada bloqueante a no bloqueante**. Esto se logra usando  wrappers provistos por bibliotecas, que previenen que un hilo ULT se bloquee por otro.

**Desventaja**: El wrapper tiene que estar preguntando constantemente si le llego el recurso pedido.

- - -

Referencias:

* https://es.wikipedia.org/wiki/Hilo_(inform%C3%A1tica)#Diferencias_entre_hilos_y_procesos