# Capítulo 5: Arquitectura de Kernel

## Monolítico (Linux y Windows)

Primer modelo. Se comporta cómo **un solo proceso** que tiene varios módulos acoplados entre si.

* Todos los componentes ejecutando en **modo kernel**
* Desde cualquier módulo se puede acceder a los recursos compartidos
  
Ventajas:

* Procesamiento más rápido

Desventajas:

* Se vuelve grande, **complejo** y **dificil de mantener**.
* Cualquier falla en un módulo puede afectar a los otros módulos acoplados

## En capas

Va desde la capa de mas abajo (hardware) hasta la de más arriba (aplicaciones de ususario).

* Todo ejecuta **en modo kernel**
* Si un módulo se tiene que comunicar con otro, lo hace mediante la comunicación de sus **capas intermedias**
* Como mejora, se destaca que las capas tienen interfaces bien definidas, haciendo que el impacto en la modificaoin de un modulo no afectara tanto a los otros.

## MicroKernel (MacOS X, QNX)

Surge la idea de un Kernel que separa módulos en modo Kernel (Microkernel) y modo Usuario

* Módulos distribuidos e independientes
* La comunicación entre procesos de Usuario está distribuida por el microkernel (como una especie de broker)
* El Microkernel atiene las **interrupciones** y las **funciones de modo Kernel** (planificador, etc)
* Los módulos de usuario al fallar no afectan al Microkernel.
* Los módulos de usuario **son servicios** del **microkernel**
  
Las ventajas:

* Interfaces definidas (protocolos)
* Se le puede agregar funcionalidad con facilidad (nuevos servicios agregando un nuevo módulo de usuario)
* Si el microkernel falla, los otros modulos del Kernel pueden reactivarlo (más **robusto**)

Desventajas:

* Todo el envio de mensajes entre módulos que tienen que pasar por el Microkernel hacen que **baje el rendimiento**.

- - -

Proceso IDLE (SO): Proceso del SO que no hace nada para que el CPU tenga algo en ejecucion.