# **Guía Operativa Consolidada: Microsoft Defender for Identity (MDI)**

## **Manual de Operaciones de Seguridad e Inteligencia de Amenazas**



## **1. Introducción y Marco Estratégico**

### **1.1 El Cambio de Paradigma: La Identidad como Perímetro**

En el panorama actual de la ciberseguridad, la infraestructura tradicional basada en perímetros de red ha quedado obsoleta frente a la movilidad de la fuerza laboral y la adopción de servicios en la nube. La identidad se ha establecido como el nuevo plano de control y, consecuentemente, en el vector de ataque primario para adversarios sofisticados. Microsoft Defender for Identity (MDI) no es simplemente una herramienta de monitoreo; es una plataforma integral de Detección y Respuesta a Amenazas de Identidad (ITDR) diseñada para proteger entornos híbridos complejos.

### **1.2 Arquitectura y Flujo de Telemetría**

MDI opera mediante sensores instalados directamente en los Controladores de Dominio (DC), servidores de Active Directory Federation Services (AD FS) y Active Directory Certificate Services (AD CS). Estos sensores capturan el tráfico de red (vía port mirroring o NDIS) y extraen eventos de Windows (ETW) específicos, enviando esta telemetría a la nube de Azure para su análisis mediante algoritmos de aprendizaje automático.

**Portal Principal de Operaciones:** https://security.microsoft.com

### **1.3 Referencias de Documentación Oficial**

●    **Guía Operativa de MDI:** https://learn.microsoft.com/en-us/defender-for-identity/ops-guide/ops-guide

●    **Guía Diaria:** https://learn.microsoft.com/en-us/defender-for-identity/ops-guide/ops-guide-daily

●    **Guía Semanal:** https://learn.microsoft.com/en-us/defender-for-identity/ops-guide/ops-guide-weekly

●    **Guía Mensual:** https://learn.microsoft.com/en-us/defender-for-identity/ops-guide/ops-guide-monthly

●    **Guía Trimestral/Ad-hoc:** https://learn.microsoft.com/en-us/defender-for-identity/ops-guide/ops-guide-quarterly

●    **Health Issues:** https://learn.microsoft.com/en-us/defender-for-identity/health-alerts

●    **Gestión de Sensores:** https://learn.microsoft.com/en-us/defender-for-identity/sensor-settings

●    **Security Assessments:** https://learn.microsoft.com/en-us/defender-for-identity/security-assessment

●    **Exclusiones de Detección:** https://learn.microsoft.com/en-us/defender-for-identity/exclusions



## **2. Operaciones Diarias**

**Roles:** Operator, Monitor, Incident Responder, Administrator

### **2.1 Gestión y Triaje de Incidentes**

#### **2.1.1 Acceso y Filtrado de la Cola de Incidentes**

**Consola:** Microsoft Defender XDR
 **URL:** https://security.microsoft.com
 **Ruta:** Investigation & response > Incidents & alerts > Incidents

**Procedimiento:**

1. **Navegación:
        
   **     

○    Acceda a https://security.microsoft.com

○    En el menú lateral izquierdo, localice **Investigation & response**

○    Seleccione **Incidents & alerts**

○    Haga clic en **Incidents**

1. **Configuración     de Filtros:
        
   **     

○    Localice el icono **Filter** (embudo) en la cabecera de la tabla

○    Configure los siguientes parámetros:

■    **Service sources:** Seleccione "Microsoft Defender for Identity" y "Microsoft Entra ID Protection"

■    **Status:** Seleccione "New" y "In Progress"

■    **Severity:** Seleccione "High" y "Medium"

○    Guarde esta vista como preset para uso recurrente

1. **Priorización:
        
   **     

○    Ordene por prioridad (Priority score)

○    Revise primero los incidentes de severidad Alta relacionados con identidad

#### **2.1.2 Análisis de la Historia del Ataque (Attack Story)**

**Procedimiento de Investigación:**

1. **Selección del Incidente:
        
   **     

○    Haga clic en la fila del incidente para abrir el panel de resumen lateral

○    Revise las métricas vitales: puntuación de prioridad, categorías de amenazas, entidades afectadas

1. **Navegación     a la Página del Incidente:
        
   **     

○    En el panel de resumen, haga clic en **Open incident page**

○    La pestaña predeterminada es **Attack story**

1. **Interpretación     del Gráfico:
        
   **     

○    Analice el gráfico de nodos de izquierda a derecha

○    Identifique el punto de entrada inicial

○    Busque movimientos laterales (líneas entre dispositivos y Domain Controllers)

○    Identifique alertas críticas como:

■    "Suspected Pass-the-Hash"

■    "Overpass-the-Hash"

■    "Suspected DCSync attack"

■    "Remote Code Execution"

1. **Revisión     de Alertas Individuales:
        
   **     

○    Navegue a la pestaña **Alerts**

○    Examine el desglose técnico de cada alerta

○    Revise comandos ejecutados, usuarios impersonados, protocolos abusados

#### **2.1.3 Clasificación y Resolución**

**Consola:** Página del incidente
 **Ruta:** Manage incident (esquina superior derecha)

**Procedimiento:**

1. **Gestión del Incidente:
        
   **     

○    Haga clic en **Manage incident**

1. **Clasificación:
        
   **     

○    **True Positive (TP):** Actividad maliciosa confirmada

○    **Benign True Positive (B-TP):** Actividad legítima autorizada que coincide con patrón de ataque

○    **False Positive (FP):** Error de detección

1. **Determination     (si TP):
        
   **     

○    Especifique el tipo de amenaza: Ransomware, APT, Insider Threat, etc.

1. **Resolución:
        
   **     

○    Cambie el estado a **Resolved** después de la remediación

○    Todas las alertas vinculadas se resolverán automáticamente

### **2.2 Configuración de Reglas de Ajuste (Tuning Rules)**

**Consola:** Microsoft Defender XDR
 **Ruta:** Desde la alerta específica > Tune alert

**Procedimiento:**

1. Identifique alertas recurrentes de tipo Benign True Positive
2. Desde     la alerta, seleccione **Tune alert**
3. Defina     condiciones lógicas específicas
4. Ejemplo: "SI alerta =     'Reconnaissance' Y IP = '192.168.1.50' → Auto-resolver"
5. Revise reglas existentes mensualmente para mantener relevancia

### **2.3 Revisión del Dashboard ITDR**

**Consola:** Microsoft Defender XDR
 **URL:** https://security.microsoft.com
 **Ruta:** Identities > Dashboard

**Widgets Críticos a Revisar:**

#### **2.3.1 Top Insights**

●    Revise las principales insights de riesgo de identidad

●    Priorice acciones basadas en severidad

#### **2.3.2 Identity Related Incidents**

●    Analice tendencia de incidentes en últimos 30 días

●    Un pico repentino requiere investigación inmediata

#### **2.3.3 Entra ID Users at Risk**

**Procedimiento:**

1. Identifique usuarios con riesgo High, Medium, Low
2. Haga     clic en el número de usuarios con riesgo **High**
3. Analice     el "Risk Detail" de cada usuario:

○    Leaked Credentials

○    Atypical Travel

○    Anonymous IP address

○    Malware linked IP address

1. **Acción:** Requiera cambio de     contraseña o bloquee usuarios de alto riesgo

#### **2.3.4 Lateral Movement Paths (LMP)**

**Procedimiento:**

1. Revise el número de cuentas sensibles expuestas
2. Identifique     rutas de movimiento lateral hacia cuentas VIP
3. **Objetivo:** Reducir este número a cero mediante:

○    Higiene de sesiones administrativas

○    Uso de PAW (Privileged Access Workstations)

○    Segregación de cuentas administrativas

### **2.4 Cacería Proactiva de Amenazas (Threat Hunting)**

**Consola:** Microsoft Defender XDR
 **Ruta:** Hunting > Advanced hunting

#### **2.4.1 Detección de Reconocimiento LDAP**

**Consulta KQL:**

IdentityQueryEvents

| where ActionType == "SamR query" or ActionType == "Ldap query"

| where Timestamp > ago(1d)

| summarize QueryCount = count() by DeviceName, AccountUpn, bin(Timestamp, 1h)

| where QueryCount > 500

| order by QueryCount desc

 

**Análisis:**

●    Identifique dispositivos con volumen anómalo de consultas LDAP

●    Investigue dispositivos no autorizados con >500 consultas/hora

#### **2.4.2 Monitoreo de Movimiento Lateral (Pass-the-Ticket)**

**Consulta KQL:**

IdentityLogonEvents

| where ActionType contains "LogonSuccess"

| where LogonType == "Network"

| where Timestamp > ago(1d)

| summarize DistinctDevices = dcount(DeviceName) by AccountUpn, bin(Timestamp, 1h)

| where DistinctDevices > 5

| order by DistinctDevices desc

 

**Análisis:**

●    Una cuenta accediendo a múltiples dispositivos en corto tiempo puede indicar Pass-the-Ticket

#### **2.4.3 Detección de Ejecución Remota de Código**

**Consulta KQL:**

IdentityDirectoryEvents

| where ActionType in ("WMI execution", "Remote PowerShell", "Service creation")

| where Timestamp > ago(1d)

| where TargetDeviceName contains "DC" or TargetDeviceName contains "Server"

| project Timestamp, ActionType, AccountUpn, TargetDeviceName, AdditionalFields

 

**Análisis:**

●    Revise ejecuciones remotas contra Domain Controllers

●    Investigue cuentas no administrativas ejecutando código remotamente

### **2.5 Verificación de Salud de la Infraestructura**

**Consola:** Microsoft Defender XDR
 **Rutas:**

●    **Opción 1:** Identities > Health issues

●    **Opción 2:** Settings > Identities > Sensors

#### **2.5.1 Global Health Issues**

**Procedimiento:**

1. Acceda a **Identities > Health issues**
2. Revise     alertas globales críticas:

○    "Directory Services Advanced Auditing not enabled"

○    "Capture network traffic not configured"

○    "NTLM auditing not enabled"

**Acciones:**

●    **Directory Services Auditing:** Contacte al equipo de AD para corregir GPO

●    Valide que los eventos requeridos estén habilitados:

○    Event 4776 (NTLM)

○    Event 4662 (Object Access)

○    Event 4624/4625 (Logon events)

#### **2.5.2 Sensor Health Issues**

**Consola:** Settings > Identities > Sensors

**Estados Críticos:**

●    **Stopped:** Servicio del sensor no está corriendo

●    **Not Configured:** Sin credenciales para conectar a AD

●    **Low Memory/CPU:** Límites de recursos alcanzados

●    **Outdated:** Sensor en versión antigua

**Procedimiento de Remediación:**

1. Identifique sensores con estado problemático
2. Para     sensores "Stopped":

○    Conéctese al DC afectado

○    Reinicie el servicio "Azure ATP Sensor"

1. Para     sensores "Outdated":

○    Normalmente se actualizan automáticamente

○    Si falla, descargue instalador desde portal y ejecute manualmente

### **2.6 Investigación de Alertas Críticas de Identidad**

#### **2.6.1 DCSync Attacks (Prioridad Máxima)**

**Consola:** Incidents & alerts > Alerts
 **Filtro:** AlertType contains "DCSync"

**Procedimiento:**

1. Identifique la cuenta que ejecutó DCSync
2. Verifique     si es cuenta administrativa legítima o comprometida
3. Revise     los servicios de destino (DC específico)
4. **Remediación     inmediata:**

○    Resetee contraseña de la cuenta

○    Revoque tickets Kerberos

○    Aísle dispositivo de origen si es un equipo comprometido

#### **2.6.2 Pass-the-Hash / Overpass-the-Hash**

**Procedimiento:**

1. Identifique la cuenta afectada
2. Revise     la cronología de uso de credenciales
3. Determine     el dispositivo de origen del ataque
4. **Remediación:**

○    Resetee contraseña de cuenta comprometida

○    Aísle dispositivo infectado

○    Busque persistencia (scheduled tasks, services)

#### **2.6.3 Golden Ticket Attacks**

**Procedimiento:**

1. Identifique tickets Kerberos sospechosos (duración excesiva,     cifrado débil)
2. Verifique     la cuenta KRBTGT
3. **Remediación     crítica:**

○    Resetee contraseña de KRBTGT (proceso de doble reset)

○    Invalide todos los tickets Kerberos del dominio

○    Investigue cómo el atacante obtuvo acceso al hash de KRBTGT

### **2.7 Monitoreo de Honeytoken Activity**

**Consola:** Advanced Hunting
 **Procedimiento:**

1. Configure cuentas trampa (Honeytokens) en su AD
2. Ejecute consulta para detectar cualquier actividad:

IdentityLogonEvents

| where AccountUpn in ("honeytoken1@domain.com", "honeytoken2@domain.com")

| where Timestamp > ago(1d)

 

**Análisis:**

●    **Cualquier** actividad en cuentas Honeytoken = Compromiso confirmado

●    Investigue inmediatamente el dispositivo y usuario de origen

### **2.8 Detección de Exfiltración vía DNS Tunneling**

**Consulta KQL:**

DeviceNetworkEvents

| where RemotePort == 53

| where Timestamp > ago(1d)

| summarize DNSQueries = count(), DistinctDomains = dcount(RemoteUrl) by DeviceName, InitiatingProcessAccountName

| where DNSQueries > 1000 or DistinctDomains > 500

| order by DNSQueries desc

 

**Análisis:**

●    Volumen excesivo de consultas DNS puede indicar tunelización

●    Revise dominios destino para identificar C2 malicioso



## **3. Operaciones Semanales**

**Roles:** Administrator, Operator, Monitor, Incident Responder, Manager

### **3.1 Revisión y Mejora del Identity Secure Score**

**Consola:** Microsoft Defender XDR
 **Rutas:**

●    **Opción 1:** Exposure Management > Secure score > Recommended actions

●    **Opción 2:** Identities > Dashboard (widget Secure Score)

**Procedimiento de Análisis:**

1. **Filtrado de Recomendaciones:
        
   **     

○    Haga clic en **Filter**

○    En categoría **Product**, seleccione:

■    "Microsoft Defender for Identity"

■    "Microsoft Entra ID"

1. **Priorización     de Acciones:
        
   **     

○    Ordene por **Impact on score** (descendente)

○    Enfóquese en recomendaciones High impact

#### **3.1.1 Recomendaciones Críticas de Identidad**

**A. Stop Clear Text Credentials Exposure**

**Procedimiento:**

1. Descargue el reporte CSV desde la recomendación
2. Identifique     aplicaciones que usan LDAP Simple Bind
3. Contacte     a los dueños de aplicaciones
4. **Remediación:** Migrar a LDAPS (LDAP     over SSL/TLS)

**Consulta KQL para validar:**

IdentityLogonEvents

| where Protocol == "Ldap"

| where LogonType == "SimpleBind"

| where Timestamp > ago(7d)

| summarize count() by Application, AccountUpn

 

**B. Modify Unsecure Kerberos Delegations**

**Procedimiento:**

1. Identifique servidores con Unconstrained Delegation
2. Valide     si requieren esta configuración
3. **Remediación:** Migrar a Constrained Delegation o     Resource-Based Constrained Delegation

**PowerShell para detectar:**

Get-ADComputer -Filter {TrustedForDelegation -eq $true} -Properties TrustedForDelegation

 

**C. Remove Dormant Accounts in Sensitive Groups**

**Procedimiento:**

1. Identifique cuentas inactivas en grupos privilegiados
2. Defina     umbral (ej. >90 días sin actividad)
3. **Remediación:** Remover del grupo o     deshabilitar cuenta

**D. Disable Print Spooler on Domain Controllers**

**Procedimiento:**

1. Valide que Print Spooler esté deshabilitado en todos los DCs
2. **PowerShell remoto:**

Invoke-Command -ComputerName DC01 -ScriptBlock {

  Stop-Service -Name Spooler -Force

  Set-Service -Name Spooler -StartupType Disabled

}

 

### **3.2 Revisión y Respuesta a Amenazas Emergentes**

**Consola:** Microsoft Defender XDR
 **Ruta:** Hunting > Advanced hunting

**Procedimiento:**

1. **Configuración de Custom Detections:
        
   **     

○    Cree reglas de detección personalizadas basadas en amenazas actuales

○    Acceda a **Hunting > Custom detection rules**

○    Cree nueva regla con consulta KQL específica

1. **Ejemplo - Detección de PrintNightmare:
        
   **     

DeviceEvents

| where ActionType == "ServiceInstalled"

| where FileName endswith ".dll"

| where FolderPath contains @"\spool\drivers\"

| where Timestamp > ago(7d)

 

### **3.3 Análisis de Lateral Movement Paths (LMP)**

**Consola:** Microsoft Defender XDR
 **Ruta:** Identities > Dashboard > Lateral movement paths

**Procedimiento:**

1. **Revisión Semanal:
        
   **     

○    Identifique cuentas sensibles más expuestas

○    Analice rutas de acceso desde cuentas no privilegiadas

1. **Descarga     de Reporte:
        
   **     

○    **Ruta:** Reports > Identities > Lateral movement paths to sensitive accounts

○    Descargue CSV para análisis detallado

1. **Acciones     de Remediación:
        
   **     

○    Implemente restricciones de logon (GPO)

○    Utilice Authentication Policies (Windows Server 2012 R2+)

○    Migre administradores a usar PAW exclusivamente

### **3.4 Auditoría de Grupos Sensibles**

**Consola:** Advanced Hunting

**Consulta KQL:**

IdentityDirectoryEvents

| where ActionType == "Group Membership changed"

| where Timestamp > ago(7d)

| extend GroupName = tostring(parse_json(AdditionalFields).GroupName)

| where GroupName in ("Domain Admins", "Enterprise Admins", "Schema Admins", "Administrators", "Account Operators", "Backup Operators")

| project Timestamp, ActionType, AccountUpn, TargetAccountUpn, GroupName, DeviceName

 

**Análisis:**

●    Valide cada cambio contra tickets de Change Management

●    Investigue adiciones no autorizadas inmediatamente

### **3.5 Revisión de Protocolos Inseguros**

**Procedimiento:**

1. **Informe de NTLMv1:**

IdentityLogonEvents

| where Protocol == "Ntlm"

| where Timestamp > ago(7d)

| extend NtlmVersion = tostring(parse_json(AdditionalFields).NtlmVersion)

| where NtlmVersion == "1"

| summarize count() by Application, DeviceName

 

1. **Informe de SMBv1:**

DeviceNetworkEvents

| where RemotePort == 445

| where Timestamp > ago(7d)

| extend SMBVersion = tostring(parse_json(AdditionalFields).SMBVersion)

| where SMBVersion == "1"

| summarize count() by DeviceName, RemoteIP

 

**Acción:** Planificar deshabilitación de protocolos legacy

### **3.6 Análisis de Cuentas de Servicio**

**Consulta KQL:**

IdentityLogonEvents

| where AccountUpn contains "svc_" or AccountUpn contains "service"

| where LogonType == "Interactive"

| where Timestamp > ago(7d)

| summarize LogonCount = count(), DistinctDevices = dcount(DeviceName) by AccountUpn

 

**Análisis:**

●    Cuentas de servicio NO deben tener logons interactivos

●    Investigue comportamiento anómalo inmediatamente

### **3.7 Ajuste de Umbrales de Alertas**

**Consola:** Desde alertas específicas
 **Procedimiento:**

1. Identifique alertas de "Reconnaissance" con ruido     recurrente
2. Ejemplo:     Excluir escáner de vulnerabilidades autorizado
3. **Ruta:** Alert page > Tune alert
4. Defina     exclusión específica (IP del escáner + tipo de alerta)
5. Documente la exclusión en registro de configuración

### **3.8 Validación de Integración VPN (Radius Accounting)**

**Si MDI está integrado con VPN:**

**Procedimiento:**

1. Verifique que datos de sesión VPN se correlacionan con     actividad de identidad
2. **Consola:** Advanced Hunting
3. Valide eventos de VPN en tabla IdentityLogonEvents

### **3.9 Detección de SamAccountName Spoofing**

**Consulta KQL:**

IdentityDirectoryEvents

| where ActionType contains "Account"

| where Timestamp > ago(7d)

| extend OldSamAccount = tostring(parse_json(AdditionalFields).OldValue)

| extend NewSamAccount = tostring(parse_json(AdditionalFields).NewValue)

| where OldSamAccount != NewSamAccount

| where NewSamAccount endswith "$"

 

**Análisis:**

●    Cambios sospechosos de sAMAccountName pueden indicar exploits como noPac

### **3.10 Planificación de Actualización de Sensores**

**Consola:** Settings > Identities > Sensors

**Procedimiento:**

1. Revise versiones de sensores instalados
2. Valide     que actualizaciones automáticas funcionan correctamente
3. Si hay sensores "Outdated", planifique actualización     manual



## **4. Operaciones Mensuales**

**Roles:** Architect, Administrator, Manager

### **4.1 Evaluación de Postura de Identidad (Identity Secure Score)**

**Consola:** Exposure Management > Secure score

**Procedimiento:**

1. **Análisis de Tendencias:
        
   **     

○    Revise progreso mensual del score

○    Identifique áreas de retroceso

○    Genere reporte para gerencia

1. **Priorización     de Remediación:
        
   **     

○    Enfóquese en "Quick Wins" (High impact, Low effort)

○    Planifique proyectos para items de High effort

### **4.2 Revisión de Alertas Ajustadas (Tuned Alerts)**

**Consola:** Settings > Identities > Alert tuning (o desde configuración de exclusiones)

**Procedimiento:**

1. Revise todas las reglas de tuning activas
2. Valide     que siguen siendo relevantes
3. Elimine     reglas temporales que ya no aplican
4. Ajuste umbrales si el entorno cambió

**Documentación:**

●    Mantenga registro de todas las exclusiones

●    Incluya justificación de negocio

●    Defina fecha de revisión para cada exclusión

### **4.3 Auditoría de Permisos Delegados en AD**

**PowerShell para análisis:**

\# Detectar delegaciones peligrosas

Get-ADObject -Filter * -SearchBase "OU=Domain Controllers,DC=contoso,DC=com" `

  -Properties nTSecurityDescriptor |

  Select-Object Name, {$_.nTSecurityDescriptor.Access}

 

**Procedimiento:**

1. Identifique delegaciones que permiten:
        
        

○    Reset de contraseñas de cuentas privilegiadas

○    Modificación de AdminSDHolder

○    Replicación de directorio

1. Valide     con equipo de AD si son necesarias
        
        
2. Remedie delegaciones excesivas
        
        

### **4.4 Tracking de Cambios en la Plataforma**

**Consola:** Microsoft 365 Admin Center
 **URL:** https://admin.microsoft.com
 **Ruta:** Health > Message center

**Procedimiento:**

1. Filtre por "Microsoft Defender for Identity"
        
        
2. Revise     anuncios sobre:
        
        

○    Deprecación de sistemas operativos

○    Nuevos requisitos de auditoría

○    Nuevas detecciones disponibles

○    Cambios en sensores

1. **También     revise:
        
   **     

○    Blog "What's New": https://learn.microsoft.com/en-us/defender-for-identity/whats-new

○    Planifique adopción de nuevas capacidades

### **4.5 Informe de Actividad de Administradores On-Prem**

**Consola:** Reports > Identities

**Reportes a Generar:**

| **Reporte**                                       | **Objetivo**                           | **Frecuencia** |
| ------------------------------------------------- | -------------------------------------- | -------------- |
| **Summary Report**                                | Métricas de salud y amenazas           | Mensual        |
| **Lateral movement  paths to sensitive accounts** | Tendencia de exposición de cuentas VIP | Mensual        |
| **Passwords exposed in cleartext**                | Apps con LDAP inseguro                 | Mensual        |
| **Sensitive groups modification**                 | Auditoría de cambios privilegiados     | Mensual        |

**Ruta:** Reports > Identities > Report management

**Procedimiento:**

1. Configure reportes programados (scheduled     reports)
2. Defina     destinatarios (CISO, Security Manager, Audit)
3. Revise     reportes antes de distribución
4. Agregue análisis ejecutivo con tendencias y recomendaciones

### **4.6 Análisis de Tendencias de Ataque**

**Consola:** Advanced Hunting

**Consulta para tendencias mensuales:**

IdentityLogonEvents

| where Timestamp > ago(30d)

| where ActionType == "LogonFailed"

| summarize FailedAttempts = count() by bin(Timestamp, 1d), AccountUpn

| render timechart

 

**Análisis:**

●    Identifique incrementos en:

○    Intentos de fuerza bruta

○    Reconocimiento interno

○    Movimientos laterales

●    Correle con eventos externos (campañas APT, vulnerabilidades publicadas)

### **4.7 Planificación de Reducción de NTLM**

**Procedimiento:**

1. **Análisis de Uso Actual:**

IdentityLogonEvents

| where Protocol == "Ntlm"

| where Timestamp > ago(30d)

| summarize count() by Application, DeviceName, AccountUpn

| order by count_ desc

 

1. **Identificación de Aplicaciones:
        
   **     

○    Clasifique apps por criticidad

○    Contacte a dueños de apps legacy

1. **Planificación     de Migración:
        
   **     

○    Apps críticas → Migrar a Kerberos

○    Apps deprecadas → Plan de retirement

○    Timeline: 6-12 meses

### **4.8 Revisión de Topología de Red**

**Si se añadieron nuevas subredes o sitios:**

**Consola:** Settings > Identities > Sensors

**Procedimiento:**

1. Verifique que sensores detectan nueva topología
2. Actualice     Network Mapping si es necesario
3. Valide detección de Lateral Movement en nuevos segmentos

### **4.9 Prueba de Detección (Purple Team Exercise)**

**Coordinación con Red Team:**

**Escenarios a Probar:**

1. **DCSync simulado:
        
   **     

○    Ejecutar Mimikatz DCSync desde máquina de prueba

○    Validar que MDI genera alerta

○    Medir MTTD (Mean Time To Detect)

1. **Pass-the-Hash:
        
   **     

○    Simular ataque PtH

○    Validar alerta y correlación

1. **Reconnaissance:
        
   **     

○    Ejecutar BloodHound en entorno controlado

○    Validar detección de consultas LDAP masivas

**Documentación:**

●    Registre resultados

●    Identifique gaps de detección

●    Ajuste configuración según hallazgos

### **4.10 Auditoría de Cuentas con Unconstrained Delegation**

**PowerShell:**

\# Buscar cuentas de usuario

Get-ADUser -Filter {TrustedForDelegation -eq $true} -Properties TrustedForDelegation

 

\# Buscar cuentas de equipo

Get-ADComputer -Filter {TrustedForDelegation -eq $true} -Properties TrustedForDelegation

 

**Procedimiento:**

1. Documente todas las cuentas con Unconstrained Delegation
2. Valide     necesidad de negocio con dueños
3. **Remediación:** Migrar a Constrained Delegation
4. **Excepción:** Solo DCs deben tener     Unconstrained Delegation

### **4.11 Validación de Envío de Logs a SIEM/Sentinel**

**Si integrado con Azure Sentinel:**

**Consola:** Azure Sentinel
 **Procedimiento:**

1. Valide conector de Microsoft Defender XDR
2. Ejecute consulta de validación:

SecurityAlert

| where ProviderName == "MDATP-DefenderForIdentity"

| where TimeGenerated > ago(30d)

| summarize count() by AlertSeverity

 

1. Confirme volumen esperado de alertas
2. Valide reglas de correlación híbridas (on-prem + cloud)



## **5. Operaciones Trimestrales / Ad-hoc**

**Roles:** Architect, Administrator, Manager, CISO

### **5.1 Revisión de Service Health**

**Consola:** Microsoft 365 Admin Center
 **URL:** https://admin.microsoft.com
 **Ruta:** Health > Service health

**Procedimiento:**

1. Filtre por "Microsoft Defender"
        
        
2. Revise:
        
        

○    Incidentes activos

○    Mantenimientos planificados

○    Avisos de servicio

1. Planifique contingencias para mantenimientos que afecten     sensores
        
        

### **5.2 Validación de Prerrequisitos de Dominio con PowerShell**

**Script:** Test-MdiReadiness.ps1
 **Ubicación:** Repositorio GitHub de Microsoft o portal MDI

#### **5.2.1 Preparación**

**Descarga:**

●    **Consola MDI:** Settings > Identities > Tools > Download Test-MdiReadiness.ps1

●    O desde: https://github.com/microsoft/Azure-Advanced-Threat-Protection

**Ubicación en DC:** C:\Temp\MDI\

#### **5.2.2 Ejecución en Modo Domain**

**PowerShell (como administrador):**

.\Test-MdiReadiness.ps1 -Mode Domain -OpenHtmlReport

 

**Este comando valida:**

●    Políticas de auditoría (GPO)

●    Configuración de "Audit Credential Validation"

●    Advanced Audit Policy settings

#### **5.2.3 Ejecución en Modo LocalMachine**

**Para servidores sin GPO centralizada:**

.\Test-MdiReadiness.ps1 -Mode LocalMachine -OpenHtmlReport

 

#### **5.2.4 Interpretación de Resultados**

**Reporte HTML - Puntos Críticos:**

**A. NTLM Auditing:**

●    **Requerido:** Event 8004 habilitado

●    **Ubicación GPO:** Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options

●    **Setting:** Network security: Restrict NTLM: Audit NTLM authentication in this domain

**B. Advanced Audit Policy:**

Audit Credential Validation: Success and Failure

Audit Account Logon Events: Success and Failure 

Audit Directory Service Changes: Success and Failure

Audit Directory Service Access: Success

 

**Ubicación GPO:** Computer Configuration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration

**C. Object Auditing (Event 4662):**

●    Crítico para detectar DCSync y DCShadow

●    **Ubicación:** Aplica SACL en objeto de dominio

**D. Power Settings:**

●    Esquema de energía debe ser "High Performance"

●    Esquema "Balanced" puede causar pérdida de eventos durante picos de tráfico

**PowerShell para validar:**

powercfg /list

powercfg /getactivescheme

 

### **5.3 Gestión de Cuentas de Servicio (gMSA)**

**Síntomas de Fallo:**

●    "Directory Services user password incorrect"

●    "Sensor stopped communicating"

#### **5.3.1 Verificación de gMSA**

**PowerShell:**

\# Verificar que gMSA existe y está activa

Get-ADServiceAccount -Identity MDI-ServiceAccount -Properties *

 

\# Verificar permisos para recuperar contraseña

Get-ADServiceAccount -Identity MDI-ServiceAccount -Properties PrincipalsAllowedToRetrieveManagedPassword

 

#### **5.3.2 Validación de Permisos**

**Procedimiento:**

1. Verifique que grupo de seguridad de sensores está en     PrincipalsAllowedToRetrieveManagedPassword
2. Si nuevo DC fue agregado, añádalo al grupo de seguridad     autorizado:

Add-ADGroupMember -Identity "MDI-Sensors" -Members "DC03$"

 

#### **5.3.3 Reinstalación de Contraseña gMSA en Sensor**

**Si sensor no puede recuperar contraseña:**

\# En el DC afectado

Install-ADServiceAccount -Identity MDI-ServiceAccount

 

\# Reiniciar servicio MDI

Restart-Service -Name "AATPSensorUpdater"

Restart-Service -Name "AATPSensor"

 

### **5.4 Proceso de Instalación/Actualización de Sensores**

**Consola:** Settings > Identities > Sensors

#### **5.4.1 Instalación de Nuevo Sensor**

**Procedimiento:**

1. **Descarga del Instalador:
        
   **     

○    **Ruta:** Settings > Identities > Sensors > Add sensor

○    Descargue el paquete .zip más reciente

1. **Obtención     de Access Key:
        
   **     

○    Se muestra durante el proceso "Add sensor"

○    Copie el Access Key (se usa durante instalación)

1. **Instalación     en DC:
        
   **     

○    Copie instalador al servidor

○    Ejecute como Administrator

○    Ingrese Access Key cuando se solicite

○    Seleccione si el DC es Read-Only DC (RODC) o no

1. **Validación     Post-Instalación:
        
   **     

○    Espere 5-10 minutos

○    Verifique en portal que sensor aparece como "Running"

#### **5.4.2 Actualización Manual de Sensor**

**Cuando falla actualización automática:**

**Procedimiento:**

1. Identifique sensor con estado "Update failed" o     "Outdated"
2. Conéctese     vía RDP al servidor afectado
3. Descargue     instalador más reciente desde portal
4. Ejecute     instalador (detectará versión existente)
5. Seleccione     opción "Repair" o "Update"
6. Reinicie servicios si es necesario

#### **5.4.3 Configuración de Delayed Update Ring**

**Para entornos críticos:**

**Consola:** Settings > Identities > Sensors > Delayed update

**Procedimiento:**

1. Seleccione sensores para delayed update ring
2. Beneficio:     Estos sensores actualizan 72 horas después del resto
3. Permite validar actualizaciones en producción sin riesgo total

### **5.5 Revisión de Setup de Nuevos Domain Controllers**

**Proceso de Onboarding:**

**Checklist al añadir DC:**

1. Instalar sensor MDI inmediatamente
2. Validar     que gMSA está disponible en nuevo DC
3. Confirmar     configuraciones de auditoría
4. Verificar     conectividad a Azure (outbound 443)
5. Validar que sensor aparece en portal (5-10 min)

**Validación de Configuración:**

\# Ejecutar en nuevo DC

.\Test-MdiReadiness.ps1 -Mode LocalMachine -OpenHtmlReport

 

### **5.6 Validación de Configuración de Firewall/Proxy**

**Requisitos de Conectividad:**

**Endpoints requeridos:**

●    *.atp.azure.com (puerto 443)

●    *.blob.core.windows.net (para actualizaciones)

**PowerShell para validar:**

Test-NetConnection -ComputerName sensorapi.atp.azure.com -Port 443

 

**Si usa Proxy:**

●    Configure proxy en archivo de configuración del sensor

●    Ubicación: C:\Program Files\Azure Advanced Threat Protection Sensor\<version>\Microsoft.Tri.Sensor.Deployment.Package.exe.config

### **5.7 Capacity Planning y Sizing**

**Herramienta:** MDI Sizing Tool
 **Documentación:** https://learn.microsoft.com/en-us/defender-for-identity/deploy/capacity-planning

**Procedimiento:**

1. **Recopilación de Métricas:
        
   **     

○    Número de eventos de autenticación/día

○    Número de eventos LDAP/día

○    Número de controladores de dominio

○    Tráfico de red promedio a DCs

1. **Cálculo     de Recursos:
        
   **     

○    CPU: Basado en eventos/segundo

○    RAM: Mínimo 6GB, recomendado 8-16GB

○    Disco: Espacio para logs locales

1. **Validación de Capacidad:
        
   **     

\# Verificar uso de recursos en DC con sensor

Get-Counter '\Processor(_Total)\% Processor Time'

Get-Counter '\Memory\Available MBytes'

 



## **6. Operaciones Semestrales**

### **6.1 Sensor Update Strategy Review**

**Consola:** Settings > Identities > Sensors

**Procedimiento:**

1. **Revisión de Delayed Update Ring:
        
   **     

○    Valide si la estrategia actual sigue siendo apropiada

○    Considere ajustar basándose en criticidad de DCs

1. **Análisis     de Historial de Actualizaciones:
        
   **     

○    Revise si hubo problemas en últimos 6 meses

○    Documente lecciones aprendidas

### **6.2 Coverage & Architecture Validation**

**Procedimiento:**

**Inventario de Domain Controllers:
 
** Get-ADDomainController -Filter * | Select-Object Name, Site, OperatingSystem

1.  
2. **Validación     de Cobertura:
        
   **     

○    Cruce inventario de DCs con sensores instalados

○    Identifique DCs sin sensor

○    Planifique instalación en DCs faltantes

1. **Revisión     de Nuevos Forests/Domains:
        
   **     

○    Si hubo adquisiciones o expansión, valide cobertura

○    Instale sensores en nuevos dominios

### **6.3 Operational Drills (Tabletop Exercise)**

**Escenarios a Simular:**

#### **6.3.1 Compromiso de Cuenta Privilegiada**

**Escenario:**

●    Alerta de DCSync en cuenta de administrador

●    ¿Cómo responde el equipo?

**Validación:**

●    Tiempo de detección

●    Escalamiento correcto

●    Ejecución de playbook

●    Coordinación con IT/AD Team

#### **6.3.2 Movimiento Lateral Masivo**

**Escenario:**

●    Múltiples alertas de Pass-the-Hash

●    Indicadores de ransomware pre-detonación

**Validación:**

●    Capacidad de aislar dispositivos

●    Reseteo masivo de contraseñas

●    Comunicación con stakeholders



## **7. Operaciones Anuales**

### **7.1 Capacity y Performance Reassessment**

**Procedimiento:**

1. **Análisis de Crecimiento:
        
   **     

○    Compare métricas actuales vs año anterior:

■    Número de usuarios

■    Número de dispositivos

■    Eventos de autenticación/día

■    Volumen de tráfico a DCs

1. **Validación     de Recursos:
        
   **     

○    Verifique que DCs/sensores tienen recursos adecuados

○    Re-ejecute MDI Sizing Tool

○    Planifique expansión de infraestructura si es necesario

### **7.2 Policy & Standard Review**

**Revisión de Documentación Operativa:**

1. **SLAs de Triaje:
        
   **     

○    Valide tiempos de respuesta actuales

○    Ajuste SLAs según capacidad del equipo

1. **Alert     Tuning Governance:
        
   **     

○    Revise proceso de aprobación de exclusiones

○    Actualice documentación

1. **Reporting     Requirements:
        
   **     

○    Valide que reportes satisfacen necesidades de auditoría

○    Ajuste frecuencia/contenido según feedback

### **7.3 Roadmap Alignment**

**Consola:** What's New
 **URL:** https://learn.microsoft.com/en-us/defender-for-identity/whats-new

**Procedimiento:**

1. **Revisión de Nuevas Capacidades:
        
   **     

○    Nuevas detecciones

○    Nuevas versiones de sensores

○    Mejoras en posture assessments

1. **Planning     de Adopción:
        
   **     

○    Priorice features por impacto

○    Defina timeline de implementación

○    Asigne recursos

1. **Budget     Planning:
        
   **     

○    Valide si se requieren licencias adicionales

○    Planifique expansión de team si es necesario



## **8. Métricas y KPIs**

### **8.1 Métricas de Detección**

| **Métrica**                      | **Objetivo**              | **Frecuencia** |
| -------------------------------- | ------------------------- | -------------- |
| **Mean Time To  Detect (MTTD)**  | <15 minutos para críticos | Mensual        |
| **Mean Time To  Respond (MTTR)** | <1 hora para críticos     | Mensual        |
| **False Positive Rate**          | <10% de total de alertas  | Mensual        |
| **Sensor Health**                | 100% Running              | Diaria         |
| **Identity Secure Score**        | >80%                      | Mensual        |

### **8.2 Métricas de Cobertura**

| **Métrica**                      | **Objetivo**           | **Frecuencia** |
| -------------------------------- | ---------------------- | -------------- |
| **DC Coverage**                  | 100% de DCs con sensor | Trimestral     |
| **Advanced Auditing Compliance** | 100% de DCs            | Trimestral     |
| **LMP Reduction**                | 0 rutas críticas       | Mensual        |



## **9. Troubleshooting Guide**

### **9.1 Sensor No Comunica**

**Síntoma:** Estado "Stopped communicating"

**Diagnóstico:**

**Validar Servicio:
 
** Get-Service -Name "AATPSensor", "AATPSensorUpdater"

1.  
2. **Revisar     Logs:
        
   **     

○    Ubicación: C:\Program Files\Azure Advanced Threat Protection Sensor\<version>\Logs

○    Archivo: Microsoft.Tri.Sensor.log

**Validar Conectividad:
 
** Test-NetConnection -ComputerName sensorapi.atp.azure.com -Port 443

1.  

**Remediación:**

●    Reiniciar servicios

●    Verificar firewall/proxy

●    Reinstalar sensor si persiste

### **9.2 Directory Services User Credentials Incorrect**

**Síntoma:** Alerta en Health Issues

**Diagnóstico:**

1. Validar que gMSA existe y está activa
2. Verificar permisos del DC para recuperar contraseña

**Remediación:**

\# Reinstalar gMSA en DC

Install-ADServiceAccount -Identity MDI-ServiceAccount

Restart-Service -Name "AATPSensor"

 

### **9.3 NTLM Auditing Not Enabled**

**Síntoma:** Alerta en Health Issues

**Remediación via GPO:**

**Ubicación:** Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options

**Setting:**

●    Network security: Restrict NTLM: Audit NTLM authentication in this domain = Enable auditing for all accounts

### **9.4 Advanced Audit Policy Incorrectly Configured**

**Remediación via PowerShell (en cada DC):**

\# Habilitar auditorías requeridas

auditpol /set /subcategory:"Credential Validation" /success:enable /failure:enable

auditpol /set /subcategory:"Kerberos Authentication Service" /success:enable /failure:enable

auditpol /set /subcategory:"Directory Service Access" /success:enable

auditpol /set /subcategory:"Directory Service Changes" /success:enable

 



## **10. Roles y Responsabilidades**

| **Rol**                | **Responsabilidades Primarias**                              |
| ---------------------- | ------------------------------------------------------------ |
| **SOC Operator**       | Triaje diario, monitoreo de dashboard,  investigación inicial |
| **SOC Analyst**        | Investigación profunda, hunting,  análisis forense           |
| **Incident Responder** | Respuesta a incidentes críticos,  coordinación con IT        |
| **Administrator**      | Gestión de sensores, tuning,  configuración de exclusiones   |
| **Architect**          | Diseño de cobertura, planning de  expansión, validación trimestral |
| **Manager**            | Revisión de reportes, asignación de  recursos, escalamiento  |
| **CISO**               | Revisión de postura estratégica,  aprobación de cambios críticos |



## **11. Conclusión**

Esta guía operativa consolidada proporciona el marco completo para la operación efectiva de Microsoft Defender for Identity. El éxito depende de:

1. **Disciplina Operativa:** Ejecución     consistente de tareas diarias/semanales
2. **Mejora     Continua:** Análisis mensual de postura y     ajuste proactivo
3. **Validación     Técnica:** Verificación trimestral de     configuración de infraestructura
4. **Alineación Estratégica:** Revisión     anual de roadmap y capacidades

**Recursos Adicionales:**

●    Documentación oficial: https://learn.microsoft.com/en-us/defender-for-identity/

●    Tech Community: https://techcommunity.microsoft.com/t5/microsoft-defender-for-identity/bd-p/AzureAdvancedThreatProtection

●    GitHub: https://github.com/microsoft/Azure-Advanced-Threat-Protection



**Versión:** 1.0
 **Última Actualización:** [Fecha actual]
 **Próxima Revisión:** [Fecha + 1 año]

 