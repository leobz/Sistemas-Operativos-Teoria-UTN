## Concurrencia

### Condicion de Carrera (Race condition)
Condicion por la cual varios hilos compiten por un recurso

### Seccion Critica
Seccion donde se produce la condicion de carrera
* Recursos de E/S
* Para las variables aplica solo para las que son MODIFICADAS en algun momento




### Iteracciones entre procesos
* Comunicacion entre procesos
* Competencia por los recursos
* Cooperacion via comparticion (ej: comparten informacion mediante un archivo, no necesitan conocer al otro proceso)
* Cooperacion via comunicacion (ej: envio de mensajes por sockets, necesita conocer al otro proceso, implica que haya un protoco entre medio)


## Requisitos de la seccion critica

Dado un:
Codigo de Entrada a seccion critica  
SECCION CRITICA
Codigo de SALIDA de seccion critica

Se debe cumplir que:  
* Exclusion mutua (Un solo proceso en la seccion critica a la vez)
* Progreso (al salir de la seccion critica, debe permitir el uso para otro proceso)
* Espera limitada (Que no mueran por inanicion los otros procesos que esperan la seccion critica)
* Velocidad Relativa (Tratar de hacer el uso de la manera mas eficiente posible)


## Como armar una entrada y salida de una seccion critica?

Soluciones: 
* De software 
* De hardware
* Provistas por el SO: Semaforos
* Provistas por los lenguajes de programacion: mMonitor



### De software:
[ Pegar imagenes]
Problemas
Mutua exclusion no se cumple
Alternancia:
No hay progreso
Livelock: buscar en google (mutua cortesia)


### De hardware:
* Deshabilitar interrupciones (no es muy conveniente pero se puede hacer)
* Por instrucciones especiales del procesador:
	* Test and Set: En una instruccion que setea una seccion de memoria llamada 'lock' que es global y nos dice si se puede ejecutar. Hay espera activa. La diferencia es que al ser una instruccion, es atomica y se ejecuta antes de que la puedan interrumpir.


### Semaforos
Son una variable compartida.
Son usados mediante las llamadas while(semaforo) y signal(semaforo)
* Permite Exclusion mutua
* Permite sincronizar
* Permite controlar acceso a recursos

Estructura:
* Un valor entero (representa la cantidad de recursos que se comparten)
* Una lista de procesos bloqueados

Funciones sobre los semaforos:
* Iniciar/Finalizar un semaforo
* wait(semaforo): Decrementa en uno el valor del semaforo
* signal(semaforo): Incrementa en uno el valor del semaforo
Wait y signal pueden ser consideradas atomicos por ser del sistema operativo y no se interrumpiran


Tipos de semaforos:
* General o de contador
* Binario (0 o 1)
* Mutex ( 0 o 1)


Los semaforos siempre se inicializan con valor 0 o superior

### Monitores
Estrucutras abstractas que nos proveen sincronizacion facil de usar
