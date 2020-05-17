**Resumen sobre Sistemas Operativos**, Leonel Bazán Otin.

# Capítulo 1: Introducción a los Sistemas Operativos.
Conceptos básicos para entender los sistemas operativos.

**Arquitectura del Computador**
* Firmware
* Memoria Ram
* Controladora de Memoria
* Jerarquía de Memoria
* Ciclo de Instrucción
* Registros del CPU
* Interrupciones

**Sistemas Operativos**
* Sistemas Operativos
* Kernel
* Llamadas al sistema (syscalls)
* Envoltorios (Wrappers)
* Modos de ejecución (Kernel y Usuario)


# Arquitectura del computador

## Firmware
**Software de arranque**, tiene directa interacción con el hardware. Generalmente se almacena en la [ROM(Memoria de solo lectura)](https://es.wikipedia.org/wiki/Memoria_de_solo_lectura).

**Funciones:**
* **Inicializa** desde los **registros del CPU**, hasta los **controladores de los dispositivos**.
* **Carga el kernel del SO en memoria**, para otorgarle el control.

## Memoria de acceso aleatorio (RAM)
Es la **única área de almacenamiento** a la cual puede acceder el CPU **directamente**.

El procesador puede:
* Mover **palabras**(cadenas finitas de bits) **desde la RAM** y cargarlas a sus **registros** mediante la instrucción **LOAD**.
* Mover el contenido de sus registros **hacia la RAM** mediante la instrucción **STORE**.

### Controladora de Memoria
Se encarga de administrar los ciclos de memoria y gestionar el acceso, ya que los dispositivos y la CPU **compiten para acceder a la memoria**

## Jerarquía de memoria
Es la organización piramidal de la memoria en niveles. Los criterios son **velocidad**, **coste** y **capacidad**.
![jerarquia-de-memoria](https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/Jerarquia_memoria.png/450px-Jerarquia_memoria.png)


## Ciclo de instrucción (Arquitectura Von Neumann)
Es el **período que tarda** la CPU en **ejecutar una instrucción** de lenguaje máquina.
Se basa en la primicia **fetch-decode-execute**, es decir:

* **Buscar:** Se extrae una instrucción de memoria y se almacena en el **Registro de  Datos de Memoria** [(MDR)](https://es.wikipedia.org/wiki/Registro_MDR)
* **Decodificar** la instrucción.
* **Ejecutar** la instrucción.
* (Si corresponde, almacenar el resultado de la operación en memoria).



## Registros del CPU
Un registro es una memoria de alta velocidad y poca capacidad, integrada en el microprocesador, que permite guardar transitoriamente y acceder a valores muy usados, generalmente en operaciones matemáticas.

### Tipos de Registros

### * Visibles por el Usuario:
Generalmente son registros de:

* **Datos**: se usan para guardar números enteros.
* **Códigos de Condición**: contiene indicadores o flags.
* **Dirección**:
	* Punteros de segmento: Apunta al inicio de un segmento.
	* Registro índice: Contiene un índice que se suma a una base del segmento, para obtener la dirección completa.
	* Puntero de pila: Apunta a la cima de una pila

### * Control y Estado:
Controlan el funcionamiento del CPU, no los ve el usuario.

Los **Registros de Control** son los registros de:

* **Program Counter** (PC): Dirección de la próxima instrucción a leer.
* **Instrucción** (IR): Contiene la última instrucción leída.
* **Dirección de Memoria** (RDIM): Especifica la dirección de memoria de la siguiente lectura/escritura.
* **Datos de Memoria** (RDRAM): Contiene los datos que se leen/escribirán en memoria
* **TODO**: agregar RDIE/S RDAE/S

**Registro de estado**:
* **Palabra de estado del programa** (PSW): Contiene información sobre el estado de un programa. Además contiene un campo de error y un código de condición.


## Interrupciones
Son mecanismos por el cual otros módulos(Memoria y E/S) pueden interrumpir la secuencia normal del procesador.  
Todas las interrupciones **están dirigidas al procesador**.

### Tipos de Interrupciones

* **De programa**: Resultado de la ejecución de una instrucción(desbordamiento, división por cero,etc).
* **Por temporizador**: Permite al SO realizar ciertas funciones de forma regular.
* **De E/S**: Generada por un controlador  de E/S para indicar el éxito o el error de una operación.
* **Por fallo de Hardware**: Ejemplo un fallo en la energía.

### Ciclo de Instrucción: Fase de Interrupción
* Esta fase se ejecuta **DESPUÉS DE COMPLETARSE LA INSTRUCCIÓN**.
* Comprueba interrupciones pendientes y **determina si serán atendidas**.
* En caso de atenderse, se ejecuta el [**Manejador de Interrupciones**](https://en.wikipedia.org/wiki/Interrupt_handler)

### Manejador de interrupciones y múltiples interrupciones
El **Manejador de Interrupciones** es una parte del Sistema Operativo. La **rutina** que utilizará **variará según la naturaleza de la interrupción**.

En caso de **múltiples interrupciones**, el manejador las ejecutará en orden según un criterio determinado. El criterio puede ser por **prioridad** o por **fecha de llegada**(secuencial)



### Clasificación de Interrupciones
* **Enmascarables**: Se pueden deshabilitar. Pueden ser postergadas (ej: una lectura de disco).
* **No Enmascarables**: Siempre habilitadas. No pueden ser postergadas (ej: fallo crítico de energía).
* **Síncronas**: Provocadas por instrucción es internas de un programa
* **Asíncronas**: Provocadas por eventos externos al programa. Su objetivo es notificar al SO algún cambio en el ambiente.


### Secuencia de una Interrupción:
En el **procesador**:
* Se **genera** interrupción
* **Finaliza** instrucción actual (No se puede interrumpir la instrucción ACTUAL)
* **Determina** si se atiende interrupcion
* **Guarda** el PC y PWS con el estado actual (En realidad TODOS los registros)
* Carga el **manejador de interrupciones**

En el **SO**:
* **Guarda** la información del procesador
* **Procesa** Interrupción
* **Restaura** PC, PWS y los demás registros.


* * *

# Sistemas operativos

## Sistema Operativo (S.O.)
Es el software más importante de una computadora.

Desde la perspectiva del sistema, es un **asignador de recursos de Hardware** y maneja el **control de los programas de usuario**.  
Desde la perspectiva del usuario, **permite la utilización de los recursos**.


## Kernel
Es el **núcleo del Sistema Operativo**, y la parte del software que ejecuta en **modo privilegiado**.  
Entre muchas cosas, se encarga de **permitir la conexión de los periféricos** al sistema, **administrar la memoria** para que se utilice de manera eficiente, **planificar los procesos** y asignarles tiempo de procesamiento, etc.  
Además es importante mencionar la existencia **modo kernel** y un **modo usuario** tanto en el **procesador** cómo en el **sistema operativo**, como una doble proteccion.

## Llamadas al sistema (Syscalls)
Son **funciones pertenecientes al kernel**, incluyen **instrucciones privilegiadas** que **solo son permitidas al SO**.  
Mediante estas syscalls, los programas de usuario pueden **pedir el acceso a los recursos privilegiados**.  
Ejemplo: write(Permite la escritura en un [descriptor de archivo](https://es.wikipedia.org/wiki/Descriptor_de_archivo))


## Envoltorios (Wrappers)
Funciones que **encapsulan** a las syscalls. Permiten simplificar lógica y abstraer al usuario sobre que syscall usar(ya que las syscall pueden variar de un S.O. a otro)

Ejemplo: printf (que imprime en pantalla) usa una syscall para linux (write) y otra para windows


## Modos de ejecución (CPU)
#### Modo Kernel:
* Ejecuta tanto instrucciones **privilegiadas** cómo **no privilegiadas**. (Ej: Acceder al hardware, Habilitar interrupción, etc)
* **Solo las puede ejecutar el S.O**.

#### Modo Usuario:
* Solo ejecuta instrucciones **no privilegiadas**. (Ej: sumar, restar, acceder a zonas de memoria asignadas, etc)
* Las puede ejecutar el usuario.

Si un programador necesita que el programa ejecute una instrucción privilegiada, le tiene que pedir al sistema operativo que lo haga mediante una **syscall**.  


#### Cambio de modo (CPU):
Se producen cuando el procesador pasa de **modo usuario** a **modo kernel** y viceversa.
El CPU **pasa a modo kernel** cuando ocurre una **interrupción** o una **syscall**.


* * *
Ésta es una recopilación de apuntes tomados del curso de Sistemas Operativos, de la Universidad Tecnológica Nacional (UTN), Factultad Regional Buenos Aires.

Además se incluyen datos obtenidos de las siguientes **referencias**:

https://es.wikipedia.org/wiki/Ciclo_de_instrucci%C3%B3n

https://es.wikipedia.org/wiki/Registro_MDR

https://es.wikipedia.org/wiki/Registro_(hardware)

https://postparaprogramadores.com/wrapper-informatica/

https://www.youtube.com/watch?v=6EI3Ig7efWY
