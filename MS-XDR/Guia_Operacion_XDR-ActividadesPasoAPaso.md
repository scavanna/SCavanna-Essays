# **Guía Operativa Consolidada de Microsoft Defender XDR para M365 E5**

## **Resumen Ejecutivo**

Esta guía consolidada integra todas las recomendaciones oficiales de Microsoft para la operación de Defender XDR, combinando los procedimientos detallados paso a paso con las mejores prácticas de los diferentes componentes del ecosistema (Defender for Endpoint, Office 365, Identity y Cloud Apps).



## **1. TAREAS DIARIAS (Daily Operations)**

### **1.1 Monitoreo y Triaje de Incidentes**

**Consola:** Microsoft Defender Portal (https://security.microsoft.com)
 **Ruta:** Navigation Pane > **Incidents & alerts** > **Incidents**

**Procedimiento:**

1. **Configuración inicial de vista:
        
   **     

○    Selector de tiempo: **Últimas 24 horas** o **3 días**

○    Click en **Customize columns** y habilitar:

■    Severity, Status, Assigned to, Impact

■    Service sources, Tags, Last activity

1. **Aplicar     filtros de prioridad:
        
   **     

○    Click en **Filter** > **Incident severity**: Seleccionar **High** y **Medium**

○    **Status**: Seleccionar **New** e **In progress**

○    **Assigned to**: Filtrar por **Me** o **Unassigned**

1. **Triaje     inicial:
        
   **     

○    Seleccionar fila del incidente (sin abrir pestaña nueva)

○    Revisar **Panel de Resumen Lateral** para ver Priority Score y categorías

○    Usar flechas Arriba/Abajo para navegar secuencialmente

1. **Investigación     profunda:
        
   **     

○    Click en nombre del incidente para abrir página completa

○    Ir a pestaña **Attack story** para visualizar cadena de eventos

○    Revisar sección **Alerts** para detalles técnicos

1. **Clasificación     y asignación:
        
   **     

○    Click en **Manage incident**

○    **Assign to**: Asignar analista responsable

○    **Status**: Cambiar de *New* a *In progress*

○    **Tags**: Añadir etiquetas descriptivas

○    **Classification**: Pre-clasificar si es evidente

1. **Resolución     de incidentes:
        
   **     

○    Una vez remediado, cambiar **Status** a **Resolved**

○    **Classification**: Marcar como **True positive** o **False positive**

○    Si es true positive, especificar **Threat type**

**SLA Recomendado:**

●    High: < 1 hora

●    Medium: < 4 horas

●    Low: < 24 horas



### **1.2 Revisión del Centro de Acciones (Action Center)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Actions & submissions** > **Action center
** **URL directa:** https://security.microsoft.com/action-center

**Procedimiento:**

1. **Acciones pendientes (Pending tab):
        
   **     

○    Revisar acciones como: Quarantine file, Soft delete email, Stop service, Isolate device

○    Seleccionar acción para ver panel lateral con evidencia

○    Click en **Open investigation page** para contexto completo

○    **Decisión:**

■    **Approve**: Si la amenaza es confirmada

■    **Reject**: Si es comportamiento legítimo

1. **Historial     de acciones (History tab):
        
   **     

○    Revisar todas las acciones completadas (automáticas y manuales)

○    Verificar que las remediaciones automáticas no afectaron procesos críticos

○    **Corrección de errores**: Seleccionar acción y click **Undo** si fue incorrecta

○    *Nota: Requiere permisos de Security Administrator*



### **1.3 Gestión de Falsos Positivos/Negativos**

**A. Para Amenazas de Correo (MDO):**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Actions & submissions** > **Submissions** > **Submit to Microsoft for analysis**

**Procedimiento:**

1. Seleccionar tipo: **Email**, **URL**, o **Email     attachment**
2. **Falso Positivo**: Seleccionar "Should not have been     blocked"

○    Cargar archivo .msg o ingresar Network Message ID

○    Marcar opción para regla temporal Allow/Block automática

1. **Falso Negativo**: Seleccionar "Should have been     blocked"

○    Proporcionar evidencia del mensaje malicioso

**B. Para Alertas de Endpoint (MDE):**

**Consola:** Página de detalles de la alerta
 **Procedimiento:**

1. Click en **Manage alert**
2. **Status**: Cambiar a **Resolved**
3. **Classification**: **False alert**
4. Crear     regla de supresión si aplica:

○    Definir alcance (dispositivo específico u organización)

○    Basar en IOCs específicos (hash, ruta, etc.)

**C. Envío de archivos para análisis:**

**Ruta:** **Actions & submissions** > **Submissions** > **Files**

●    Cargar archivo para re-escaneo profundo



### **1.4 Revisión de Campañas de Email**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Campaigns** o usar **Threat Explorer**

**Procedimiento:**

1. Revisar campañas activas enfocándose en correo **delivered**
2. Identificar     mensajes no remediados por incidentes/ZAP
3. Seleccionar     mensajes maliciosos
4. Ejecutar acción de remediación manual (Hard delete si es     necesario)



### **1.5 Configuración de Reglas de Afinación (Identity)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Identities** > **Health issues** o **Settings** > **Microsoft Defender for Identity**

**Procedimiento:**

1. Revisar alertas de Defender for Identity
2. Para     benign true positives o falsos positivos:

○    Crear **exclusiones** o **tuning rules**

○    Documentar justificación



### **1.6 Revisión del Dashboard ITDR (Identities)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Identities** > **Dashboard**

**Procedimiento:**

1. Revisar métricas de exposición de identidades
2. Identificar     usuarios de alto riesgo
3. Verificar     configuración de autenticación multifactor
4. Revisar movimientos laterales sospechosos



### **1.7 Revisión de Health Issues (Identity)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Identities** > **Health center**

**Procedimiento:**

1. Pestaña **Global**: Revisar problemas del servicio
2. Pestaña     **Sensor**: Verificar estado de sensores en Domain Controllers
3. Resolver problemas críticos que afecten cobertura



### **1.8 Verificación de Device Discovery**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Endpoints** > **Device discovery**

**Verificaciones diarias:**

1. Confirmar modo **Standard** activo (no Basic)
2. Revisar     dispositivos descubiertos recientemente
3. *Ruta forense local (opcional)*: C:\ProgramData\Microsoft\Windows     Defender Advanced Threat Protection\Downloads\*.ps1



### **1.9 Verificación de Integración SIEM (Sentinel)**

**Consola:** Defender Portal o Azure Portal
 **Ruta Defender:** **Settings** > **Microsoft Defender XDR** > **Microsoft Sentinel
** **Ruta Azure:** **Sentinel** > **Data connectors**

**Procedimiento:**

1. Verificar estado del conector: **Connected** (Verde)
2. Validar flujo de datos con consulta KQL:

SecurityIncident

| where TimeGenerated > ago(24h)

| summarize count() by ProviderName

 

1. Si count() es 0 o bajo, investigar     conectividad inmediatamente



### **1.10 Verificación de Notificaciones por Email**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Microsoft Defender XDR** > **Email notifications** > **Incidents**

**Procedimiento:**

1. Verificar regla activa configurada
2. Abrir     regla y validar pestaña **Recipient**
3. Usar **Send test email** para confirmar entrega



## **2. TAREAS SEMANALES (Weekly Operations)**

### **2.1 Revisión Profunda de Incidentes y Patrones (Threat Hunting)**

**A. Análisis de Tendencias**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Incidents & alerts** > **Incidents**

**Procedimiento:**

1. Configurar rango: **Last 7 days** o **Last     30 days**
2. Exportar     lista a Excel si necesario
3. Buscar     patrones:

○    Usuarios recurrentes en múltiples incidentes

○    Grupos de dispositivos atacados consistentemente

○    Revisar incidentes *Informational/Low* en masa

**B. Advanced Hunting**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Hunting** > **Advanced hunting**

**Ejemplos de consultas:**

**Búsqueda de persistencia:**

// Tareas programadas sospechosas

DeviceProcessEvents

| where Timestamp > ago(7d)

| where FileName == "schtasks.exe"

| where ProcessCommandLine has "create"

| project Timestamp, DeviceName, ProcessCommandLine, InitiatingProcessFileName

 

**Procedimiento post-consulta:**

1. Usar pestaña **Charts** para visualizar picos
2. Si hay patrón interesante: **Create detection rule**



### **2.2 Threat Analytics Review**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Threat intelligence** > **Threat analytics**

**Procedimiento:**

1. Revisar informes **High impact**
2. Abrir **Analyst     report** de amenazas relevantes
3. Leer TTPs (Tactics, Techniques, Procedures)
4. Revisar     tarjeta **Mitigations review**:

○    Verificar contramedidas aplicadas

○    Identificar dispositivos expuestos

○    Implementar recomendaciones



### **2.3 Revisión de Tendencias de Detección de Email**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Reports** > **Email & collaboration** > **Email & collaboration reports**

**Procedimiento:**

1. Revisar **Mailflow status report**
2. Revisar     **Threat protection status report**
3. Analizar     tendencias de phishing/malware
4. Identificar picos anómalos



### **2.4 Análisis de Usuarios Más Atacados**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Explorer** (Threat Explorer)

**Procedimiento:**

1. Filtrar por rango de tiempo (última semana)
2. Agrupar     por **Recipient**
3. Identificar     top targeted users
4. Considerar agregar a **Priority accounts** (User tags)
5. Ajustar protecciones específicas si necesario



### **2.5 Revisión de Campaign Views**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Campaigns**

**Procedimiento:**

1. Revisar campañas de malware/phishing detectadas
2. Descargar     **threat reports** para análisis
3. Compartir IOCs con equipo de inteligencia



### **2.6 Evaluación de Configuraciones de Endpoint**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Endpoints** > **Configuration management** > **Endpoint security policies**

**Procedimiento:**

1. Revisar políticas de:

○    Antivirus

○    Disk Encryption

○    Firewall

1. Investigar     estado **Error** o **Conflict**
2. Resolver     conflictos con GPOs locales
3. Verificar progresión de reglas ASR de Audit a Block



### **2.7 Configuration Analyzer (Office 365)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Policies & rules** > **Threat policies** > **Configuration analyzer**

**Procedimiento:**

1. Comparar configuración actual vs. plantillas:

○    **Standard recommendations**

○    **Strict recommendations**

1. Revisar     **Settings not following recommendations**
2. Para     cada desviación:

○    Click **Apply recommendation** para alinear

○    O documentar excepción justificada



### **2.8 Revisión de Secure Score**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Secure Score**

**Procedimiento:**

1. Revisar **Recommended actions**
2. Filtrar     por impacto (puntos) y estatus
3. Seleccionar     **Top improvement actions**
4. Para     acciones no implementables:

○    Marcar como **Risk accepted**

○    Documentar justificación



### **2.9 Revisión de Vulnerability Management**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Vulnerability management** > **Dashboard**

**Procedimiento:**

1. Revisar nuevas recomendaciones de remediación
2. Verificar     vulnerabilidades de alta severidad
3. Ajustar     prioridades de remediación
4. Generar reportes para asset owners



### **2.10 Proactive Hunting (Identity)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Hunting** > **Advanced hunting**

**Consultas recomendadas para Identity:**

// Búsqueda de movimientos laterales

IdentityLogonEvents

| where Timestamp > ago(7d)

| where LogonType == "Network"

| summarize count() by AccountName, DeviceName

| where count_ > 10

 



### **2.11 Revisión del Message Center**

**Consola:** Microsoft 365 Admin Center
 **Ruta:** **Health** > **Message center
** **URL:** https://admin.microsoft.com

**Procedimiento:**

1. Filtrar por categoría **Security**
2. Identificar     cambios que afectan:

○    Defender portal

○    Features

○    Licensing

○    Operaciones

1. Planificar adaptaciones necesarias



## **3. TAREAS MENSUALES (Monthly Operations)**

### **3.1 Revisión y Actualización de Tenant Allow/Block List**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Policies & rules** > **Threat policies** > **Tenant Allow/Block Lists**

**Procedimiento:**

1. Revisar entradas temporales expiradas
2. Convertir     a permanentes o eliminar según necesidad
3. Limpiar     reglas de bloqueo obsoletas
4. Documentar todas las excepciones



### **3.2 Afinamiento de Attack Surface Reduction (ASR)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Reports** > **Endpoints** > **Attack surface reduction rules**

**Procedimiento:**

1. Revisar reportes de reglas ASR
2. Identificar     reglas en modo **Audit** con bajo impacto
3. Analizar     falsos positivos:

○    Crear exclusiones si son legítimos

○    Bloquear si son riesgosos

1. Cambiar reglas maduras de Audit a **Block**



### **3.3 Afinamiento de Custom Detection Rules**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Hunting** > **Custom detection rules**

**Procedimiento:**

1. Revisar métricas de reglas creadas
2. Identificar     reglas con muchos falsos positivos
3. Editar     consultas KQL para hacerlas más específicas
4. Desactivar reglas obsoletas



### **3.4 Revisión de Device Onboarding Gaps**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Assets** > **Devices**

**Procedimiento:**

1. Aplicar filtro **Onboarding status: Can     be onboarded**
2. Exportar     lista a CSV
3. Generar     tickets para IT/Soporte
4. Trackear progreso de instalación de agentes



### **3.5 Validación de Exclusiones de Device Discovery**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Endpoints** > **Device discovery** > **Exclusions**

**Procedimiento:**

1. Auditar lista de dispositivos excluidos
2. Verificar     justificación de cada exclusión
3. Remover     exclusiones innecesarias
4. Documentar exclusiones válidas



### **3.6 Auditoría de Salud de Sensores**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Reports** > **Endpoints** > **Device health**

**Procedimiento:**

1. Identificar dispositivos **Inactive** (>7 días sin     señal)
2. Identificar     dispositivos **Impaired communications**
3. Generar reporte para remediar dispositivos problemáticos



### **3.7 Revisión de Setup Guides**

**Consola:** Microsoft 365 Admin Center
 **Ruta:** **Setup** > Filtrar por **Security
** **URL:** https://admin.microsoft.com

**Procedimiento:**

1. Seleccionar guías como:

○    "Protect against advanced threats with Defender"

○    "Enroll devices in Defender for Endpoint"

1. Verificar     pasos **Not started**
2. Completar o documentar excepciones



### **3.8 Revisión de Detection Overrides**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Policies & rules** > **Threat policies**

**Procedimiento:**

1. Identificar phishing delivered due to     overrides
2. Revisar     cada override activo
3. Remover     o ajustar overrides riesgosos
4. Documentar overrides necesarios



### **3.9 Revisión de Spoof & Impersonation Insights**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Policies & rules** > **Threat policies** > **Anti-phishing**

**Procedimiento:**

1. Revisar **Spoof intelligence insight**
2. Revisar     **Impersonation insight**
3. Ajustar     políticas basado en hallazgos
4. Actualizar dominios permitidos/bloqueados



### **3.10 Revisión de Priority Account Membership**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Email & collaboration** > **User tags**

**Procedimiento:**

1. Revisar lista de **Priority accounts**
2. Agregar     nuevos ejecutivos/VIPs
3. Remover     usuarios que ya no califican
4. Mantener lista actualizada para protecciones especiales



### **3.11 Revisión de Alertas Afinadas (Identity)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Identities** > **Exclusions**

**Procedimiento:**

1. Revisar reglas de tuning configuradas
2. Validar     que siguen siendo válidas
3. Remover     reglas obsoletas
4. Ajustar alcance si necesario



### **3.12 Tracking de Cambios en Productos**

**Fuentes:**

●    **Message Center**: https://admin.microsoft.com

●    **Defender XDR What's New**: https://learn.microsoft.com/en-us/defender-xdr/whats-new

●    **Defender for Identity What's New**: https://learn.microsoft.com/en-us/defender-for-identity/whats-new

**Procedimiento:**

1. Revisar actualizaciones mensuales
2. Evaluar     impacto en operaciones
3. Planificar     adopción de nuevas features
4. Actualizar procedimientos según cambios



### **3.13 Operacional Review (Governance)**

**Métricas a revisar:**

●    Volumen de incidentes (trend)

●    Time-to-triage promedio

●    Time-to-contain promedio

●    Ratio de falsos positivos/negativos

●    Rutas de ataque recurrentes

●    Coverage gaps identificados

**Entregables:**

1. Documento de acciones y owners
2. Ajustes     a playbooks
3. Recomendaciones de mejora



## **4. TAREAS AD-HOC (As Needed)**

### **4.1 Respuesta a Incidentes Mayores**

**A. Aislamiento de Dispositivos**

**Consola:** Página del incidente
 **Procedimiento:**

1. Pestaña **Devices** del incidente
2. Seleccionar     dispositivo comprometido
3. Click **...**     (tres puntos) > **Isolate device**
4. **Opción**: Permitir Outlook, Teams, Skype para comunicación SOC
5. Confirmar acción (corta conexiones C2)

**B. Bloqueo de Indicadores (IoCs)**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Endpoints** > **Indicators**

**Procedimiento:**

1. Click **Add indicator**
2. Tipo: File hash, IP, URL o Domain
3. Acción:     **Alert and Block**
4. Alcance:     Toda la organización
5. Propagación: Minutos

**C. Purga de Correos Maliciosos**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Email & collaboration** > **Explorer**

**Procedimiento:**

1. Buscar correos por Subject o Sender IP
2. Seleccionar     todos los coincidentes (incluso delivered)
3. Ejecutar **Hard delete** (elimina de buzones)



### **4.2 Threat Hunting Proactivo**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Hunting** > **Advanced hunting**

**Escenarios:**

●    Búsqueda de nuevas TTPs de amenazas emergentes

●    Validación de hipótesis post-incidente

●    Búsqueda de indicadores de compromiso específicos

**Procedimiento:**

1. Desarrollar consultas KQL basadas en inteligencia
2. Ejecutar     queries en tablas relevantes
3. Visualizar     resultados en Charts
4. Compartir     queries en **Shared queries**
5. Crear **Custom detection rules** si aplica



### **4.3 Despliegue de Nuevas Características**

**A. Preview Features**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Microsoft Defender XDR** > **General** > **Preview features**

**Procedimiento:**

1. Activar **Turn on preview features**
2. Acceso     a nuevos modelos de detección
3. Testear     en ambiente controlado
4. Documentar hallazgos

**B. Onboarding de Nuevos Sistemas Operativos**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Endpoints** > **Onboarding**

**Procedimiento:**

1. Seleccionar SO (Linux, macOS, etc.)
2. Descargar     script o perfil de configuración
3. Desplegar     vía Puppet/Ansible/manual
4. Verificar aparición en Device inventory



### **4.4 Tabletop Exercises (IR Readiness)**

**Escenarios recomendados:**

●    Ransomware outbreak

●    Business email compromise

●    Data exfiltration

●    Insider threat

**Procedimiento:**

1. Definir escenario y objetivos
2. Convocar stakeholders (SOC, IT, Legal, Comms)
3. Ejecutar     simulación
4. Documentar     gaps y acciones correctivas
5. Actualizar playbooks



### **4.5 Despliegue de Automation**

**Consola:** Microsoft Defender Portal
 **Opciones:**

●    **Automated investigation & response (AIR)** settings

●    **Power Automate** playbooks

●    **Logic Apps** workflows

**Procedimiento:**

1. Identificar tareas repetitivas candidatas
2. Desarrollar/testear     automation en piloto
3. Desplegar     a producción con aprobación
4. Monitorear     efectividad
5. Iterar basado en resultados



### **4.6 Ajuste de Priorización de Incidentes**

**Consola:** Microsoft Defender Portal
 **Ruta:** **Settings** > **Microsoft Defender XDR** > **Incident prioritization**

**Procedimiento:**

1. Revisar lógica de scoring actual
2. Ajustar     pesos basados en:

○    Criticidad de activos

○    Severidad de alertas

○    Impacto al negocio

1. Testear     con incidentes históricos
2. Implementar     cambios
3. Monitorear mejora en triaje



## **5. TAREAS SEMESTRALES (Every 6 Months)**

### **5.1 Revisión de Roles y Responsabilidades (SOC Governance)**

**Procedimiento:**

1. Reconfirmar modelo de SOC oversight
2. Validar     tiers de monitoreo (Tier 1/2/3)
3. Revisar     separación de roles:

○    SecOps vs Security Engineering

○    Threat Intel/Analytics

○    CSIRT engagement

1. Documentar     cambios organizacionales
2. Actualizar matriz RACI

**Referencia:** https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-roles



### **5.2 Mantenimiento de Runbooks y Playbooks**

**Procedimiento:**

1. Revisar todos los playbooks de respuesta
2. Incorporar     lecciones aprendidas (últimos 6 meses)
3. Actualizar     basado en cambios de plataforma
4. Validar     rutas de escalamiento
5. Actualizar     checklists operacionales
6. Versionar y publicar actualizaciones



### **5.3 Revisión de Service Health & Domain Configuration (Identity)**

**Consola:** Microsoft 365 Admin Center
 **Ruta:** **Health** > **Service health**

**Procedimiento:**

1. Revisar incidentes históricos del servicio
2. Verificar     setup de sensores en nuevos Domain Controllers
3. Ejecutar cmdlet PowerShell de verificación de dominio:

Test-MDIReadiness

 

1. Documentar hallazgos y remediar



### **5.4 Revisión de Licenciamiento y Feature Register**

**Procedimiento:**

1. Auditar features habilitadas vs. licencias adquiridas
2. Identificar     features no utilizadas
3. Evaluar     ROI de cada componente
4. Considerar     ajustes de licensing
5. Documentar decisiones



### **5.5 Revisión de Coordinación con Stakeholders**

**Stakeholders clave:**

●    IT Operations

●    Legal/Compliance

●    Privacy Office

●    HR (para phishing/DLP campaigns)

●    Communications

**Procedimiento:**

1. Reunión de revisión con cada stakeholder
2. Actualizar     acuerdos de coordinación
3. Refinar     flujos de escalamiento
4. Documentar     lecciones aprendidas
5. Planificar mejoras para próximos 6 meses



## **6. TAREAS ANUALES (Annual Operations)**

### **6.1 Revisión Completa del Programa**

**Componentes a revisar:**

1. **Procesos operativos:
        
   **     

○    Efectividad de cadencias (diaria/semanal/mensual)

○    Tiempos de respuesta vs. SLAs

○    Calidad de documentación

1. **Staffing:
        
   **     

○    Carga de trabajo por analista

○    Necesidades de capacitación

○    Gaps de skillset

1. **Controles     de acceso:
        
   **     

○    Revisión de permisos RBAC

○    Segregación de funciones

○    Acceso a sistemas críticos

1. **Registro     de features:
        
   **     

○    Features habilitadas vs. disponibles

○    Roadmap de adopción

1. **Mecanismos     de coordinación:
        
   **     

○    IT, Legal, Compliance, Privacy

○    Efectividad de comunicación

○    Mejoras necesarias

**Referencia:** https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-roles



### **6.2 Rebaseline Estratégico de Postura**

**Procedimiento:**

1. Revisar **Threat Analytics** del año completo
2. Identificar     tendencias de amenazas en sector/región
3. Comparar     controles actuales vs. nuevas amenazas
4. Actualizar     **threat model**
5. Definir     prioridades estratégicas para próximo año
6. Ajustar presupuesto y roadmap técnico

**Referencia:** https://learn.microsoft.com/en-us/defender-endpoint/mde-sec-ops-guide



### **6.3 Auditoría Externa y Compliance**

**Procedimiento:**

1. Preparar evidencia para auditorías:

○    Logs de incidentes resueltos

○    Reportes de vulnerability management

○    Evidencia de remediaciones

1. Revisar     compliance con frameworks:

○    NIST CSF

○    ISO 27001

○    Regulaciones locales

1. Documentar     gaps identificados
2. Plan de remediación para próximo ciclo



### **6.4 Revisión de Anti-Phishing / DLP Campaigns**

**Procedimiento:**

1. Evaluar efectividad de campañas del año
2. Métricas     de phishing simulation:

○    Click rate trends

○    Reporting rate trends

1. Feedback de HR/Legal/Training stakeholders
2. Ajustar     approach para próximo año
3. Actualizar políticas DLP basadas en incidentes

**Referencia:** https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-tasks



## **7. MATRIZ DE VERIFICACIÓN CONSOLIDADA**

| **Mecanismo**          | **Frecuencia** | **Ruta de Verificación**                | **KPI de Éxito**                                             |
| ---------------------- | -------------- | --------------------------------------- | ------------------------------------------------------------ |
| **Incidents Queue**    | Diaria         | Incidents & alerts > Incidents          | High/Medium incidentes resueltos en SLA                      |
| **Action Center**      | Diaria         | Actions & submissions > Action  center  | Pending queue vacía o <24h; History  con actividad regular   |
| **Device Discovery**   | Semanal        | Settings > Device discovery             | Modo Standard; Seen devices  actualizados; Can be onboarded decreciendo |
| **SIEM Integration**   | Semanal        | Settings > Microsoft Sentinel           | Connected; latencia  <15 min (query KQL)                     |
| **Email Alerts**       | Mensual        | Settings > Email notifications          | Test email exitoso;  reglas cubren High/Medium               |
| **Setup Guides**       | Trimestral     | M365 Admin > Setup                      | Guías Completed o excepciones  documentadas                  |
| **Threat Analytics**   | Semanal        | Threat intelligence > Threat  analytics | Revisar High impact;  mitigations implementadas              |
| **Secure Score**       | Semanal        | Secure Score dashboard                  | Score trending up;  top actions implementadas/accepted       |
| **Custom Detections**  | Mensual        | Hunting > Custom detection rules        | Reglas con <10% false positive rate                          |
| **Vulnerability Mgmt** | Semanal        | Vulnerability management > Dashboard    | Critical/High vulns  con remediation assigned                |



## **8. MATRIZ RACI CONSOLIDADA**

| **Tarea**                     | **Responsable (R)** | **Aprobador (A)** | **Consultado (C)**         | **Informado (I)**     |
| ----------------------------- | ------------------- | ----------------- | -------------------------- | --------------------- |
| Monitoreo Diario Incidentes   | Tier 1 Analyst      | SOC Lead          | -                          | -                     |
| Triaje y Priorización         | Tier 1 Analyst      | SOC Lead          | -                          | -                     |
| Aprobación Acciones AIR       | Tier 1 Analyst      | SOC Lead          | -                          | IT Admin (servidores) |
| Threat Hunting Semanal        | Tier 2/3 Analyst    | SOC Lead          | CISO                       | -                     |
| Revisión Threat Analytics     | Tier 2/3 Analyst    | SOC Lead          | -                          | -                     |
| Ajuste Políticas (ASR/Spam)   | Security Admin      | CISO              | IT Ops                     | End Users             |
| Integración SIEM              | Security Engineer   | SOC Lead          | Azure Admin                | -                     |
| Onboarding Dispositivos       | IT Operations       | Security Admin    | -                          | Asset Owners          |
| Custom Detection Rules        | Tier 2/3 Analyst    | SOC Lead          | -                          | -                     |
| Vulnerability Remediation     | IT Operations       | IT Manager        | Security Admin             | Asset Owners          |
| Tabletop Exercises            | CSIRT Lead          | CISO              | SOC, IT, Legal             | Executive Team        |
| Governance Review (Semestral) | SOC Manager         | CISO              | All Stakeholders           | Executive Team        |
| Program Review (Anual)        | CISO                | CIO/CEO           | SOC, IT, Legal, Compliance | Board                 |



## **9. REFERENCIAS OFICIALES DE MICROSOFT**

### **Documentación Core:**

1. **Microsoft Defender XDR en el Portal**:     https://learn.microsoft.com/en-us/unified-secops/defender-xdr-portal
2. **Incident Queue Management**:     https://learn.microsoft.com/en-us/defender-xdr/incident-queue
3. **Investigate Incidents**:     https://learn.microsoft.com/en-us/defender-xdr/investigate-incidents
4. **Manage Incidents**:     https://learn.microsoft.com/en-us/defender-xdr/manage-incidents
5. **Action Center**:     https://learn.microsoft.com/en-us/defender-xdr/m365d-autoir-actions
6. **Investigate Alerts**:     https://learn.microsoft.com/en-us/defender-xdr/investigate-alerts

### **Guías Operativas por Workload:**

1. **Defender for Endpoint SecOps Guide**:     https://learn.microsoft.com/en-us/defender-endpoint/mde-sec-ops-guide
2. **Defender for Office 365 SecOps Guide**:     https://learn.microsoft.com/en-us/defender-office-365/mdo-sec-ops-guide
3. **Defender for Identity Operational Guide**:     https://learn.microsoft.com/en-us/defender-for-identity/ops-guide/ops-guide

### **SOC Integration & Tasks:**

1. **SOC Maintenance Tasks**:     https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-tasks
2. **SOC Roles & Responsibilities**:     https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-roles
3. **Deploy & Configure Defender XDR**:     https://learn.microsoft.com/en-us/defender-xdr/deploy-configure-m365-defender

### **Features & Configuration:**

1. **Device Discovery**:     https://learn.microsoft.com/en-us/defender-endpoint/configure-device-discovery
2. **Sentinel Integration**:     https://learn.microsoft.com/en-us/azure/sentinel/microsoft-365-defender-sentinel-integration
3. **Email Notifications**:     https://learn.microsoft.com/en-us/defender-xdr/m365d-notifications-incidents
4. **Threat Analytics**:     https://learn.microsoft.com/en-us/defender-xdr/threat-analytics
5. **Advanced Hunting**:     https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-modes
6. **Configuration Analyzer (MDO)**:     https://learn.microsoft.com/en-us/defender-office-365/step-by-step-guides/optimize-and-correct-security-policies-with-configuration-analyzer
7. **False Positives/Negatives**:     https://learn.microsoft.com/en-us/defender-xdr/m365d-autoir-report-false-positives-negatives
8. **Microsoft Secure Score**:     https://learn.microsoft.com/en-us/defender-xdr/microsoft-secure-score
9. **Preview Features**:     https://learn.microsoft.com/en-us/defender-xdr/preview



## **10. NOTAS FINALES**

### **Adaptabilidad del Documento**

Este manual consolidado es un "organismo vivo" que debe:

●    Revisarse **trimestralmente** para reflejar cambios en Defender XDR

●    Actualizarse cuando Microsoft lance nuevas features

●    Adaptarse a cambios en tolerancia al riesgo organizacional

●    Incorporar lecciones aprendidas de incidentes mayores

### **Personalización Recomendada**

Cada organización debe adaptar:

●    **SLAs** basados en criticidad de negocio

●    **Cadencias** según tamaño de SOC y volumen de alertas

●    **Alcance de automation** según madurez operacional

●    **Roles RACI** según estructura organizacional

### **Contacto y Feedback**

Para mejoras a este documento o preguntas sobre implementación, utilizar los canales oficiales de feedback de Anthropic o contactar al CISO responsable del deployment.

 

 