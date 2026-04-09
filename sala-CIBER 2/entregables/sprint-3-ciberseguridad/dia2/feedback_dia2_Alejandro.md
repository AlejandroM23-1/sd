## Sección A — Verificación de Persistencia
Tras ejecutar el diagnóstico de control, se confirma que las directivas de seguridad implementadas en la jornada anterior permanecen íntegras. 
El servicio mantiene su restricción a la interfaz de bucle local (loopback) y la capa de autenticación continúa operativa, validando la persistencia de las 
configuraciones tras el reinicio del sistema.

## Sección B — Análisis de Superficie de Exposición
Durante la inspección de puertos, se identificaron múltiples servicios nativos de Windows en estado de escucha (listening). Se observó que aplicaciones de terceros,
como Steam, mantienen un binding en la dirección 0.0.0.0, lo que representa una exposición mayor frente al aislamiento configurado para OpenClaw.

## Sección C — Aislamiento de Red y Firewall
Al aplicar las restricciones de red, se verificó la interrupción del acceso a periféricos (impresora WiFi) y la sincronización con dispositivos móviles. Este
comportamiento es el resultado esperado de la política de bloqueo de conexiones entrantes, confirmando que el host se encuentra en modo de "sigilo" dentro de la red
del campus.

## Sección D — Auditoría de Credenciales (Tokens)
Tras realizar un escaneo exhaustivo de los archivos del proyecto y del historial de versiones, se descarta cualquier riesgo de exposición de la API Key. No se 
detectaron fugas de secretos en el código fuente ni rastros de texto plano en los metadatos de Git.

## Sección E — Gestión de Secretos en Entornos de Contenedores
Debido al uso de Docker, el flujo de configuración del archivo .env se ha optimizado. Ahora es imperativo declarar la ruta de las variables de entorno dentro del 
manifiesto de despliegue antes de instanciar el contenedor; de lo contrario, el servicio de OpenClaw no podrá inyectar las credenciales necesarias para su correcto 
funcionamiento.

## Sección F — Higiene de Seguridad y Control de Versiones
Se han establecido dos protocolos fundamentales para el mantenimiento del proyecto:

Rotación de Claves: Se adopta la rotación periódica como medida preventiva para invalidar posibles filtraciones silenciosas y mitigar riesgos financieros.

Gestión de Índices en Git: Se validó que, mediante el comando git rm --cached, es posible excluir archivos sensibles del seguimiento de versiones sin afectar su 
disponibilidad local. Esto permite mantener la integridad del repositorio remoto sin sacrificar la operatividad del entorno de desarrollo.
