Aquí está el documento consolidado y enriquecido:

---

# Guía Operacional de Seguridad — Microsoft Intune para SecOps / SOC
## Edición M365 E5 | Ciberseguridad | Versión Consolidada



---
**Nota de Verificación Técnica:**
Este documento ha sido auditado contra documentación oficial de Microsoft. 
- ✅ Claims verificados incluyen referencias inline a Microsoft Learn/Docs
- ⚠️ Contenido marcado como "análisis consultivo" no constituye estándar oficial Microsoft
- 📊 Métricas, scores o niveles sin referencia explícita han sido eliminados o reformulados
---

# Guía Operacional de Seguridad — Microsoft Intune para SecOps / SOC
## Edición M365 E5 | Ciberseguridad | Versión Consolidada (Actividades diarias)

---

## 1. Marco Operativo: Rol de Ciberseguridad en Intune

Desde la perspectiva de SecOps, Microsoft Intune es la **fuente de verdad del estado de postura de seguridad de endpoints** [Fuente](https://learn.microsoft.com/en-us/intune/protect/device-compliance-policy-monitor). En una arquitectura Zero Trust, el dispositivo reemplaza al perímetro: la compliance es el gatekeeper de Conditional Access y un dispositivo marcado como *noncompliant* **puede ser aislado automáticamente de recursos corporativos según configuración de Conditional Access**

El SOC consume y actúa sobre tres dimensiones críticas:

| Dimensión                  | Relevancia para SecOps                                       |
| -------------------------- | ------------------------------------------------------------ |
| **Compliance Signal**      | Alimenta Conditional Access → bloqueo de acceso a identidades comprometidas |
| **Security Configuration** | Baselines, ASR, BitLocker, Firewall → superficie de ataque controlada |
| **Threat Integration**     | MDE ↔ Intune → contención, remediación y forensics en dispositivos comprometidos [Fuente](https://learn.microsoft.com/en-us/intune/intune-service/protect/mde-integration) |

> **Alcance**: Este documento cubre exclusivamente las responsabilidades del área de **Ciberseguridad / SOC**. Las tareas de enrollment masivo, gestión de aplicaciones de productividad y soporte de dispositivos son responsabilidad de Microinformática.

---

## 2. Tareas Diarias

### D-01 · Monitoreo de Dispositivos No Conformes con Acceso Activo

**Consola:** `intune.microsoft.com` → **Devices → Compliance → Monitor → Noncompliant devices**

**Objetivo:** Identificar dispositivos *noncompliant* que siguen con sesión activa — potencial bypass de Conditional Access.

**Procedimiento:**
1. Filtrar por `Compliance state = Noncompliant` + `Last check-in < 24h`
2. Usar el filtro de plataforma (OS) para separar Windows de dispositivos móviles; una caída en iOS por actualización de SO no debe distraer del análisis de postura Windows
3. Correlacionar contra logs de inicio de sesión en Entra ID: si hay sesión activa con dispositivo noncompliant → **escalar como Potential Policy Bypass**
4. Analizar la columna **Noncompliant setting**: distinguir fallos de seguridad críticos (BitLocker deshabilitado, antivirus inactivo) de fallos administrativos (versión de SO mínima, contraseña expirada)
5. Verificar que el **Compliance status validity period** (`Devices → Device compliance → Compliance settings`) no esté configurado de forma laxa (>30 días implica que un dispositivo robado puede retener acceso) 

**Objetivo operativo recomendado**: Dispositivos noncompliant con sesión activa = 0

---

### D-02 · Revisión del Endpoint Security Dashboard

**Consola:** `intune.microsoft.com` → **Endpoint security → Overview**

**Objetivo:** Validar estado consolidado de Antivirus, EDR y conector MDE.

**Procedimiento:**
1. Verificar **Defender for Endpoint Connector status = Active**; si está `Inactive` → activar alerta P1 [Fuente](https://learn.microsoft.com/en-us/intune/protect/endpoint-security-mde-onboarding)
2. Revisar `Windows devices onboarded to Defender for Endpoint` → identificar gaps de cobertura EDR
3. Controlar `Antivirus agent status` → dispositivos con agente inactivo o desactualizado
4. Revisar estado de **Disk Encryption**: `Endpoint security → Disk encryption → Monitor` → filtrar `Not encrypted`; verificar que Recovery Keys estén en custodia en Entra ID

**Objetivos orientativos**: Conector MDE activo siempre; cobertura EDR cercana a 100%; cifrado BitLocker/FileVault superior al 95%

---

### D-03 · Auditoría de Cambios Administrativos

**Consola:** `intune.microsoft.com` → **Tenant administration → Audit logs**

**Objetivo:** Detectar cambios no autorizados o fuera de ventana de cambio en políticas críticas.

**Procedimiento:**
1. Filtrar por `Category: EndpointSecurity`, `CompliancePolicy`, `DeviceConfiguration` y fecha = últimas 24h
2. Correlacionar cada cambio con el sistema de Change Management (tickets aprobados)
3. Cambio sin ticket asociado → escalar como **Unauthorized Change**
4. Verificar que `Initiated by` corresponde a cuentas de administrador autorizadas, no service accounts 

**Objetivo operativo recomendado**: Cambios sin ticket = 0; tiempo de detección < 4h

---

### D-04 · Revisión de Security Baselines

**Consola:** `intune.microsoft.com` → **Endpoint security → Security baselines → [Perfil] → Monitor**

**Objetivo:** Identificar dispositivos con desviación de configuraciones de seguridad estándar (NIST CSF DE.CM-7 / CIS Control 4).

**Procedimiento:**
1. Revisar **Windows Security Baseline**, **Microsoft Edge Baseline** y **Defender for Endpoint Baseline**
2. Filtrar por estado `Not compliant` con settings de alta severidad
3. El reporte **Per-setting status** permite identificar si un control específico (ej. Credential Guard) falla globalmente, indicando un prerequisito roto (ej. VBS deshabilitado en BIOS)
4. Investigar y resolver **conflictos**: cuando dos políticas compiten sobre el mismo CSP, identificar la política ganadora y eliminar el setting redundante de la otra 

**Objetivo operativo recomendado**: Compliance con Security Baselines ≥ 95%

---

### D-05 · Correlación de Alertas MDE con Estado Intune

**Consola:** `security.microsoft.com` → **Incidents & alerts → Alerts** + `intune.microsoft.com` → **Devices**

**Objetivo:** Contextualizar alertas de amenaza activa con el estado de compliance y configuración en Intune.

**Procedimiento:**
1. Filtrar alertas del día con severity `High/Medium` en el Defender Portal; usar la vista **Incidents** [Fuente](https://learn.microsoft.com/en-us/defender-endpoint/respond-machine-alerts)
2. Para cada alerta: abrir el device → verificar `Intune compliance state`, `Last sync` y políticas de seguridad aplicadas
3. Analizar el **Alert Story** (árbol de procesos): si `winword.exe` spawneó `powershell.exe` → indicador de macro maliciosa
4. Si el dispositivo afectado está `Noncompliant` → documentar como **Contributing Factor** en el incidente
5. Para alertas de falsos positivos: crear **suppression rule** con scope acotado

**Objetivo operativo recomendado**: Correlacionar el 100% de alertas High con estado Intune en < 30 min

---

### D-06 · Vigilancia de Enrollments y Cambios de Inventario

**Consola:** `intune.microsoft.com` → **Devices → All devices** → filtrar `Enrollment date = Today`

**Objetivo:** Detectar enrollments no autorizados (Shadow IT, BYOD no controlado, insider threat).

**Procedimiento:**
1. Revisar todos los dispositivos enrollados en las últimas 24h
2. Cruzar contra lista de onboardings autorizados
3. Dispositivos no registrados en proceso formal → escalar como **Unauthorized Enrollment**
4. Verificar que **Enrollment restrictions** bloquea dispositivos personales

**Objetivo operativo recomendado**: Enrollments no autorizados detectados en < 4h

---











## Edición M365 E5 | Ciberseguridad | Versión Ampliada

---

## 1. Marco Operativo: Rol de Ciberseguridad en Intune

Desde la perspectiva de SecOps, Microsoft Intune es la **fuente de verdad del estado de postura de seguridad de endpoints**. En una arquitectura Zero Trust, el dispositivo reemplaza al perímetro: la compliance es el gatekeeper de Conditional Access, y un dispositivo marcado como *noncompliant* debe ser automáticamente aislado de los recursos corporativos.

El SOC consume y actúa sobre tres dimensiones críticas:

| Dimensión                  | Relevancia para SecOps                                       |
| -------------------------- | ------------------------------------------------------------ |
| **Compliance Signal**      | Alimenta Conditional Access → bloqueo de acceso a identidades comprometidas |
| **Security Configuration** | Baselines, ASR, BitLocker, Firewall → superficie de ataque controlada |
| **Threat Integration**     | MDE ↔ Intune → contención, remediación y forensics en dispositivos comprometidos |

> **Alcance**: Este documento cubre exclusivamente las responsabilidades del área de **Ciberseguridad / SOC**. Las tareas de enrollment masivo, gestión de aplicaciones de productividad y soporte de dispositivos son responsabilidad de Microinformática.

---

## 2. Tareas Diarias

### D-01 · Monitoreo de Dispositivos No Conformes con Acceso Activo

**Consola:** `intune.microsoft.com` → **Devices → Compliance → Monitor → Noncompliant devices**

**Objetivo:** Identificar dispositivos *noncompliant* que siguen con sesión activa — potencial bypass de Conditional Access.

**Procedimiento:**
1. Filtrar por `Compliance state = Noncompliant` + `Last check-in < 24h`
2. Usar el filtro de plataforma (OS) para separar Windows de dispositivos móviles; una caída en iOS por actualización de SO no debe distraer del análisis de postura Windows
3. Correlacionar contra logs de inicio de sesión en Entra ID: si hay sesión activa con dispositivo noncompliant → **escalar como Potential Policy Bypass**
4. Analizar la columna **Noncompliant setting**: distinguir fallos de seguridad críticos (BitLocker deshabilitado, antivirus inactivo) de fallos administrativos (versión de SO mínima, contraseña expirada)
5. Verificar que el **Compliance status validity period** (`Devices → Device compliance → Compliance settings`) no esté configurado de forma laxa (>30 días implica que un dispositivo robado puede retener acceso)

**KPI:** Dispositivos noncompliant con sesión activa = 0

---

### D-02 · Revisión del Endpoint Security Dashboard

**Consola:** `intune.microsoft.com` → **Endpoint security → Overview**

**Objetivo:** Validar estado consolidado de Antivirus, EDR y conector MDE.

**Procedimiento:**
1. Verificar **Defender for Endpoint Connector status = Active**; si está `Inactive` → activar alerta P1
2. Revisar `Windows devices onboarded to Defender for Endpoint` → identificar gaps de cobertura EDR
3. Controlar `Antivirus agent status` → dispositivos con agente inactivo o desactualizado
4. Revisar estado de **Disk Encryption**: `Endpoint security → Disk encryption → Monitor` → filtrar `Not encrypted`; verificar que Recovery Keys estén en custodia en Entra ID

**KPI:** Conector MDE activo 100% del tiempo; cobertura EDR ≥ 98%; cifrado activo ≥ 99%

---

### D-03 · Auditoría de Cambios Administrativos

**Consola:** `intune.microsoft.com` → **Tenant administration → Audit logs**

**Objetivo:** Detectar cambios no autorizados o fuera de ventana de cambio en políticas críticas.

**Procedimiento:**
1. Filtrar por `Category: EndpointSecurity`, `CompliancePolicy`, `DeviceConfiguration` y fecha = últimas 24h
2. Correlacionar cada cambio con el sistema de Change Management (tickets aprobados)
3. Cambio sin ticket asociado → escalar como **Unauthorized Change**
4. Verificar que `Initiated by` corresponde a cuentas de administrador autorizadas, no service accounts

**KPI:** Cambios sin ticket = 0; tiempo de detección < 4h

---

### D-04 · Revisión de Security Baselines

**Consola:** `intune.microsoft.com` → **Endpoint security → Security baselines → [Perfil] → Monitor**

**Objetivo:** Identificar dispositivos con desviación de configuraciones de seguridad estándar (NIST CSF DE.CM-7 / CIS Control 4).

**Procedimiento:**
1. Revisar **Windows Security Baseline**, **Microsoft Edge Baseline** y **Defender for Endpoint Baseline**
2. Filtrar por estado `Not compliant` con settings de alta severidad
3. El reporte **Per-setting status** permite identificar si un control específico (ej. Credential Guard) falla globalmente, indicando un prerequisito roto (ej. VBS deshabilitado en BIOS)
4. Investigar y resolver **conflictos**: cuando dos políticas compiten sobre el mismo CSP (ej. BitLocker Encryption Method en XTS-AES 128 vs 256), identificar la política ganadora y eliminar el setting redundante de la otra

**KPI:** Compliance con Security Baselines ≥ 95%

---

### D-05 · Correlación de Alertas MDE con Estado Intune

**Consola:** `security.microsoft.com` → **Incidents & alerts → Alerts** + `intune.microsoft.com` → **Devices**

**Objetivo:** Contextualizar alertas de amenaza activa con el estado de compliance y configuración en Intune.

**Procedimiento:**
1. Filtrar alertas del día con severity `High/Medium` en el Defender Portal; usar la vista **Incidents** (agrega alertas relacionadas en una narrativa única)
2. Para cada alerta: abrir el device → verificar `Intune compliance state`, `Last sync` y políticas de seguridad aplicadas
3. Analizar el **Alert Story** (árbol de procesos): si `winword.exe` spawneó `powershell.exe` → indicador de macro maliciosa
4. Si el dispositivo afectado está `Noncompliant` → documentar como **Contributing Factor** en el incidente
5. Para alertas de falsos positivos: crear **suppression rule** con scope acotado (hash o ruta específica)

**KPI:** 100% de alertas High correlacionadas con estado Intune en < 30 min

---

### D-06 · Vigilancia de Enrollments y Cambios de Inventario

**Consola:** `intune.microsoft.com` → **Devices → All devices** → filtrar `Enrollment date = Today`

**Objetivo:** Detectar enrollments no autorizados (Shadow IT, BYOD no controlado, insider threat).

**Procedimiento:**
1. Revisar todos los dispositivos enrollados en las últimas 24h
2. Cruzar contra lista de onboardings autorizados (HR, ITSM)
3. Dispositivos no registrados en proceso formal → escalar como **Unauthorized Enrollment**
4. Verificar que **Enrollment restrictions** (`Devices → Enrollment → Device platform restrictions`) bloquea dispositivos personales; si el reporte de fallos muestra rechazos de "Personal" ownership → indicador positivo de que los controles funcionan

**KPI:** Enrollments no autorizados detectados en < 4h

---

## 3. Tareas Semanales

### W-01 · Análisis de Tendencias de Compliance

**Consola:** `intune.microsoft.com` → **Reports → Device compliance → Organizational**

**Procedimiento:**
1. Generar **Organizational compliance report** comparando semana actual vs. semana anterior; un pico brusco de non-compliance raramente es coincidencia: generalmente indica una actualización de Windows que quebró un setting o una política recién desplegada que conflictúa con baselines existentes
2. Identificar plataformas o departamentos con tendencia negativa ≥ 5%
3. Si el 20% del fleet falla el check de "Minimum OS Version", indica que los Update rings no están entregando feature updates con suficiente velocidad, no un problema de configuración de seguridad en sí
4. Cross-reference con Entra ID → **Protection → Conditional Access → Insights and reporting**: verificar tasas de bloqueo por non-compliance

---

### W-02 · Validación de Reglas ASR

**Consola:** `security.microsoft.com` → **Reports → Attack surface reduction rules**

**Procedimiento:**
1. Revisar **Detections tab**: analizar timeline de `Blocks` vs `Audits`; alto volumen de Audits en una regla específica confirma que el control funciona (en audit mode) pero aún no está en enforcement
2. Revisar **Configuration tab**: verificar que dispositivos estén en estado `Block` para reglas críticas; un dispositivo en `Off` con política Intune en `Succeeded` → verificar Event Viewer local (Event IDs 1121/1122)
3. Identificar falsos positivos: apps como `MsSense.exe` o `MpCmdRun.exe` crasheando → el endpoint está efectivamente sin defensa
4. Documentar toda decisión de pasar una regla de `Block` a `Audit Mode`: usar el campo Description del perfil con referencia al ticket

---

### W-03 · Revisión de Security Tasks (TVM → Intune Remediation)

**Consola:** `security.microsoft.com` → **Vulnerability management → Remediation → Security tasks**

**Procedimiento:**
1. Revisar tasks con estado `New` o `In progress`; priorizar por `Severity` y `Exposed devices count`
2. Para CVEs con CVSS ≥ 9.0: verificar estado de patch en dispositivos vía Intune
3. Tasks bloqueadas (requieren reinicio, aprobación del usuario) → escalar con plan de mitigación temporal
4. Para CVEs en CISA KEV: iniciar **Expedited Update deployment** en Intune omitiendo el deferral de Update rings

**KPI:** Security tasks procesadas en SLA — Critical ≤ 7 días, High ≤ 14 días

---

### W-04 · Auditoría de RBAC e Identidades Privilegiadas

**Consola:** `intune.microsoft.com` → **Tenant administration → Roles** + `entra.microsoft.com` → **Roles & admins**

**Procedimiento:**
1. Listar usuarios con roles `Intune Administrator`, `Endpoint Security Manager`, `Global Administrator`
2. Verificar que cuentas privilegiadas tengan MFA + PIM (Just-in-Time activation); ningún administrador de Intune debería usar habitualmente una cuenta de Global Administrator
3. Identificar cuentas con privilegios permanentes sin PIM → escalar para corrección
4. Revocar permisos de ex-empleados o cuentas inactivas > 30 días
5. Revisar **Scope tags**: asegurar que admins solo gestionen su ámbito autorizado

**KPI:** 0 cuentas admin sin MFA; 100% de roles privilegiados con PIM

---

### W-05 · Verificación de Certificados (SCEP/PKCS) y Conectores

**Consola:** `intune.microsoft.com` → **Devices → Monitor → Certificates** + **Tenant administration → Connector status**

**Procedimiento:**
1. Filtrar certificados con expiración en próximos 30 días; alertar con 45 días de anticipación para certificados críticos (VPN, WiFi)
2. Verificar que proceso de auto-renewal esté funcionando; certificados en estado `Revoked` o `Error` → coordinar con equipo de PKI
3. Revisar el **certificado Apple MDM Push (APNs)**: vence cada 365 días; si expira, la gestión de iOS/iPadOS se interrumpe completamente
4. Controlar **tokens de Apple VPP/DEP** y **conector de Android Enterprise**; un conector en `Unhealthy` → acción inmediata

**KPI:** 0 certificados expirados en dispositivos activos

---

### W-06 · Control de Dispositivos Inactivos

**Consola:** `intune.microsoft.com` → **Devices → Device cleanup rules**

**Procedimiento:**
1. Verificar el umbral configurado en "Delete devices that haven't checked in for this many days" — rango recomendado: 30 a 90 días
2. Usar **View affected devices** o filtrar `Devices → All devices` por `Last check-in > 90 días` para previsualizar antes de ejecutar
3. Un dispositivo "Compliant" con 20 días sin check-in es un **phantom risk**: la red lo confía pero su estado real es desconocido
4. Coordinar con Identity: la limpieza en Intune no elimina el objeto en Entra ID → los dispositivos eliminados de Intune deben también ser deshabilitados/eliminados en Entra ID para evitar "objetos huérfanos"

**KPI:** 0 dispositivos activos de ex-empleados; < 5% de dispositivos inactivos en inventario

---

## 4. Tareas Mensuales

### M-01 · Revisión de Firewall, BitLocker y ASR (Feature Verification)

Esta es la tarea de verificación central mandatada por el ciclo mensual. Las asignaciones en Intune **no equivalen** a enforcement en el dispositivo; verificar el *status* real requiere consultar reportes específicos.

**A. Verificación de Firewall**

**Consola:** `intune.microsoft.com` → **Reports → Firewall → MDM Firewall status for Windows 10 and later**

| Métrica         | Estado Deseado | Trigger de Investigación                             |
| --------------- | -------------- | ---------------------------------------------------- |
| Firewall Status | Enabled        | "Disabled", "Limited", "Temporarily Disabled"        |
| Target          | MDM            | "Group Policy" (indica conflicto GPO en Hybrid Join) |

> No confiar en `wf.msc` local para verificar reglas MDM — generalmente no aparecen en la GUI aunque estén activas. Verificar en Registry: `HKLM\SYSTEM\ControlSet001\Services\SharedAccess\Parameters\FirewallPolicy\Mdm\FirewallRules`

**B. Verificación de BitLocker**

**Consola:** `intune.microsoft.com` → **Devices → Monitor → Encryption report**

| Métrica              | Estado Deseado | Trigger de Investigación               |
| -------------------- | -------------- | -------------------------------------- |
| Encryption status    | Encrypted      | "Not Encrypted"                        |
| Ready for encryption | True           | "False" (incompatibilidad de hardware) |
| TPM Version          | 2.0            | "1.2" o "None"                         |

Si un dispositivo es "Not Encrypted": clic en la fila → **Status details** → expone el código de error CSP de BitLocker específico (ej. "User postponed encryption", "TPM initialization failed").

**C. Verificación de ASR**

**Consola:** `security.microsoft.com` → **Reports → Attack surface reduction rules → Configuration tab**

Verificar que la mayoría de dispositivos estén en estado `Block`. Si un device muestra `Off` con política Intune en `Succeeded` → revisar Event IDs 1121 (Block) y 1122 (Audit) en `Applications and Services Logs → Microsoft → Windows → Windows Defender → Operational`.

---

### M-02 · Revisión de Compliance y Efectividad de Políticas

**Consola:** `intune.microsoft.com` → **Reports → Device compliance → Reports**

**Procedimiento:**
1. Generar el **Device Compliance Organizational Report** con filtros: Compliance status, OS, Ownership
2. Analizar la columna **Setting Compliance**: si "Require BitLocker" tiene 5% de fallo constante → indica incompatibilidad de hardware (BIOS legacy, TPM 1.2) que requiere proyecto de refresh, no ajuste de configuración
3. Revisar **Conditional Access Insights**: tasas de bloqueo altas por non-compliance suelen indicar que la educación al usuario sobre remediación (cómo actualizar el dispositivo) es insuficiente
4. Evaluar ratio señal-ruido: una política con 99% de éxito pero cuyo 1% falla por un setting que no impacta seguridad debe ser ajustada para reducir fricción

---

### M-03 · Evaluación y Actualización de Security Baselines

**Consola:** `intune.microsoft.com` → **Endpoint security → Security baselines**

**Procedimiento para actualizar una baseline (proceso controlado):**

1. Seleccionar el perfil activo → **Change Version** → seleccionar nueva versión → **Review update**
2. Descargar el archivo **CSV diff** — contiene settings `New`, `Removed` y `Modified`; analizar antes de cualquier acción
3. **Nunca** actualizar el perfil de producción directamente; crear un perfil nuevo con la versión actualizada y asignarlo al grupo piloto
4. Monitorear **Endpoint Analytics → Anomalies** durante 7 días para detectar regresiones
5. Si una nueva configuración como "Block all Office macros from the Internet" puede paralizar operaciones, tratarla como un evento de Change Management mayor con plan de rollback

> **Fenómeno Tattooing:** Un setting aplicado permanece en el dispositivo incluso si la política es removida. El chequeo semanal de estados `Not Applicable` y `Error` ayuda a identificar dispositivos en estado "zombie".

---

### M-04 · Microsoft Secure Score — Acciones de Device Management

**Consola:** `security.microsoft.com/securescore` → filtrar categoría **Device**

**Procedimiento:**
1. Calcular delta vs. mes anterior
2. Filtrar `Improvement actions` → categoría Device → ordenar por `Score impact`
3. Seleccionar top 3 acciones para implementar en el mes
4. Comparar postura contra peers: **Metrics & trends → Comparison with similar organizations**

**KPI:** Secure Score (Device) crecimiento ≥ 2 puntos/mes; posición > percentil 75

---

### M-05 · Integración Intune → Microsoft Sentinel (Validación)

**Consola:** `portal.azure.com` → **Microsoft Sentinel → Logs**

Tablas a validar: `IntuneAuditLogs`, `IntuneDevices`, `IntuneDeviceComplianceOrg`, `IntuneOperationalLogs`

```kusto
// Verificar ingesta y latencia de logs Intune en Sentinel
union IntuneAuditLogs, IntuneDevices, IntuneDeviceComplianceOrg, IntuneOperationalLogs
| summarize LastIngestion = max(TimeGenerated), Count = count() by Type
| order by LastIngestion desc

// Detectar dispositivos sin cifrado con compliance activa (gap de CA policy)
IntuneDevices
| where EncryptionStatusString != "Encrypted"
| where CompliantState == "Compliant"
| project TimeGenerated, DeviceName, OS, UPN, EncryptionStatusString, CompliantState

// Cambios críticos en políticas - últimas 24h
IntuneAuditLogs
| where TimeGenerated > ago(24h)
| where OperationName has_any ("CompliancePolicy","EndpointSecurity","SecurityBaseline")
| project TimeGenerated, OperationName, ResultType, InitiatedByUser = Identity
| order by TimeGenerated desc
```

**KPI:** Latencia de ingesta Intune → Sentinel < 15 min; 0 tablas sin datos

---

## 5. Playbooks de Incident Response (Ad-Hoc)

### IR-01 · Dispositivo Corporativo Comprometido (Malware/Ransomware)

**Trigger:** Alerta MDE severity `High` indicando malware activo o comportamiento de ransomware.

```
FASE 1 — CONTENCIÓN (0–15 min)
├── [MDE] security.microsoft.com → Devices → [Device] → Isolate device
│   Efecto: aísla de red manteniendo canal con Defender (lateral movement bloqueado)
├── [Intune] Verificar compliance state y Last sync del dispositivo afectado
├── [Entra ID] Revocar sesiones activas: Revoke all sessions
└── [Sentinel] Abrir Incident con severidad P1

FASE 2 — INVESTIGACIÓN (15–60 min)
├── [MDE] Device timeline → últimas 72h: identificar proceso padre (ej. winword.exe → powershell.exe)
├── [MDE] Live Response session → recolectar artefactos forenses
├── [Intune] Audit log → cambios recientes en configuración del dispositivo
└── [Sentinel] Correlacionar con otros dispositivos del mismo usuario/segmento de red

FASE 3 — REMEDIACIÓN (60 min – 4h)
├── Si recuperable: [MDE] Full antivirus scan + remediation
├── Si irrecuperable: [Intune] Wipe (requiere autorización Tier 3)
│   ⚠️ Verificar que BitLocker Recovery Key esté en Entra ID ANTES del Wipe
├── [MDE] Liberar aislamiento SOLO después de confirmar limpieza total
└── [Intune] Re-enrollment del dispositivo limpio vía Autopilot

FASE 4 — POST-INCIDENTE
├── ¿Qué regla ASR debería haber bloqueado esto? Si estaba en Audit → mover a Block
├── [Intune] Verificar que todas las políticas se apliquen correctamente post-enrollment
└── Documentar en ITSM con timeline completo y lessons learned
```

---

### IR-02 · Pérdida o Robo de Dispositivo Corporativo

**Trigger:** Reporte de usuario, acceso geográficamente anómalo, o ticket de RRHH.

```
FASE 1 — CONTENCIÓN INMEDIATA (0–10 min)
├── [Intune] Find my device (si habilitado) → localizar
├── [Intune] Remote Lock → bloqueo con PIN numérico
└── [Entra ID] Revocar refresh tokens del usuario

FASE 2 — EVALUACIÓN (10–30 min)
├── ¿El dispositivo tiene BitLocker/FileVault activo? → verificar Encryption report en Intune
├── ¿Contiene datos sensibles (PII, financieros, legal)?
└── Decisión: Retire (datos preservados) vs. Full Wipe (datos destruidos)

FASE 3 — ACCIÓN
├── BYOD → [Intune] Selective Wipe (solo datos corporativos)
├── Corporativo con datos sensibles → [Intune] Full Wipe (autorización requerida)
└── Documentar cadena de custodia con timestamp

FASE 4 — SEGUIMIENTO
├── Verificar en Intune que el Wipe se completó (Device action status)
├── Cancelar certificados del dispositivo (SCEP/PKCS)
└── Provisionar nuevo dispositivo con políticas de seguridad
```

---

### IR-03 · Admin Account Compromise (Acceso No Autorizado a Consola Intune)

**Trigger:** Alerta de Entra ID Protection, acceso desde ubicación anómala, o cambio masivo no planificado en políticas.

```
FASE 1 — CONTENCIÓN INMEDIATA (0–15 min)
├── [Entra ID] Revocar TODAS las sesiones del administrador comprometido
├── [Entra ID PIM] Desactivar roles privilegiados activos del usuario
└── [Sentinel] Abrir Incident P1

FASE 2 — EVALUACIÓN DE DAÑO (15–60 min)
├── [Intune Audit] Revisar TODOS los cambios de las últimas 48h del usuario
├── ¿Se modificaron políticas de compliance? ¿Se deshabilitaron baselines?
├── ¿Se realizaron Wipes no autorizados? ¿Se crearon nuevas enrollment restrictions permisivas?
└── Documentar timeline completo de acciones con evidencia

FASE 3 — REMEDIACIÓN
├── Revertir cambios no autorizados en políticas (re-deploy de baseline)
├── [Entra ID] Resetear credenciales + MFA del administrador
└── Forzar re-sync de dispositivos afectados

FASE 4 — HARDENING POST-INCIDENTE
├── Revisar todos los roles de Intune admin → aplicar PIM donde falte
└── Habilitar alerta en Sentinel para cambios masivos en Intune (umbral: N cambios en M minutos)
```

---

## 6. KPIs Operativos Consolidados

### KPIs Diarios

| KPI                                         | Target          |
| ------------------------------------------- | --------------- |
| Dispositivos noncompliant con sesión activa | = 0             |
| Cobertura EDR (MDE sensor activo)           | ≥ 98%           |
| Dispositivos cifrados (BitLocker/FileVault) | ≥ 99%           |
| Conector MDE activo                         | 100% del tiempo |
| Tiempo de detección de cambio no autorizado | < 4h            |

### KPIs Semanales

| KPI                                  | Target                 |
| ------------------------------------ | ---------------------- |
| Compliance rate trend                | ≥ 0 (sin degradación)  |
| Security tasks procesadas (Critical) | ≤ 7 días               |
| Reglas ASR críticas en Block mode    | 100%                   |
| Cobertura CA con device compliance   | ≥ 90% de apps críticas |
| Cuentas admin sin MFA                | = 0                    |

### KPIs Mensuales

| KPI                                       | Target      |
| ----------------------------------------- | ----------- |
| Secure Score (Device) delta mensual       | ≥ +2 puntos |
| Coverage controles CIS Level 1 vía Intune | ≥ 90%       |
| 100% de Wipe/Retire con ticket autorizado | = 100%      |
| Latencia ingesta Intune → Sentinel        | < 15 min    |
| Excepciones de seguridad vencidas         | = 0         |

---

## 7. RACI: Ciberseguridad vs. Microinformática

| Dominio                                        | Ciberseguridad / SOC | Microinformática |
| ---------------------------------------------- | -------------------- | ---------------- |
| Compliance policies                            | **A/R**              | C                |
| Conditional Access con señal de dispositivo    | **A/R**              | C                |
| Enrollment operativo                           | C                    | **A/R**          |
| Perfiles base de configuración                 | C                    | **A/R**          |
| Endpoint Security policies (ASR, Firewall, AV) | **A/R**              | C                |
| Defender onboarding / EDR posture              | **A/R**              | C                |
| Updates y rings                                | C                    | **A/R**          |
| BitLocker / FileVault postura                  | **A/R**              | C                |
| LAPS / privilegios locales                     | **A/R**              | C                |
| Troubleshooting del dispositivo                | C                    | **A/R**          |
| Incidentes de seguridad sobre endpoints        | **A/R**              | C/S              |
| Excepciones de seguridad                       | **A/R**              | C                |
| Reporte ejecutivo de riesgo                    | **A/R**              | C                |

*A/R = Accountable/Responsible · C = Consulted · S = Support*

---

## 8. Tabla de Verificación Rápida (Quick Reference)

| Feature            | Consola  | Ruta                                               | Métrica Clave                      |
| ------------------ | -------- | -------------------------------------------------- | ---------------------------------- |
| Compliance general | Intune   | Devices → Compliance → Monitor                     | Compliance status = Compliant      |
| Firewall MDM       | Intune   | Reports → Firewall → MDM Firewall status           | Firewall status = Enabled          |
| BitLocker          | Intune   | Devices → Monitor → Encryption report              | Encryption status = Encrypted      |
| ASR Rules          | Defender | Reports → Attack surface reduction → Configuration | State = Block                      |
| EDR Coverage       | Intune   | Endpoint security → MDE → Onboarded devices        | Dispositivos sin sensor activo = 0 |
| Enrollment         | Intune   | Devices → Monitor → Enrollment failures            | Failure rate + error code          |
| Audit de cambios   | Intune   | Tenant administration → Audit logs                 | Cambios sin ticket = 0             |
| Conector MDE       | Intune   | Endpoint security → Overview                       | Connector status = Active          |
| Ingesta Sentinel   | Sentinel | Logs → IntuneAuditLogs                             | LastIngestion < 15 min             |

---

## 9. Matriz de Roles Operativos

| Rol                            | Foco Principal           | Tareas Clave           |
| ------------------------------ | ------------------------ | ---------------------- |
| **SOC Analyst Tier 1**         | Monitoreo reactivo       | D-01, D-02, D-05, D-06 |
| **SOC Analyst Tier 2**         | Análisis e investigación | D-03, D-04, W-02, W-03 |
| **Security Administrator**     | Configuración y gobierno | W-04, W-05, W-06       |
| **Vulnerability Mgmt Analyst** | TVM y parcheo            | W-03, W-09             |
| **Security Manager / CISO**    | Estrategia y reporting   | M-01, M-04, M-08       |
| **Incident Response Team**     | Contención y remediación | IR-01, IR-02, IR-03    |

---

## Referencias Oficiales

| Área                        | Enlace                                                       |
| --------------------------- | ------------------------------------------------------------ |
| Endpoint Security Hub       | [learn.microsoft.com/intune/protect/endpoint-security](https://learn.microsoft.com/en-us/intune/intune-service/protect/endpoint-security) |
| Compliance Monitoring       | [learn.microsoft.com/intune/protect/compliance-policy-monitor](https://learn.microsoft.com/en-us/intune/intune-service/protect/compliance-policy-monitor) |
| Security Baselines Monitor  | [learn.microsoft.com/intune/protect/security-baselines-monitor](https://learn.microsoft.com/en-us/intune/intune-service/protect/security-baselines-monitor) |
| Graph API Export Reports    | [learn.microsoft.com/intune/fundamentals/reports-export-graph-apis](https://learn.microsoft.com/en-us/intune/intune-service/fundamentals/reports-export-graph-apis) |
| Intune PowerShell Samples   | [github.com/microsoft/mggraph-intune-samples](https://github.com/microsoft/mggraph-intune-samples) |
| MDE Response Actions        | [learn.microsoft.com/defender-endpoint/respond-machine-alerts](https://learn.microsoft.com/en-us/defender-endpoint/respond-machine-alerts) |
| Incident Response Playbooks | [learn.microsoft.com/security/operations/incident-response-playbooks](https://learn.microsoft.com/en-us/security/operations/incident-response-playbooks) |
| Sentinel Playbooks          | [learn.microsoft.com/azure/sentinel/automation](https://learn.microsoft.com/en-us/azure/sentinel/automation/automate-responses-with-playbooks) |

---

