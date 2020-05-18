# Arquitectura de Kernel

### Monolitico (Linux y Windows)
Primer modelo. Era un solo proceso que tenia varios modulos acoplados entre si.
	*Todos ejecutando en modo kernel
	* Desde cualquier modulo se podia acceder a los recursos compartidos, etc.
	* Se vuelve grande, complejo y dificil de mantener.

### En capas
Va desde la capa de mas abajo (hardware) hasta la de mas arriba (aplicaciones de ususario).
	*Todo ejecuta en modo kernel. 
	*Si un modulo se tiene que comunicar con otro lo hace mediante la comunicacion con sus capas intermedias.
	*Como mejora se destaca que las capas tenian interfaces bien definidas, haciendo que el impacto en la modificaoin de un modulo no afectara tanto a los otros.

### MicroKernel (MacOS X, QNX)
Surge la idea de un Kernel que separa modulos en modo Kernel y modo Usuario
	* Modulos distribuidos e independientes
	* La comunicacion entre procesos de Usuario estaba distribuida por el microKernel (como una especie de broker)
	* MicroKernel atiene las interrupciones y las funciones de modo Kernel (planificador, etc)
	* Los modulos de usuario al fallar no afectan al microKernel. Los modulos de usuario son servicios del microKernel
	* Las ventajas: 
		* Interfaces definidas(protocolos)
		* Se le puede agregar funcionalidad con facilidad (nuevos servicios agregando un nuevo modulo de usuario)
		* Si el microKernel falla, los otros modulos del Kernel pueden reactivarlo(mas robusto)
	* Desventajas:
		* Todo el envio de mensajes entre modulos que tienen que pasar por el MicroKernel hacen que baje el rendimiento.


###

Proceso IDLE (SO): Proceso del SO que no hace nada para que el CPU tenga algo en ejecucion.