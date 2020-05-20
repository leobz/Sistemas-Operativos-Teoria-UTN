# Capítulo 7: Deadlock

## Repaso de conceptos previos (Cortar y pegar en sus respectivos capítulos)

* El PCB siempre se conserva en memoria (Datos, Pila, Heap pueden estar en disco)

* Un proceso puede pasar a exit desde cualquier estado

* En el diagrama de 7 estados se puede dividir en :
  * Corto plazo: (Ready, Running y Blocked)
  * Medio Plazo: (Bocked Supended, Read Suspended)
  * Largo plazo: (New y Exit)

* Con el uso de hilos:
  * El mismo proceso tiene **ejecución concurrente**.
  * Se obtiene la ventaja de la ejecución **asincrónica**

* En los ejercicios con hilos ULT asumiremos como normal que no usan jacketing.

* La espera activa es **poco eficiente**. En algunos casos puede bloquear un proceso indefinidamente.

* Las soluciones de hardware son deseables, ya que al desactivar las interrupciones dejan
a un monton de procesos sin posiblididad de usarlas. Ademas solo pueden usarse en modo kernel

* Hay algunas instrucciones de hardware como las internas al procesador que son atómicas y permiten hacer la reserva del area critica en una sola instruccion (Test and set).

* Los **semáforos** son estructuras que tienen un valor numerico y una cola de procesos bloqueados.  
Los funciones que se les aplican son wait y signal.

* **Tipos de semáforos**: Contador , binarios(solo estados 0 y 1), mutex (para proteger estructuras o variables compartidas, se inician siempre en 1)

* **Semáforos fuertes**: Al desbloquear un proceso usan algun algoritmos para ver cual es el proximo proceso a ejecutar.

* **Semáforos débiles**: Al desbloquear un proceso desbloquean cualquiera

* Adentro del semáforo también se usa Mutua Exclusión

* Los monitores son soluciones que brindan los lenguajes de programción (generalmente los orientados a objetos)

# DeadLock

Interbloqueo entre dos o más procesos

**Definición**: Bloqueo permanetne de un conjunto de procesos, donde cada uno necesita de un recurso del otro.

**En el dedlock**:

* Los procesos se quedan en la cola de bloqueados
* Al pasar a bloqueados no usan el procesador

**En el LiveLock**:

* Los procesos pasan  de ready/blocked a ejecutando todo el tiempo, haciendo un loop infinito, y al llegar a ejecutando no realizan ninguna instrucción
* Al usar constantemente el procesador no permiten a otros procesos usar el procesador.

## Tipos de Recursos

* Reutilizables (Ej: Archivo)
* Consumibles (Ej: Cuando el consumidor saca un elemento del buffer)

Nosotros vamos a hacer incapié en deadlock en recursos reutilizables

## Condiciones para existencia de un deadlock

Se **deben cumplir todas** estas:

* **Mutua exclusión** entre procesos
* **Retención y espera** (Cada uno tiene recurso asignado que retiene y además espera un recurso del otro)
* **Sin desalojo de recursos** (Nadie puede sacarle un recurso asignado a un proceso)
* **Espera circular** (VER)

## Uso de Grafos para detectar Deadlocks

El grafo nos indicará que hay deadlock si y solo si:

* Si hay un ciclo, **PUEDE** haber un Deadlock
* Si hay un ciclo, y todos los recursos son de **una instancia**, entonces **HAY** Deadlock

## 4 Maneras de tratar al Deadlock

### 1) Prevencion de Deadlock

* Garantiza que no ocurrira Deadlock
* Impedir que se produzca la Mutua exclusion

**Soluciones** para:

* Problema de mutua exclusión:
  * **No generar Mutua Exclusión** de recursos(puede haber race-condition)
  
* Problema de Retencion y Espera:
  * Solicitar todos los recursos juntos (Esto puede causar inanicion en otros procesos)
  * Solicitar los recursos de uno o varios, utilizarlos y liberarlos

* Problema de desalojo de recuros:
  * Si un proceso que tiene recursos asignados solicita uno que no esta disponible, debe liberar sus recursos(puede haber inanicion)
  * Si un proceso A solicita un recurso que esta asignado a otro proceso B que esta a la espera de mas recursos. El recurso asignado al proceso B puede asignarse al proceso A.

* Problema de: Espera circular:
  * Asignar un numero de orden a los recursos. Los recursos solo pueden solicitarse en orden creciente.(puede haber inanicion)

### 2) Evasión o Predicción de Deadlock

* Garantiza que no ocurrira Deadlock
* Tecnicas:
  * Denegear el inicio del proceso
  * **Algoritmo del Banquero**: Siempre esta en estado seguro

### Algoritmo del Banquero

El algoritmo del banquiero **simula** la asignacion de recursos. Luego aplica un algoritmo de seguridad.

Puede definir dos diagnosticos:

* Seguro
* Inseguro

Estado del sistema:

* Vector de Recursos totales del sistema
* Vector de Recursos disponibles del sistema

#### Algoritmo de seguridad

Cuando buscamos estados seguros, con encontrar una secuencia segura ya esta.  
Puede ver mas de una secuencia segura. Con llegar a una, ya concluimos que hay en un estado seguro, si no encontramos ninguna, el estado es inseguro

**TIP:** Verificar que los recursos liberados al final del algoritmo, son iguales a los recursos totales.

### 3) Detección y recuperación de Deadlock

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

### 4) No hacer nada!

No soluciono nada, ¿cuál sería la ventaja? Tengo menos overhead (porque no estoy haciendo algoritmos todo el tiempo)
