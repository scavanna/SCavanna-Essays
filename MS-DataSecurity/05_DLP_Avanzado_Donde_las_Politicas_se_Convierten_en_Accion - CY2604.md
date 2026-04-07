# DLP Avanzado: El Brazo Ejecutor de las Políticas

## Nivel 2 — Conceptos Clave

### Las Siete Superficies de Control: El Mapa Completo de la Contención

Cada superficie de control DLP cuenta con lógica propia, distintas acciones y modelo de licenciamiento. Comprender toda la arquitectura es fundamental para diseñar una estrategia sin brechas.

**Superficie 1 — Exchange Online (E3 + E5):** Inspecciona correos salientes en tiempo real. Acciones: bloquear envío, notificar, requerir justificación, enviar copia a supervisión, cifrar mensajes. Detecta contenido y etiqueta de sensibilidad en adjuntos, permitiendo interceptar mensajes con archivos “Altamente Confidenciales” [Fuente: Exchange Online DLP [Data Loss Prevention policy reference | Microsoft Learn](https://learn.microsoft.com/en-us/purview/dlp-policy-reference)

**Superficie 2 — SharePoint/OneDrive (E3 + E5):** Controla archivos en reposo y en movimiento. DLP actúa al compartir archivos sensibles con externos o enlaces anónimos. Acciones: bloquear compartición, notificar propietarios, restringir acceso hasta revisión [Fuente: SharePoint/OneDrive DLP [Data Loss Prevention policy reference | Microsoft Learn](https://learn.microsoft.com/en-us/purview/dlp-policy-reference)

**Superficie 3 — Microsoft Teams (E5 exclusivo):** Analiza mensajes, chats y canales en tiempo real, cubriendo interacciones con usuarios externos o invitados. Acciones: bloquear mensajes, notificar usuario, alertar equipo de seguridad [Fuente: Teams DLP: https://learn.microsoft.com/en-us/purview/dlp-microsoft-teams].

**Superficie 4 — Endpoint DLP en Windows/macOS (E5 exclusivo):** Actúa en operaciones físicas y digitales en el endpoint. Intercepta copias a USB, impresión, captura de pantalla, cargas en navegador, sincronización a apps personales, transferencia Bluetooth o uso de apps no autorizadas [Fuente: Endpoint DLP deployment requirements: https://learn.microsoft.com/en-us/purview/endpoint-dlp-learn-about].

**Superficie 5 — Browser DLP (E5, Edge gestionado):** Interviene en actividades web corporativas y aplicaciones SaaS accedidas vía Edge for Business con extensiones Purview. Acciones sobre uploads, "pegar" en formularios y descargas de datos sensibles [Fuente: Browser DLP documentation: https://learn.microsoft.com/en-us/purview/endpoint-dlp-using].

**Superficie 6 — Network DLP (E5 / adicional):** Protección a nivel red, independiza el control del agente de endpoint. Requiere licenciamiento adicional [Fuente: Network DLP overview: [Configure endpoint DLP settings | Microsoft Learn](https://learn.microsoft.com/en-us/purview/dlp-configure-endpoint-settings)

**Superficie 7 — Cloud Apps vía Defender for Cloud Apps (E5, incluye MDCA):** Visibilidad sobre uso de SaaS terceros, App Discovery y Inline Control para aplicar políticas DLP en tiempo real [Fuente: Defender for Cloud Apps overview: https://learn.microsoft.com/en-us/defender-cloud-apps/what-is-defender-for-cloud-apps].

### El Motor de Políticas Centralizado: Una Regla para Gobernarlas a Todas

El motor centralizado en purview.microsoft.com permite definir políticas unificadas para varias superficies. Al crear una política DLP, se seleccionan las superficies. Las acciones disponibles pueden variar, es importante ajustar por superficie [Fuente: Policy deployment per workload: https://learn.microsoft.com/en-us/purview/dlp-policy-reference].

**Estructura de una política DLP:** - Condiciones: Sensitivity Info Types (SITs), etiquetas de sensibilidad, destinatarios, contexto de la acción.

- Acciones: bloquear, bloquear con justificación, cifrar, notificar al usuario/administrador, generar alerta.
- Notificaciones/reportes: eventos de auditoría en Unified Audit Log, notificaciones configurables [Fuente: Unified Audit Log and DLP: https://learn.microsoft.com/en-us/purview/audit-log-search].

### Endpoint DLP en Profundidad: El Control Donde el Riesgo es Más Alto

Intercepta acciones a nivel sistema operativo antes de la transferencia de datos. Respuestas graduadas según sensibilidad de datos y perfil de usuario [Fuente: Configure responses in endpoint DLP: [Learn about Endpoint data loss prevention | Microsoft Learn](https://learn.microsoft.com/en-us/purview/endpoint-dlp-learn-about)

Data media a USB, usuario sin historial de riesgo → Auditar

- “Confidencial” a USB → Notificar y requerir justificación
- “Altamente Confidencial” a USB → Bloquear absolutamente
- Cualquier dato a USB, usuario de riesgo alto → Bloquear y alertar

Modelo disponible en GA para Windows, preview en macOS [Fuente: macOS Preview: [Configure endpoint DLP settings | Microsoft Learn](https://learn.microsoft.com/en-us/purview/dlp-configure-endpoint-settings)

### Adaptive Protection: El DLP que Aprende del Comportamiento

Integra Data Loss Prevention con Insider Risk Management [Fuente: Adaptive Protection overview: [Learn about Adaptive Protection in data loss prevention | Microsoft Learn](https://learn.microsoft.com/en-us/purview/dlp-adaptive-protection-learn)

Políticas estáticas pasan a ser dinámicas según niveles de riesgo: bajo, elevado, crítico. Subidas de riesgo detectadas por IRM hacen que las restricciones DLP se eleven automáticamente para ese usuario.

### Shadow AI como Caso Límite: El Nuevo Perímetro de Exfiltración

El uso de IA generativa externa (ChatGPT, Claude, etc.) es un nuevo vector de exfiltración. DLP lo aborda combinando Browser DLP (interceptando el “pegar” en apps IA no autorizadas) y Defender for Cloud Apps (descubriendo apps de IA y aplicando controles en tiempo real) [Fuente: Browser DLP documentation: https://learn.microsoft.com/en-us/purview/endpoint-dlp-using], [Fuente: Defender for Cloud Apps AI filtering: [Considerations for deploying Microsoft Purview Data Security Posture Management (DSPM) for AI | Microsoft Learn](https://learn.microsoft.com/en-us/purview/dspm-for-ai-considerations)

El objetivo de DLP es canalizar el uso de IA hacia herramientas gobernadas y seguras (como Copilot), no necesariamente prohibir el acceso a IA.

## Nivel 3 — Notas de Soporte / Insights de Profundidad

### Nota 1 — El Costo Humano del DLP Mal Calibrado

La fricción por umbrales bajos o acciones agresivas genera falsos positivos y disrupción. Se recomienda iniciar en modo auditoría y ajustar iterativamente según mejores prácticas.

### Nota 2 — DLP como Herramienta de Educación

Las notificaciones a usuarios pueden servir para microformación en tiempo real. Los textos de notificación son configurables y estratégicos para concientización [Fuente: Policy tips in DLP: https://learn.microsoft.com/en-us/purview/dlp-policy-tips-reference].

### Nota 3 — Integración con eDiscovery

Eventos y alertas DLP se capturan en el Unified Audit Log y se pueden usar como evidencia en eDiscovery Premium [Fuente: eDiscovery integration: [Set up compliance boundaries in eDiscovery | Microsoft Learn](https://learn.microsoft.com/en-us/purview/edisc-compliance-boundaries)

### Nota 4 — Relevancia en LATAM

Los casos de uso prioritarios varían por sector. El motor DLP permite adaptar reglas específicas según sector y tipo de dato.