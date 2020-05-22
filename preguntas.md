# Preguntas

Cual es la diferencia entre un semaforo binario y un mutex?

Los semaforos sirven para distintos procesos o solo para hilos?

Que parametros puede recibir wait() y signal ademas de un semaforo?
Que hace wait(semaforoA,20)?

Que es starvation? -> Inanicion en ingles

- - -
Explique por qué las llamadas al sistema deben realizar un cambio a modo kernel.

```
Porque utilizan instrucciones que requieren que el bit de modo esté seteado en modo kernel, ya que acceden a hardware o bien a secciones de memoria que un proceso no puede (ni debe) acceder en modo usuario.
```

¿Qué sucedería si la misma llamada se mantuviera en modo usuario durante toda su ejecución?
```
 Si la misma se mantuviera en modo usuario, se producirá un error (en forma de interrupción) a la hora de ejecutar alguna instrucción privilegiada 
 ```

 - - -
 Explique por qué un proceso suspendido no siempre está a la espera de un evento.

 ```
 Puede estar suspendido listo, lo cual significa que estuvo esperando un evento y mientras eso ocurría fue suspendido. Luego dicho evento se produjo, y por lo tanto el proceso quedó en estado suspendido/listo.
 ```

- - -
Un Sistema Operativo bancario corre algunos grupos de procesos. A primera hora de la mañana, se  corren procesos financieros que deben finalizar lo antes posible, ya que de sus resultados depende la  compra de acciones y bonos durante el dia. Durante toda la jornada, se corren procesos que atienden “virtualmente” a los clientes, realizando pequeñas entradas-salidas ante cada pregunta que reciben. Explique qué algoritmo de planificación utilizaría, indicando configuración de colas, transiciones y al menos dos criterios para evaluar su funcionamiento.

```
Algoritmo de Planificación: 
En principio alguna variante de colas multinivel.
Idealmente, los procesos financieros deben estar en una cola de mayor prioridad, y las atenciones virtuales en una cola de menor. La primera cola probablemente utilice un FIFO, la segunda podría usar RR, para poder distribuir la atención de los clientes.

Transacciones: No hay transiciones entre las colas, ya que la idea es separarlos.

Posibles Criterios:

*Deadlines (sobre todo para la cola de mayor prioridad) y quizás
*Tasa de procesamiento (para todos, cuantas más consultas se contesten, mejor será el algoritmo).

Alternativa:
* Imposición de prioridades
* Previsibilidad del funcionamiento correcto
```

- - -

Explique cómo funcionan los semáforos bloqueantes, indicando ventajas y desventajas.

```
El semáforo bloqueante decrementa la variable semáforo con wait, y si el valor es < a cero bloquea al proceso. Lleva una cola de procesos bloqueados. La función signal la incrementa, y si corresponde, desbloquea un proceso de la cola. 
```

Explique   cómo implementa el Sistema Operativo la mutua exclusión sobre la “variable semáforo”

```
El sistema operativo puede implementar mutua exclusión, sobre el semáforo en sí, deshabilitando las interrupciones o bien con instrucciones de hw (por ej, test and set)
```

- - -

Describa brevemente la estrategia de detección y recupero de deadlock (no es necesario describir el algoritmo de detección en sí mismo).

```
La estrategia consiste en no otorgar límite alguno en el inicio y solicitud de recursos de parte de los procesos. Agregado a eso, se ejecuta periódicamente un algoritmo de detección de deadlock sobre el estado actual, y en caso de encontrar alguno se intenta eliminar el mismo a través de alguna estrategia de recupero (finalización de proceso, expropiación de recurso, etc). 
```

Mencione al menos dos desventajas de aplicar la estrategia de detección y recupero de deadlock

```
Desventajas podrían ser el overhead que genera correr el algoritmo cada cierto periodo de tiempo, y también el hecho de que los procesos podrían verse finalizados abruptamente si fueron parte de un deadlock.
```

- - -

Defina brevemente llamada al sistema (syscall).

```
Herramienta utilizada por los procesos para solicitar servicios al sistema operativo que de otra forma le serían imposibles poder realizar, como el uso de recursos de hardware (leer disco por ejemplo) o de otra índole (crear un klt por ejemplo).
```

¿Qué relación tiene con los modos de ejecución y las instrucciones privilegiadas?

```
Los procesos ejecutan en modo usuario, bajo el cual no se pueden ejecutar instrucciones privilegiadas. Al realizar una llamada al sistema, se cambia a modo kernel, y solamente en dicho modo se pueden ejecutar instrucciones privilegiadas.
```

- - -

En qué situaciones convendría usar hilos de usuario en lugar de hilos de kernel?

```
Convendría usar ULT por sobre KLT cuando se requiera overhead al mínimo (debido a que no hay cambios de modo), o se requiera portabilidad del programa (dado que sobre bibliotecas estándares podria usarse en cualquier SO), o se requiera una planificación particular no provista por el sistema operativo.
```

Mencione al menos dos atributos propios del TCB (Thread Control Block)

```
Dos atributos pueden ser el TID (thread id) y el contexto de ejecución (PC, flags, etc)
```

- - -

Explique las implicancias de utilizar un planificador de corto plazo sin desalojo (non preemptive) en  sistemas operativos de tiempo compartido (o multitarea).

```
El sistema operativo solo puede recuperar el control de la CPU cuando el proceso finaliza o se bloquea, por lo que un usuario/proceso podría nunca ejecutar si otro proceso nunca libera el procesador, ya sea por un error o porque deliberadamente no se bloquea nunca.
```

- - -

Explique cómo funcionan los semáforos con espera activa.

```
Los mismos garantizan la mutua exclusión a través de alguna solución de software (algoritmo de peterson por ejemplo) o de hw(como la instrucción test_and_set).
```

Indique ventajas y desventajas de semáforos de espera activa.

```
Tienen la ventaja de que sirven para sistemas multiprocesador sin generar overhead extra (como sí lo haría el deshabilitar las interrupciones) o no realizan cambios de contexto como si lo harian los bloqueantes. Tienen la desventaja de generar una pequeña carga de espera activa. 
```

- - -

En la estrategia de evasión de Deadlock mediante algoritmo del banquero, ¿Que significa estado  seguro?

```
Estado seguro significa que se tiene la certeza de que nunca ocurrirá un deadlock y que existe al menos una secuencia segura de finalización de los procesos. 
```

Usando evasión, podría el sistema quedar en estado inseguro ante alguna situación particular? 

```
El sistema nunca podría quedar en estado inseguro, debido a que la estrategia consiste en no otorgar recursos que pudieran dejarlo en dicha situación.
```

- - -

Indique en qué forma/s se realizan los cambios de modo (en ambos sentidos). 

```
Los cambios de modo se realizan modificando el bit de estado que contiene el PSW. Si está 0 esta en modo kernel. Si está en 1 está en modo usuario.


```

Explique para qué son necesarios los cambios de modo.

```
Es necesario cambiar de modo cuando se produce una syscall ya que estas son las que llaman a las instrucciones privilegiadas y estas se pueden ejecutar a nivel kernel. De forma contraria, la gestión de los hilos usuario deben ser gestionados por una biblioteca y el sistema operativo no conoce de ellos.
```

- - -

Relacione brevemente los siguientes conceptos: PCB - Proceso - Imagen - Memoria.

```
El  PCB es el bloque de control del proceso, que almacena en el el pid, el estado del proceso, el program counter, registros de cpu, información de planificación , de gestión de memoria, contable, de E/S y punteros. Cuando el proceso se encuentra en memoria en la cola de ready, la imagen del proceso es una parte de código que contiene este mismo, una parte de datos que contiene las variables globales, una parte de stack que contiene variables locales, argumentos y retornos, y un parte de heap que contiene las estructuras dinámicas.
```

Muestre un ejemplo simple en pseudocódigo y muestre cada sección en que parte de la imagen del proceso se encontraría. 

```
Int Global = 1; // Datos
Int main(int argc, char* argv)
argv = malloc(10); // Heap
argc = 3 // Pila
return global + 1;
```

- - -

En un instante X ocurre una interrupción por n de quantum y otra por n de E/S. Explique qué  ocurriría a bajo nivel, justicando qué proceso será elegido para ejecutar y por qué. Considere dicha  situación   por   un   lado   para   RR   y   por   otro   para   VRR.