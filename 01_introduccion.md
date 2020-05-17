**Resumen sobre Sistemas Operativos**, Leonel Bazán Otin.
# Capitulo 1: Introducción a los Sistemas Operativos.

# Conceptos básicos de los sistemas operativos.
## Arquitectura del computador

### Firmware
**Software de arranque**, tiene directa interacción con el hardware. Generalmente se almacena en la [ROM(Memoria de solo lectura)](https://es.wikipedia.org/wiki/Memoria_de_solo_lectura).

**Funciones:**
* **Inicializa** desde los **registros del CPU**, hasta los **controladores de los dispositivos**.
* **Carga el kernel del SO en memoria**, para otorgarle el control.

### Memoria de acceso aleatorio (RAM)
Es la **única área de almacenamiento** a la cual puede acceder el CPU **directamente**.

El procesador puede:
* Mover **palabras**(cadenas finitas de bits) **desde la RAM** y cargarlas a sus **registros** mediante la instrucción **LOAD**.
* Mover el contenido de sus registros **hacia la RAM** mediante la instrucción **STORE**.

### Controladora de Memoria
Se encarga de administrar los ciclos de memoria y gestionar el acceso, ya que los dispositivos y la CPU **compiten para acceder a la memoria**

### Jerarquía de memoria
Es la organización piramidal de la memoria en niveles. Los criterios son **velocidad**, **coste** y **capacidad**.
![jerarquia-de-memoria](https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/Jerarquia_memoria.png/450px-Jerarquia_memoria.png)
## Ciclo del CPU (Arquitectura Von Neumann)
Busqueda(en la memoria) - Decodificacion - Ejecucion - PC +1 

* Se extrae una instruccion de memoria y se almacena en el Registro de Instrucciones(MDR).  
* Se decodifica la instruccion  
* Se ejecuta la instruccion  
* Se almacena el resultado de la operacion en memoria  


## Registros del CPU

### Registros Visibles por el usuario
>Generalmente son registros de datos, de direccion de memoria y codigos de condicion.

Registros de Datos  

Registros de Codigos de Condicion  

Registros de Direccion:  
* Punteros de segmento: Apunta al inicio de un segmento
* Registro Indice: Contiene un indice que se suma a una base del segmento, para obtener la direccion completa   
* Puntero de pila: Apunta a la cima de una pila

### Registros de Control y Estado  
>Controlan el funcionamiento del CPU

Registros de Control:
 
* RDIM (Reg. de direccion de Memoria): Especifica la direccion de memoria de la siguiente lectura/escritura.
* RDRAM (Reg. de datos de Memoria): Contiene los datos que se leen/escribiran en memoria
* PC (Program Counter): Direccion de la proxima instruccion a leer.
* IR (Reg. de Instruccion): Contiene la ultima instruccion leida.
* TODO: agregar RDIE/S RDAE/S



Registro de condiciones y estado:
* PSW (Program Status Word): Contiene codigos de Condicion y de Estado(Manejo de Interrupciones y Modo de Usuario)


# Interrupciones
>Mecanismos por el cual otros modulos(Memoria y E/S) pueden interrumpir el secuenciamiento normal del procesador.

### Tipos de Interrupciones

* De programa: Resultado de la ejecucion de una instruccion(desbodamiento, division por cero,etc).
* Por temporizador: Permite al SO realizar ciertas funciones de forma regular
* De E/S: Generada por un controlador  de E/S para indicar el exito o el error de una operacion.
* Por fallo de Hardware: Ejemplo un fallo en la energia.

### Fase de Interrupcion
* Esta fase se ejecuta despues de completarse una instruccion.
* Comprueba interrupciones pendientes y determina si seran atendidas.
* En caso de atenderse, se ejecuta el Manejador de Interrupciones.


### Manejador de iterrupciones
> Es una parte del Sistema Operativo, la rutina a utilizar variara segun la naturaleza de la interrupcion.


### Clasificacion de Interrupciones
* Enmascarables: Se pueden deshabilitar. Pueden ser postergadas (ej: una lectura de disco). 
* No Enmascarables: Siempre habilitadas. No pueden ser postergadas (ej: TODO).
* Sincronas: Provocadas por instrucciones internas de un programa
* Asincronas: Provocadas por eventos externos al programa. Su objetivo es notificar al SO algun cambio en el ambiente.


### Secuencia de una Interrupcion:
* En el procesador:
    * Se genera interrupcion
    * Finaliza instruccion actual (No se puede interrumpir la instruccion ACTUAL)
    * Determina si se atiende interrupcion
    * Guarda el PC y PWS con el estado actual (En realidad TODOS los registros)
    * Carga el manejador de interrupciones
* En el SO:
    * Guarda la informacion del procesador 
    * Procesa Interrupcion
    * Restaura informacion del procesador guardada
    * Restaura PC y PWS

### Multiples interrupciones:
Existe un manejador de interrupciones que ejecuta las instrucciones en orden segun un criterio  
Criterio de manejo de Interrupciones: Por prioridad o por fecha de llegada(secuencial)


# Sistemas operativos

## Sistema Operativo (SO)
>Programa que se ejecuta continuamente en la computadora(kernel).

Vista del Usuario: Utilizacion de Recursos.  
Vista del Sistema: Asignador de Recursos de Hardware y Programa de Control de los programas de usuario. 

## Syscalls(Llamadas al sistema):
Funciones incluidas en el kernel del SO. (ej: write)  
Permiten el acceso a recursos que el programa no puede acceder.  
Algunas syscalls (TODO ¿o todas?) incluyen instrucciones privilegiadas que solo son permitidas al SO.

## Wrappers:
Funciones, que llaman syscalls del SO para el cual se compilo el programa.  
Permiten una abstraccion al usuario de la syscall a usar.  
(ej: printf usa un syscall para linux y otro para windows)

## Modos de Ejecucion del procesador:
* Modo Kernel: 
    * Ejecuta instrucciones privilegiadas y NO privilegiadas(ej: apagar pc, habilitar interrupcion)
    * Solo las puede ejecutar el SO.
* Modo Usuario:
    * Solo ejecuta instrucciones NO priviligiadas. (ej: sumar, restar, etc) 
    * Las pueden ejecutar el usuarios.

Si un programador necesita que el programa ejecute una instruccion privilegiada,
le tiene que pedir al sistema operativo que lo haga mediante una syscall.

## Cambio de modo en el procesador:
* Se producen cuando el procesador pasa de modo Usuario a modo Kernel y biseversa.
* El modo Kernel del procesador ocurre cuando hay una interrupcion o una syscall.
