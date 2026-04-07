Sección A — Diagnóstico de Exposición
Al desplegar OpenClaw mediante Docker, observamos que la configuración inicial sugería el uso de la interfaz 0.0.0.0
para facilitar el acceso desde el navegador. Aunque esta configuración permite que el servicio sea visible para cualquier 
dispositivo en la red local, el riesgo se ve mitigado parcialmente por el sistema de validación interno que exige un token para 
interactuar con la interfaz.

Sección B — Análisis de Interfaz de Red
Durante el escaneo, confirmamos que el servicio escucha en la IP 0.0.0.0. En un entorno de software tradicional, esto
representaría una vulnerabilidad crítica de exposición; sin embargo, en el contexto de Docker, es el comportamiento estándar 
necesario para que el tráfico del host pueda alcanzar el contenedor. Aun así, identificamos esto como un punto de mejora para
la seguridad del host.

Sección C — Validación de Autenticación (API vs UI)
Realizamos pruebas de intrusión en cinco rutas críticas del sistema. El resultado fue positivo: cuatro de ellas validaron
correctamente la identidad del usuario (Auth Real). La quinta ruta devolvió un error "not found", el cual clasificamos como un 
Falso Positivo, probablemente debido a cambios en los endpoints tras una actualización del software, y no a una falta de
protección.

Sección D — Verificación de Acceso Remoto
Confirmamos la exposición de la red mediante una prueba desde un dispositivo móvil. La página cargó correctamente debido a que 
el contenedor Docker actuaba como un puente abierto hacia la red WiFi. Esto demostró que, sin medidas adicionales, la superficie
de ataque se extendía más allá de nuestra máquina de trabajo.

Sección E — Implementación de Hardening Mínimo
Para elevar el nivel de seguridad, restringimos el acceso exclusivamente a la máquina local. En lugar de alterar la
configuración interna del archivo .json, aplicamos el endurecimiento directamente en la infraestructura de Docker.
Modificamos el archivo YAML definiendo el mapeo de puertos como 127.0.0.1:PUERTO:PUERTO. Con este cambio, el sistema operativo
ahora rechaza automáticamente cualquier intento de conexión proveniente de dispositivos externos.

Sección F — Conclusión y Mínimo Privilegio
Al aplicar el principio de mínimo privilegio a nivel de red, hemos reducido drásticamente la superficie de ataque. Al limitar
la visibilidad de OpenClaw, protegemos tanto la integridad de nuestros datos como los recursos de hardware y créditos de API,
garantizando que solo los usuarios autorizados en el host local puedan operar la herramienta.
