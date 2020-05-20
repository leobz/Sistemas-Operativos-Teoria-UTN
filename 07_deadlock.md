## Repaso: 
El PCB siempre se conserva en memoria (DATOS, PILA HEAP pueden estar en disco)
Un proceso puede pasar a exit desde cualquier estado
En el diagrama de 7 estados se puede dividir en :
Corto plazo: Ready, Running y Blocked
Medio Plazo: Bocked Supended, Read Suspended
Largo plazo: New y Exit

Con el uso de hilos, el mismo proceso tiene ejecucion concurrente(dos o mas proceso ejecutando en un mismo intervalo de tiempo)
Ademas una ventaja es la ejecucion asincronica entre hilos

En los ejercicios con hilos ULT asumiremos como normal que no usan jacketing.

Mientras que una variable no sea de escritura, y sea solo de lectura, no se necesita la mutua
exclusion. Caso contrario, en programacion concurrente si se necesita.

La espera activa es una espera poco eficiente. En algunos casos puede bloquear un proceso indefinidamente.

Las soluciones de hardware no es muy deseable, ya que al desactivar las interrupciones dejan
a un monton de procesos sin posiblididad de usarlas. Ademas solo pueden usarse en modo kernel
Hay algunas instrucciones de hardware como las internas al procesador que son atomicas y permiten hacer la reserva del area critica en una sola instruccion (SET y __).

Los semaforos son estructuras que tienen un valor numerico y una cola de procesos bloqueados.
Los funciones que se les aplican son wait y signal.


Tipos de semaforos: Contador , binarios(solo estados 0 y 1), mutex (para proteger estructuras o variables compartidas, se inician siempre en 1)

Semaforos fuertes: Al desbloquear un proceso usan algun algoritmos para ver cual es el proximo proceso a ejecutar.
Semaforos debiles: Al desbloquear un proceso desbloquean cualquiera

Adentro del semaforo tambien se usa mutua exclusion

Los monitores son soluciones que brindan los lenguajes de programcion( generalmente los orientados a objetos)



# DeadLock
Interbloqueo entre mas o dos procesos

Definicion: Bloqueo permanetne de un conjunto de procesos, donde cada uno necesita 


En el dedlock: Los procesos se quedan en la cola de bloqueados. Al pasar a bloqueados no usan el procesador

En el LiveLock: Los procesos pasan constantemente de ready a ejecutando todo el tiempo, haciendo un loop infinito, y al llegar a ejecutnado no realizan ninguna instruccion. Al usar constantemente el procesador no permiten a otros procesos usar el procesador.

## Tipos de Recursos

* Reutilizables (Ej: Archivo)
* Consumibles (Ej: Cuando el consumidor saca un elemento del buffer)

Nosotros vamos a hacer incapie en deadlock en recursos reutilizables


## Grafos

Ciclos:
* Si no hay ciclos, no hay Deadlock
* Si hay un ciclo, **PUEDE** haber un Deadlock
* Si hay un ciclo, y todos los recursos son de **una instancia**, entonces **HAY** Deadlock


## Condiciones Necesarias y suficientes para un Deadlock (tienen que estar estas 4 condiciones si o si)
* Mutua exclusion entre procesos
* Retencion y espera (tiene un recurso asignado[lo retiene] y espera a otro)
* Sin desalojo de recursos (Cuando nadie puede sacarle un recurso asignado a un proceso) 
* Espera circular


## 4 Maneras de tratar al Deadlock
1) Prevencion de Deadlock:

* Garantiza que no ocurrira Deadlock
* Impedir que se produzca la Mutua exclusion


Condicion1: Mutua-Exclusion
No generar mutua exclusion de recursos(puede haber race-condition)

Condicion2: Retencion y Espera
* Solicitar todos los recursos juntos (Esto puede causar inanicion en otros procesos)
* Solicitar los recursos de uno o varios, utilizarlos y liberarlos


Condicion3: Sin desalojo de recuross
* Si un proceso que tiene recursos asignados solicita uno que no esta disponible, debe liberar sus recursos(puede haber inanicion)

* Si un proceso A solicita un recurso que esta asignado a otro proceso B que esta a la espera de mas recursos. El recurso asignado al proceso B puede asignarse al proceso A.


Condicion4: Espera circular
* Asignar un numero de orden a los recursos. Los recursos solo pueden solicitarse en orden creciente.(puede haber inanicion)


## Evasion o Prediccion de Deadlock:
* Garantiza que no ocurrira Deadlock
* Tecnicas:
	* Denegear el inicio del proceso

### Tecnicas
Denegar el inicio del proceso: 


Algoritmo del Banquero:
Siempre esta en estado seguro

Puede definir dos diagnosticos:
Seguro
Inseguro

Estado del sistema:
* Vector de Recursos totales del sistema
* Vector de Recursos disponibles del sistema



Matriz de necesidades maximas declaradas por el proceso.


El algoritmo del banquiero **simula** la asignacion de recursos. Luego aplica un algoritmo de seguridad.

Algoritmo de seguridad:

Cuando buscamos estados seguros, con encontrar una secuencia segura ya esta. Puede ver mas de una secuencia segura. Con llegar a una, ya concluimos que hay en un estado seguro, si no encontramos ninguna, el estado es inseguro
**TIP:** Verificar que los recursos liberados al final del algoritmo, son iguales a los recursos totales.



3) Deteccino y recuperacion de Deadlock
* Puede ocurrir Deadlock
* No hay restricciones para asignar recursos disponibles
* Periodicamente se ejecuta el Algoritmo de Deteccion para determinar la existencia de Deadlock

Opciones de Recuperacion:
* Terminar los procesos involucrados
* Retroceder el proceso a un estado anterior
* Expropiar Recursos hasta que no exista Deadlock
* Terminar algun proceso involucrado hasta que deje de existir Deadlock


Criterios para seleccion de procesos para terminar o expropiar:
* Menor tiempo de procesador consumido
* Menor prioridad
* Menor numero total de recursos asignados
* Mayor tiempo restante estimado
* Menor cantidad de salida producida

Por default: Tratar de finalizar la menor cantidad de procesos

4) No hacer nada!
No soluciono nada
Pero tengo menos overhead (porque no estoy haciendo algoritmos todo el tiempo)