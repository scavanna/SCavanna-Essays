# Mínimo Privilegio y Control de Acceso

Gestión Dinámica de Permisos para Agentes de IA

El principio de mínimo privilegio —otorgar únicamente los permisos estrictamente necesarios para ejecutar una tarea— es uno de los pilares más antiguos de la ciberseguridad. También es uno de los más incumplidos. En la práctica, los permisos tienden a acumularse: se otorgan por comodidad, se heredan por inercia y rara vez se revocan cuando dejan de ser necesarios. Con usuarios humanos, esta dinámica genera riesgos significativos. Con agentes autónomos, los convierte en vectores de ataque de primera magnitud.
La llegada de la IA agéntica no crea un problema nuevo en este dominio. Lo amplifica de forma exponencial.

## Por Qué el Mínimo Privilegio es Más Difícil con Agentes

Tres factores convierten la gestión de privilegios en un desafío cualitativamente diferente cuando se aplica a agentes:

### **LOS TRES FACTORES DE COMPLEJIDAD**

1. Escala: una organización que despliega decenas o cientos de agentes multiplica el número de identidades con acceso a sistemas críticos en un factor que los procesos manuales no pueden gestionar.
2. Dinamismo: a diferencia de un usuario humano cuyo rol cambia pocas veces al año, un agente puede necesitar distintos permisos según la tarea que esté ejecutando en un momento dado, cambiando su perfil de acceso de forma continua.
3. Opacidad: los agentes no tienen una presencia visual en la organización. No están en el directorio, no aparecen en el organigrama, y su actividad no es intuitivamente interpretable para un analista humano.

El Propósito como Base del Control de Acceso
La respuesta a esta complejidad comienza antes de asignar un solo permiso: en la definición del propósito del agente. Un agente con un propósito claro y acotado es un agente cuyos permisos pueden definirse con precisión. Un agente con un propósito difuso termina acumulando accesos que nadie sabe justificar.
El proceso correcto sigue una lógica simple: primero definir qué resuelve el agente, luego identificar a qué recursos necesita acceder para cumplir ese propósito, y finalmente otorgar únicamente esos accesos. Un agente de triage de phishing necesita APIs de threat intelligence y acceso a la cola de reportes; no necesita acceso al CRM, ni a los sistemas de facturación, ni a los repositorios de código fuente.
"Cuando el propósito no está claro, los permisos se expanden sin control. El diseño del agente es el primer control de seguridad."

## Separación de Funciones: Ejemplos Concretos

La separación de funciones —que una misma entidad no pueda iniciar y aprobar una acción crítica— aplica a los agentes con la misma rigurosidad que a los humanos:

### **CAPACIDAD DEL AGENTE > LO QUE NO DEBE PODER HACER SOLO**

Escribir código / Aprobar sus propios pull requests
Analizar código / Hacer deployment a producción
Acceder a datos sensibles / Exportar esos datos externamente
Generar propuestas de pago / Aprobar y ejecutar los pagos
Clasificar correos de phishing / Modificar reglas del motor antispam

## Autorización Just-in-Time: Privilegios Temporales

Para responder al dinamismo de los agentes, cobra relevancia el modelo de autorización just-in-time: los privilegios se otorgan temporalmente para una tarea específica y se revocan automáticamente al finalizar. Este mecanismo no es nuevo —existe para usuarios humanos en herramientas como Privileged Identity Management— pero su aplicación a agentes requiere integración profunda entre tres sistemas:

### **INTEGRACIÓN REQUERIDA PARA JIT EN AGENTES**

- Sistema de identidad: que conozca el agente, su propósito y su contexto de ejecución actual.
- Motor de políticas: que evalúe si la solicitud de acceso es consistente con el propósito declarado del agente.
- Plataforma de ejecución: que pueda recibir instrucciones de acceso en tiempo real y revocar permisos sin interrumpir la ejecución cuando el acceso ya no es necesario.

## El Agente Hereda los Permisos de Quien lo Crea

Un principio de diseño práctico, defendido tanto desde la arquitectura de seguridad como desde la lógica operativa, es que un agente no debería poder acceder a recursos que su creador no puede acceder. Si quien construye el agente no tiene acceso a la base de datos de finanzas, el agente tampoco debería tenerlo.
Este principio resuelve una parte del problema, pero introduce una hipótesis crítica: que los permisos del usuario que crea el agente están bien gestionados. En la mayoría de las organizaciones, esa hipótesis es optimista. El punto de partida, entonces, no puede ser solo el agente: debe incluir una revisión del estado real de los privilegios humanos en la organización.

## Gobernanza de la Creación: Quién Puede Construir Agentes

Permitir que cualquier persona de la organización pueda construir agentes con acceso a sistemas críticos conduce a un estado que los equipos de seguridad conocen bien: anarquía de agentes, o agent sprawl. Más del 80% de las empresas Fortune 500 ya tienen agentes activos construidos con herramientas low-code/no-code por personas de áreas no técnicas.
La gobernanza de la creación de agentes debe ser explícita: definir quién puede construir qué tipo de agente, bajo qué proceso de aprobación, con qué límites de acceso predefinidos. No como un freno a la innovación, sino como la condición que hace posible escalarla de forma sostenible.

# Productos Microsoft que materializan este principio:

Entra Agent ID
Registro, autenticación y gestión de ciclo de vida de identidades de agentes. Integrado con ServiceNow y Workday para gobierno en sistemas de RRHH y IT.


Entra PIM
Privileged Identity Management para autorización just-in-time, aplicable a identidades no humanas con activación temporaria y auditoría automática.


Entra Suite
Acceso condicional unificado para usuarios y agentes, con políticas que evalúan identidad, dispositivo, comportamiento y contexto de ejecución.


Azure Key Vault
Bóveda de secretos para gestión segura de credenciales, tokens y API keys de agentes, con rotación automática y auditoría de accesos.

> "Los mismos principios de Zero Trust que aplican a empleados humanos aplican a los agentes de IA. Ahora se pueden usar las mismas herramientas para gestionar ambos." — Microsoft Security, 2026
>



 [Microsoft Security CSU MCSA - ZeroTrust Academy (E01) Zero Trust en la Era de los Agentes de IA + Ticket de salida.md](https://github.com/scavanna/SCavanna-PoV/blob/main/ZeroTrustAcademy/E01/Microsoft%20Security%20CSU%20MCSA%20-%20ZeroTrust%20Academy%20(E01)%20Zero%20Trust%20en%20la%20Era%20de%20los%20Agentes%20de%20IA%20%2B%20Ticket%20de%20salida.md)