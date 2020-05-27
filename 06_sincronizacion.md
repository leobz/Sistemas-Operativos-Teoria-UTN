# Capítulo 6: Sincronización

## Concurrencia de procesos

La concurrencia hace referencia a la situación en la que 2 o más procesos desean acceder a un mismo recurso al mismo tiempo.

Cómo la ejecución por el planificador del S.O puede ser aleatoria, la **Programación Concurrente** permite la ejecución en desorden u orden parcial de los procesos **sin afectar el resultado** final.

**Nota**: La concurrencia solo puede ocurrir en sistemas **multiprogramados**. Dependiendo del sistema, puede hacer uso del **multiprocesamiento**.

## Condición de Carrera (Race condition)

Condición por la cual varios procesos **intentan acceder a un recurso compartido al mismo tiempo**, de manera que el estado y la salida del sistema **dependerán de cuál llegó antes**, pudiendo provocar **inconsistencias**, comportamientos **impredecibles y no deterministas**.

## Programación concurrente: Interacciones entre Procesos

En la **programación concurrente** podemos encontrar algunas de las siguientes **interacciones** entre los **procesos**:

* **Comunicación** : Los datos de un proceso pueden ser de **utilidad** para otro.
* **Competencia por los recursos** (Ej: Competencia por tiempo de CPU, una impresora, etc.)
* **Cooperación via compartición** : Compartición de variables **GLOBALES**, **no se necesita conocer al otro proceso** (Ej: Compartir información mediante un archivo)
* **Cooperación via comunicación** : Envío de recursos mediante mensajes, se **necesita conocer al otro proceso**, implica la existencia de un **protocolo** de por medio (Ej: Envío de mensajes por sockets)

## Sección Crítica

Sección del código donde se produce la **condición de carrera**

Generalmente los recursos son:

* Recursos de E/S
* Variables o estructuras compartidas que son MODIFICADAS (Sólo lectura no aplica)

## Requisitos de la sección crítica

Los requisitos de la sección crítica, aseguran que **no se produzca una condición de carrera** en la misma.

Además se creará una **sección de Entrada** a la sección crítica y una **sección de Salida**

Los **requisitos de la sección crítica** son:  

* **Exclusión Mutua** (Solo puede haber un proceso a la vez)
* **Progreso** (Al salir se debe permitir entrar a otro proceso)
* **Espera limitada** (La espera en la entrada debe ser limitada, sino los procesos morirán por **inanición**)
* **Velocidad Relativa** (Usarla de la manera más eficiente posible)

## Soluciones para crear secciones críticas

¿Cómo armar una entrada y salida de una sección crítica?

Soluciones:

* De **software**
* De **hardware**
* Provistas por el **SO**: Semafáros
* Provistas por los **lenguajes** de programacion: Monitor

### De software

Problemas que pueden ocurrir:

* No se cumple Mutua exclusion
* **Alternancia**: Aunque se utilize la sección crítica, **no hay progreso**
* **Livelock**: (Mutua cortesia)

**Solución** : **Algoritmo de Decker**

### De hardware

* **Deshabilitar interrupciones** (No es muy conveniente pero se puede hacer)
* Por **instrucciones especiales del procesador**:
  * **Test and Set**: (TSL) Instrucción que lee y modifica **atómicamente** (por ser atómica esta
instrucción **no puede ser interrumpida**) una variable global de memoria llamada 'lock'. (Hay espera activa).

### Semáforos

Son una variable especiales compartidas que contienen un valor entero.  
Son usados mediante las llamadas **while**() y **signal**()

* Permite **Mutua Exclusión**
* Permite **sincronizar**
* Permite **controlar acceso a recursos**

Estructura:

* Un valor entero (representa la cantidad de recursos que se comparten)
* Una lista de procesos bloqueados

Funciones sobre los semaforos:

* Iniciar/Finalizar un semaforo
* **wait**(semaforo): Decrementa en uno el valor del semaforo
* **signal**(semaforo): Incrementa en uno el valor del semaforo

**Wait** y **signal** pueden ser consideradas **atómicas** por ser del sistema operativo y no se interrumpirán

Tipos de semáforos:

* **General o de contador**: permiten llevar la cuenta del número de unidades de recurso compartido disponible, que va desde 0 hasta N.
* **Binario** (0 o 1)  
* **Mutex** ( 0 o 1): inicialmente su contador vale 1 permite que haya un único proceso simultáneamente dentro de la sección crítica.

**Nota**: Los semáforos siempre se inicializan con valor 0 o superior



### Monitores

Estrucutras abstractas que nos proveen sincronizacion facil de usar

- - -

Referencias:

* https://stackoverflow.com/questions/34510/what-is-a-race-condition
* https://1984.lsi.us.es/wiki-ssoo/index.php/Concurrencia_de_procesos
* https://es.wikipedia.org/wiki/Concurrencia_(inform%C3%A1tica)
* https://es.wikipedia.org/wiki/Condici%C3%B3n_de_carrera