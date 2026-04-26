---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art07-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art07-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art07-MS_CSU_Security_MCSA

Este documento explica cómo Microsoft Purview ayuda a gestionar investigaciones legales y de seguridad en Microsoft 365. Describe el flujo completo de eDiscovery Premium, desde la identificación de custodios hasta la exportación con cadena de custodia. Explica las capacidades avanzadas de Audit Premium y del evento Mail Items Accessed para reconstruir incidentes. Aclara la diferencia entre Legal Hold y las políticas de retención de ciclo de vida. Finalmente muestra cómo eDiscovery, auditoría e Insider Risk Management se integran para producir evidencia sólida y utilizable ante reguladores y tribunales. También incluye notas prácticas sobre costos, residencia de datos y roles internos.  

Primero se presenta el flujo legal end to end de eDiscovery Premium. Organiza un caso en seis estaciones técnicas: identificar, preservar, recolectar, revisar, analizar y exportar. Cada transición queda registrada y alimenta la cadena de custodia. Los custodios se vinculan a sus buzones, OneDrive, sitios y canales. Sobre esas fuentes se aplican holds y se ejecutan búsquedas y recolecciones selectivas. La evidencia viaja luego a Review Sets en Azure, dentro de la frontera de cumplimiento del tenant.  

El segundo eje es Audit Premium como registro continuo de evidencia. Amplía la retención de noventa días y ofrece hasta un año, o diez años con un complemento específico. Registra eventos de alto valor forense, como accesos a mensajes, descargas de archivos y búsquedas. Estos datos permiten reconstruir qué ocurrió y cuándo. Sin Audit Premium, eventos desaparecen antes de que se detecte la brecha. Eso obliga a trabajar con suposiciones, consultores externos y escenarios de peor caso.  

Un caso especial dentro de Audit Premium es el evento Mail Items Accessed. En un compromiso de correo, indica qué mensajes se leyeron, en qué momento y desde qué cliente. El equipo ya no infiere el daño; lo delimita con precisión. Puede identificar qué correos contenían datos regulados o credenciales sensibles. Así decide con fundamento si debe notificar a autoridades y clientes. También dimensiona mejor la remediación, sin asumir que todo el buzón quedó comprometido.  

Otro concepto central es la diferencia entre Legal Hold y las políticas de retención. Las políticas gobiernan el ciclo de vida de la información y suelen aplicarse a ubicaciones completas. Buscan minimizar datos y cumplir plazos regulatorios, con eliminación automática. El Legal Hold es reactivo y selectivo. Se activa ante litigios o investigaciones. Congela contenido asociado a custodios o ubicaciones concretas. Tiene prioridad sobre la retención, siempre que se aplique antes de que ocurra la eliminación.  

Finalmente, el texto destaca el valor de integrar eDiscovery Premium, Audit Premium e Insider Risk Management. IRM detecta comportamientos anómalos. Audit aporta la narrativa forense detallada. eDiscovery convierte esos hechos en evidencia lista para uso legal, respetando residencia de datos y fronteras de cumplimiento. Para aprovechar todo, se recomienda un rol de eDiscovery Manager que conecte Legal, Compliance, TI y Seguridad. Este rol guía decisiones como usar retención de diez años solo cuando el sector lo exige.





# NIVEL 2: Conceptos Clave

## Concepto 1: El flujo legal end-to-end y las seis estaciones de la cadena de custodia

eDiscovery Premium organiza la investigación legal en seis fases que son estados técnicos del sistema con transiciones controladas y registro automático de cada acción. De esa progresión ordenada se deriva la cadena de custodia documentada, que es el activo de mayor valor legal del proceso. [`eDiscovery process`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-process)

### Estación 1: Identificar custodios y fuentes de datos

El primer paso es identificar:

- Custodios: personas clave cuyos datos son relevantes para el caso.
- Fuentes de datos: ubicaciones donde residen esos datos.

En eDiscovery Premium, los custodios son entidades gestionadas con su propio estado e historial. Para cada custodio se vinculan explícitamente sus fuentes de datos:

- Buzón de Exchange.
- OneDrive personal.
- Sitios de SharePoint relevantes.
- Canales de Teams en los que participa.  

Esta vinculación es la base para aplicar holds y realizar colecciones posteriores. [`Identificación en eDiscovery`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium#step-1-identify-custodians-and-locations)

### Estación 2: Preservar con Legal Hold custodial y non-custodial

Una vez identificados los custodios, se debe preservar su contenido para impedir su modificación o eliminación por usuarios o políticas automáticas:

- **Hold custodial**  
  Se aplica a las fuentes de datos asociadas a un custodio.  
  Ejemplo: cuando un buzón de Exchange está bajo hold, los mensajes eliminados por el usuario se mueven a una carpeta de preservación invisible para él, pero accesible para los investigadores. Las políticas de retención no pueden eliminar contenido bajo hold activo. [`Legal Hold` vs `Retención`](https://learn.microsoft.com/en-us/microsoft-365/compliance/microsoft-365-compliance-retention?tabs=exchange&view=o365-worldwide)

- **Hold non-custodial**  
  Se aplica a fuentes de datos organizacionales que no pertenecen a un individuo concreto, como:
  - Sitios de SharePoint de equipo.
  - Buzones compartidos.
  - Canales de Teams de proyecto.

El sistema también gestiona la **notificación de hold** a los custodios:

- Plantillas configurables de notificación.
- Seguimiento de confirmación de lectura.
- Recordatorios automáticos mientras dure la obligación de preservación.  

Este flujo de notificación es en sí mismo evidencia con valor legal. [`Notificaciones Hold`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-legal-hold-notifications)

### Estación 3: Recolectar con Collections inteligentes

Con los holds activos, el objetivo es recolectar solo el contenido relevante:

- Búsquedas sobre contenido preservado usando:
  - Palabras clave.
  - Rangos de fechas.
  - Tipo de archivo.
  - Remitente y destinatario.
  - Etiquetas de sensibilidad.
  - Presencia de adjuntos específicos.

En E5, las Collections ofrecen **estimación previa a la recolección**:

- Número aproximado de elementos que coinciden con la consulta.
- Volumen de almacenamiento estimado.  

Esto permite:

- Afinar criterios para reducir volumen.
- Anticipar el tamaño y costo del caso para el equipo legal. [`Collection estimate`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium#collection-estimates)

### Estación 4: Revisar en Review Sets como espacio de trabajo legal

Los Review Sets son el entorno controlado donde se almacena y revisa la evidencia:

- El contenido se copia desde su ubicación original a almacenamiento de Azure gestionado por Microsoft dentro de la frontera de cumplimiento del tenant.  
- La revisión no modifica los datos originales y el conjunto de evidencia permanece estable durante todo el proceso. [`Review Sets, data storage location`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium#data-storage-location)

Dentro de un Review Set se puede:

- Etiquetar documentos: relevante, no relevante, privilegiado, revisión adicional.
- Añadir notas y comentarios.
- Conceder acceso a abogados externos con permisos controlados, sujeto a políticas de acceso y cumplimiento. [`Add users`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium#add-users-to-an-ediscovery-premium-case)
- Exportar subconjuntos de documentos con todos sus metadatos.

### Estación 5: Analizar con inteligencia asistida por IA

En esta fase se hace visible la diferencia entre eDiscovery Standard (E3) y eDiscovery Premium (E5). E5 añade capacidades de análisis asistido por IA para acelerar y priorizar la revisión:

- **Threading de conversaciones**  
  Reconstruye hilos completos de correo y mensajes de Teams para revisar el contexto completo y evitar leer versiones duplicadas.

- **Near-deduplication**  
  Agrupa documentos sustancialmente idénticos para revisar una versión representativa en lugar de cada copia.

- **Análisis temático**  
  Agrupa documentos por temas o patrones semánticos, facilitando la navegación por temas de interés en lugar de solo por fecha.

- **Relevance scoring y predictive coding**  
  Tras etiquetar un subconjunto de documentos, el sistema aprende el patrón de relevancia del caso y prioriza el resto del contenido según su probabilidad de ser relevante. [`Predictive coding`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium-review?view=o365-worldwide#apply-predictive-coding)

### Estación 6: Exportar con cadena de custodia documentada

La exportación final incluye:

- Archivos en los formatos requeridos por los equipos legales o por tribunales.
- Metadatos asociados a la cadena de custodia:
  - Hash de integridad.
  - Timestamps de cada acción relevante.
  - Etiquetas aplicadas durante la revisión.
  - Historial de accesos al Review Set.  

Esta información es lo que convierte los archivos digitales en evidencia admisible, demostrando que no han sido alterados desde su preservación hasta su presentación. [`Export with chain of custody`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium#export-data)


## Concepto 2: Audit Premium como registro continuo de evidencia

Audit Premium complementa a eDiscovery Premium. Mientras eDiscovery organiza evidencia cuando se necesita, Audit Premium genera esa evidencia continuamente, registrando acciones significativas en Microsoft 365 con detalle suficiente para reconstruir eventos con precisión forense. [`Audit log investigations`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-investigations)

La diferencia con Audit Standard (E3) no es solo la duración de retención. Cambian también el tipo de eventos registrados y la capacidad de consulta.

### Dimensión 1: Retención extendida

- **Audit Standard (E3)**  
  Retención típica de 90 días para la mayoría de los eventos.

- **Audit Premium (E5)**  
  - Un año de retención para usuarios con licencia E5.  
  - Opción de extender a 10 años mediante un add-on para sectores con requisitos de retención prolongada, como:
    - Servicios financieros.
    - Salud.
    - Sector público. [`Retention add-on`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-long-term-retention)

Esta diferencia es decisiva en investigaciones que se inician meses después del evento.

### Dimensión 2: Eventos de alto valor forense

Audit Premium registra eventos adicionales que son críticos en investigaciones de seguridad y cumplimiento, entre ellos:

- **`MailItemsAccessed`**  
  Registra qué mensajes de correo fueron accedidos, leídos o sincronizados, con:
  - Timestamp preciso.
  - Identificación del cliente o protocolo usado (Outlook, navegador, IMAP, app de terceros).  

  Es clave en investigaciones de compromiso de cuentas de correo. Sin este evento solo es posible estimar qué pudo ver un atacante. Con él se sabe qué mensajes específicos fueron accedidos. [`MailItemsAccessed`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-mailitemsaccessed)

- Otros eventos exclusivos o enriquecidos:
  - `Send` con mayor granularidad sobre mensajes enviados.
  - `SearchQueryInitiated` para búsquedas en Exchange y SharePoint.
  - `FileDownloaded` con detalle sobre descargas en OneDrive y SharePoint. [`Audit Premium events`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-premium-activities)

### Dimensión 3: API con mayor capacidad

Audit Premium ofrece:

- Límites superiores de throughput en la API de auditoría.
- Mejor soporte para:
  - Extracción masiva de eventos en ventanas de tiempo reducidas.
  - Consultas concurrentes durante respuestas a incidentes.

Esto permite investigaciones más ágiles cuando se manejan grandes volúmenes de datos. [`Audit API throughput`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-search-api)


## Concepto 3: `MailItemsAccessed` en investigaciones de compromiso de correo

El evento `MailItemsAccessed` es el ejemplo más claro de por qué Audit Premium es una capacidad operativa crítica para organizaciones con perfil de riesgo medio o alto. [`MailItemsAccessed`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-mailitemsaccessed)

### Escenario típico

Un ejecutivo financiero es víctima de un ataque de spear phishing y entrega sus credenciales de Microsoft 365. El atacante accede a su buzón de Exchange durante cuatro días antes de ser detectado.

La organización debe responder, como mínimo, dos preguntas:

1. **Desde la perspectiva legal y regulatoria**  
   ¿Se accedió a correos con datos de clientes sujetos a notificación obligatoria bajo GDPR, LGPD u otra regulación? Si hubo exposición, puede existir una ventana de 72 horas para notificar al regulador.

2. **Desde la perspectiva de seguridad**  
   ¿Se accedió a correos con credenciales, información de arquitectura interna o datos que permitan movimiento lateral? ¿El incidente está realmente contenido?

### Sin `MailItemsAccessed` (Audit Standard)

- Solo se puede determinar que la cuenta fue comprometida y aproximar el periodo de acceso.
- No es posible saber qué mensajes específicos se leyeron.
- El equipo suele asumir el peor caso:
  - Todo el contenido del buzón se considera potencialmente comprometido.
  - Se amplía el alcance de notificación a clientes y reguladores.
  - La respuesta de seguridad se dimensiona en exceso.  

Esto puede generar costos regulatorios, de reputación y de remediación muy superiores a los necesarios.

### Con `MailItemsAccessed` (Audit Premium)

- El equipo extrae todos los eventos `MailItemsAccessed` en la ventana de compromiso.
- Obtiene la lista exacta de mensajes accedidos:
  - Identificador de cada mensaje.
  - Timestamp del acceso.
  - Cliente o canal utilizado.

Con esa lista se revisa:

- If alguno de esos mensajes contenía datos sujetos a notificación.
- Si incluían credenciales, información sensible de arquitectura o datos de alto impacto.

La organización puede así:

- Determinar con precisión el alcance real del incidente.
- Justificar ante reguladores por qué notifica o decide no notificar.
- Ajustar la respuesta de seguridad al riesgo real en lugar de al peor caso.

### Principio general

`MailItemsAccessed` ejemplifica cómo Audit Premium transforma la respuesta a incidentes:

- De estimaciones probabilísticas basadas en indicios.
- A conclusiones apoyadas en evidencia forense detallada.


## Concepto 4: Legal Hold frente a políticas de retención

Una confusión frecuente y costosa es tratar los Legal Holds de eDiscovery y las políticas de retención de Data Lifecycle Management como herramientas intercambiables. En realidad difieren en propósito, alcance y comportamiento técnico. [`Retention vs Hold`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-legal-hold?view=o365-worldwide)

### Políticas de retención

- Naturaleza: proactivas y organizacionales.
- Alcance típico: ubicaciones completas (por ejemplo, todos los buzones de Exchange o todos los sitios de SharePoint).
- Objetivo: minimización de datos y cumplimiento de períodos de retención regulatoria.
- Funcionamiento:
  - Definen durante cuánto tiempo se retiene el contenido.
  - Tras ese periodo, ejecutan eliminación automática si así se configura.  

Responden a la pregunta:  
**“Por cuánto tiempo retenemos este tipo de contenido en condiciones normales de operación”**.

### Legal Holds de eDiscovery Premium

- Naturaleza: reactivas y selectivas.
- Se aplican cuando existe:
  - Anticipación razonable de litigio.
  - Investigación activa.
  - Requerimiento regulatorio específico.
- Alcance típico:
  - Custodios concretos.
  - Ubicaciones específicas relevantes para un caso.

Objetivo:

- Preservar indefinidamente contenido potencialmente relevante para una disputa o investigación, con independencia de las políticas de retención vigentes.

Responden a la pregunta:  
**“Cómo garantizamos que el contenido asociado a este custodio o ubicación no se elimine mientras dure esta obligación legal”**.

### Interacción técnica crítica

Cuando un buzón o ubicación está bajo Legal Hold:

- Las políticas de retención que normalmente eliminarían contenido no pueden eliminar contenido sujeto a hold.
- El sistema resuelve el conflicto en favor de la preservación:
  - La prioridad es el Legal Hold.
  - El contenido se conserva hasta que el hold se levanta explícitamente. [`Holding vs Retention`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-legal-hold?view=o365-worldwide)

Esto solo funciona si el hold se aplica **antes** de que se cumpla la condición de eliminación de la política de retención. Por tanto:

- Confiar solo en políticas de retención para preservar evidencia futura es insuficiente.
- La combinación correcta es:
  - Políticas de retención para gobierno de datos en condiciones normales.
  - Legal Holds para preservar selectivamente evidencia mientras dura una obligación legal concreta.


## Concepto 5: Integración eDiscovery + Audit + Insider Risk Management

El máximo valor de eDiscovery Premium y Audit Premium se obtiene cuando se integran con otros componentes de Purview, en particular Insider Risk Management (IRM). Juntos forman un triángulo de evidencia donde cada vértice aporta una parte complementaria de la narrativa investigativa. [`IRM + eDiscovery`](https://learn.microsoft.com/en-us/microsoft-365/compliance/insider-risk-management)

### El triángulo en un escenario integrado

1. **Vértice IRM: detección de comportamiento anómalo**

   IRM detecta, por ejemplo, que un empleado en proceso de salida:

   - Descargó 2.3 GB de archivos en tres días.
   - Lo hizo en horarios inusuales.
   - Se conectó desde ubicaciones o dispositivos atípicos.

   El riesgo escala a nivel crítico, se genera una alerta de alta severidad y se solicita desanonimizar al usuario para iniciar una investigación formal.

2. **Vértice Audit Premium: narrativa forense detallada**

   Con la identidad ya conocida, el investigador usa el Unified Audit Log para:

   - Determinar qué archivos específicos fueron descargados, cuándo y desde qué dispositivo.
   - Revisar búsquedas realizadas en SharePoint y Exchange (`SearchQueryInitiated`).
   - Identificar accesos a buzones de terceros (`MailItemsAccessed` con acceso delegado).
   - Analizar operaciones de sincronización de OneDrive y otros patrones relevantes.

   El resultado es una reconstrucción detallada del comportamiento del usuario.

3. **Vértice eDiscovery Premium: evidencia preparada para uso legal**

   Si se confirma un posible incumplimiento contractual o una violación legal:

   - Se crea un caso de eDiscovery.
   - Se añade al empleado como custodio y se aplican Legal Holds sobre su buzón, OneDrive y otros repositorios relevantes.
   - Se construyen Collections basadas en:
     - Archivos y periodos identificados por Audit Premium.
     - Criterios adicionales del equipo legal.
   - Se cargan los datos en un Review Set para revisión y etiquetado.
   - Se exporta la evidencia con su cadena de custodia completa para uso en litigios, acciones disciplinarias o procesos regulatorios.

Este flujo muestra que los pilares de Purview funcionan como un sistema cohesionado:  
- IRM genera la señal inicial.  
- Audit Premium proporciona la narrativa forense.  
- eDiscovery Premium convierte esa narrativa en un paquete de evidencia legalmente utilizable.


# NIVEL 3: Notas de soporte e insights de profundidad

## Nota 1: Costo real de no disponer de Audit Premium durante una brecha

Organizaciones que operan solo con Audit Standard suelen descubrir sus limitaciones en plena respuesta a incidentes:

- Logs limitados a 90 días:
  - Si el incidente se detecta o investiga 4 meses después, los eventos clave pueden haber desaparecido.
- Ausencia de `MailItemsAccessed`:
  - Imposibilita delimitar qué correos fueron efectivamente accedidos en compromisos de correo.

Consecuencias típicas:

- Mayor dependencia de consultores forenses externos.
- Semanas adicionales de análisis con costos elevados.
- Decisiones basadas en escenarios de peor caso por falta de datos, lo que incrementa:
  - Alcance de notificación a clientes y reguladores.
  - Volumen de medidas de remediación.

Para organizaciones medianas y grandes, la probabilidad de tener al menos un incidente que requiera análisis forense en un periodo de 12 meses es significativa, lo que hace de Audit Premium una medida justificable en términos de costo de riesgo.

## Nota 2: Fronteras de cumplimiento y soberanía de los datos de evidencia

Los Review Sets de eDiscovery Premium almacenan los datos:

- En Azure, gestionado por Microsoft.
- Dentro de la región y frontera de cumplimiento del tenant de la organización. [`Review Set compliance`](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-premium#data-storage-location)

Esto es relevante para organizaciones sujetas a:

- Requisitos de residencia de datos.
- Normativas de protección de datos que limitan transferencias internacionales de información (por ejemplo, GDPR, LGPD y normativas locales de protección de datos en América Latina).

Ventajas:

- La evidencia se mantiene bajo las mismas garantías de soberanía y cumplimiento que el resto del contenido de Microsoft 365.
- Reduce la necesidad de exportar grandes volúmenes de datos a soluciones de eDiscovery externas ubicadas en otras jurisdicciones, lo que podría crear problemas de cumplimiento adicionales.

## Nota 3: Rol del equipo legal y el eDiscovery Manager

Aunque eDiscovery Premium es una solución técnicamente avanzada, su obstáculo principal suele ser organizacional:

- Equipos legales:
  - Conocen los requisitos de litigio y regulación.
  - A menudo desconocen detalles técnicos de Microsoft 365 y Purview.

- Equipos de TI o seguridad:
  - Conocen la plataforma y sus capacidades.
  - No siempre comprenden requerimientos procesales y probatorios de los casos legales.

Las implementaciones más efectivas suelen contar con un rol específico:

- **eDiscovery Manager**  
  Persona o equipo que:
  - Entiende el flujo legal de eDiscovery y las obligaciones de preservación.
  - Conoce las capacidades y límites técnicos de Microsoft 365.
  - Opera como punto de unión entre Legal, Compliance, TI y Seguridad. [`eDiscovery Manager role`](https://learn.microsoft.com/en-us/microsoft-365/compliance/permissions?view=o365-worldwide#ediscovery-roles)

Sin este rol, es frecuente que las capacidades avanzadas de eDiscovery Premium y Audit Premium queden infrautilizadas.

## Nota 4: El add-on de retención de 10 años como decisión sectorial

El add-on de retención de 10 años de Audit Premium no es necesario en todas las organizaciones:

- Sectores donde suele ser crítico:
  - Servicios financieros sujetos a regulaciones que exigen conservar comunicaciones durante 7 a 10 años.
  - Proveedores de salud con requisitos de conservación de historias clínicas y registros médicos electrónicos.
  - Entidades gubernamentales con obligaciones de archivo y acceso público prolongado. [`Retention add-on`](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-long-term-retention)

- Para muchas otras organizaciones:
  - La retención de 1 año de Audit Premium es suficiente para cubrir la mayoría de escenarios de investigación y litigio.
  - La extensión a 10 años debe justificarse en requisitos regulatorios concretos y en un análisis de riesgo y costo, no solo en la idea genérica de que más retención siempre es mejor.

La decisión debe tomarse combinando:

- Exigencias regulatorias.
- Riesgo de litigios de largo plazo.
- Costos de almacenamiento y gestión asociados a una retención ultra prolongada.
