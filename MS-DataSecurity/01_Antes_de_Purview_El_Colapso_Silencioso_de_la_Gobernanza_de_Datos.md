# Silos, Visibilidad Limitada, Fragmentación y Riesgo: El Escenario Pre-Purview

### Silos de Políticas — "El Idioma Roto"

En el modelo pre-Purview, las organizaciones operaban con al menos dos universos de políticas desconectados:

  - Las reglas de seguridad de Microsoft 365 (Exchange, SharePoint, Teams) residían en el portal de cumplimiento de M365 y estaban diseñadas para proteger la productividad y colaboración [[https://learn.microsoft.com/en-us/purview/microsoft-365-compliance-center](https://learn.microsoft.com/en-us/purview/microsoft-365-compliance-center)].
  - Las políticas de gobernanza de Azure, orientadas a datos estructurados, existían en Azure Portal, con lógicas y administradores separados [https://learn.microsoft.com/en-us/azure/azure-sql/](https://learn.microsoft.com/en-us/azure/azure-sql/).

Un documento clasificado como "Confidencial" en SharePoint podía migrar a una base de datos de Azure SQL sin arrastrar la señal de sensibilidad. Aunque Microsoft Information Protection permite extender etiquetas más allá de servicios M365, el proceso no era automático ni uniforme [https://learn.microsoft.com/en-us/purview/sensitivity-labels](https://learn.microsoft.com/en-us/purview/sensitivity-labels).

La consecuencia era que un mismo dato podía estar sobreprotegido en un entorno y menos protegido en otro, según qué equipo lo gestionara y qué herramienta aplicara las políticas.

-----

### Visibilidad Limitada — "El Dato Sin Pasaporte"

En arquitecturas fragmentadas, rastrear el ciclo completo de un dato era imposible. Un archivo podía originarse en el buzón de un ejecutivo, ser transferido a Teams, descargado a dispositivos personales y cargado en servicios externos, sin registro unificado de su trayecto [Learn about Microsoft Purview Data Map | Microsoft Learn](https://learn.microsoft.com/en-us/purview/data-map)

  - **Seguridad:** Sin linaje de datos, era inviable identificar el alcance de una brecha, requiriendo investigaciones manuales prolongadas.
  - **Cumplimiento:** Responder a una DSAR (Data Subject Access Request) implicaba búsquedas manuales en múltiples sistemas, con alta probabilidad de respuestas incompletas.
  - **Operación:** La falta de visibilidad sobre ubicación de datos sensibles impedía aplicar retención y eliminación coherente, acumulando riesgos y costos [https://learn.microsoft.com/en-us/purview/retention](https://learn.microsoft.com/en-us/purview/retention).

-----

### Fragmentación de Gobernanza — "El Archipiélago de Consolas"

Los equipos de seguridad debían navegar entre múltiples consolas de administración con nociones y flujos incompatibles [[https://learn.microsoft.com/en-us/purview/microsoft-365-compliance-center](https://learn.microsoft.com/en-us/purview/microsoft-365-compliance-center)]

  - Exchange Admin Center para reglas de correo.
  - SharePoint Admin para permisos y sitios.
  - Azure Portal para datos estructurados.
  - Herramientas de terceros para registros, eDiscovery o monitoreo.
  - Hojas de cálculo manuales para consolidación.

El resultado: un archipiélago de islas administrativas sin puentes, donde coordinar incidentes que cruzaban sistemas requería comunicación manual y alta probabilidad de pérdida de contexto en transferencias. Ejemplo: retener documentos de un empleado bajo investigación forzaba acciones manuales en varios sistemas, sin garantía de coordinación ni de captura completa [https://learn.microsoft.com/en-us/purview/retention](https://learn.microsoft.com/en-us/purview/retention).

-----

### Riesgo de Incumplimiento Normativo — "La Auditoría como Ruleta Rusa"

Las organizaciones rara vez tenían visibilidad consolidada de su cumplimiento [https://learn.microsoft.com/en-us/purview/compliance-manager](https://learn.microsoft.com/en-us/purview/compliance-manager).

Preguntas frecuentes imposibles de responder con precisión:

  - ¿Qué documentos contienen datos personales sin clasificar?
  - ¿Se eliminan datos de ex-empleados en plazos legales?
  - ¿Puede algún administrador de Microsoft acceder a datos sensibles sin ser detectado?
  - ¿Qué datos sensibles han salido de la organización recientemente?

El resultado era un riesgo regulatorio activo: multas bajo GDPR pueden alcanzar 4% de la facturación global [https://learn.microsoft.com/en-us/compliance/regulations/gdpr](https://learn.microsoft.com/en-us/compliance/regulations/gdpr). Además, existen riesgos de daño reputacional y exposición jurídica.

En auditorías externas, las evidencias debían reconstruirse manualmente, exponiendo a la organización a sanciones inesperadas.

-----

## Notas de Soporte

### Costo Oculto de la Fragmentación

Cada regulación nueva obligaba a implementar controles en sistemas independientes, creando más configuración, más validaciones y más capacitación, alzando el costo operativo y la complejidad [Microsoft Purview Compliance Manager | Microsoft Learn](https://learn.microsoft.com/en-us/purview/compliance-manager)El Dato No Estructurado como Epicentro

La mayoría del dato corporativo es no estructurado: correos, documentos, mensajes de chat, grabaciones. Las herramientas de gobernanza tradicionales estaban diseñadas para datos estructurados (bases de datos, tablas). El crecimiento de Microsoft 365 —especialmente Teams— amplió la brecha entre producción y gobernanza de este tipo de dato [Secure data with Zero Trust | Microsoft Learn](https://learn.microsoft.com/en-us/security/zero-trust/deploy/data)  /  [Implement your Zero Trust strategy for data protection by using Microsoft Purview | Microsoft Learn](https://learn.microsoft.com/en-us/purview/zero-trust-microsoft-purview)  

-----

### La Trampa de la Falsa Certeza

El control fragmentado daba confianza engañosa. Las políticas de retención parecían cubrir todos los canales hasta que una auditoría mostraba huecos, como mensajes de Teams no gestionados. Las reglas DLP no bloqueaban la exfiltración si existían endpoints fuera de gestión centralizada [https://learn.microsoft.com/en-us/purview/retention](https://learn.microsoft.com/en-us/purview/retention).

-----

### Relevancia en LATAM

La simultaneidad de marcos regulatorios en Latinoamérica eleva la complejidad: Ley 25.326 (Argentina), LGPD (Brasil), Ley 19.628 (Chile) y otros, además de requerimientos internacionales (GDPR). Microsoft reconoce que la gestión multinorma en ambientes fragmentados representa un reto significativo y costoso [https://learn.microsoft.com/en-us/purview/compliance-manager](https://learn.microsoft.com/en-us/purview/compliance-manager).

-----

## Conexiones Externas

| Conexión                                                     | Bloque Destino  | Naturaleza de la Conexión                                    |
| ------------------------------------------------------------ | --------------- | ------------------------------------------------------------ |
| Los 4 problemas identificados son exactamente los que la unificación de abril 2022 vino a resolver | **→ Bloque 02** | Causa-efecto directo: este bloque es el "por qué", el Bloque 02 es el "cómo" |
| El Purview Data Map como respuesta al problema de visibilidad limitada | **→ Bloque 03** | El corazón técnico compartido nace de la necesidad de linaje de datos unificado |
| La imposibilidad de responder preguntas de cumplimiento conecta con Compliance Manager | **→ Bloque 09** | Zero Trust y postura de cumplimiento son la respuesta arquitectónica al riesgo normativo |
| El dato no estructurado como epicentro del problema anticipa la necesidad de Information Protection | **→ Bloque 04** | La mayoría del dato corporativo es el dominio específico de Information Protection |

