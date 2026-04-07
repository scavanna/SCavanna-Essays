# **Guía Operativa Consolidada: Microsoft Defender for Cloud Apps**

## **Procedimientos Detallados por Consola, Rutas y Pasos**



## **TAREAS DIARIAS**

### **1. Revisión de Alertas e Incidentes**

**Roles:** Operator, Monitor, Incident Responder
 **Objetivo:** Triaje sistemático de incidentes de alta prioridad generados por anomalías cloud y violaciones de políticas

| **Paso** | **Acción**                   | **Consola/Ruta**                   | **Procedimiento Detallado**                                  |
| -------- | ---------------------------- | ---------------------------------- | ------------------------------------------------------------ |
| 1        | Acceder al portal            | **Microsoft Defender Portal**      | Navegar a https://security.microsoft.com                     |
| 2        | Localizar cola de incidentes | **Incidents & alerts > Incidents** | Click en pestaña **Incidents** en  panel izquierdo           |
| 3        | Aplicar filtros MDCA         | **Filter > Service source**        | Deseleccionar todas las opciones,  seleccionar solo **Microsoft Defender for Cloud Apps** |
| 4        | Priorizar por severidad      | **Filter > Severity**              | Ordenar por **Severity** (High → Low),  enfocarse en etiquetas "True Positive" |
| 5        | Abrir incidente              | Click en título del incidente      | Seleccionar **Open incident page**                           |
| 6        | Revisar Attack Story         | Tab **Attack Story**               | Visualizar correlación entre identidad,  IP y aplicación accedida |
| 7        | Analizar timeline            | Drill-down en alerta específica    | Verificar timestamp,  Location metadata, User Agent          |
| 8        | Verificar usuario            | Tab **Users**                      | Revisar **Investigation Priority Score**  (salto significativo indica comportamiento anómalo) |
| 9        | Remediar o cerrar            | **Manage incident**                | Si malicioso: Status **Active**,  asignar a Tier 2, trigger "Suspend User". Si benigno: **Resolved**  + **False Positive** |

**Ejemplo de Investigación - Impossible Travel:**

●    Comparar dos ubicaciones de login: ¿físicamente imposibles de atravesar en el tiempo transcurrido?

●    Verificar si IP está etiquetada como **Corporate VPN** o **Sanctioned Proxy**

●    Si User Agent indica dispositivo gestionado en ambas conexiones → posible falso positivo por túnel VPN



### **2. Análisis de Detección de Amenazas y Anomalías**

**Roles:** Operator, Incident Responder
 **Objetivo:** Investigación proactiva de anomalías comportamentales (UEBA)

| **Paso** | **Acción**          | **Consola/Ruta**             | **Procedimiento Detallado**                                  |
| -------- | ------------------- | ---------------------------- | ------------------------------------------------------------ |
| 1        | Acceder políticas   | **Cloud Apps > Policies**    | Navegar a **Policy management**                              |
| 2        | Filtrar por tipo    | **Type > Anomaly detection** | Mostrar solo **Anomaly detection  policies**                 |
| 3        | Revisar alertas     | Click en política específica | Ejemplo:  "Activity from infrequent country", "Mass download",  "Ransomware activity" |
| 4        | Drill-down          | Click en instancia de alerta | Revisar **Activity Log** asociado                            |
| 5        | Análisis contextual | Revisar detalles             | Volumen, tipos de archivo, comparación  histórica con **User Page** |

**Query Advanced Hunting (si UI inconcluyente):**

CloudAppEvents

| where ActionType == "FileDownloaded"

| where AccountDisplayName == "[Target User]"

| summarize count() by Application, bin(Timestamp, 1h)

 



### **3. Monitoreo de Gobernanza de Aplicaciones y App Risk Scores**

**Roles:** Operator, Administrator
 **Objetivo:** Detectar aplicaciones OAuth maliciosas o con privilegios excesivos

| **Paso** | **Acción**             | **Consola/Ruta**                | **Procedimiento Detallado**                                  |
| -------- | ---------------------- | ------------------------------- | ------------------------------------------------------------ |
| 1        | Acceder App Governance | **Cloud Apps > App governance** | Dashboard de gobernanza de aplicaciones                      |
| 2        | Revisar dashboard      | Tab **Overview**                | Examinar widgets **High  privilege apps** y **Overprivileged apps** |
| 3        | Filtrar riesgos        | **Apps > Risk level: High**     | Listar apps de alto riesgo                                   |
| 4        | Inspeccionar app       | Click en app de alto riesgo     | Abrir detalles                                               |
| 5        | Auditar permisos       | Tab **Permissions**             | Buscar scopes  sensibles: Mail.Read, Files.ReadWrite.All, Directory.ReadWrite.All |
| 6        | Revisar uso de datos   | Tab **Data usage**              | Verificar volúmenes de extracción activa                     |
| 7        | Verificar publisher    | Columna **Publisher**           | Apps de publishers  "Unverified" requieren escrutinio        |
| 8        | Acción si maliciosa    | Botón **Revoke app**            | Revocar inmediatamente y contactar  usuario                  |

**Señal de alerta:** App "Email Optimizer" de publisher no verificado solicitando acceso completo a buzón



### **4. Verificación de Estado de Conditional Access App Control**

**Roles:** Administrator
 **Objetivo:** Asegurar que controles de sesión en tiempo real (reverse proxy) están activos

| **Paso** | **Acción**            | **Consola/Ruta**                      | **Procedimiento Detallado**                                  |
| -------- | --------------------- | ------------------------------------- | ------------------------------------------------------------ |
| 1        | Verificar en Entra ID | **Entra Admin Center**                | Ir a entra.microsoft.com > **Protection** > **Conditional  Access** |
| 2        | Verificar políticas   | Revisar políticas                     | Confirmar que políticas con controles  "Session" están **On** |
| 3        | Verificar en MDCA     | **Settings > Cloud Apps**             | Ir a **Connected  apps > Conditional Access App Control apps** |
| 4        | Revisar tabla de apps | Columna **Status**                    | Apps onboarded  (Teams, Salesforce) deben mostrar **Connected** |
| 5        | Troubleshooting       | Si "Setup  required" o "Disconnected" | Verificar configuración SAML/OIDC en  Entra ID, verificar rollover de certificados |

**Impacto:** Si desconectado, usuarios pueden ser bloqueados completamente O bypass de seguridad



### **5. Evaluación de Shadow IT y Cloud Discovery Dashboard**

**Roles:** Operator, Monitor
 **Objetivo:** Identificar uso de aplicaciones SaaS no sancionadas

| **Paso** | **Acción**                 | **Consola/Ruta**                 | **Procedimiento Detallado**                                  |
| -------- | -------------------------- | -------------------------------- | ------------------------------------------------------------ |
| 1        | Acceder Discovery          | **Cloud Apps > Cloud Discovery** | Dashboard principal  de Cloud Discovery                      |
| 2        | Analizar overview          | **Dashboard > High-level usage** | Buscar picos en **Total Traffic** o **Discovered  Apps**     |
| 3        | Filtrar apps riesgosas     | Tab **Discovered apps**          | Click en pestaña                                             |
| 4        | Aplicar filtros avanzados  | **Advanced Filters**             | **Risk Score: 0-4** (High Risk) + **Traffic > 100 MB**       |
| 5        | Investigar app             | Click en app descubierta         | Ejemplo: "Mega.nz",  "WeTransfer"                            |
| 6        | Revisar factores de riesgo | Tab **Info**                     | Verificar gaps:  "No SOC2", "Data ownership not claimed", "Poor legal  terms" |
| 7        | Correlación de usuarios    | Tab **Users**                    | Identificar quién usa la app (equipo  completo vs usuario individual) |
| 8        | Acción de bloqueo          | **Tag as Unsanctioned**          | Sincroniza con MDE para bloqueo en capa  de endpoint         |



### **6. Inspección de Estado de Protección de Información y Triggers de DLP**

**Roles:** Operator, Administrator
 **Objetivo:** Monitorear exfiltración de datos sensibles o violaciones de políticas

| **Paso** | **Acción**                   | **Consola/Ruta**                | **Procedimiento Detallado**                                  |
| -------- | ---------------------------- | ------------------------------- | ------------------------------------------------------------ |
| 1        | Acceder gestión de políticas | **Cloud Apps > Policies**       | Policy management                                            |
| 2        | Filtrar categoría            | Tab **Information protection**  | Click en pestaña                                             |
| 3        | Revisar matches              | Ordenar por columna **Matches** | Descendente, enfocarse en "PII  Detected in External Share"  |
| 4        | Ver violaciones              | Click en count de  matches      | Lista de archivos que dispararon  política                   |
| 5        | Detalles de archivo          | Click en archivo específico     | Verificar **External users**, **Public  links**, snippet de datos que disparó DLP |
| 6        | Remediar                     | Acciones disponibles            | **Make Private** (remover externos), **Quarantine** (mover a carpeta  segregada), **Apply Label** (forzar etiqueta "Confidential") |



### **7. Validación de Salud de App Connectors y Log Collectors**

**Roles:** Administrator
 **Objetivo:** Asegurar pipelines de datos funcionando correctamente

| **Paso** | **Acción**               | **Consola/Ruta**                       | **Procedimiento Detallado**                                  |
| -------- | ------------------------ | -------------------------------------- | ------------------------------------------------------------ |
| 1        | Verificar conectores     | **Settings > Cloud Apps**              | **Connected apps > App Connectors**                          |
| 2        | Verificar estado         | Columna **Status**                     | Todas las instancias (Office 365, Box,  Okta) deben mostrar checkmark verde |
| 3        | Troubleshooting          | Si status **Error**                    | Click **Show error details**, renovar  API tokens si necesario |
| 4        | Verificar log collectors | **Cloud Discovery  > Auto log upload** | **Settings >  Cloud Apps > Cloud Discovery > Automatic log upload** |
| 5        | Verificar logs activos   | Columna **Status**                     | Data sources (ej. "Palo Alto")  deben mostrar **Receiving data**. "Disconnected" = análisis Shadow IT obsoleto |



### **8. Escalamiento de Incidentes No Resueltos**

**Roles:** Operator, Incident Responder
 **Objetivo:** Transferir incidentes complejos a Tier 2/3

| **Paso** | **Acción**                | **Consola/Ruta**                | **Procedimiento Detallado**                               |
| -------- | ------------------------- | ------------------------------- | --------------------------------------------------------- |
| 1        | Abrir página de incidente | Incident blade                  | Dentro del incidente abierto                              |
| 2        | Gestionar incidente       | Click **Manage incident**       | Abrir opciones de gestión                                 |
| 3        | Asignar                   | Campo **Assigned to**           | Cambiar a analista Tier 2 específico o  cola "SOC Tier 2" |
| 4        | Etiquetar                 | Agregar tags                    | Ejemplos:  "Escalation_Required", "Forensics_Needed"      |
| 5        | Notificación externa      | Botón **Flow** (Power Automate) | Si integrado con  ITSM: "Create SNOW Ticket"              |



### **9. Actualización de Documentación de Incidentes**

**Roles:** Operator
 **Objetivo:** Mantener audit trail de acciones investigativas

| **Paso** | **Acción**             | **Consola/Ruta**                 | **Procedimiento Detallado**                                  |
| -------- | ---------------------- | -------------------------------- | ------------------------------------------------------------ |
| 1        | Sección de comentarios | Incident pane > Tab **Comments** | Acceder a comentarios                                        |
| 2        | Entrada estructurada   | Formato requerido                | [Timestamp] - [Usuario] - [Acción]                           |
| 3        | Ejemplo                | Entrada                          | "10:00 AM - Analista SOC -  Verificado ubicación usuario vía HRIS. Usuario confirmado en vacaciones en  Francia. Alerta válida" |
| 4        | Adjuntar evidencia     | Tab **Evidence**                 | Screenshots o logs exportados                                |



### **10. Comunicación de Hallazgos Críticos a Manager/CISO**

**Roles:** Incident Responder
 **Objetivo:** Informar a liderazgo sobre incidentes de alto impacto

| **Paso** | **Acción**                 | **Consola/Ruta**                   | **Procedimiento Detallado**                          |
| -------- | -------------------------- | ---------------------------------- | ---------------------------------------------------- |
| 1        | Recolectar evidencia       | Incident page                      | Seleccionar **Download report**  (resumen PDF)       |
| 2        | Preparar executive summary | Usar datos del investigation graph | Incluir **Main Findings**                            |
| 3        | Canal de comunicación      | Fuera de banda si necesario        | Signal/Teléfono si sistema email  comprometido       |
| 4        | Notificación por consola   | **Settings > Cloud Apps > System** | **Mail settings** > configurar alias seguro del CISO |



## **TAREAS SEMANALES**

### **1. Revisión de Recomendaciones SSPM (SaaS Security Posture Management)**

**Roles:** Administrator, Architect
 **Objetivo:** Identificar y remediar misconfigurations en apps SaaS conectadas

| **Paso** | **Acción**                  | **Consola/Ruta**                           | **Procedimiento Detallado**                                  |
| -------- | --------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| 1        | Acceder Exposure Management | **Exposure management >  Recommendations** | Defender Portal                                              |
| 2        | Filtrar por producto        | **Filter > Product**                       | Seleccionar **Microsoft Defender for  Cloud Apps** + apps específicas (Salesforce, ServiceNow) |
| 3        | Analizar gaps               | Click en recomendación                     | Ejemplo:  "Ensure multifactor authentication is enabled"     |
| 4        | Revisar impacto             | Detalles de recomendación                  | **User Impact** y **Implementation Cost**                    |
| 5        | Remediar                    | Link **Open in App**                       | Usar link para abrir consola admin de  SaaS y aplicar fix    |



### **2. Seguimiento de Cambios y Actualizaciones en Defender XDR**

**Roles:** Administrator, Architect
 **Objetivo:** Mantenerse actualizado con nuevas capacidades de detección

| **Paso** | **Acción**        | **Consola/Ruta**    | **Procedimiento Detallado**                                  |
| -------- | ----------------- | ------------------- | ------------------------------------------------------------ |
| 1        | M365 Admin Center | admin.microsoft.com | **Health > Message center**                                  |
| 2        | Filtrar servicio  | Filter > Service    | **Microsoft  Defender for Cloud Apps**                       |
| 3        | Defender Portal   | Defender portal     | Revisar banner **What's new**                                |
| 4        | Acción            | Revisar items       | Items etiquetados  "Plan for Change" requieren scheduling para test/implementación |



### **3. Auditoría de Logs de Gobernanza y Efectividad de Políticas**

**Roles:** Administrator, Manager
 **Objetivo:** Verificar que acciones automatizadas de gobernanza ejecutan correctamente

| **Paso** | **Acción**           | **Consola/Ruta**               | **Procedimiento Detallado**                                  |
| -------- | -------------------- | ------------------------------ | ------------------------------------------------------------ |
| 1        | Acceder activity log | **Cloud Apps > Activity log**  | Log global de actividad                                      |
| 2        | Filtrar acciones     | **Activity type > Governance** | Tipos: **User  suspended**, **File quarantined**             |
| 3        | Validar status       | Columna **Status**             | Asegurar **Success**. Si **Failed**,  investigar permisos (¿cuenta Connector tiene write access?) |
| 4        | Quality check        | Sampleo                        | Revisar 5 eventos "File  quarantined". Verificar si archivos eran verdaderamente maliciosos |
| 5        | Tuning               | Si muchos falsos positivos     | Ajustar filtros de **File Policy**                           |



### **4. Verificación de Salud de Agentes SIEM e Integraciones**

**Roles:** Administrator
 **Objetivo:** Confirmar flujo de alertas/logs a SIEM para retención y correlación

| **Paso** | **Acción**          | **Consola/Ruta**                          | **Procedimiento Detallado**                                  |
| -------- | ------------------- | ----------------------------------------- | ------------------------------------------------------------ |
| 1        | SIEM genérico       | **Settings > Cloud Apps > System**        | **SIEM agents**                                              |
| 2        | Verificar status    | Columna status                            | Agentes deben mostrar **Active**.  Troubleshooting: verificar servicio Java en collector on-premise |
| 3        | Microsoft Sentinel  | **Microsoft Sentinel > Data  connectors** | Buscar **Microsoft  Defender for Cloud Apps**                |
| 4        | Verificar recepción | Chart **Last log received**               | Debe mostrar datos dentro de últimos 15  minutos             |
| 5        | Tipos de datos      | Verificar checkboxes                      | Confirmar "Alerts" y  "Cloud Discovery Logs" están marcados  |



### **5. Revisión de User and Entity Behavior Analytics (UEBA)**

**Roles:** Monitor, Incident Responder
 **Objetivo:** Identificar ataques "low and slow" mediante top risky users agregados semanalmente

| **Paso** | **Acción**         | **Consola/Ruta**                         | **Procedimiento Detallado**                                  |
| -------- | ------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| 1        | Identity Dashboard | **Assets > Identities**                  | Dashboard de identidades                                     |
| 2        | Ordenar por riesgo | Columna **Investigation Priority Score** | Ordenar descendente                                          |
| 3        | Analizar Top 10    | Seleccionar top users                    | Revisar **Lateral  movement paths** y **Alert history**      |
| 4        | Insight            | Score >80 sostenido por semana           | Sin incidente = potencial amenaza  insider compleja o service account mal configurado |



### **6. Prueba y Validación de Playbooks Automatizados**

**Roles:** Operator
 **Objetivo:** Asegurar que flows de Power Automate funcionan y tokens no han expirado

| **Paso** | **Acción**            | **Consola/Ruta**                                       | **Procedimiento Detallado**                                  |
| -------- | --------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| 1        | Power Automate Portal | make.powerautomate.com                                 | Portal de automatización                                     |
| 2        | Run history           | **My flows** > Seleccionar playbook > **Run  history** | Historial de ejecuciones                                     |
| 3        | Error check           | Buscar runs **Failed**                                 | Click en failed run para ver step  específico con error (ej. "Unauthorized" en conector Teams) |
| 4        | Simulación            | En MDCA trigger test  policy                           | Ejemplo: "Test - Risky IP"  para forzar ejecución de playbook y verificar latencia end-to-end |



### **7. Auditoría de Permisos de Apps y Acceso OAuth**

**Roles:** Administrator
 **Objetivo:** Sweep semanal de nuevos grants OAuth para detectar "Shadow Apps"

| **Paso** | **Acción**     | **Consola/Ruta**            | **Procedimiento Detallado**                                  |
| -------- | -------------- | --------------------------- | ------------------------------------------------------------ |
| 1        | Vista OAuth    | **Cloud Apps > OAuth apps** | Apps OAuth                                                   |
| 2        | Filtrar lógica | Filtros combinados          | **Permission level:  High** AND **Authorized on:  Last 7 days** |
| 3        | Revisar        | Investigar apps en lista    | Si  "Community Use" es "Rare" y publisher desconocido → **Revoke** inmediatamente y contactar  usuario |



### **8. Revisión de Cumplimiento con Políticas Organizacionales**

**Roles:** Manager, CISO
 **Objetivo:** Asegurar que realidad operativa coincide con política escrita

| **Paso** | **Acción**              | **Consola/Ruta**                        | **Procedimiento Detallado**                                  |
| -------- | ----------------------- | --------------------------------------- | ------------------------------------------------------------ |
| 1        | Cloud Discovery         | **Cloud Apps > Cloud Discovery**        | Dashboard Discovery                                          |
| 2        | Análisis de storage     | Filtrar por **Category: Cloud Storage** | Apps de almacenamiento                                       |
| 3        | Verificación de volumen | Total traffic a apps  **Unsanctioned**  | Si tráfico unsanctioned >5% del total  storage = política inefectiva, revisar reglas de bloqueo en MDE |



### **9. Reporte Semanal de Tendencias de Incidentes**

**Roles:** Incident Responder
 **Objetivo:** Resumen táctico del landscape de amenazas de la semana

| **Paso** | **Acción**         | **Consola/Ruta**             | **Procedimiento Detallado**                                  |
| -------- | ------------------ | ---------------------------- | ------------------------------------------------------------ |
| 1        | Cola de incidentes | Filter **Time: Last 7 days** | Incidents queue                                              |
| 2        | Exportar           | Click **Export** to CSV      | Exportación de datos                                         |
| 3        | Análisis           | Crear pivot table            | Top Alert Types, Top  Impacted Users, Mean Time to Resolve (MTTR) |
| 4        | Deliverable        | Email summary                | Highlighting de problemas sistémicos  (ej. "Spike en alertas DLP por campaña Marketing") |



### **10. Actualización de Matriz de Escalamiento y Procedimientos**

**Roles:** Manager, Incident Responder
 **Objetivo:** Mantener listas de contacto y playbooks actualizados

| **Paso** | **Acción**          | **Consola/Ruta**                    | **Procedimiento Detallado**                        |
| -------- | ------------------- | ----------------------------------- | -------------------------------------------------- |
| 1        | Revisar documento   | "SecOps Runbook" interno            | Almacenado en SharePoint/Wiki                      |
| 2        | Verificar contactos | Lista de contactos                  | Confirmar números de Legal, HR, CISO  actualizados |
| 3        | Actualizar lógica   | Si nuevo tipo de alerta esta semana | Agregar sección a procedimientos de  respuesta     |



## **TAREAS MENSUALES**

### **1. Evaluación de Efectividad y Enforcement de Políticas**

**Roles:** Manager, Architect, Administrator
 **Objetivo:** Tuning de políticas para reducir falsos positivos y asegurar cobertura

| **Paso** | **Acción**            | **Consola/Ruta**          | **Procedimiento Detallado**                                  |
| -------- | --------------------- | ------------------------- | ------------------------------------------------------------ |
| 1        | Lista de políticas    | **Cloud Apps > Policies** | Policy management                                            |
| 2        | Análisis de matches   | Ordenar por **Matches**   | Descendente                                                  |
| 3        | Alto ruido            | Política >1000 matches    | Crear alert fatigue. Editar política  para agregar **Exclusions** (subnet o user group específico) |
| 4        | Cero señal            | Política 0 matches        | Verificar si threshold muy alto                              |
| 5        | Revisión de templates | **Policy templates**      | Nuevas políticas recomendadas por  Microsoft, instanciarlas  |



### **2. Revisión de Activity Logs y Acciones de Gobernanza**

**Roles:** Manager, Administrator
 **Objetivo:** "Audit the Auditors" - asegurar acciones administrativas fueron autorizadas

| **Paso** | **Acción**                 | **Consola/Ruta**                            | **Procedimiento Detallado**                                  |
| -------- | -------------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| 1        | Activity log               | **Cloud Apps > Activity log**               | Log de actividad                                             |
| 2        | Filtrar actos admin        | **Activity type: Administrative  activity** | Actividades administrativas                                  |
| 3        | Revisar acciones sensibles | Buscar                                      | **Dismiss alert**, **Unsanction app**, **Change policy  settings** |
| 4        | Validación                 | Cross-reference                             | Verificar contra  Change Management tickets. Relajación no autorizada  = incidente de seguridad serio |



### **3. Análisis de Tendencias de Incidentes y Ajuste de Controles**

**Roles:** Manager, Architect
 **Objetivo:** Identificar debilidades sistémicas

| **Paso** | **Acción**     | **Consola/Ruta**                   | **Procedimiento Detallado**                                  |
| -------- | -------------- | ---------------------------------- | ------------------------------------------------------------ |
| 1        | Trend analysis | Revisar reporte mensual incidentes | Análisis estadístico                                         |
| 2        | Ajuste         | Si "Phishing vía Teams" +20%       | Considerar **Session Policies** más  estrictas para Teams (bloquear uploads de archivos para guests) |
| 3        | Documentación  | Actualizar                         | Documento  "Threat Landscape" con findings                   |



### **4. Actualización de Documentación de Arquitectura de Seguridad**

**Roles:** Architect
 **Objetivo:** Asegurar diagramas de red y flujos de datos reflejan deployment actual

| **Paso** | **Acción**         | **Consola/Ruta**                                | **Procedimiento Detallado**                                  |
| -------- | ------------------ | ----------------------------------------------- | ------------------------------------------------------------ |
| 1        | Revisar conectores | **Settings > App Connectors**                   | Verificar instancias                                         |
| 2        | Nueva instancia    | Si agregado (ej. "Salesforce  Marketing Cloud") | Actualizar diagrama de arquitectura                          |
| 3        | Log collectors     | Verificar                                       | Si nuevo firewall de oficina integrado,  actualizar documentación "Log Ingestion" |



### **5. Revisión de Integración con Otras Herramientas de Seguridad**

**Roles:** Architect
 **Objetivo:** Verificar fabric de seguridad holístico

| **Paso** | **Acción**             | **Consola/Ruta**                             | **Procedimiento Detallado**                                  |
| -------- | ---------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| 1        | MDE integration        | **Settings >  Cloud Apps > Cloud Discovery** | **Microsoft Defender for Endpoint**                          |
| 2        | Verificar toggle       | Estado                                       | Asegurar integration toggle **On**                           |
| 3        | Entra ID Protection    | **Settings > Cloud Apps > System**           | **Microsoft Entra ID Protection**                            |
| 4        | Verificar data sharing | Estado                                       | Asegurar habilitado. Falla rompe loop  "Signal-to-Enforcement" |



### **6. Validación de Mecanismos de Reporting a CISO/Board**

**Roles:** Manager
 **Objetivo:** Asegurar reportes automatizados siendo entregados

| **Paso** | **Acción**              | **Consola/Ruta**                             | **Procedimiento Detallado**                     |
| -------- | ----------------------- | -------------------------------------------- | ----------------------------------------------- |
| 1        | Reportes programados    | **Settings >  Cloud Apps > Cloud Discovery** | **Scheduled reports**                           |
| 2        | Verificar destinatarios | Lista de distribución                        | Actualizada (remover leavers, agregar  joiners) |
| 3        | Test run                | Seleccionar reporte                          | Click **Send now**  para verificar delivery     |



### **7. Presentación Mensual de Postura de Seguridad a Board**

**Roles:** Manager, CISO
 **Objetivo:** Comunicar valor y riesgo usando reportes ejecutivos

| **Paso** | **Acción**       | **Consola/Ruta**                 | **Procedimiento Detallado**                                  |
| -------- | ---------------- | -------------------------------- | ------------------------------------------------------------ |
| 1        | Executive report | **Cloud Apps > Cloud Discovery** | **Actions >  Generate Cloud Discovery executive report** (PDF) |
| 2        | Secure Score     | **Secure Score**                 | Capturar trend line de **History**                           |
| 3        | Presentación     | Métricas clave                   | "Risk Reduction" (ej.  "Reducido Shadow IT 15%")             |



### **8. Revisión y Renovación de Licencias/Subscriptions**

**Roles:** Administrator
 **Objetivo:** Asegurar compliance de licencias

| **Paso** | **Acción**      | **Consola/Ruta**                   | **Procedimiento Detallado**                                  |
| -------- | --------------- | ---------------------------------- | ------------------------------------------------------------ |
| 1        | Admin Center    | admin.microsoft.com                | **Billing > Your products**                                  |
| 2        | Usage check     | Comparar                           | **Assigned**  vs **Total** licenses                          |
| 3        | MDCA específico | **Settings > Cloud Apps > System** | **Scoped deployment**                                        |
| 4        | Verificar scope | Alcance                            | Asegurar scope incluye todos usuarios  nuevos. Usuarios fuera de scope **no son monitoreados** |



### **9. Ejercicios Tabletop para Respuesta a Incidentes**

**Roles:** Manager, Incident Responder
 **Objetivo:** Simular crisis para probar readiness del equipo

**Escenario Ejemplo:** "Ransomware infection vía OneDrive Sync"

| **Paso** | **Acción** | **Procedimiento**                                            |
| -------- | ---------- | ------------------------------------------------------------ |
| 1        | Simulación | Crear archivo test ransom.crypt, subir a OneDrive de usuario test |
| 2        | Detección  | Medir tiempo hasta que policy **Ransomware  activity** trigger alerta |
| 3        | Respuesta  | Medir tiempo para Operator suspenda  usuario                 |
| 4        | Review     | Debrief y actualizar "Ransomware  Playbook" basado en gaps identificados |



### **10. Actualización de Materiales de Entrenamiento para Personal SOC**

**Roles:** Manager, Administrator
 **Objetivo:** Knowledge transfer

| **Paso** | **Acción**       | **Procedimiento**           |
| -------- | ---------------- | --------------------------- |
| 1        | Identificar gaps | Basado en tabletop exercise |
| 2        | Actualizar Wiki  | Agregar                     |



## **TAREAS AD-HOC**

### **1. Respuesta a Amenazas Emergentes o Zero-Day**

**Roles:** Incident Responder, Architect, CISO
 **Objetivo:** Mitigar vulnerabilidades extendidas (ej. Log4j) en apps SaaS

| **Paso** | **Acción**                 | **Consola/Ruta**           | **Procedimiento**                                            |
| -------- | -------------------------- | -------------------------- | ------------------------------------------------------------ |
| 1        | Identificar app vulnerable | **Cloud Discovery** search | Buscar app (ej. "GoToMeeting")                               |
| 2        | Assess usage               | Revisar                    | **Top Users** y **Traffic volume**                           |
| 3        | Bloquear                   | Si vendor no ha patcheado  | **Tag as  Unsanctioned** → push block signal a endpoints vía MDE |
| 4        | Hunting                    | Advanced Hunting query     | Ver quién usó recientemente                                  |

**Query:**

CloudAppEvents

| where Application == "VulnerableApp"

| summarize count() by AccountDisplayName

 



### **2. Implementación de Nuevas Features de Seguridad**

**Roles:** Architect, Administrator
 **Objetivo:** Roll out de nuevas protecciones (ej. activar App Governance)

| **Paso** | **Acción** | **Procedimiento**                                            |
| -------- | ---------- | ------------------------------------------------------------ |
| 1        | Pilot      | Habilitar feature  para "Test Group" usando **Scoped deployment** |
| 2        | Monitor    | Watch para falsos positivos                                  |
| 3        | GA         | Remover scope                                                |



### **3. Atender Hallazgos Inesperados de Compliance/Auditoría**

**Roles:** Manager, CISO
 **Objetivo:** Remediation de fallos de auditoría

**Ejemplo:** "Data retention para ex-empleados no definida"

| **Paso** | **Acción**  | **Procedimiento**                   |
| -------- | ----------- | ----------------------------------- |
| 1        | MDCA action | Crear **File Policy**               |
| 2        | Governance  | **Quarantine** a carpeta legal hold |



### **4. Comunicación de Incidentes Mayores a Executive Leadership**

**Roles:** CISO, Board of Directors
 **Objetivo:** Crisis management

| **Paso** | **Acción**        | **Procedimiento**       |
| -------- | ----------------- | ----------------------- |
| 1        | Impact assessment | Usar **Incident Graph** |
| 2        | Briefing          | SBAR brief              |



### **5. Coordinación con Agencias Externas para Threat Intelligence**

**Roles:** CISO, Manager
 **Objetivo:** Enriquecer datos internos con señales externas

| **Paso** | **Acción**          | **Consola/Ruta**                       | **Procedimiento**                      |
| -------- | ------------------- | -------------------------------------- | -------------------------------------- |
| 1        | Ingerir indicadores | Si agencia provee lista IPs maliciosas | **IP address ranges** settings en MDCA |
| 2        | Agregar tag         | Nuevo tag                              | **Unsanctioned IP**                    |
| 3        | Policy update       | Actualizar políticas                   | "Block access  from Unsanctioned IPs"  |



## **MECANISMOS DE VERIFICACIÓN DE USO DE FEATURES M365 E5**

### **1. Cloud Discovery Dashboard (Shadow IT)**

| **Verificación**      | **Consola/Ruta**                                       | **Prueba de Actividad**                                      |
| --------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| Console check         | **Cloud Apps > Cloud Discovery**                       | Dashboard debe mostrar **datos  recientes** (últimas 24h)    |
| Data freshness        | "Discovered apps" list                                 | Poblada con apps diversas (no solo  Microsoft)               |
| Log source validation | **Settings >  Cloud Discovery > Automatic log upload** | **Last Report** date current. Si vacío = feature **inactiva** |



### **2. Activity and Governance Logs**

| **Verificación** | **Consola/Ruta**                     | **Prueba de Actividad**                                      |
| ---------------- | ------------------------------------ | ------------------------------------------------------------ |
| Console check    | **Cloud Apps > Activity log**        | Stream continuo de  eventos: UserLoggedIn, FilePreviewed, FileDownloaded |
| Filter test      | Filter by **App = Office 365**       | Si no hay eventos = App Connector roto                       |
| Governance check | Filter **Activity Type: Governance** | Ver acciones del sistema (Update user,  Suspend user) = enforcement engine activo |



### **3. Advanced Hunting Queries (KQL)**

| **Verificación** | **Consola/Ruta**               | **Prueba de Actividad**                               |
| ---------------- | ------------------------------ | ----------------------------------------------------- |
| Console check    | **Hunting > Advanced hunting** | Queries contra tabla CloudAppEvents                   |
| Data flow query  | Ver query abajo                | Si tabla vacía = MDCA no ingesting a  hunting backend |

**Query de Verificación:**

CloudAppEvents

| summarize LastLog = max(Timestamp) by Application

| order by LastLog desc

 

**Resultado esperado:** Múltiples apps con timestamps última hora



### **4. Health Checks para Connectors y Agents**

| **Verificación** | **Consola/Ruta**                                             | **Prueba de Actividad**                                  |
| ---------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
| Console check    | **Settings >  Cloud Apps > Connected apps > App Connectors** | Todas las instancias (Salesforce, O365)  green checkmark |
| API token check  | **Settings > System > API tokens**                           | Tokens para SIEM agents/scripts no  expirados            |



### **5. Policy Assessments**

| **Verificación** | **Consola/Ruta**                                 | **Prueba de Actividad**                   |
| ---------------- | ------------------------------------------------ | ----------------------------------------- |
| Console check    | **Cloud Apps > Policies**                        | Ordenar por **Matches**                   |
| Validation       | Si **Total Matches = 0** para *todas*  políticas | Detection engine  failing o misconfigured |
| Policy status    | Seleccionar política (ej. "DLP -  Credit Card")  | **Edit** → asegurar **Status: Enabled**   |



### **6. Role-Based Access Control (RBAC) Scoping**

| **Verificación** | **Consola/Ruta**                             | **Prueba de Actividad**                                      |
| ---------------- | -------------------------------------------- | ------------------------------------------------------------ |
| Console check    | **Settings >  System > Manage admin access** | Revisar lista de admins                                      |
| RBAC proof       | Verificar roles asignados                    | Usuarios con roles específicos  ("Cloud App Security Operator") vs generic Global Admin |



### **7. Application Inventory y Risk Scores**

| **Verificación**  | **Consola/Ruta**                                    | **Prueba de Actividad**                                      |
| ----------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| Console check     | **Cloud Apps >  Cloud Discovery > Discovered apps** | Filtrar por **Risk score 0-10**                              |
| Score calculation | Sistema muestra                                     | Score calculado para cada app basado en  90+ risk factors del catalog |
| Detail check      | Click en app info  drawer                           | **Compliance** details (GDPR, SOC2) poblados = catalog sync activo |



### **8. Secure Score y Recommendations**

| **Verificación**    | **Consola/Ruta**            | **Prueba de Actividad**                                      |
| ------------------- | --------------------------- | ------------------------------------------------------------ |
| Console check       | **Secure Score** dashboard  | Buscar "Recommended Actions"                                 |
| Source verification | Filtrar                     | Sourced from **Microsoft  Defender for Cloud Apps** (ej. "Discover  Shadow IT") |
| Change tracking     | Score fluctuating over time | Evidencia que SSPM features evaluando  entorno dinámicamente |





## **APÉNDICE: QUERIES KQL AVANZADAS**

### **Query 1: Verificar File Downloads por Usuarios Externos**

Valida que *Information Protection* y *Activity Monitoring* capturan eventos granulares de archivos

CloudAppEvents

| where ActivityType == "FileDownloaded"

| where IsExternalUser == true

| project Timestamp, ActionType, Application, AccountDisplayName, IPAddress, FileName

| limit 100

 



### **Query 2: Detectar Autorización de OAuth Apps Riesgosas**

Valida que eventos de *App Governance* siendo logged

CloudAppEvents

| where ActionType == "Consent to application"

| extend OAuthAppId = tostring(RawEventData.AppId)

| summarize Count = count() by OAuthAppId, AccountDisplayName

| order by Count desc

 



### **Query 3: Análisis de Volumen Shadow IT**

Valida que *Cloud Discovery* log collectors procesando volumen de tráfico correctamente

CloudAppEvents

| where ActionType == "TrafficLog" or ActionType == "HttpTraffic"

| summarize TotalUploadGB = sum(todouble(RawEventData.UploadBytes)) / 1024 / 1024 / 1024 by Application

| order by TotalUploadGB desc

 