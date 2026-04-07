# **Guía Operacional Consolidada: Microsoft Defender for Endpoint & Vulnerability Management**

## **Resumen Ejecutivo**

Esta guía consolidada integra todas las actividades operacionales recomendadas por Microsoft para la gestión efectiva de Microsoft Defender for Endpoint (MDE) y Microsoft Defender Vulnerability Management (MDVM). Cada tarea incluye la consola específica, la navegación paso a paso, y los roles responsables para asegurar una operación SecOps robusta y eficiente.



## **1. OPERACIONES DIARIAS**

### **1.1 Revisar Centro de Acciones (Action Center)**

**Roles:** Operador, Monitor, Incident Responder

**Consola:** Microsoft Defender Portal (https://security.microsoft.com)

**Navegación:**

1. **Actions & submissions** > **Action     center**
2. Alternativamente: Dashboard > **Automated     investigation & response** > **Approve in Action Center**

**Pasos de Ejecución:**

1. Seleccionar pestaña **Pending**
2. Filtrar     por **Severity** (priorizar High/Critical)
3. Revisar     cada acción pendiente:

○    Seleccionar el ítem para ver detalles en el panel lateral

○    Revisar **Entity** (archivo/proceso), **Threat** y **Remediation** propuesta

○    Click en **Open investigation page** para ver el gráfico de investigación

1. **Decisión:**

○    **Approve**: Si la entidad es maliciosa

○    **Reject**: Si es una aplicación legítima (LOB)

○    **Go hunt**: Para análisis forense avanzado (genera query KQL)

1. Cambiar a pestaña **History** para auditar acciones     completadas

**Objetivo:** Validar que las remediaciones automatizadas (AIR) son apropiadas y ejecutar acciones que requieren aprobación manual.



### **1.2 Monitorear Cola de Incidentes**

**Roles:** Operador, Monitor, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Incidents & alerts** > **Incidents**

**Pasos de Ejecución:**

1. **Aplicar filtros:**

○    Time range: **Last 24 hours**

○    Severity: **High** y **Medium**

○    Status: **New** y **In Progress**

1. **Análisis     de incidente:**

○    Seleccionar nombre del incidente

○    Revisar **Attack Story** (gráfico de ataque)

○    Navegar a pestaña **Alerts** (secuencia cronológica)

○    Revisar pestaña **Devices** > seleccionar dispositivo > **Timeline**

1. **Respuesta:**

○    Click en **Manage incident**

○    Asignar analista

○    Actualizar status a **In Progress**

○    Documentar acciones en **Comments & history**

**Objetivo:** Triaje y respuesta inmediata a amenazas activas detectadas en las últimas 24 horas.



### **1.3 Gestionar Falsos Positivos/Negativos**

**Roles:** Operador, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación para Falsos Positivos:**

1. **Incidents & alerts** > **Alerts**
2. Seleccionar     la alerta específica
3. Click     en **Manage alert**
4. **Status:** Resolved
5. **Classification:** False positive
6. Crear **Suppression     rule** si es necesario:

○    **Settings** > **Endpoints** > **Rules** > **Alert suppression**

**Navegación para Submission a Microsoft:**

1. **Actions & submissions** > **Submissions**
2. Click en **Submit to Microsoft for analysis**
3. Seleccionar     tipo: **File**, **File Hash**, o **URL**
4. Cargar     archivo o introducir SHA-256
5. Seleccionar     clasificación:

○    **Clean (False positive)**

○    **Malware (False negative)**

**Para detecciones sin archivo (Fileless):**

En el endpoint ejecutar:
 C:\ProgramData\Microsoft\Windows Defender\Platform\<version>\MpCmdRun.exe -GetFiles

1.  
2. Subir MpSupport.cab vía     https://www.microsoft.com/wdsi/filesubmission

**Objetivo:** Afinar el motor de detección reduciendo ruido y mejorando precisión.



### **1.4 Verificar Configuración y Salud de MDE**

**Roles:** Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Reports** > **Device health and     compliance**
2. Seleccionar pestaña **Sensor health & OS**

**Pasos de Ejecución:**

1. Revisar **Sensor health card**
2. Identificar     dispositivos:

○    **Inactive** (sin señal > 7 días)

○    **Misconfigured** (comunicaciones deterioradas)

**Query en Advanced Hunting:
** DeviceInfo| summarize arg_max(Timestamp, *) by DeviceId| where Timestamp < ago(24h)| project DeviceName, SensorHealthState, LastKnownIP, Timestamp

1.  
2. **Acciones     correctivas:**

○    Verificar conectividad de red

○    Revisar estado del servicio "Sense"

○    Re-ejecutar scripts de onboarding

**Objetivo:** Detectar "puntos ciegos" donde los sensores han dejado de reportar datos.



### **1.5 Revisar Configuración de Device Discovery**

**Roles:** Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Device discovery**

**Pasos de Ejecución:**

1. Verificar modo: **Standard discovery** (preferido sobre     Basic)

○    Standard permite sondeo activo (ARP, WSD, SSH, WINRM)

1. **Análisis     de cobertura:**

○    **Assets** > **Devices**

○    Filtrar: **Onboarding status** = **Can be onboarded**

○    Exportar lista para campaña de onboarding

1. **Configurar     redes monitoreadas:**

○    **Settings** > **Device discovery** > **Monitored networks**

○    Asegurar subnets corporativas en **Monitor**

○    Configurar subnets OT en **Ignore** si es necesario

**Objetivo:** Maximizar visibilidad de dispositivos no gestionados y reducir superficie de ataque.



### **1.6 Validar Políticas de Protección de Endpoints**

**Roles:** Operador, Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Configuration management** > **Dashboard**

**Pasos de Ejecución:**

1. Revisar widget **Device compliance**
2. Identificar     dispositivos **Non-compliant**
3. **Auditoría     de reglas ASR:**

○    **Reports** > **Attack surface reduction rules**

○    Pestaña **Configurations**

○    Verificar reglas críticas en modo **Block**:

■    "Block credential stealing from LSASS"

■    "Block Office apps from creating child processes"

1. Seleccionar     regla específica para ver dispositivos afectados
2. Identificar conflictos de política (GPO vs Intune)

**Objetivo:** Asegurar aplicación consistente de políticas de seguridad en todos los endpoints.



### **1.7 Escalar Incidentes No Resueltos**

**Roles:** Operador, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Incidents & alerts** > **Incidents**

**Pasos de Ejecución:**

1. Filtrar:

○    **Status:** In Progress

○    **Last Updated:** > 24 hours

1. Abrir     incidente estancado
2. Click     en **Manage incident**
3. Agregar     tag: Tier2_Escalation o Blocker_IntelReq
4. En     pestaña **Comments & history** documentar:

○    Barreras específicas (ej. "Require RE of binary X")

○    Información adicional necesaria

1. **Reassign** a analista Tier 2/3 o IR     team lead

**Objetivo:** Evitar que incidentes críticos se estanquen por falta de escalamiento.



### **1.8 Actualizar Estado y Documentación de Incidentes**

**Roles:** Operador

**Consola:** Microsoft Defender Portal

**Navegación:**

1. Cada incidente trabajado durante el     turno

**Pasos de Ejecución:**

1. En **Manage incident**:

○    Actualizar **Status** (de New → In Progress → Resolved)

○    Completar **Classification** al resolver:

■    True positive - Security incident

■    True positive - Benign

■    False positive

1. En **Comments**     agregar:

○    Acciones tomadas fuera de plataforma

○    Contactos realizados

○    Evidencia recolectada

1. Asegurar **Activity log** refleje trabajo completo

**Objetivo:** Mantener trazabilidad completa y alimentar ML para mejorar detecciones.



### **1.9 Comunicar Hallazgos Críticos a Manager**

**Roles:** Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. Página del incidente de alta prioridad

**Pasos de Ejecución:**

1. Click en **... (More actions)** > **Export     to PDF**
2. El PDF     incluye:

○    Attack graph

○    Alert timeline

○    Impacted assets

1. Verificar     configuración de notificaciones:

○    **Settings** > **Endpoints** > **Email notifications**

○    Confirmar manager en lista para alertas "High Severity"

1. Enviar PDF con resumen ejecutivo si es necesario

**Objetivo:** Mantener visibilidad ejecutiva de amenazas críticas en tiempo real.



### **1.10 Asegurar Queue Hygiene (Inbox Zero)**

**Roles:** Operador, Monitor

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Incidents & alerts** > **Incidents**

**Pasos de Ejecución:**

1. Filtrar **Status: New**
2. Meta:     Cero incidentes High/Critical en estado "New"
3. **Remediación     en bloque** (si aplica):

○    Seleccionar checkbox **Select all**

○    **Manage incidents**

○    **Status:** Resolved

○    **Classification:** True positive - Benign (para tests conocidos)

**Objetivo:** Prevenir acumulación de backlog que oculta amenazas reales.



### **1.11 Monitorear Exposure Score (MDVM)**

**Roles:** Operador, Manager

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Vulnerability management** > **Dashboard**

**Pasos de Ejecución:**

1. Revisar **Exposure Score** organizacional
2. Aumento     súbito indica:

○    Nuevas vulnerabilidades críticas

○    Introducción de activos muy vulnerables

1. Nota     cambios día a día
2. Click en score para drill-down de factores contribuyentes

**Objetivo:** Detección temprana de incremento de superficie de ataque.



### **1.12 Revisar Top Security Recommendations (MDVM)**

**Roles:** Administrator, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Vulnerability     management** > **Dashboard**
2. Widget **Top security recommendations**

**Pasos de Ejecución:**

1. Ordenar por **Score impact** (mayor reducción de riesgo)
2. Identificar     acciones críticas (ej. "Update Google Chrome")
3. Click     en recomendación para ver:

○    Dispositivos afectados

○    CVEs relacionados

○    Opciones de remediación

1. Comunicar     prioridades a equipo IT
2. Click **Remediation options** > **Request remediation**

**Objetivo:** Priorizar patching basado en reducción de riesgo, no solo criticidad.



### **1.13 Validar Escaneos de Vulnerabilidad**

**Roles:** Operador

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Vulnerability management** > **Dashboard**

**Pasos de Ejecución:**

1. Verificar fecha del último escaneo por dispositivo
2. Identificar dispositivos sin datos recientes

**Query Advanced Hunting:
** DeviceTvmSoftwareInventory| summarize LastSeen = max(Timestamp) by DeviceId, DeviceName| where LastSeen < ago(24h)

1.  
2. Investigar conectividad de sensores problemáticos

**Objetivo:** Asegurar datos actualizados de vulnerabilidades en todos los activos.



## **2. OPERACIONES SEMANALES**

### **2.1 Analizar Tendencias de Incidentes**

**Roles:** Operador, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Reports**     > **Endpoints** > **Threat protection**

**Pasos de Ejecución:**

1. Revisar histogramas:

○    **Incident trends**

○    **Alert trends**

1. Identificar spikes en categorías específicas

**Advanced Hunting query:
** AlertInfo| where Timestamp > ago(7d)| summarize Count = count() by Title| sort by Count desc| render barchart

1.  
2. Análisis:     ¿Qué está generando más alertas?
3. Determinar si es legítimo o requiere tuning

**Objetivo:** Identificar patrones de ataque emergentes y oportunidades de optimización.



### **2.2 Revisar y Actualizar Playbooks de Respuesta**

**Roles:** Operador, Incident Responder

**Consola:** Microsoft Defender Portal + Microsoft Sentinel (si aplica)

**Navegación:**

1. **Settings** > **Endpoints**     > **Rules** > **Automated investigation**
2. Si usa Sentinel: **Automation** > **Active playbooks**

**Pasos de Ejecución:**

1. Revisar niveles de remediación por device group
2. Verificar     **Run history** de playbooks
3. Identificar     fallos por:

○    Cambios de permisos API

○    Timeouts

○    Configuración incorrecta

1. Ajustar     automation levels:

○    **Semi - require approval** → **Full** si backlog es alto

○    **Full** → **Semi** si hay muchos FP

**Objetivo:** Optimizar balance entre automatización y control humano.



### **2.3 Auditar Permisos y Controles de Acceso (RBAC)**

**Roles:** Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints** > **Roles**

**Pasos de Ejecución:**

1. Seleccionar cada rol (ej. "Tier 1 Analyst")
2. Verificar     permisos:

○    **Live Response - Advanced** debe estar restringido a Tier 3

○    **Manage security settings** solo para Administrators

1. **Audit     log check:**

○    **Settings** > **Endpoints** > **Advanced features** > **Unified audit log**

○    Buscar: Operation = "AddRole" o Operation = "UpdateRole"

1. Investigar cambios no autorizados

**Objetivo:** Mantener least privilege y segregación de funciones.



### **2.4 Integrar/Revisar Flujos de Datos SIEM**

**Roles:** Administrator

**Consola:** Microsoft Sentinel (si aplica)

**Navegación:**

1. **Data connectors** > **Microsoft Defender for Endpoint**

**Pasos de Ejecución:**

1. Verificar status: **Connected**
2. Revisar     gráfico "Data received"

○    Flatline = integración rota

**Validación de calidad de datos:
** SecurityAlert| where ProviderName == "MDATP"| take 10

1.  
2. Verificar     latencia < 15 minutos
3. Si hay     problemas:

○    Revisar permisos del service principal

○    Verificar quotas de API

**Objetivo:** Asegurar correlación efectiva entre MDE y SIEM.



### **2.5 Evaluar Arquitectura del Sistema**

**Roles:** Architect

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints**     > **Onboarding**
2. **Evaluation tutorials** > **Evaluation     lab**

**Pasos de Ejecución:**

1. Verificar paquetes de onboarding actualizados para todos los     OS
2. **Simulación     en Lab:**

○    Provisionar dispositivo de prueba

○    Ejecutar escenario de ataque (ej. "APT29")

○    Validar cadena completa:

■    Sensor detection → Cloud reporting → AIR execution

1. Documentar     tiempos de respuesta
2. Identificar cuellos de botella

**Objetivo:** Validar resiliencia y escalabilidad de la arquitectura.



### **2.6 Revisar Feeds de Threat Intelligence**

**Roles:** Operador, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Threat intelligence** > **Threat analytics**

**Pasos de Ejecución:**

1. Filtrar por **High impact**
2. Revisar threat cards activos (ej. "Volt     Typhoon")
3. Verificar     widget **Impacted assets**
4. Para     amenazas relevantes:

○    Exportar lista de dispositivos vulnerables

○    Crear plan de mitigación

○    Comunicar a IT para patching urgente

1. Revisar sección **Analyst report** para TTPs

**Objetivo:** Proactividad ante campañas de amenazas dirigidas.



### **2.7 Probar Playbooks Automatizados**

**Roles:** Operador

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Evaluation tutorials** > **Evaluation lab**

**Pasos de Ejecución:**

1. Crear incidente de prueba en lab
2. Ejecutar archivo test que trigger playbook
3. **Validación:**

○    **Action Center** > **History**: Verificar acción ejecutada

○    **Incidents** queue: Verificar comentarios automáticos

○    Verificar estado actualizado según playbook

1. Documentar fallas y ajustar

**Objetivo:** Asegurar que automatización funciona en escenarios reales.



### **2.8 Auditar Onboarding/Offboarding de Dispositivos**

**Roles:** Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Assets**     > **Devices**

**Pasos de Ejecución:**

1. Filtrar: **Sensor health state: Inactive**     > 30 días
2. Cross-reference con Active Directory "Disabled Computers"     OU
3. Para     dispositivos decomisionados:

○    Usar API o offboarding script

○    Remover de inventario

1. Mantener higiene de licencias

**Query para dispositivos zombie:
** DeviceInfo| summarize LastSeen = max(Timestamp) by DeviceId, DeviceName| where LastSeen < ago(30d)

1.  

**Objetivo:** Mantener inventario preciso y optimizar costos de licenciamiento.



### **2.9 Reportar Tendencias Semanales a Manager**

**Roles:** Incident Responder, Manager

**Consola:** Microsoft Defender Portal + Power BI (opcional)

**Navegación:**

1. **Reports** > **Endpoints**     (para reportes básicos)
2. Power BI Desktop para reportes avanzados

**Pasos de Ejecución Power BI:**

1. **Get Data** > **OData Feed**
2. URL: https://api.securitycenter.microsoft.com/api/Incidents
3. Autenticación:     Organizational Account
4. Crear     visualizaciones:

○    Incidents by Severity

○    Mean Time to Resolve (MTTR)

○    Top Impacted Users

1. Publicar a workspace gerencial o exportar PDF

**Objetivo:** Proveer visibilidad ejecutiva de tendencias operacionales.



### **2.10 Revisar y Actualizar Matriz de Escalamiento**

**Roles:** Manager, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints** > **Email notifications**

**Pasos de Ejecución:**

1. Verificar destinatarios para alertas "High Severity"
2. Actualizar     según rotación on-call actual
3. Remover     usuarios que cambiaron de rol
4. **Gestión     de Tags:**

○    Revisar tags usados en incidentes

○    Agregar nuevos según rutas de escalamiento

○    Documentar en runbook interno

1. Sincronizar con calendario de guardias

**Objetivo:** Asegurar respuesta rápida mediante contacto correcto.



### **2.11 Revisar Microsoft 365 Message Center**

**Roles:** Administrator, Architect

**Consola:** Microsoft 365 Admin Center

**Navegación:**

1. **Health**     > **Message center**

**Pasos de Ejecución:**

1. Filtrar mensajes relacionados con:

○    Microsoft Defender XDR

○    Microsoft Defender for Endpoint

1. Identificar:

○    Cambios próximos en funcionalidad

○    Mantenimientos programados

○    Nuevas features

1. Planificar     ajustes de configuración
2. Comunicar a equipo SecOps

**Objetivo:** Preparación proactiva ante cambios de plataforma.



### **2.12 Análisis de Vulnerabilidades Críticas con Exploits (MDVM)**

**Roles:** Analyst L2, Manager

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Vulnerability management** > **Weaknesses**

**Pasos de Ejecución:**

1. Filtrar por:

○    **Severity: Critical**

○    **Exploit available: Yes**

1. Ordenar     por **Exposed devices** (descendente)
2. Exportar     lista de dispositivos
3. Para     cada CVE crítico:

○    Click para ver detalles

○    Revisar **Security recommendation**

○    Generar remediation request

1. Enviar reporte priorizado a IT con SLA

**Objetivo:** Parcheo urgente de vulnerabilidades explotables activamente.



### **2.13 Revisión de Exclusiones de Antivirus**

**Roles:** Architect, Administrator

**Consola:** Microsoft Intune / Microsoft Defender Portal

**Navegación:**

1. **Intune** > **Endpoint Security**     > **Antivirus policies**
2. Alternativamente: **Defender** > **Settings** > **Endpoints**     > **Rules**

**Pasos de Ejecución:**

1. Revisar cada política con exclusiones
2. Identificar     exclusiones amplias peligrosas:

○    C:\Users\*

○    C:\Program Files\*

○    Wildcards excesivos

1. Validar     justificación de cada exclusión
2. Solicitar     evidencia de necesidad de negocio
3. Reducir     scope o eliminar exclusiones innecesarias
4. Documentar excepciones aprobadas

**Objetivo:** Minimizar gaps de protección causados por exclusiones.



## **3. OPERACIONES MENSUALES**

### **3.1 Revisar Secure Score y Postura de Seguridad**

**Roles:** Manager, CISO

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Secure score** (o **Exposure Management** en     interfaces nuevas)

**Pasos de Ejecución:**

1. Analizar **Score breakdown**
2. Comparar     contra benchmark (industria/tamaño similar)
3. Click **Recommended     actions**
4. Filtrar     por **Score impact** (mayor impacto primero)
5. Para     acciones top:

○    Programar en Change Control Board

○    Asignar owner

○    Establecer deadline

1. Para     acciones no viables:

○    **Manage** > **Risk accepted**

○    Documentar justificación

**Objetivo:** Mejora continua de postura de seguridad medible.



### **3.2 Evaluar Cumplimiento con Políticas y Regulaciones**

**Roles:** Manager, CISO

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Reports** > **Device health and compliance**

**Pasos de Ejecución:**

1. Pestaña **Microsoft Defender Antivirus health**
2. Verificar     **Antivirus mode card**:

○    Mayoría debe estar en "Active" (no Passive)

1. Si     integrado con Intune:

○    **Endpoints** > **Configuration management** > **Dashboard**

○    Revisar **Device compliance** status

1. Identificar     dispositivos non-compliant
2. Verificar     si Conditional Access está bloqueando acceso
3. Generar reporte de compliance para auditoría

**Objetivo:** Demostrar cumplimiento regulatorio y contractual.



### **3.3 Presentar Reportes Ejecutivos a Board**

**Roles:** Manager, CISO

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Reports** > **Endpoints** > **Monthly Security Summary**

**Pasos de Ejecución:**

1. Generar **PDF report** pre-construido que incluye:

○    Prevented threats

○    Secure Score trends

○    Web protection status

1. Complementar con insights de **Threat analytics**:

○    "Bloqueamos 5 intentos de Silk Typhoon este mes"

1. Incluir     métricas clave:

○    MTTR (Mean Time to Resolve)

○    Coverage % (dispositivos protegidos)

○    Top threat families

1. Narrativa de ROI para justificar inversión

**Objetivo:** Visibilidad ejecutiva del valor de seguridad para el negocio.



### **3.4 Realizar Vulnerability Assessment Completo**

**Roles:** Administrator, Architect

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Vulnerability management** > **Dashboard**

**Pasos de Ejecución:**

1. Revisar **Exposure score** organizacional
2. Analizar     **Top security recommendations**
3. Para     vulnerabilidades críticas:

○    Seleccionar CVE

○    Click **Remediation options** > **Open remediation page**

○    **Submit request** (genera Security Task en Intune)

1. Generar     reporte de:

○    Vulnerabilities by severity

○    Time to remediate trends

○    Assets más vulnerables

1. Presentar a comité de gestión de riesgos

**Objetivo:** Gestión proactiva de riesgo basada en datos reales.



### **3.5 Actualizar Políticas de Device Management**

**Roles:** Administrator, Architect

**Consola:** Microsoft Defender Portal / Microsoft Intune

**Navegación:**

1. **Endpoints** > **Configuration management** >     **Endpoint security policies**

**Pasos de Ejecución:**

1. Auditar políticas con status **Conflict**

○    Indica políticas superpuestas contradictorias

1. Resolver     conflictos:

○    Revisar scope de asignación

○    Consolidar políticas redundantes

1. **Feature     rollout:**

○    **Settings** > **Endpoints** > **Advanced features**

○    Si feature en pilot funcionó (ej. Tamper Protection)

○    Expandir scope a "All Devices"

1. Documentar     cambios en change log
2. Comunicar a stakeholders

**Objetivo:** Configuración coherente y sin conflictos en toda la organización.



### **3.6 Revisar Integración con Otras Herramientas de Seguridad**

**Roles:** Architect

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints** > **Advanced features**

**Pasos de Ejecución:**

1. Verificar integraciones activas:

○    **Microsoft Defender for Cloud Apps**: ON

■    Enriquece Shadow IT discovery

○    **Microsoft Sentinel**: Verificar data connector

○    **Intune Connection**: ON

■    Habilita risk-based Conditional Access

1. Para     cada integración:

○    Verificar flujo de datos

○    Revisar alertas generadas

○    Validar acciones automatizadas

1. Identificar     gaps de integración
2. Planificar nuevas integraciones según roadmap

**Objetivo:** Ecosistema de seguridad integrado y correlacionado.



### **3.7 Validar Mecanismos de Reporte a CISO**

**Roles:** Manager

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints** > **Email notifications**

**Pasos de Ejecución:**

1. Verificar scheduled emails se están entregando:

○    Vulnerabilities reports

○    Alert summaries

○    Security recommendations

1. **Data     accuracy check:**

○    Comparar Monthly Security Summary PDF

○    Contra raw data en Advanced Hunting

○    Identificar discrepancias

1. Ajustar     filtros de reportes si es necesario
2. Validar     destinatarios correctos
3. Solicitar feedback de CISO sobre utilidad

**Objetivo:** Reporting preciso y accionable para liderazgo.



### **3.8 Conducir Ejercicios Tabletop de IR**

**Roles:** Manager, Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Evaluation tutorials** > **Evaluation lab**

**Pasos de Ejecución:**

1. **Setup:**

○    Provisionar 3+ dispositivos de prueba

○    Seleccionar escenario: "Fileless Attack" o "Automated Ransomware"

1. **Ejecución     del ejercicio:**

○    SOC analistas tratan como evento real

○    No informar que es simulación

1. **Métricas     a medir:**

○    Time to Detect (TTD)

○    Time to Remediate (TTR)

○    ¿Usaron Timeline correctamente?

○    ¿Aplicaron Live Response apropiadamente?

1. **Debrief:**

○    Identificar gaps en proceso

○    Actualizar playbooks

○    Programar training adicional

**Objetivo:** Validar preparación del equipo para incidentes reales.



### **3.9 Actualizar Materiales de Capacitación SOC**

**Roles:** Manager, Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Learning Hub** (en navigation pane)

**Pasos de Ejecución:**

1. Identificar nuevos módulos de training:

○    Búsqueda por features recientes (ej. "Attack Disruption")

1. Asignar     módulos a staff SOC
2. **Actualizar     wiki interno:**

○    Agregar queries KQL efectivos descubiertos

○    Documentar nuevos playbooks

○    Actualizar procedimientos basados en lecciones aprendidas

1. Programar     sesiones de knowledge sharing
2. Evaluar competencias del equipo

**Objetivo:** Equipo actualizado en capabilities de plataforma.



### **3.10 Revisar y Renovar Licencias/Subscripciones**

**Roles:** Administrator

**Consola:** Microsoft 365 Admin Center

**Navegación:**

1. **Billing** > **Your products**

**Pasos de Ejecución:**

1. Verificar licencias activas:

○    Windows E5/A5

○    Microsoft Defender for Endpoint P2

1. Revisar     fechas de expiración
2. Comparar     cantidad de licencias vs dispositivos activos en **Device Inventory**
3. Identificar:

○    Over-provisioning (licencias sin usar)

○    Under-provisioning (dispositivos sin licencia)

1. Planificar     renovaciones con procurement
2. Evaluar necesidad de licenses adicionales

**Objetivo:** Optimización de costos y coverage completo.



### **3.11 Prueba de Detección y Respuesta (Simulación)**

**Roles:** Analyst L2, Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Evaluation tutorials** > **Simulations**

**Pasos de Ejecución:**

1. Seleccionar máquina de prueba
2. Ejecutar     script de simulación benigno
3. **Validar     cadena completa:**

○    MDE genera alerta

○    Alerta aparece en portal

○    Si Sentinel integrado: alerta llega a SIEM

○    Email notification enviada

○    AIR se activa (si configurado)

1. Medir     tiempos de cada paso
2. Documentar     fallas en la cadena
3. Remediar problemas identificados

**Objetivo:** Asegurar que pipeline de detección funciona end-to-end.



### **3.12 Limpieza de Dispositivos Inactivos (MDVM)**

**Roles:** Operator L1, Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Assets**     > **Devices**

**Pasos de Ejecución:**

1. Filtrar: **Sensor health state: Inactive**     > 60 días
2. Cross-reference     con:

○    AD Disabled Computers OU

○    IT asset management system

1. Para     dispositivos decomisionados confirmados:

○    Marcar como "excluded"

○    O ejecutar offboarding script

1. Limpiar     de inventario
2. Actualizar CMDB

**Objetivo:** Higiene de base de datos para métricas precisas.



### **3.13 Evaluación de End of Support (MDVM)**

**Roles:** Security Engineer, Architect

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Endpoints** > **Vulnerability management** > **Dashboard**

**Pasos de Ejecución:**

1. Filtrar software y OS:

○    End of Support < 12 meses

1. Generar     reporte de:

○    Windows Server 2012 R2

○    Windows 7 (si aún existe)

○    Software obsoleto (ej. Java 8)

1. Calcular     riesgo de cada activo
2. Presentar     a IT con:

○    Timeline de migración

○    Costo estimado

○    Riesgo de no actuar

1. Crear proyecto de remediación

**Objetivo:** Gestión proactiva de ciclo de vida de activos.



## **4. OPERACIONES AD-HOC**

### **4.1 Responder a Incidentes Mayores (Live Response)**

**Roles:** Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Assets** > **Devices**
2. Seleccionar     dispositivo comprometido
3. **... (More actions)** > **Initiate Live Response Session**

**Pasos de Ejecución:**

1. **Conexión remota:** Shell conecta al     dispositivo
2. **Comandos     forenses:**

○    connections: Ver sesiones de red activas

○    fileinfo <path>: Analizar payload sospechoso

○    processes: Listar procesos en ejecución

1. **Remediación:**

○    stopservice <malicious_service>

○    remediate user <compromised_account>

○    isolate: Aislar dispositivo de red

1. **Recolección     de evidencia:**

○    getfile <path>: Descargar artefactos

○    collectlogs: Recopilar event logs

1. **Scripting     avanzado:**

○    Upload PowerShell script a **Library**

○    run <script_name> para cleanup complejo

**Objetivo:** Contención y remediación inmediata de amenaza activa.



### **4.2 Coordinar con Microsoft Threat Experts**

**Roles:** Incident Responder

**Consola:** Microsoft Defender Portal

**Navegación:**

1. Página del incidente complejo

**Pasos de Ejecución:**

1. Click **Consult a threat expert**
2. **Inquiry     específica:**

○    "Tráfico C2 a IP X, sospecha Cobalt Strike"

○    "Atribución de actor?"

○    "Recomendaciones de contención?"

1. Proveer     contexto:

○    Timeline de compromiso

○    Técnicas observadas (MITRE ATT&CK)

○    Datos de triage inicial

1. **Endpoint     Attack Notifications:**

○    Monitorear inbox para alertas de servicio gestionado

○    Tratarlas como "confirmed breach"

○    Respuesta inmediata requerida

**Objetivo:** Leverage de expertise de Microsoft para amenazas sofisticadas.



### **4.3 Implementar Nuevas Features o Integraciones**

**Roles:** Architect, Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints** > **Advanced features**

**Pasos de Ejecución:**

1. **Preview features:**

○    Toggle **Preview features** ON

○    Revisar lista de capabilities beta

○    Ejemplo: "Device isolation for Linux"

1. **Testing     en lab:**

○    Validar nueva feature en Evaluation lab

○    Documentar comportamiento

○    Identificar conflictos con configs existentes

1. **API     Integration (SOAR):**

○    **Settings** > **Endpoints** > **APIs** > **SIEM**

○    Enable integration

○    En Azure AD: Generar Client ID/Secret para SOAR tool

1. **Rollout     gradual:**

○    Pilot group → Department → Organization

1. Documentar en change management

**Objetivo:** Adopción controlada de nuevas capabilities.



### **4.4 Ajustar Configuraciones por Amenazas Emergentes**

**Roles:** Architect, Administrator

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Reports** > **Attack surface     reduction rules**
2. **Settings** > **Endpoints**     > **Indicators**

**Pasos de Ejecución:**

1. **ASR Rule Hardening:**

○    Respuesta a campaña de macro malware

○    Identificar regla: "Block Office apps from creating child processes"

○    Cambiar modo: **Audit** → **Block**

○    Scope: "All Users" policy

1. **Indicator of Compromise (IoC) Blocking:**

○    **Settings** > **Endpoints** > **Indicators**

○    Agregar nuevo IoC:

■    File hash (SHA-256)

■    IP address de C2

■    Certificate de actor

○    Action: **Alert and Block**

1. Comunicar     cambios a SecOps team
2. Monitorear impacto en próximas 48hrs

**Objetivo:** Respuesta rápida a intelligence de amenazas fresh.



### **4.5 Tomar Decisiones Estratégicas ante Eventos de Seguridad**

**Roles:** CISO, Board of Directors

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Threat intelligence** > **Threat     analytics**
2. **Secure score**

**Pasos de Ejecución:**

1. **Data-driven decision making:**

○    Usar analyst reports para justificar presupuesto

○    Ejemplo: "Volt Typhoon muestra vulnerabilidad en legacy servers"

○    "Necesitamos budget para migración acelerada"

1. **Risk     acceptance review:**

○    Revisar todos items **Risk accepted** en Secure Score

○    A la luz de nuevo breach trend:

■    ¿Sigue siendo aceptable el riesgo?

■    ¿Debe cambiar proceso de negocio?

1. **Crisis     communication:**

○    Preparar messaging para stakeholders

○    Coordinar con Legal/PR si es necesario

**Objetivo:** Liderazgo informado durante crisis de seguridad.



## **5. OPERACIONES SEMESTRALES**

### **5.1 Revisión RBAC y Acceso Operacional**

**Roles:** Administrator, Manager

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Settings** > **Endpoints**     > **Roles**
2. **Audit** (Microsoft Purview)

**Pasos de Ejecución:**

1. Para cada rol definido:

○    Revisar miembros actuales

○    Verificar si siguen en posición adecuada

○    Remover usuarios que cambiaron de función

1. **Attestation     de permisos:**

○    Validar principio de least privilege

○    Asegurar segregación de funciones

1. **Audit     log review:**

○    Buscar actividades sospechosas:

■    Cambios de configuración fuera de horario

■    Aislamiento de dispositivos ejecutivos

■    Modificaciones a exclusions

1. Documentar findings y remediar

**Objetivo:** Mantenimiento de controles de acceso y detección de insider threats.



### **5.2 Refresh de Detection Engineering**

**Roles:** Security Engineer, Analyst L3

**Consola:** Microsoft Defender Portal

**Navegación:**

1. **Hunting** > **Custom detection rules**

**Pasos de Ejecución:**

1. Listar todas las custom detections
2. Para     cada regla:

○    Revisar tasa de alertas generadas

○    Calcular precision (TP rate)

○    Identificar reglas obsoletas (sin alertas en 90 días)

1. **Re-baseline     queries:**

○    Optimizar performance de KQL

○    Actualizar lógica según nuevo threat landscape

1. **Retire     stale rules:**

○    Desactivar/eliminar reglas que ya no agregan valor

1. Crear     nuevas reglas basadas en:

○    Threat intelligence reciente

○    Gaps identificados en tabletop

1. Documentar cambios en runbook

**Objetivo:** Mantener detection stack relevante y eficiente.



### **5.3 Revisión Estratégica de ASR**

**Roles:** Architect, Administrator

**Consola:** Microsoft Defender Portal / Microsoft Intune

**Navegación:**

1. **Reports** > **Attack surface     reduction rules**
2. **Intune** > **Endpoint Security** > **Attack Surface Reduction**

**Pasos de Ejecución:**

1. **Reassess rule set:**

○    Revisar 13 reglas ASR disponibles

○    Determinar cuáles deberían estar en Block

1. **Exclusions     audit:**

○    Listar todas las exclusiones configuradas

○    Validar justificación de negocio

○    Eliminar exclusiones ya no necesarias

1. **Deployment     method review:**

○    GPO vs Intune vs Microsoft Defender portal

○    Asegurar método consistente

○    Documentar precedencia (conflict resolution)

1. **Pilot     para nuevas reglas:**

○    Mover de Audit → Block progresivamente

○    Monitorear impacto en negocio

1. Actualizar documentación de arquitectura

**Objetivo:** Maximizar hardening sin impacto operacional negativo.



## **6. OPERACIONES ANUALES**

### **6.1 Revisión Integral del Programa**

**Roles:** Manager, CISO

**Consola:** Microsoft Defender Portal + Business Intelligence Tools

**Pasos de Ejecución:**

1. **Análisis de métricas anuales:**

○    Total incidents handled

○    MTTR trend (improving?)

○    Top attack techniques (MITRE ATT&CK)

○    Top threat actors/campaigns

1. **Operational     KPIs:**

○    False positive rate

○    Automation effectiveness

○    Coverage % (devices protected)

○    Time to patch (vulnerability management)

1. **Playbook     review:**

○    Actualizar based on lessons learned

○    Incorporar nuevas TTPs observadas

1. **Escalation     path refresh:**

○    Validar matriz de contactos

○    Actualizar según cambios organizacionales

1. **Strategic     recommendations:**

○    Gaps identificados

○    Propuestas de mejora

○    Budget justification para próximo año

**Objetivo:** Mejora continua del programa de SecOps basada en datos históricos.



### **6.2 Revisión de Arquitectura de Plataforma**

**Roles:** Architect

**Consola:** Microsoft Defender Portal + Azure Portal

**Pasos de Ejecución:**

1. **Integrations audit:**

○    SIEM (Sentinel): ¿Está optimizado?

○    SOAR tools: ¿Playbooks efectivos?

○    Ticketing (ServiceNow): ¿Sincronización funciona?

○    EDR → XDR correlation: ¿Coverage completo?

1. **Data     retention review:**

○    Advanced Hunting: 30 días suficiente?

○    Necesidad de long-term retention en Sentinel

1. **Operating     model alignment:**

○    ¿Workflows Defender XDR están optimizados?

○    ¿SOC puede operar eficientemente?

1. **Capacity     planning:**

○    Crecimiento proyectado de endpoints

○    API quotas y límites

○    Necesidad de scaling

1. **Roadmap     technology:**

○    Nuevas capabilities de Microsoft

○    Plan de adopción

○    Dependencies identificadas

**Objetivo:** Asegurar arquitectura escalable y alineada con roadmap.



### **6.3 Ejercicio Anual de Incident Response**

**Roles:** Manager, CISO, Incident Responder

**Consola:** Microsoft Defender Portal + Coordination Tools

**Pasos de Ejecución:**

1. **Diseño de escenario realista:**

○    Multi-stage attack

○    Involucra múltiples vectores (email, endpoint, cloud)

○    Requiere coordinación cross-functional

1. **Participantes:**

○    SOC completo

○    IT operations

○    Legal

○    PR/Communications

○    Executive leadership

1. **Ejecución:**

○    Simulación en Evaluation Lab + manual inject

○    Seguir IR playbook real

○    Activar cadena de escalamiento

1. **Evaluación:**

○    Detection effectiveness

○    Response timeliness

○    Communication clarity

○    Decision-making quality

○    Tool utilization

1. **Post-mortem:**

○    After-action report

○    Actualizar procedimientos

○    Identificar training needs

○    Implementar mejoras

**Objetivo:** Validación annual de preparación organizacional para breach real.



## **MATRIZ DE ROLES Y RESPONSABILIDADES**

| **Rol**                     | **Permisos Clave**                                           | **Scope Operacional**                             |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------- |
| **Operator (L1)**           | View Data, Alerts,  Basic Remediation                        | Triage, Queue  hygiene, Initial classification    |
| **Incident Responder (L2)** | Active Remediation,  Live Response (Basic), Manage Incidents | Deep investigation,  Device isolation, Quarantine |
| **Analyst (L3)**            | Advanced Hunting,  Live Response (Advanced), Custom Detections | Threat hunting,  Forensics, Script execution      |
| **Administrator**           | Manage Security  Settings, Onboarding, Policy Management     | Configuration, Device management,  Integration    |
| **Architect**               | Full Configuration,  API Access, Architecture Design         | Strategic design, Integrations,  Optimization     |
| **Manager**                 | View Reports,  Approve Exceptions, Team Management           | Oversight, Reporting, Resource  allocation        |
| **CISO**                    | Strategic View, Risk  Acceptance, Budget Authority           | Governance, Risk  management, Executive reporting |



## **REFERENCIAS Y DOCUMENTACIÓN**

●    **MDE Security Operations Guide**: https://learn.microsoft.com/en-us/defender-endpoint/mde-sec-ops-guide

●    **Defender XDR Portal Documentation**: https://learn.microsoft.com/en-us/defender-xdr/microsoft-365-security-center-mde

●    **Action Center Operations**: https://learn.microsoft.com/en-us/defender-xdr/m365d-autoir-actions

●    **Incident Investigation**: https://learn.microsoft.com/en-us/defender-xdr/investigate-incidents

●    **Device Health Reports**: https://learn.microsoft.com/en-us/defender-endpoint/device-health-reports

●    **Threat Analytics**: https://learn.microsoft.com/en-us/defender-xdr/threat-analytics

●    **Custom Detection Rules**: https://learn.microsoft.com/en-us/defender-xdr/custom-detection-rules

●    **ASR Rules**: https://learn.microsoft.com/en-us/defender-endpoint/attack-surface-reduction-rules-reference

●    **RBAC Documentation**: https://learn.microsoft.com/en-us/defender-endpoint/rbac

●    **Live Response**: https://learn.microsoft.com/en-us/defender-endpoint/live-response

●    **Vulnerability Management**: https://learn.microsoft.com/en-us/defender-vulnerability-management/tvm-dashboard-insights



**Nota Final:** Esta guía debe ser complementada con procedimientos específicos de la organización, matriz de contactos interna, y playbooks detallados por tipo de incidente. Se recomienda mantener este documento vivo, actualizándolo trimestralmente basado en cambios de plataforma y lecciones aprendidas operacionales.

 