# **Guía Operativa Detallada: Microsoft Defender for Office 365**

## **Paso a Paso por Consola y Rol**



## **TAREAS DIARIAS**

### **1. Monitoreo de Cola de Incidentes**

**Roles:** Security Operator, Security Monitor, Incident Responder
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/incidents

**Paso a Paso:**

1. Acceder al portal de Microsoft Defender
2. Navegar:     **Investigación y respuesta > Incidentes y alertas > Incidentes**
3. Configurar     filtros:

○    **Estado:** Seleccionar "Nuevo" (New) y "En curso" (In progress)

○    **Período de tiempo:** Últimas 24-48 horas

○    **Gravedad:** Ordenar descendente (Alta → Media → Baja)

○    (Opcional) **Fuente de servicio:** Filtrar por "Microsoft Defender for Office 365"

1. Guardar     la vista personalizada para uso futuro
2. Revisar     columnas clave:

○    Severity (Gravedad)

○    Status (Estado)

○    Assigned to (Asignado a)

○    Tags (Etiquetas)



### **2. Triaje de Incidentes Críticos (Alta y Media Prioridad)**

**Roles:** Security Operator, Incident Responder
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/incidents

**Paso a Paso:**

1. Desde la cola de incidentes, **seleccionar incidentes con     gravedad Alta**
2. Hacer     clic en el incidente para abrir el panel lateral
3. **Asignar     el incidente:**

○    Click en "Assign to" → Seleccionar "Assign to me" o asignar a colega

1. **Cambiar     estado:**

○    Click en "Manage incident" → Cambiar de "New" a "In progress"

1. Revisar     las pestañas del incidente:

○    **Alerts:** Revisar todas las alertas correlacionadas

○    **Devices:** Verificar endpoints afectados

○    **Users:** Identificar cuentas comprometidas

○    **Mailboxes:** Ver buzones impactados

○    **Evidence and Response:** Analizar archivos, URLs, IPs

1. **Ejecutar     acciones de respuesta** según hallazgos:

○    Desde pestaña "Evidence and Response" → Seleccionar entidad → "Actions"

○    Opciones: Block URL, Delete email, Isolate device, etc.

1. **Clasificar     el incidente** antes de resolver:

○    Click en "Manage incident"

○    Seleccionar clasificación:

■    **True positive:** Especificar tipo de amenaza (Phishing, Malware, etc.)

■    **False positive:** Marcar como benigno

■    **Not enough information:** Para casos ambiguos

1. **Resolver     incidente:**

○    Cambiar estado a "Resolved"

○    Agregar comentarios sobre la remediación ejecutada

1. Repetir para incidentes de gravedad Media



### **3. Revisión de Envíos de Usuarios (User Reported Messages)**

**Roles:** Security Operator, Security Monitor
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/reportsubmission?viewid=user

**Paso a Paso:**

1. Navegar: **Acciones y envíos > Envíos > Reportado por     el usuario**
2. Revisar     la cola de mensajes reportados por usuarios
3. Analizar     cada envío:

○    Click en el mensaje para ver detalles

○    Revisar el **veredicto preliminar** del sistema:

■    "Should have been blocked" (Debió bloquearse)

■    "Clean" (Limpio)

■    "Phishing"

■    "Malware"

1. **Para     mensajes maliciosos no bloqueados:**

○    Seleccionar el mensaje

○    Click en "Submit to Microsoft for analysis"

○    Marcar como "Phishing" o "Malware"

○    **Activar:** "Mark as and notify" para enviar feedback al usuario

○    Click "Submit"

1. **Para     falsos positivos (mensajes legítimos bloqueados):**

○    Seleccionar mensaje

○    Click "Mark as not junk"

○    Submit to Microsoft

1. **Documentar patrones recurrentes**     para ajuste de políticas



### **4. Gestión del Centro de Acciones (Action Center) - Aprobación de Remediaciones AIR**

**Roles:** Security Operator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/action-center/pending

**Paso a Paso:**

1. Navegar: **Investigación y respuesta > Acciones y envíos     > Centro de acciones**
2. Click     en pestaña **"Pending"** (Pendiente)
3. Revisar     acciones en espera de aprobación:

○    Soft delete email

○    Hard delete email

○    Block URL

○    Block sender

○    Turn off external mail forwarding

1. **Para     cada acción pendiente:**

○    Click en la acción para ver detalles

○    Revisar:

■    **Investigation details:** Razón de la acción

■    **Evidence:** Capturas, análisis de detonación, IOCs

■    **Affected items:** Cantidad de mensajes/usuarios impactados

1. **Tomar     decisión:**

○    **Aprobar:** Si la evidencia es concluyente → Click "Approve"

○    **Rechazar:** Si es falso positivo → Click "Reject"

1. Verificar     pestaña **"History"** para confirmar ejecución
2. **Documentar acciones aprobadas/rechazadas** para auditoría



### **5. Gestión de Entidades Restringidas (Restricted Users)**

**Roles:** Security Operator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/restrictedentities

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Revisar     > Entidades restringidas**
2. Identificar     usuarios bloqueados por envío de spam
3. **Antes     de desbloquear, verificar:**

○    Revisar el incidente asociado al bloqueo

○    Confirmar con el usuario/IT que:

■    La contraseña fue cambiada

■    MFA está habilitada/actualizada

■    El dispositivo fue escaneado

1. **Ejecutar     desbloqueo:**

○    Seleccionar el usuario bloqueado

○    Click en "Unblock"

○    Revisar el panel de recomendaciones de seguridad

○    Confirmar que se implementaron las mitigaciones

○    Click "Yes, unblock"

1. **Documentar:**

○    Razón del bloqueo

○    Acciones de remediación tomadas

○    Hora del desbloqueo

1. Informar     al usuario que el desbloqueo puede tardar **hasta 60 minutos**
2. Monitorear el comportamiento de envío post-desbloqueo



### **6. Revisión de Campañas de Correo Entregadas**

**Roles:** Security Operator, Incident Responder
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/campaigns

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Campañas**
2. Filtrar     campañas con mensajes **entregados** (no bloqueados):

○    En columna "Delivery location" buscar "Inbox"

1. Identificar     campañas activas (últimas 24-48 horas)
2. **Para     cada campaña sospechosa:**

○    Click en la campaña para ver detalles

○    Analizar:

■    **Campaign type:** Phishing, Malware, Spam

■    **Targeted users:** Cantidad y perfiles

■    **Click rate:** Tasa de usuarios que hicieron clic

■    **Sender infrastructure:** IPs, dominios

1. **Ejecutar     remediación:**

○    Click en pestaña "Remediation"

○    Opciones:

■    Soft delete: Mover a elementos eliminados

■    Hard delete: Purga permanente

■    Move to junk: Mover a carpeta spam

○    Seleccionar acción y confirmar

1. **Extraer     IOCs para bloqueo preventivo:**

○    Copiar URLs, dominios, file hashes

○    Agregar a Tenant Allow/Block List (ver tarea ad-hoc)

1. Documentar campaña y acción tomada



### **7. Validación de Detecciones ZAP (Zero-hour Auto Purge)**

**Roles:** Security Operator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/threatexplorer

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Explorador**     (o Real-time detections si Plan 1)
2. Seleccionar     vista: **"All email"**
3. Configurar     filtros:

○    **Delivery action:** Seleccionar "Delivered"

○    **Latest delivery action:** Seleccionar "ZAP"

○    **Período:** Últimas 24 horas

1. Analizar     resultados:

○    Revisar volumen de mensajes eliminados por ZAP

○    Identificar patrones comunes (remitentes, asuntos, tipos de archivo)

1. **Para     mensajes con alto impacto:**

○    Click en el mensaje

○    Verificar si el usuario **leyó el mensaje antes de ZAP**

○    Revisar si hubo clics en URLs antes de la purga

1. **Evaluar     necesidad de ajuste de políticas:**

○    Si ZAP es muy frecuente → Considerar endurecer filtros anti-phishing

○    Si ZAP es tardío → Evaluar Safe Links/Safe Attachments

1. Documentar tendencias para revisión semanal



### **8. Monitoreo de Cuarentena (Quarantine)**

**Roles:** Security Operator, Security Monitor
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/quarantine

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Revisar     > Cuarentena**
2. Revisar     mensajes en cuarentena solicitados por usuarios
3. Filtrar     por:

○    **Quarantine reason:** Phishing, Malware, High confidence phishing

○    **Time received:** Últimas 24 horas

1. **Para     solicitudes de liberación de usuarios:**

○    Click en el mensaje

○    Revisar:

■    **Message headers:** Verificar SPF, DKIM, DMARC

■    **Sender reputation:** Verificar en Threat Intelligence

■    **URLs/Attachments:** Verificar en VirusTotal/análisis sandbox

1. **Tomar     acción:**

○    **Si es falso positivo:**

■    Click "Release message"

■    Seleccionar "Release to all recipients"

■    Activar "Report to Microsoft as false positive"

○    **Si es amenaza real:**

■    Click "Block sender"

■    Agregar a Tenant Block List

■    Notificar al usuario que es malicioso

1. Documentar decisiones para auditoría



### **9. Revisión de Top Targeted Users**

**Roles:** Security Operator, Incident Responder
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/threatexplorer

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Explorador**
2. Seleccionar     vista: **"Phishing"** o **"All email"**
3. Configurar:

○    **Período:** Últimas 24-48 horas

○    **Group by:** Recipient (Destinatario)

1. Ordenar     por cantidad de mensajes recibidos (descendente)
2. Identificar     usuarios en el **top 10**
3. **Para     cada usuario altamente atacado:**

○    Click en el usuario para ver detalles

○    Revisar:

■    Tipos de amenazas recibidas

■    Tasa de entrega vs. bloqueo

■    Si hicieron clic en enlaces maliciosos

1. **Acciones     preventivas:**

○    **Si el usuario es VIP/Ejecutivo:**

■    Agregar a "Priority Accounts" (ver configuración mensual)

○    **Si hay indicios de compromiso:**

■    Forzar cambio de contraseña

■    Revisar actividad en Azure AD Sign-ins

■    Verificar reglas de buzón (forwarding rules)

1. Documentar usuarios críticos para monitoreo continuo



### **10. Verificación de Patrones de Envío Sospechosos**

**Roles:** Security Operator, Incident Responder
 **Consola:** Microsoft Defender Portal + Exchange Admin Center
 **URLs:**

●    Defender: https://security.microsoft.com/threatexplorer

●    Exchange: https://admin.exchange.microsoft.com

**Paso a Paso:**

1. **En Microsoft Defender Portal:**

○    Navegar: **Correo electrónico y colaboración > Explorador**

○    Filtrar por:

■    **Sender domain:** [dominio de la organización]

■    **Directionality:** Outbound

■    **Período:** Últimas 24 horas

1. Buscar     anomalías:

○    Volumen de envío inusualmente alto

○    Destinatarios externos desconocidos

○    Asuntos genéricos (ej. "Re:", "Invoice", "Payment")

1. **En     Exchange Admin Center (para reglas de reenvío):**

○    Navegar: **Recipients > Mailboxes**

○    Buscar el usuario sospechoso

○    Click en el buzón → **Mail flow settings**

○    Revisar:

■    **Forwarding:** Verificar si hay reglas de reenvío a dominios externos

■    **Inbox rules:** Revisar reglas de eliminación automática

1. **Si     se confirma compromiso:**

○    Bloquear capacidad de envío (ver Entidades Restringidas)

○    Eliminar reglas maliciosas

○    Forzar reset de credenciales

○    Revisar logs de auditoría para entender el alcance



## **TAREAS SEMANALES**

### **1. Caza de Amenazas con Threat Explorer (Threat Hunting)**

**Roles:** Security Operator, Incident Responder
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/threatexplorer

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Explorador**
2. **Cacería     de evasiones (mensajes entregados sin detección):**

○    Vista: "All email"

○    Filtros:

■    **Delivery location:** Inbox

■    **Detection technology:** None

■    **Attachment:** .html, .htm, .js, .vbs (archivos sospechosos)

○    Período: Últimas 7 días

1. Analizar     resultados:

○    Click en mensajes sospechosos

○    Revisar contenido, remitentes, URLs

1. **Cacería     de malware específico:**

○    Vista: "Malware"

○    **Group by:** Malware family

○    Identificar familias prevalentes (ej. Emotet, Qakbot)

1. **Cacería     de phishing avanzado:**

○    Vista: "Phishing"

○    Filtros:

■    **Delivery location:** Inbox

■    **Phishing confidence level:** High

○    Buscar señuelos efectivos que pasaron filtros

1. **Búsqueda     por palabras clave (ingeniería social):**

○    Campo "Subject/Attachment name"

○    Términos: "Urgente", "Bono", "Factura", "Password reset", "Verify account"

1. **Extraer     IOCs para bloqueo:**

○    Export de resultados

○    Agregar URLs/dominios/hashes a Block List

1. **Crear     consultas avanzadas (Advanced Hunting):**

○    Para análisis más profundo, usar KQL en Advanced Hunting

**Consulta KQL de ejemplo para Advanced Hunting:**

EmailEvents

| where Timestamp > ago(7d)

| where DeliveryLocation == "Inbox"

| where ThreatTypes == "" or isempty(ThreatTypes)

| where AttachmentCount > 0

| where AttachmentFileExtension in (".html", ".htm", ".js")

| summarize Count=count() by SenderFromAddress, Subject, AttachmentFileExtension

| order by Count desc

 



### **2. Análisis de Tendencias de Detección (Email Detection Trends)**

**Roles:** Security Operator, Manager
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/reports/TPSEmailPhishReportV2

**Paso a Paso:**

1. **Informe de Estado de Protección contra Amenazas:**

○    Navegar: **Informes > Correo electrónico y colaboración > Estado de protección contra amenazas**

1. Configurar     período: **Últimos 7 días**
2. Analizar     métricas clave:

○    **Phishing blocked:** Cantidad de correos de phishing bloqueados

○    **Malware blocked:** Cantidad de malware bloqueado

○    **Spam blocked:** Volumen de spam detenido

1. **Comparar     con semana anterior:**

○    Cambiar período a "Previous 7 days"

○    Identificar incrementos o decrementos significativos (>20%)

1. **Revisar     efectividad por capa de protección:**

○    Gráfico de "Detection by technology"

○    Verificar:

■    Edge filtering (EOP)

■    Safe Attachments

■    Safe Links

■    ZAP

1. **Informe     de Estado de Flujo de Correo:**

○    Navegar: **Informes > Correo electrónico y colaboración > Estado del flujo de correo**

○    Verificar anomalías:

■    Retrasos en entrega

■    Fallos de entrega masivos

■    Incremento inusual en spam/malware

1. **Documentar     hallazgos:**

○    Crear reporte semanal para Manager

○    Identificar necesidad de ajuste de políticas



### **3. Revisión de Inteligencia de Amenazas (Threat Analytics)**

**Roles:** Security Operator, Incident Responder
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/threatanalytics3

**Paso a Paso:**

1. Navegar: **Inteligencia de amenazas > Análisis de amenazas**
2. Revisar     sección **"Analyst reports":**

○    Identificar amenazas emergentes (últimos 7 días)

○    Click en reportes relevantes para la industria

1. **Para     cada amenaza relevante:**

○    Leer resumen ejecutivo

○    Revisar sección **"Related incidents":**

■    Verificar si la organización fue impactada

○    Revisar **"Mitigations":**

■    Implementar recomendaciones si aplica

1. **Indicadores     de Compromiso (IOCs):**

○    Click en pestaña "IOCs"

○    Export de IOCs (IPs, dominios, hashes)

○    Bloquear proactivamente en Tenant Block List

1. **Advanced     Hunting queries:**

○    Click en pestaña "Hunting queries"

○    Copiar consultas KQL

○    Ejecutar en Advanced Hunting para buscar señales en el entorno

1. Documentar amenazas críticas para comunicación al Manager



### **4. Auditoría de Configuración (Configuration Analyzer)**

**Roles:** Security Administrator, Manager
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/configurationAnalyzer

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Políticas y     reglas > Políticas de amenazas > Analizador de configuración**
2. El     analizador compara automáticamente las políticas actuales con:

○    **Standard protection** (Protección estándar)

○    **Strict protection** (Protección estricta)

1. Revisar     cada sección:

○    **Anti-spam policies**

○    **Anti-phishing policies**

○    **Anti-malware policies**

○    **Safe Attachments**

○    **Safe Links**

1. **Identificar     desviaciones:**

○    Políticas marcadas en **naranja/rojo** indican configuración más débil que la recomendada

○    Click en cada política para ver diferencias específicas

1. **Evaluar     impacto de cambios:**

○    Click en "View recommendations"

○    Revisar qué ajustes se sugieren

○    **Evaluar con el negocio** antes de aplicar (riesgo de bloqueos legítimos)

1. **Aplicar     recomendaciones:**

○    Click "Apply recommendations" para políticas específicas

○    O ajustar manualmente:

■    Navegar a la política en cuestión

■    Editar configuración según recomendación

1. **Re-ejecutar     analyzer** post-cambios para confirmar     alineación
2. Documentar cambios en registro de configuración



### **5. Gestión de Inteligencia de Suplantación (Spoof Intelligence)**

**Roles:** Security Administrator, Security Operator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/spoofintelligence

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Políticas y     reglas > Políticas de amenazas > Spoof intelligence**
2. Revisar     remitentes que están suplantando el dominio de la organización
3. Analizar     columnas:

○    **Spoofed sender:** Dirección que se está suplantando

○    **Sending infrastructure:** IP/dominio real de origen

○    **Message count:** Volumen de mensajes

○    **User complaints:** Reportes de usuarios

○    **SPF, DKIM, DMARC:** Estado de autenticación

1. **Para     cada entrada:**

○    **Si es legítimo (ej. proveedor de marketing):**

■    Verificar autenticación (SPF/DKIM)

■    Click en entrada → Cambiar a "Allowed to spoof"

○    **Si es malicioso:**

■    Cambiar a "Not allowed to spoof"

■    Click "Block sender"

1. **Revisar     impersonation insights:**

○    Navegar: **Correo electrónico y colaboración > Políticas y reglas > Impersonation insights**

○    Verificar intentos de suplantar usuarios/dominios protegidos

1. Documentar decisiones de allow/block para auditoría



### **6. Revisión de Simulaciones de Ataque (Attack Simulation Training)**

**Roles:** Security Administrator, Manager
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/attacksimulator

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración >     Entrenamiento de simulación de ataque**
2. Click en     pestaña **"Simulations"**
3. Revisar     simulaciones activas de la última semana
4. **Para     cada simulación:**

○    Click en la simulación

○    Analizar métricas:

■    **Compromised rate:** % de usuarios que cayeron

■    **Reported rate:** % de usuarios que reportaron

■    **Completion rate:** % que completó el training asignado

1. **Identificar     grupos débiles:**

○    Click en "Users" dentro de la simulación

○    Ordenar por "Compromised" status

○    Identificar departamentos/roles con alta tasa de fallo

1. **Asignar     training correctivo:**

○    Seleccionar usuarios comprometidos

○    Click "Assign training"

○    Seleccionar módulo de la biblioteca

1. **Revisar     training campaigns:**

○    Click en pestaña "Training campaigns"

○    Verificar progreso de entrenamientos obligatorios

1. **Planificar     próxima simulación:**

○    Basado en amenazas reales observadas en Threat Explorer

○    Click "Launch a simulation"

○    Seleccionar técnica (ej. Credential Harvest si hubo muchos ataques de ese tipo)

1. Documentar resultados para reporte mensual al CISO



### **7. Auditoría de Reglas de Transporte (Mail Flow Rules)**

**Roles:** Security Administrator
 **Consola:** Exchange Admin Center
 **URL:** https://admin.exchange.microsoft.com/#/transportrules

**Paso a Paso:**

1. Navegar: **Mail flow > Rules**
2. Revisar     todas las reglas activas
3. **Buscar     configuraciones peligrosas:**

○    Reglas que bypasean spam filtering

○    Reglas que permiten dominios/remitentes amplios

○    Reglas que redirigen correos a externos

1. **Para     cada regla sospechosa:**

○    Click en la regla

○    Revisar:

■    **Created by:** Quién la creó

■    **Last modified:** Cuándo fue modificada

■    **Conditions:** Qué activa la regla

■    **Actions:** Qué hace la regla

1. **Verificar     justificación:**

○    Confirmar con el creador/negocio la necesidad

○    Si no hay justificación válida → Deshabilitar o eliminar

1. **Reglas     críticas a revisar:**

○    "Set SCL to -1" (bypass spam)

○    "Forward to" external domains

○    "Delete message silently"

1. Documentar reglas revisadas y acciones tomadas



### **8. Análisis de Priority Accounts**

**Roles:** Security Administrator, Security Operator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/userTags

**Paso a Paso:**

1. Navegar: **Settings > Email &     collaboration > User tags**
2. Click     en **"Priority accounts"**
3. Revisar     lista de usuarios etiquetados
4. **Actualizar     membresía:**

○    Agregar nuevos ejecutivos/VIPs

○    Remover usuarios que ya no califican

○    Click "Edit tag" → "Members" → Add/Remove users

1. **Verificar     protecciones especiales:**

○    Navegar a: **Políticas y reglas > Políticas de amenazas > Anti-phishing**

○    Verificar que exista política específica para Priority Accounts con:

■    **Impersonation protection:** Enabled

■    **Mailbox intelligence:** Enabled

■    **Advanced phishing thresholds:** Nivel más agresivo

1. **Revisar     reporte de usuarios atacados:**

○    Usar Threat Explorer

○    Filtrar por tag: "Priority account"

○    Verificar volumen y tipos de ataques

1. Documentar cambios en la lista



### **9. Revisión de Detection Overrides Report**

**Roles:** Security Administrator, Manager
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/reports/TPSEmailSecurityReportV2

**Paso a Paso:**

1. Navegar: **Informes > Correo electrónico y colaboración     > Email security report**
2. Click     en pestaña **"Overrides"**
3. Analizar     sobrescrituras que afectan detecciones:

○    **User overrides:** Configuraciones de usuarios finales (ej. Safe senders list)

○    **Organization overrides:** Allow lists, IP allow lists, ETR bypasses

1. **Identificar     patrones problemáticos:**

○    Alto volumen de mensajes permitidos por overrides

○    Dominios/IPs en allow lists que envían amenazas

1. **Tomar     acción:**

○    Eliminar entradas obsoletas de allow lists

○    Educar usuarios sobre no agregar remitentes a safe senders

○    Ajustar Exchange Transport Rules que bypasean filtros

1. Documentar overrides críticos y plan de limpieza



### **10. Validación de Conectores de Exchange**

**Roles:** Security Administrator
 **Consola:** Exchange Admin Center
 **URL:** https://admin.exchange.microsoft.com/#/connectors

**Paso a Paso:**

1. Navegar: **Mail flow > Connectors**
2. Revisar     conectores de tipo **"Inbound"** (recepción)
3. **Para     cada conector:**

○    Click en el conector

○    Verificar:

■    **Connector status:** Debe estar "On"

■    **Certificate validation:** Recomendado habilitado

■    **IP ranges:** Solo IPs autorizadas (ej. impresoras, app servers)

1. **Auditar     uso legítimo:**

○    Confirmar con IT que los conectores son necesarios

○    Verificar que las IPs listadas son actuales

1. **Buscar     configuraciones inseguras:**

○    Conectores que aceptan de "Any IP"

○    Conectores sin TLS enforcement

1. **Revisar     conectores outbound:**

○    Verificar que solo apps autorizadas pueden relay

1. Documentar conectores validados y cambios realizados



## **TAREAS MENSUALES**

### **1. Revisión Integral de Postura de Seguridad (Security Posture Review)**

**Roles:** Manager, Security Administrator, Architect
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/securescore

**Paso a Paso:**

1. **Revisar Microsoft Secure Score:**

○    Navegar: https://security.microsoft.com/securescore

○    Analizar puntuación actual vs. mes anterior

○    Meta: Incremento de al menos 5-10 puntos mensuales

1. **Filtrar     acciones de mejora:**

○    Click en "Improvement actions"

○    Filtrar por categoría: "Email & collaboration"

○    Ordenar por "Score impact" (descendente)

1. **Seleccionar     3-5 acciones de alto impacto:**

○    Ejemplos:

■    "Enable Safe Attachments for SharePoint, OneDrive, and Teams"

■    "Configure anti-phishing policies"

■    "Enable automated investigation and response"

1. **Planificar     implementación:**

○    Click en cada acción

○    Revisar:

■    **Implementation:** Pasos técnicos

■    **User impact:** Impacto en usuarios finales

■    **Related threats:** Amenazas que mitiga

1. **Ejecutar     cambios:**

○    Implementar las acciones seleccionadas

○    Documentar en plan de proyecto

1. **Re-evaluar Secure Score post-cambios:**

○    Las acciones pueden tardar 24-72 horas en reflejarse

○    Verificar incremento de puntuación

1. **Comparativa     con peers (benchmarking):**

○    Click en pestaña "Metrics & trends"

○    Comparar con organizaciones similares

1. **Crear     reporte mensual:**

○    Export de Secure Score history

○    Presentar a Manager/CISO



### **2. Ajuste Fino de Políticas Anti-Phishing**

**Roles:** Security Administrator, Architect
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/antiphishing

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Políticas y     reglas > Políticas de amenazas > Anti-phishing**
2. **Revisar     políticas existentes:**

○    Click en cada política

○    Verificar configuraciones clave:

■    **Impersonation protection:**

■    Users to protect (usuarios VIP)

■    Domains to protect (dominio corporativo)

■    **Spoof protection:** Enabled

■    **Advanced phishing thresholds:** Configurar según tolerancia

■    **Actions:**

■    If message is detected as impersonation: Quarantine

■    If message is detected as spoof: Quarantine

1. **Analizar     falsos positivos del mes:**

○    Navegar a: **Envíos > Reportado por el usuario**

○    Filtrar mensajes legítimos marcados como phishing

○    Identificar patrones (ej. newsletters, proveedores específicos)

1. **Ajustar     umbrales:**

○    Si muchos FP → Reducir agresividad

○    Si muchos FN (phishing entregado) → Incrementar agresividad

1. **Configurar     Mailbox Intelligence:**

○    En política anti-phishing

○    Habilitar "Enable mailbox intelligence"

○    Habilitar "Enable intelligence for impersonation protection"

○    Esto aprende patrones de comunicación normales del usuario

1. **Configurar     Safety Tips:**

○    Habilitar "Show first contact safety tip"

○    Habilitar "Show user impersonation safety tip"

1. **Probar     cambios:**

○    Usar Attack Simulation Training para validar

○    Monitorear User Reported Messages post-cambio

1. Documentar ajustes y razones



### **3. Gestión de Listas de Permitidos/Bloqueados (Tenant Allow/Block List Cleanup)**

**Roles:** Security Administrator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/tenantAllowBlockList

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Políticas y     reglas > Políticas de amenazas > Listas de permitidos y bloqueados     de inquilinos**
2. **Revisar     pestaña "Senders":**

○    Verificar remitentes en allow list

○    **Principio:** Allow list debe ser MÍNIMA (idealmente cero)

○    Para cada entrada:

■    Verificar justificación de negocio

■    Confirmar si sigue siendo necesaria

■    Eliminar entradas temporales que ya no aplican

1. **Revisar     pestaña "URLs":**

○    Verificar URLs bloqueadas

○    Confirmar que siguen siendo amenazas activas

○    Eliminar blocks de campañas antiguas (>90 días)

1. **Revisar     pestaña "Files" (hashes):**

○    Verificar hashes bloqueados

○    Mantener solo amenazas recurrentes

1. **Revisar     pestaña "Spoofed senders":**

○    Validar remitentes permitidos para spoofing

○    Confirmar autenticación SPF/DKIM

1. **Principios     de limpieza:**

○    Allow entries: Justificar cada 30 días o eliminar

○    Block entries: Mantener si la amenaza es recurrente

○    Preferir submit to Microsoft over allow list

1. **Documentar:**

○    Entradas eliminadas

○    Justificación para entradas mantenidas

1. Crear alerta para revisar mensualmente



### **4. Auditoría de Políticas de Cuarentena**

**Roles:** Security Administrator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/quarantinePolicies

**Paso a Paso:**

1. Navegar: **Correo electrónico y colaboración > Políticas y     reglas > Políticas de amenazas > Políticas de cuarentena**
2. Revisar     políticas existentes:

○    **AdminOnlyAccessPolicy:** Solo admins pueden ver/liberar

○    **DefaultFullAccessPolicy:** Usuarios pueden ver/liberar

○    **NotificationEnabledPolicy:** Usuarios reciben notificaciones

1. **Validar     alineación con modelo operativo:**

○    ¿Los usuarios deben poder liberar mensajes de phishing? (Riesgoso)

○    ¿Se envían notificaciones de cuarentena periódicas?

1. **Para     cada política:**

○    Click en política

○    Revisar:

■    **End-user spam notification frequency:** Cada cuánto se notifica

■    **Permissions:** Qué pueden hacer los usuarios (Preview, Release, Delete, Block sender)

1. **Mejores     prácticas recomendadas:**

○    **High confidence phishing:** AdminOnlyAccess (usuarios NO pueden liberar)

○    **Phishing:** Limited access (usuarios pueden ver pero NO liberar)

○    **Spam:** Full access (usuarios pueden gestionar)

○    **Malware:** AdminOnlyAccess

1. **Ajustar     políticas según necesidad:**

○    Click "Edit policy"

○    Modificar permisos

○    Guardar cambios

1. **Verificar     asignación de políticas:**

○    Revisar en políticas anti-spam/anti-phishing qué quarantine policy se usa

1. Documentar modelo de cuarentena aprobado



### **5. Programa de Attack Simulation Training - Análisis y Planificación**

**Roles:** Security Administrator, Manager
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/attacksimulator

**Paso a Paso:**

1. **Revisión de resultados del mes:**

○    Navegar: **Correo electrónico y colaboración > Entrenamiento de simulación de ataque > Overview**

○    Analizar dashboard:

■    **Recent simulations:** Últimas simulaciones ejecutadas

■    **Simulation coverage:** % de usuarios cubiertos

■    **Training completion:** % de trainings completados

1. **Análisis     de Repeat Offenders:**

○    Click en pestaña "Users"

○    Ordenar por "Simulations compromised" (descendente)

○    Identificar usuarios con 3+ fallos

1. **Asignar Training Campaigns a usuarios     débiles:**

○    Click en pestaña "Training campaigns"

○    Click "Create training campaign"

○    Configurar:

■    **Campaign name:** "Phishing Awareness Q1 2026"

■    **Target users:** Seleccionar repeat offenders

■    **Training modules:** Seleccionar de la biblioteca (ej. "How to identify phishing")

■    **Duration:** 30 días

■    **Completion requirements:** Mandatory

1. **Planificar     próximas simulaciones:**

○    Click "Simulations" → "Launch a simulation"

○    **Seleccionar técnica basada en amenazas reales:**

■    Si Threat Explorer mostró muchos ataques de credential harvesting → Usar esa técnica

■    Si hubo malware via attachments → Usar "Malware Attachment"

○    **Configurar payload:**

■    Usar payloads de la biblioteca global

■    O crear custom payload que imite amenazas específicas observadas

○    **Target users:**

■    Rotar entre departamentos para cobertura

■    Excluir usuarios simulados en últimos 30 días (fatiga)

1. **Configurar     Automation:**

○    Click "Automation"

○    Click "Create automation"

○    Configurar:

■    **Frequency:** Mensual

■    **Target:** Rotar entre grupos

■    **Techniques:** Mix de credential harvest, malware, link in attachment

1. **Crear     reporte mensual para CISO:**

○    Export de resultados

○    Incluir:

■    Tasa de compromiso global

■    Departamentos débiles

■    Mejora mes a mes

■    Plan de remediación

1. Documentar simulaciones planificadas



### **6. Revisión de Configuración OME (Office Message Encryption)**

**Roles:** Security Administrator, Compliance Officer
 **Consola:** Exchange Admin Center + Microsoft Purview
 **URLs:**

●    Exchange: https://admin.exchange.microsoft.com

●    Purview: https://compliance.microsoft.com

**Paso a Paso:**

1. **Verificar estado de OME:**

○    Navegar (Purview): **Data security > Information protection > Encryption**

○    Verificar que OME esté habilitado

1. **Revisar     reglas de transporte para encriptación:**

○    Navegar (Exchange): **Mail flow > Rules**

○    Identificar reglas que aplican encriptación (Modify the message security → Apply Office 365 Message Encryption)

1. **Para     cada regla:**

○    Verificar condiciones:

■    ¿Se encripta info sensible (PII, PHI, PCI)?

■    ¿Los destinatarios externos reciben encriptación?

○    Verificar que las etiquetas de sensibilidad (Sensitivity labels) están activando encriptación

1. **Probar     encriptación:**

○    Enviar correo de prueba con contenido sensible

○    Verificar que el destinatario externo recibe portal OME

○    Confirmar experiencia de desencriptación

1. **Revisar     branding de OME:**

○    Navegar (Purview): **Data security > Encryption > Branding**

○    Verificar que el portal OME tiene branding corporativo

1. **Verificar     logs de encriptación:**

○    Usar Message Trace para verificar eventos "Encrypt"

○    Confirmar que mensajes sensibles se encriptan

1. Documentar políticas de encriptación activas



### **7. Análisis de Feedback de Usuarios (User Reporting Trends)**

**Roles:** Security Administrator, Manager
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/reportsubmission?viewid=user

**Paso a Paso:**

1. Navegar: **Acciones y envíos > Envíos > Reportado por     el usuario**
2. **Analizar     tendencias del mes:**

○    Click en "Export" para descargar datos

○    Análisis en Excel:

■    **Volumen de reportes:** ¿Incrementó o decreció?

■    **Accuracy rate:** % de reportes que fueron realmente maliciosos

■    **Top reporters:** Usuarios más vigilantes

1. **Calcular     métricas:**

○    **True Positive Rate:** (Reportes maliciosos / Total reportes) * 100

○    **False Positive Rate:** (Reportes benignos / Total reportes) * 100

1. **Identificar     patrones:**

○    Si FP rate > 30% → Usuarios están reportando legítimos (necesitan educación)

○    Si TP rate > 70% → Usuarios son efectivos sensores

1. **Acciones     basadas en análisis:**

○    **Si FP rate es alto:**

■    Crear campaña de awareness sobre qué es phishing real

■    Ejemplificar con casos reales vs. falsos

○    **Si volumen de reportes es bajo:**

■    Promover el uso del botón de reporte

■    Gamificar (reconocer top reporters)

1. **Reconocer     usuarios vigilantes:**

○    Identificar top 10 reporters con alta accuracy

○    Reconocimiento público (con permiso)

1. **Ajustar     configuración de reporte:**

○    Navegar: **Configuración > Email & collaboration > User reported settings**

○    Verificar que el reporte va a buzón monitoreado

○    Configurar notificaciones automáticas a usuarios

1. Crear reporte de tendencias para Manager



### **8. Verificación de Integración SIEM (Microsoft Sentinel)**

**Roles:** Architect, Security Administrator
 **Consola:** Microsoft Sentinel
 **URL:** https://portal.azure.com/#view/Microsoft_Azure_Security_Insights

**Paso a Paso:**

1. **Verificar conector de Defender XDR:**

○    Navegar (Sentinel): **Configuration > Data connectors**

○    Buscar "Microsoft Defender XDR"

○    Verificar estado: **Connected**

1. **Configurar     sincronización de incidentes:**

○    Click en el conector

○    Click "Open connector page"

○    Verificar:

■    **Incident creation:** Habilitado

■    **Sync all incidents:** Seleccionado

■    **Bi-directional sync:** Habilitado (cambios en Sentinel reflejan en Defender y viceversa)

1. **Verificar     flujo de datos:**

○    Navegar (Sentinel): **Logs**

○    Ejecutar consulta KQL:

EmailEvents

| where Timestamp > ago(24h)

| summarize Count=count() by bin(Timestamp, 1h)

| render timechart

 

●    Verificar que hay datos recientes

1. **Revisar     tablas clave de MDO:**

○    EmailEvents

○    EmailUrlInfo

○    EmailAttachmentInfo

○    EmailPostDeliveryEvents

1. **Configurar     reglas de analítica:**

○    Navegar: **Configuration > Analytics**

○    Verificar que existen reglas para MDO:

■    "Suspicious email forwarding rules"

■    "Mass phishing campaign detected"

■    "User clicked malicious link"

1. **Probar     creación de incidente:**

○    Forzar un incidente en Defender (ej. simulación)

○    Verificar que aparece en Sentinel dentro de 5-10 minutos

1. **Revisar     workbooks de MDO:**

○    Navegar: **Threat management > Workbooks**

○    Buscar "Office 365" o "Email Security"

○    Validar visualizaciones

1. Documentar estado de integración



### **9. Revisión de Roles y Permisos (RBAC Audit)**

**Roles:** Security Administrator, Manager
 **Consola:** Microsoft Entra ID + Microsoft Defender Portal
 **URLs:**

●    Entra: https://entra.microsoft.com

●    Defender: https://security.microsoft.com/permissions

**Paso a Paso:**

1. **Auditoría de roles en Defender:**

○    Navegar (Defender): **Permissions > Email & collaboration roles**

○    Revisar grupos de roles:

■    Organization Management

■    Security Administrator

■    Security Operator

■    Security Reader

■    Data Investigator (para Search and Purge)

1. **Para     cada grupo de roles:**

○    Click en el grupo

○    Revisar membresía:

■    ¿Todos los miembros siguen requiriendo acceso?

■    ¿Hay usuarios inactivos?

○    Aplicar principio de **menor privilegio**

1. **Roles     sensibles a auditar:**

○    **Search and Purge:** Permite eliminar correos de buzones

■    Solo analistas senior deben tener este rol

○    **Organization Management:** Control total

■    Solo administradores principales

1. **Auditoría     de roles en Entra ID:**

○    Navegar (Entra): **Identity > Roles & admins > Roles**

○    Buscar:

■    Security Administrator

■    Global Administrator

■    Exchange Administrator

1. **Revisar     asignaciones:**

○    Click en cada rol

○    Click "Assignments"

○    Verificar que no hay asignaciones permanentes innecesarias

1. **Implementar Privileged Identity Management     (PIM):**

○    Para roles críticos, usar PIM para acceso just-in-time

○    Navegar (Entra): **Identity governance > Privileged Identity Management**

1. **Revisar     logs de auditoría de cambios de permisos:**

○    Navegar (Defender): **Auditoría**

○    Buscar: "Add member to role" en últimos 30 días

○    Verificar que todos los cambios fueron autorizados

1. Documentar membresías y justificaciones



### **10. Informe Ejecutivo Mensual al CISO/Board**

**Roles:** Manager, CISO
 **Consola:** Microsoft Defender Portal + PowerPoint/Word
 **URL:** https://security.microsoft.com

**Paso a Paso:**

1. **Recopilar métricas clave:**

○    **Incidents:**

■    Total incidentes del mes

■    Incidentes por severidad (High, Medium, Low)

■    MTTD (Mean Time to Detect)

■    MTTR (Mean Time to Respond/Resolve)

○    **Email Security:**

■    Total emails procesados

■    Phishing bloqueado

■    Malware bloqueado

■    Spam bloqueado

■    ZAP activations

○    **User Behavior:**

■    User reported messages

■    Attack simulation results (compromised rate)

■    Training completion rate

○    **Secure Score:**

■    Puntuación actual vs. mes anterior

■    Acciones de mejora implementadas

1. **Extraer     datos:**

○    Usar reports en Defender Portal

○    Export a CSV/Excel

○    Procesar en dashboard (Power BI o Excel)

1. **Crear     narrativa:**

○    **Sección 1: Resumen Ejecutivo**

■    Estado general de seguridad (Green/Yellow/Red)

■    Incidentes críticos del mes

■    Acciones de remediación clave

○    **Sección 2: Métricas de Protección**

■    Gráficos de tendencias

■    Comparativa mes a mes

○    **Sección 3: Threat Landscape**

■    Principales amenazas observadas

■    Campañas dirigidas

■    Nuevas TTPs observadas

○    **Sección 4: User Awareness**

■    Resultados de simulaciones

■    Training completado

■    Departamentos que requieren refuerzo

○    **Sección 5: Postura de Seguridad**

■    Secure Score progress

■    Políticas optimizadas

■    Configuraciones mejoradas

○    **Sección 6: Recomendaciones**

■    Inversiones propuestas

■    Cambios de política necesarios

■    Riesgos residuales

1. **Visualizaciones     clave:**

○    Trend charts (últimos 6 meses)

○    Heat maps de usuarios más atacados

○    Funnel de detección (cuánto se bloqueó en cada capa)

1. **Presentar     al CISO:**

○    Reunión mensual de 30-45 minutos

○    Focus en business impact, no solo métricas técnicas

1. **Presentar     al Board (trimestral):**

○    Versión simplificada

○    Focus en riesgo, compliance, ROI

○    Duración: 15 minutos máximo

1. Archivar reporte para compliance



## **TAREAS AD-HOC**

### **1. Rastreo de Mensajes (Message Trace) - Investigación Forense**

**Roles:** Incident Responder, Security Operator
 **Consola:** Microsoft Defender Portal / Exchange Admin Center
 **URLs:**

●    Defender: https://security.microsoft.com/messagetrace

●    Exchange: https://admin.exchange.microsoft.com/#/messagetrace

**Paso a Paso:**

1. **Acceder a Message Trace:**

○    Navegar (Defender): **Correo electrónico y colaboración > Rastreo de mensajes de Exchange**

1. **Configurar     búsqueda básica (últimos 10 días):**

○    **Senders:** Dirección del remitente (o dejarlo vacío)

○    **Recipients:** Usuario afectado

○    **Date range:** Seleccionar rango (máximo 10 días)

○    **Delivery status:** All, Delivered, Failed, Pending

○    Click "Search"

1. **Para     búsquedas >10 días (hasta 90 días):**

○    Click "Start a trace"

○    Configurar:

■    **Senders/Recipients**

■    **Date range:** Hasta 90 días atrás

■    **Report type:** Extended

○    Click "Search"

○    La consulta se procesa en background

○    Descargar resultados cuando estén listos (puede tardar horas)

1. **Analizar     resultados:**

○    Click en un mensaje para ver **detalles extendidos**:

■    **Message ID:** Identificador único

■    **Subject:** Asunto

■    **Status:** Delivered, Failed, FilteredAsSpam, Quarantined

■    **Events:** Cada salto del mensaje

■    Receive (recepción)

■    Transport Rule processing

■    Anti-spam/Anti-malware evaluation

■    Safe Attachments detonation

■    Delivery or Quarantine

1. **Interpretación     de eventos clave:**

○    **RECEIVE:** Mensaje llegó a Exchange Online

○    **SEND:** Mensaje salió a destinatario

○    **FAIL:** Fallo en entrega (ver reason code)

○    **QUARANTINE:** Mensaje fue cuarentenado

○    **DROP:** Mensaje fue eliminado (ver reason)

1. **Casos     de uso comunes:**

○    "¿Por qué no llegó este correo?" → Buscar mensaje, ver si fue quarantined/dropped

○    "¿Se bloqueó este phishing?" → Verificar status "FilteredAsSpam" o "QuarantinePhishing"

○    "¿Quién recibió este correo malicioso?" → Buscar por Message ID, ver todos los recipients

1. **Export     de resultados:**

○    Click "Download" para obtener CSV

○    Útil para análisis masivo o evidencia legal

1. Documentar hallazgos en caso de incidente



### **2. Búsqueda y Purga de Correos (Search and Purge)**

**Roles:** Incident Responder (con permisos Data Investigator)
 **Consola:** Microsoft Purview Compliance
 **URL:** https://compliance.microsoft.com/contentsearch

**Paso a Paso:**

1. **Navegar a Content Search:**

○    Navegar (Purview): **Content search**

1. **Crear     nueva búsqueda:**

○    Click "New search"

○    **Name:** "Phishing Campaign - [Date] - [IOC]"

○    **Locations:** Seleccionar "Exchange mailboxes" → Specific mailboxes o All

1. **Configurar     query (KQL):**

○    Ejemplos de queries:

\# Por Subject

subject:"Urgent: Verify your account"

 

\# Por Sender

from:attacker@malicious.com

 

\# Por URL en body

body:"http://phishing-site.com"

 

\# Por attachment hash

attachments:SHA256=abc123...

 

\# Combinación

from:attacker@malicious.com AND received>=2026-02-01

 

1. **Ejecutar búsqueda:**

○    Click "Save & run"

○    La búsqueda puede tardar minutos/horas según volumen

1. **Revisar     resultados:**

○    Click en la búsqueda completada

○    Verificar:

■    **Items found:** Cantidad de mensajes

■    **Locations with hits:** Buzones afectados

○    Click "Preview results" para ver muestra

1. **Validar     antes de purgar:**

○    **CRÍTICO:** Asegurar que la query es precisa

○    Revisar sample de resultados

○    Confirmar que NO hay falsos positivos

1. **Ejecutar     purga:**

○    Click "Actions" → "Purge results"

○    Opciones:

■    **Soft delete:** Mover a Deleted Items (recuperable)

■    **Hard delete:** Purga permanente (NO recuperable)

○    **RECOMENDACIÓN:** Usar Soft delete primero

1. **Monitorear     progreso:**

○    La purga puede tardar horas

○    Verificar en pestaña "Results" cuando complete

1. **Confirmar     éxito:**

○    Ejecutar Message Trace para validar que los mensajes fueron eliminados

1. **Documentar     acción:**

○    Registrar en incidente:

■    Query utilizada

■    Cantidad de mensajes purgados

■    Tipo de purga (soft/hard)

■    Justificación

■    Aprobador

1. **Notificar     a usuarios afectados:**

○    Informar que se eliminó correo malicioso

○    Instrucciones si clickearon enlaces



### **3. Bloqueo de Emergencia (Emergency Block) - Tenant Allow/Block List**

**Roles:** Incident Responder, Security Operator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/tenantAllowBlockList

**Paso a Paso:**

1. **Identificar indicador a bloquear:**

○    Puede ser:

■    URL/Dominio

■    File hash (SHA256)

■    Sender email/domain

1. **Acceder a Tenant Allow/Block List:**

○    Navegar: **Correo electrónico y colaboración > Políticas y reglas > Políticas de amenazas > Listas de permitidos y bloqueados de inquilinos**

1. **Para     bloquear URL:**

○    Click en pestaña **"URLs"**

○    Click "+ Block"

○    Ingresar URL(s):

■    Formato: http://malicious-site.com/*

■    Se pueden agregar múltiples (una por línea)

○    **Remove after:** Seleccionar fecha de expiración (recomendado: 30 días)

○    **Notes:** Describir la amenaza

○    Click "Add"

1. **Para     bloquear File (hash):**

○    Click en pestaña **"Files"**

○    Click "+ Block"

○    Ingresar SHA256 hash del archivo malicioso

○    Configurar expiración

○    Click "Add"

1. **Para     bloquear Sender:**

○    Click en pestaña **"Senders"**

○    Click "+ Block"

○    Ingresar email o dominio:

■    Específico: attacker@evil.com

■    Dominio completo: evil.com

○    **Remove after:** 30 días (temporal)

○    Click "Add"

1. **Verificar     bloqueo efectivo:**

○    Esperar 5-10 minutos para propagación

○    Enviar correo de prueba (si es seguro hacerlo)

○    O usar Message Trace para verificar mensajes subsecuentes son bloqueados

1. **Limpieza     post-crisis:**

○    Revisar blocks temporales después de 30 días

○    Eliminar si la amenaza ya no es activa

○    Preferir submission to Microsoft vs. permanent block

1. **IMPORTANTE:**

○    Blocks son efectivos inmediatamente

○    Sobrescriben otras políticas

○    Usar con precaución (riesgo de bloquear legítimos)



### **4. Búsqueda de Auditoría (Audit Log Search) - Investigación de Actividades Sospechosas**

**Roles:** Incident Responder, Security Administrator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/auditlogsearch

**Paso a Paso:**

1. **Acceder a Audit Log Search:**

○    Navegar (Defender): **Auditoría**

1. **Configurar     búsqueda:**

○    **Date range:** Seleccionar período

○    **Activities:** Filtrar por tipo de actividad (o dejar "All activities")

○    **Users:** Usuario específico o todos

○    Click "Search"

1. **Actividades     clave de MDO a buscar:**

○    **Mailbox access:**

■    MailboxLogin

■    MailItemsAccessed (lectura de correos)

○    **Mailbox forwarding:**

■    Set-Mailbox (búsqueda de ForwardingSmtpAddress changes)

■    New-InboxRule (reglas de reenvío)

○    **Policy changes:**

■    New-AntiPhishPolicy

■    Set-AntiPhishPolicy

■    New-TransportRule

○    **Role assignments:**

■    Add-RoleGroupMember (asignación de permisos)

1. **Filtrar     resultados:**

○    Click en columna headers para ordenar

○    Usar search box para buscar términos específicos

1. **Analizar     entradas sospechosas:**

○    Click en un evento para ver detalles completos

○    Revisar:

■    **User:** Quién realizó la acción

■    **IP Address:** Desde dónde

■    **User Agent:** Qué cliente

■    **Parameters:** Qué cambió exactamente

1. **Casos     de uso comunes:**

○    **Investigar compromiso de cuenta:**

■    Buscar New-InboxRule para detectar reglas de auto-forward maliciosas

○    **Auditar cambios de configuración:**

■    Buscar modificaciones a políticas de seguridad no autorizadas

○    **Detectar acceso no autorizado:**

■    Buscar MailItemsAccessed por usuario VIP desde IPs anómalas

1. **Export     de resultados:**

○    Click "Export" → Download CSV

○    Útil para análisis forense detallado

1. **Crear     alertas proactivas:**

○    Basado en hallazgos, crear alert policy (ver tareas mensuales)

1. Documentar hallazgos en caso de incidente



### **5. Respuesta a Brote de Phishing/Malware (Outbreak Response)**

**Roles:** Incident Responder, Manager, CISO (para decisiones críticas)
 **Consola:** Microsoft Defender Portal
 **Múltiples URLs dependiendo de la fase**

**Paso a Paso:**

1. **Fase 1: Identificación y Scope**

○    **Detectar outbreak:**

■    Spike en alertas de phishing

■    Múltiples user reports similares

■    Campaña en Campaign Views

○    **Determinar scope:**

■    Navegar a Threat Explorer

■    Filtrar por características comunes (subject, sender, URL)

■    Identificar:

■    Cantidad de mensajes enviados

■    Cuántos fueron entregados

■    Cuántos usuarios afectados

■    Cuántos clickearon/abrieron

1. **Fase     2: Contención Inmediata**

○    **Bloquear IOCs:**

■    Extraer URLs, dominios, file hashes

■    Agregar a Tenant Block List (ver tarea Emergency Block)

○    **Purgar mensajes entregados:**

■    Ejecutar Search and Purge (ver tarea correspondiente)

■    Usar Hard delete si es crítico

1. **Fase     3: Ajuste de Políticas (Temporal)**

○    **Incrementar agresividad anti-phishing:**

■    Navegar a Anti-phishing policies

■    Temporalmente ajustar:

■    Advanced phishing thresholds → Aggressive

■    Impersonation protection → Más restrictivo

○    **Crear regla de transporte temporal:**

■    Si el outbreak usa patrón específico (ej. subject line)

■    Crear ETR para bloquear/cuarentenar

■    **IMPORTANTE:** Marcar como temporal y revisar en 48 horas

1. **Fase     4: Verificación de Efectividad**

○    **Monitorear Threat Explorer:**

■    Verificar que nuevos mensajes de la campaña son bloqueados

■    Revisar cada hora durante las primeras 4 horas

○    **Revisar Safe Links:**

■    Verificar que los clics a URLs del outbreak son bloqueados

■    Navegar a: Informes > Safe Links report

1. **Fase     5: Identificación de Usuarios Comprometidos**

○    **Buscar indicadores de compromiso:**

■    Usuarios que clickearon antes de la contención

■    Buscar en Threat Explorer: "Click action" = "Allowed"

○    **Para cada usuario comprometido:**

■    Forzar reset de contraseña

■    Revocar tokens de sesión (Azure AD)

■    Revisar inbox rules (buscar auto-forward malicioso)

■    Revisar actividad en Azure AD Sign-in logs

1. **Fase     6: Comunicación**

○    **A usuarios afectados:**

■    Notificar sobre el outbreak

■    Instrucciones si clickearon (cambiar password, no proveer info)

○    **A Manager/CISO:**

■    Resumen ejecutivo del outbreak

■    Acciones tomadas

■    Usuarios comprometidos

■    Recomendaciones

1. **Fase     7: Post-Incident Review (48-72 horas después)**

○    **Revertir ajustes temporales:**

■    Restaurar políticas a configuración normal

■    Eliminar ETRs temporales

○    **Lecciones aprendidas:**

■    ¿Por qué pasó el filtro inicial?

■    ¿Qué políticas necesitan ajuste permanente?

■    ¿Qué training adicional se requiere?

1. **Documentar     todo el incidente:**

○    Timeline completa

○    Decisiones tomadas

○    Métricas (mensajes bloqueados, usuarios afectados, tiempo de respuesta)

○    Archivar para compliance y futuras referencias



### **6. Gestión de Priority Accounts (Agregar/Modificar Usuarios VIP)**

**Roles:** Security Administrator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/userTags

**Paso a Paso:**

1. Navegar: **Settings > Email &     collaboration > User tags**
2. Click     en **"Priority accounts"**
3. **Agregar     usuarios:**

○    Click "Edit tag"

○    Click "Members" section

○    Click "Add members"

○    Buscar y seleccionar usuarios (ejecutivos, finanzas, HR, etc.)

○    Click "Add"

○    Click "Save"

1. **Verificar     protecciones automáticas:**

○    Los priority accounts obtienen automáticamente:

■    Mailbox intelligence más sensible

■    Impersonation protection mejorada

■    Alertas diferenciadas

1. **Crear     política anti-phishing específica para VIPs:**

○    Navegar: **Políticas y reglas > Políticas de amenazas > Anti-phishing**

○    Click "Create"

○    Configurar:

■    **Name:** "VIP Protection Policy"

■    **Users and groups:** Seleccionar priority accounts

■    **Phishing threshold:** Most aggressive (nivel 4)

■    **Impersonation protection:**

■    Enable user impersonation protection

■    Add the VIPs themselves to protected users

■    **Mailbox intelligence:** Enable

■    **Actions:**

■    Quarantine para cualquier detección

○    **Priority:** Mover a top de la lista de políticas

1. Monitorear ataques a VIPs semanalmente



### **7. Creación de Custom Detection Rules (Advanced Hunting)**

**Roles:** Incident Responder, Security Analyst
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/v2/advanced-hunting

**Paso a Paso:**

1. Navegar: **Hunting > Advanced hunting**
2. **Desarrollar     query KQL:**

○    Ejemplo: Detectar reglas de inbox que reenvían a externos

EmailEvents

| where Timestamp > ago(24h)

| where ActionType == "InboxRuleCreated"

| extend RuleDetails = parse_json(AdditionalDetails)

| where RuleDetails.ForwardTo contains "@" and not(RuleDetails.ForwardTo contains "yourdomain.com")

| project Timestamp, AccountUpn, RuleDetails, IPAddress

 

●    Ejecutar query para validar resultados

1. **Crear     Custom Detection Rule:**

○    Click "Create detection rule" (arriba derecha)

○    Configurar:

■    **Name:** "Suspicious Inbox Rule - External Forwarding"

■    **Frequency:** Every 1 hour

■    **Alert title:** "User created suspicious forwarding rule"

■    **Severity:** High

■    **Category:** DefenseEvasion

■    **MITRE tactics:** Collection, Exfiltration

○    **Entities:** Map AccountUpn to User

1. **Configurar     acciones de respuesta:**

○    **Impacted entities:** Select User

○    **Actions:** (opcional)

■    Run automated investigation

■    Isolate device

1. **Probar     la regla:**

○    Crear inbox rule de prueba que cumple condición

○    Esperar 1 hora

○    Verificar que se genera alerta

1. **Documentar     query:**

○    Click "Save" en Advanced Hunting

○    Agregar descripción y tags

○    Compartir con equipo

1. Revisar alertas generadas semanalmente



### **8. Integración de Threat Intelligence Externa (IOC Import)**

**Roles:** Architect, Security Administrator
 **Consola:** Microsoft Defender Portal
 **URL:** https://security.microsoft.com/threatintelligence

**Paso a Paso:**

1. **Obtener feed de Threat Intelligence:**

○    Fuentes comunes:

■    CISA (US-CERT)

■    MITRE ATT&CK

■    Commercial TI feeds (CrowdStrike, Recorded Future)

■    OSINT (abuse.ch, URLhaus)

1. **Formato     requerido:**

○    CSV con columnas:

■    Indicator (URL, IP, Hash, Domain)

■    Type (URL, IPv4, FileSha256, DomainName)

■    Action (Alert, Block)

■    Description

■    ExpirationDate

1. **Importar     IOCs vía API (Graph API):**

○    Endpoint: https://graph.microsoft.com/beta/security/tiIndicators

○    Método: POST

○    Ejemplo payload:

{

 "targetProduct": "Microsoft Defender ATP",

 "threatType": "Malware",

 "indicator": "http://malicious-site.com",

 "indicatorType": "URL",

 "action": "Block",

 "description": "Known C2 infrastructure",

 "expirationDateTime": "2026-03-01T00:00:00Z"

}

 

1. **Importar IOCs vía Portal (manual para volúmenes pequeños):**

○    Navegar a Tenant Allow/Block List

○    Usar opción "Bulk add" si está disponible

○    Pegar lista de URLs/dominios/hashes

1. **Automatizar     con Logic App/Power Automate:**

○    Crear workflow que:

■    Se conecta a TI feed (API)

■    Parsea IOCs

■    Llama a Graph API para importar

■    Ejecutar diariamente

1. **Verificar     importación:**

○    Revisar Tenant Block List

○    Verificar que los IOCs están activos

1. **Monitorear     hits:**

○    Usar Threat Explorer para ver si IOCs bloquearon amenazas

○    Query: Filtrar por "Blocked by custom policy"

1. **Cleanup     de IOCs expirados:**

○    Configurar expiración automática (30-90 días)

○    Revisar mensualmente y eliminar IOCs obsoletos



### **9. Disaster Recovery - Restauración de Políticas de Seguridad**

**Roles:** Architect, Security Administrator
 **Consola:** PowerShell + Microsoft Defender Portal

**Paso a Paso:**

1. **Backup de políticas (preventivo - hacer mensualmente):**

○    Conectarse a Exchange Online PowerShell:

Connect-ExchangeOnline -UserPrincipalName admin@domain.com

 

●    Exportar políticas:

\# Anti-phishing

Get-AntiPhishPolicy | Export-Clixml -Path "C:\Backup\AntiPhish_$(Get-Date -Format 'yyyyMMdd').xml"

Get-AntiPhishRule | Export-Clixml -Path "C:\Backup\AntiPhishRules_$(Get-Date -Format 'yyyyMMdd').xml"

 

\# Anti-spam

Get-HostedContentFilterPolicy | Export-Clixml -Path "C:\Backup\AntiSpam_$(Get-Date -Format 'yyyyMMdd').xml"

 

\# Anti-malware

Get-MalwareFilterPolicy | Export-Clixml -Path "C:\Backup\AntiMalware_$(Get-Date -Format 'yyyyMMdd').xml"

 

\# Safe Links

Get-SafeLinksPolicy | Export-Clixml -Path "C:\Backup\SafeLinks_$(Get-Date -Format 'yyyyMMdd').xml"

 

\# Safe Attachments

Get-SafeAttachmentPolicy | Export-Clixml -Path "C:\Backup\SafeAttachments_$(Get-Date -Format 'yyyyMMdd').xml"

 

\# Transport Rules

Get-TransportRule | Export-Clixml -Path "C:\Backup\TransportRules_$(Get-Date -Format 'yyyyMMdd').xml"

 

1. **En caso de desastre (políticas borradas/corruptas):**

○    Importar backup más reciente:

$AntiPhish = Import-Clixml -Path "C:\Backup\AntiPhish_20260201.xml"

 

1. **Re-crear política:**

\# Ejemplo para Anti-phishing

$AntiPhish | ForEach-Object {

  New-AntiPhishPolicy -Name $_.Name `

​    -EnableSpoofIntelligence $_.EnableSpoofIntelligence `

​    -PhishThresholdLevel $_.PhishThresholdLevel `

​    \# ... (resto de parámetros)

}

 

1. **Verificar restauración:**

○    En portal Defender, navegar a políticas

○    Verificar que aparecen y tienen configuración correcta

1. **Probar     funcionalidad:**

○    Enviar correos de prueba

○    Verificar que políticas se aplican correctamente

1. **Documentar     incidente:**

○    Qué causó la pérdida de configuración

○    Qué se restauró

○    Tiempo de recuperación

○    Mejoras para prevenir recurrencia



### **10. Comunicación de Incidentes Críticos a Ejecutivos**

**Roles:** Manager, CISO
 **Formato:** Email + Reunión si es crítico

**Plantilla de Comunicación:**

**Subject:** [CRÍTICO] Incidente de Seguridad - Campaña de Phishing Dirigida

**Para:** CEO, CFO, Legal, HR (según impacto)
 **CC:** IT Director, SOC Manager



**Resumen Ejecutivo:** Se detectó una campaña de phishing dirigida contra [X] ejecutivos de la organización el [fecha]. El equipo de SecOps ha contenido la amenaza y está investigando el alcance completo.

**Impacto:**

●    **Usuarios afectados:** [Número] ([% del total])

●    **Usuarios comprometidos:** [Número confirmados]

●    **Datos potencialmente expuestos:** [Credenciales / Información financiera / PII]

●    **Tiempo de actividad de la amenaza:** [X horas antes de contención]

**Acciones Tomadas (Respuesta Inmediata):**

1. Bloqueados [X] URLs maliciosas y [Y] dominios en toda la     organización
2. Purgados     [Z] correos de phishing de buzones de usuarios
3. Forzado     reset de contraseñas para [N] usuarios comprometidos
4. Revocados     tokens de sesión activos de cuentas afectadas
5. Habilitado monitoreo intensificado para usuarios VIP

**Estado Actual:**

●    ✅ Amenaza contenida (sin nuevos mensajes entregados desde [hora])

●    ✅ Usuarios comprometidos asegurados

●    🔄 Investigación forense en progreso

●    🔄 Análisis de exfiltración de datos (resultados en [plazo])

**Próximos Pasos (24-48 horas):**

1. Completar análisis forense para determinar si hubo     exfiltración
2. Notificar     individualmente a usuarios afectados
3. Ejecutar     simulación de phishing de refuerzo para todo el personal
4. Implementar controles adicionales [describir]

**Recomendaciones de Negocio:**

●    [Si aplica] Notificar a clientes/partners si sus datos fueron afectados

●    [Si aplica] Coordinar con Legal para cumplimiento regulatorio (GDPR, etc.)

●    [Si aplica] Preparar statement para prensa si el incidente se hace público

**Punto de Contacto:** [Nombre CISO/Manager]
 [Email]
 [Teléfono 24/7]

**Nivel de Confidencialidad:** RESTRINGIDO - NO REENVIAR



**Siguiente Actualización:** [Fecha/Hora] o inmediatamente si hay cambios significativos



## **CONSOLAS Y URLS DE REFERENCIA RÁPIDA**

| **Función**             | **URL Directa**                                              |
| ----------------------- | ------------------------------------------------------------ |
| Incidents Queue         | https://security.microsoft.com/incidents                     |
| Threat Explorer         | https://security.microsoft.com/threatexplorer                |
| Action Center           | https://security.microsoft.com/action-center/pending         |
| User Submissions        | https://security.microsoft.com/reportsubmission?viewid=user  |
| Quarantine              | https://security.microsoft.com/quarantine                    |
| Campaigns               | https://security.microsoft.com/campaigns                     |
| Message Trace           | https://security.microsoft.com/messagetrace                  |
| Configuration Analyzer  | https://security.microsoft.com/configurationAnalyzer         |
| Secure Score            | https://security.microsoft.com/securescore                   |
| Attack Simulation       | https://security.microsoft.com/attacksimulator               |
| Tenant Allow/Block List | https://security.microsoft.com/tenantAllowBlockList          |
| Anti-phishing Policies  | https://security.microsoft.com/antiphishing                  |
| Audit Log               | https://security.microsoft.com/auditlogsearch                |
| Advanced Hunting        | https://security.microsoft.com/v2/advanced-hunting           |
| Exchange Admin Center   | https://admin.exchange.microsoft.com                         |
| Microsoft Purview       | https://compliance.microsoft.com                             |
| Microsoft Sentinel      | https://portal.azure.com/#view/Microsoft_Azure_Security_Insights |



Esta guía operativa proporciona el paso a paso detallado para ejecutar todas las operaciones de seguridad en Microsoft Defender for Office 365. Cada tarea incluye la consola específica, la ruta de navegación exacta y los pasos técnicos requeridos, permitiendo que cualquier miembro del equipo de SecOps pueda ejecutar las operaciones de manera consistente y efectiva.

 