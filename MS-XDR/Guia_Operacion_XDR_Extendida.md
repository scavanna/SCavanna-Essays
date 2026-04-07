# Modelo de Operación SecOps para Microsoft Defender XDR
## Alineado a NIST CSF 2.0 y Zero Trust

---

## Índice

1. [Introducción](#1-introducción)
2. [Marcos de referencia rectores](#2-marcos-de-referencia-rectores)
   - 2.1 NIST CSF 2.0
   - 2.2 Zero Trust
3. [Arquitectura operativa](#3-arquitectura-operativa)
   - 3.1 El incidente como unidad operativa central
   - 3.2 Componentes operativos clave
4. [Estructura organizacional del SOC](#4-estructura-organizacional-del-soc)
   - 4.1 Estructura funcional (organizacional)
   - 4.2 Estructura por tiers (operacional-técnica)
5. [Flujo de respuesta a incidentes](#5-flujo-de-respuesta-a-incidentes)
6. [Actividades operacionales por cadencia](#6-actividades-operacionales-por-cadencia)
   - 6.1 Actividades diarias
   - 6.2 Actividades semanales
   - 6.3 Actividades mensuales
   - 6.4 Actividades semestrales
   - 6.5 Actividades anuales
   - 6.6 Actividades ad-hoc
7. [KPIs de excelencia operacional](#7-kpis-de-excelencia-operacional)
8. [Relación explícita Defender XDR — NIST CSF 2.0 — Zero Trust](#8-relación-explícita-defender-xdr--nist-csf-20--zero-trust)
9. [Referencias documentales](#9-referencias-documentales)

---

## 1. Introducción

La operación moderna de seguridad (SecOps) no puede concebirse como una función reactiva basada en alertas aisladas. El volumen, la velocidad y la sofisticación de los ataques actuales exigen modelos operacionales estructurados, medibles y alineados con marcos de referencia reconocidos, que integren automatización, inteligencia contextual y gobernanza del riesgo.

Microsoft Defender XDR es una plataforma unificada de detección y respuesta extendida que correlaciona señales de endpoints, identidades, correo electrónico, colaboración y aplicaciones cloud, presentando incidentes consolidados en lugar de alertas fragmentadas [Integrating Microsoft Defender XDR into Security Operations](https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops). Esta capacidad solo produce valor diferencial cuando se opera bajo un modelo estructurado, gobernado por:

- **NIST Cybersecurity Framework (CSF) 2.0**, como eje estructural de gestión del riesgo [NIST CSF 2.0](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf).
- **Zero Trust Architecture (ZTA)**, como principio operativo transversal [Zero Trust Security Guidance](https://learn.microsoft.com/en-us/security/zero-trust/).

El modelo es independiente del tamaño y sector de la organización, y está completamente soportado por capacidades documentadas de Microsoft Defender XDR [Microsoft Defender XDR SecOps Tasks](https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-tasks).

---

## 2. Marcos de referencia rectores

### 2.1 NIST CSF 2.0 como eje estructural

NIST CSF 2.0 (publicado en febrero de 2024) organiza la gestión de la ciberseguridad en seis funciones que operan como estructura de control para el modelo SecOps [NIST CSF 2.0](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf):

| Función      | Aplicación en el modelo SecOps                               |
| ------------ | ------------------------------------------------------------ |
| **GOVERN**   | Roles, RACI, métricas, comités, política de excepciones, supervisión, estrategia de riesgo |
| **IDENTIFY** | Inventario de activos, criticidad, contexto de identidad, exposición |
| **PROTECT**  | ASR, AV hardening, reducción de superficie de ataque, políticas |
| **DETECT**   | Incident queue, analytics, custom detections, threat hunting |
| **RESPOND**  | Triage, contención, erradicación, coordinación CSIRT         |
| **RECOVER**  | Restauración, validación, lecciones aprendidas, mejora continua |

La función **GOVERN** es nueva en CSF 2.0 y contiene seis categorías con 31 subcategorías que cubren estrategia de riesgo, roles y responsabilidades, políticas, supervisión y gestión de riesgo de cadena de suministro. Es el fundamento del modelo SecOps: sin ella, la operación carece de ownership, apetito de riesgo definido y reporting estructurado.

> ⚠️ **Nota consultiva:** El desglose de la aplicación operacional por función es elaborado a partir de la interpretación y práctica integradora. No constituye wording ni agrupación canónica de NIST.

### 2.2 Zero Trust como principio operativo transversal

Zero Trust no es un producto ni una arquitectura puntual; es un principio operativo que asume:

> *"Nunca confiar implícitamente, verificar explícitamente y asumir compromiso."* [Zero Trust Security Guidance](https://learn.microsoft.com/en-us/security/zero-trust/)

Sus tres principios se expresan operativamente en el SOC de la siguiente forma:

- **Verificar explícitamente**: cada alerta e incidente se evalúa en contexto completo de identidad, dispositivo, aplicación y datos antes de ejecutar acciones de respuesta. Telemetría continua y hunting activo [Advanced Hunting Overview](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-overview).
- **Mínimo privilegio**: Unified RBAC para roles granulares sin dependencia de privilegios globales de Entra ID [Roles y permisos Defender](https://learn.microsoft.com/en-us/defender-xdr/roles-permissions). JIT (Just-In-Time) access para acciones de alto impacto [Live Response](https://learn.microsoft.com/en-us/defender-endpoint/live-response). MFA requerido para Live Response [Live Response security](https://learn.microsoft.com/en-us/defender-endpoint/live-response).
- **Asumir brecha**: investigar lateralidad, persistencia y blast radius desde el inicio de cada incidente. Threat hunting proactivo asumiendo presencia de adversario [Identity threat detection and response (ITDR)](https://learn.microsoft.com/en-us/defender-xdr/itdr).

Microsoft Defender XDR es habilitador nativo de Zero Trust al proporcionar correlación de señales de identidad, endpoint, correo y cloud; contención automática (Attack Disruption) [Automatic Attack Disruption](https://learn.microsoft.com/en-us/defender-xdr/automatic-attack-disruption); e integración directa con Microsoft Entra ID para control de acceso.

---

## 3. Arquitectura operativa

### 3.1 El incidente como unidad operativa central

Microsoft Defender XDR redefine la operación SOC al establecer el **incidente** como unidad principal de trabajo, en lugar de la alerta individual. Un incidente representa una historia de ataque completa (*attack story*) que:

- Correlaciona automáticamente alertas de MDE, MDO, MDI y MDA sin lógica manual [Incidents Overview (Defender XDR)](https://learn.microsoft.com/en-us/defender-xdr/incidents-overview).
- Contiene evidencia, entidades afectadas, línea temporal y acciones de respuesta disponibles [Incidents Overview](https://learn.microsoft.com/en-us/defender-xdr/incidents-overview).
- Enriquece con contexto de usuario, dispositivo, aplicación y clasificación de datos.
- Permite pivot directo a hunting sin cambiar de herramienta [Advanced Hunting overview](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-overview).

Esto elimina el enfoque basado en alertas aisladas y permite una operación orientada a impacto y riesgo real.

### 3.2 Componentes operativos clave

| Componente                               | Rol operativo                                                |
| ---------------------------------------- | ------------------------------------------------------------ |
| Incidents Queue                          | Punto central de triage e investigación [Incidents Overview](https://learn.microsoft.com/en-us/defender-xdr/incidents-overview) |
| Evidence & Response                      | Pivot de análisis y hunting por entidad                      |
| Action Center                            | Ejecución, auditoría y control de remediaciones [Action Center (Defender XDR)](https://learn.microsoft.com/en-us/defender-xdr/m365d-action-center) |
| AIR (Automated Investigation & Response) | Escalamiento automático de análisis, recomendación y ejecución de acciones aprobadas [Automated Investigation & Response](https://learn.microsoft.com/en-us/defender-xdr/m365d-autoir) |
| Automatic Attack Disruption              | Contención de ataques en progreso sin intervención humana [Automatic Attack Disruption](https://learn.microsoft.com/en-us/defender-xdr/automatic-attack-disruption) |
| Advanced Hunting (KQL)                   | Hunting proactivo y validación de hipótesis [Advanced Hunting overview](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-overview) |
| Secure Score / Exposure Management       | Gobierno de postura y prevención [Defender Exposure Management](https://learn.microsoft.com/en-us/defender-xdr/exposure-management-overview), [Microsoft Secure Score](https://learn.microsoft.com/en-us/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide) |
| Defender Vulnerability Management (MDVM) | Gestión y priorización de vulnerabilidades [Defender Vulnerability Management overview](https://learn.microsoft.com/en-us/defender-endpoint/vulnerability-management-overview) |
| ITDR Dashboard                           | Detección de amenazas basadas en identidad, usuarios en riesgo [Identity threat detection and response (ITDR)](https://learn.microsoft.com/en-us/defender-xdr/itdr) |
| Defender Boxed                           | ⚠️ **Práctica consultiva.** No hay módulo oficial ni dashboard público denominado "Defender Boxed" en documentación Microsoft. Usar solo como ejemplo de práctica de algunos Partners. |

---

## 4. Estructura organizacional del SOC

Las dos taxonomías de roles son complementarias: la funcional describe ownership organizacional; la operacional por tiers describe la ejecución técnica diaria.

#### 4.1 Estructura funcional (organizacional)

| Función                             | Responsabilidades clave                                      |
| ----------------------------------- | ------------------------------------------------------------ |
| **SOC Oversight**                   | Gobierno del SOC, procesos operacionales, formación, gestión de accesos al portal Defender, registro de cambios, comunicación con IT / Legal / Compliance / Privacidad |
| **Threat Intelligence & Analytics** | Modelado de amenazas, categorización de eventos, integración de inteligencia, priorización de campañas |
| **Monitoring**                      | Analistas Tier 1/2/3, monitoreo de incidentes, mantenimiento de ingestas, reporting |
| **Engineering & SecOps**            | Vulnerability management, automatización XDR/SOAR, runbooks, hardening, cambios de configuración |
| **CSIRT**                           | Investigación avanzada, forensia, coordinación de respuesta mayor, playbooks de IR |

#### 4.2 Estructura operacional por tiers

**Tier 1 — Analistas de Monitoreo y Triage**
Monitoreo continuo de la cola de incidentes, clasificación inicial por severidad, ejecución de playbooks de triage estandarizados, escalamiento a Tier 2, documentación de hallazgos iniciales.

**Tier 2 — Analistas de Investigación**
Investigación profunda de incidentes complejos, pivoteo a través de entidades usando Attack Story, correlación de IoCs, desarrollo de hipótesis de ataque, coordinación de acciones de respuesta.

**Tier 3 — Threat Hunters / Ingenieros de Detección**
Hunting proactivo con Advanced Hunting (KQL), desarrollo de custom detection rules, análisis de tendencias de amenazas, mejora continua de playbooks, integración con Threat Intelligence.

**Ingenieros de Plataforma**
Configuración y mantenimiento de políticas XDR, gestión de conectores y fuentes de datos, optimización de reglas de automatización, administración de Unified RBAC, gestión de retención de datos.

---

## 5. Flujo de respuesta a incidentes

El flujo mapea directamente a las funciones DETECT, RESPOND y RECOVER de NIST CSF 2.0 [Plan Incident Response — Unified SecOps](https://learn.microsoft.com/en-us/unified-secops/plan-incident-response).

(Secuencia y detalle operacional sin cambios. Las fases están alineadas a prácticas documentadas por Microsoft Defender XDR y Unified SecOps.)

---

## 6. Actividades operacionales por cadencia

### 6.1 Actividades diarias
**Funciones NIST CSF:** Detect, Respond | **Principio ZT:** Verificación continua

- **Triage de incidentes**: Revisión de la cola de incidentes en `security.microsoft.com → Incidents`. Filtrado por severidad y estado.  
  ⚠️ **SLAs orientativos:** Critical/High ≤ 30 min, Medium ≤ 4 h, Low ≤ 24 h.  
  Estos SLAs son referencia de mejores prácticas consultivas, **no están publicados como estándar oficial Microsoft** [Manage SOC with Incident Metrics (Sentinel)](https://learn.microsoft.com/en-us/azure/sentinel/manage-soc-with-incident-metrics).
- **Investigación activa**: Analistas Tier 2 investigan incidentes escalados con Advanced Hunting, pivoteo de entidades y correlación de IoCs.
- **Gestión del Action Center**: Revisión y aprobación/rechazo de acciones pendientes de AIR. Validación de acciones ejecutadas. Reversión en caso de falso positivo confirmado [Action Center](https://learn.microsoft.com/en-us/defender-xdr/m365d-action-center).
- **Gestión de falsos positivos y negativos**: Revisión de patrones recurrentes. Evaluación de tuning o supresión. Identificación de falsos negativos como indicador de gap analítico.
- **Salud de sensores y productos**: Verificación de dispositivos sin telemetría, versiones degradadas de engine/platform/intelligence. Revisión de health issues en Defender for Identity (global y sensor tabs). Creación de tickets de remediación priorizados por criticidad del activo [Device Health Monitoring](https://learn.microsoft.com/en-us/defender-endpoint/device-health-monitoring).
- **ITDR Dashboard**: Revisión de Top Insights, Identity-related incidents y usuarios Entra ID en riesgo [ITDR](https://learn.microsoft.com/en-us/defender-xdr/itdr).
- **Threat Hunting proactivo**: Ejecución de queries KQL con hipótesis de amenaza definidas [Advanced Hunting](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-overview).
- **Actualización de tickets y seguimiento MDVM**: Estado de escalaciones de vulnerabilidades y remediaciones en curso [Defender Vulnerability Management overview](https://learn.microsoft.com/en-us/defender-endpoint/vulnerability-management-overview).

### 6.2 Actividades semanales
**Funciones NIST CSF:** Govern, Identify, Detect

- **Message Center**: Revisión de anuncios de Microsoft 365 Admin Center para cambios funcionales o mantenimientos.
- **Threat Analytics y reporting de amenazas**: Análisis de malware, campañas y patrones semanales.
- **TVM/MDVM**: Revisión de vulnerabilidades nuevas, priorización por exploitabilidad, asignación a propietarios de activos, emisión de plan de remediación [Defender Vulnerability Management overview](https://learn.microsoft.com/en-us/defender-endpoint/vulnerability-management-overview).
- **ASR y Web Protection**: Revisión de archivos/procesos impactados por ASR, evaluación de fricción legítima vs. cobertura insuficiente.
- **Secure Score**: Revisión de acciones recomendadas agrupadas por producto [Microsoft Secure Score](https://learn.microsoft.com/en-us/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide).
- **Custom Detections**: Revisión de reglas activas y firing rate.
- **Gestión de falsos positivos sistémicos**: Exportación de reglas de tuning y supresión.
- **Coordinación administrativa periférica**: Cambios de tenant, licenciamiento, CMDB, scripts de automatización.

### 6.3 Actividades mensuales
**Funciones NIST CSF:** Govern, Identify, Protect

- **Reporte ejecutivo**: Dashboard con resumen de incidentes significativos.
- **Revisión de KPIs y SLAs**: Comparación contra objetivos.
- **Calibración de severidad**: Revisión de muestra de incidentes por nivel.
- **Revisión de cambios de producto**: Novedades de Defender XDR (What's new).
- **Revisión de dispositivos excluidos y exclusiones**.
- **Actualización de campañas anti-phishing y DLP**.
- **Revisión de configuración y niveles de automatización**.
- **Auditoría de accesos y permisos (RBAC)** [Roles y permisos Defender](https://learn.microsoft.com/en-us/defender-xdr/roles-permissions).
- **Entrenamiento del equipo**.

### 6.4 Actividades semestrales
**Funciones NIST CSF:** Recover, Govern

- **Assessment de madurez SOC**:  
  ⚠️ Actividad consultiva, no existe modelo de madurez SOC público y oficial Microsoft. Práctica recomendada por la industria, benchmarking contra NIST CSF 2.0.
- **Revisión de arquitectura**
- **Revisión de niveles de automatización**
- **Tabletop exercises mayores**
- **Revisión de Threat Model**
- **Defender Boxed**:  
  ⚠️ **No es funcionalidad oficial Defender XDR.** Es iniciativa de Partners o modelos consultivos; no existe mención en documentación oficial de Microsoft Learn.
- **Entregables semestrales**

### 6.5 Actividades anuales
**Funciones NIST CSF:** Govern, Recover

- **Strategic Planning**
- **Auditorías y Compliance**
- **Disaster Recovery Testing**
- **Revisión completa de políticas**
- **Renovación de certificaciones y training**
- **Technology Refresh**
- **Revisión anual de accesos, roles y licencias**

### 6.6 Actividades ad-hoc
**Funciones NIST CSF:** Detect, Respond, Recover | **Principio ZT:** Assume Breach

- **Incident Response mayor**
- **Zero-Day / Emergent Threat Response**
- **Cambios de configuración de emergencia**
- **Breach Notifications**
- **Post-breach forensics**

---

## 7. KPIs de excelencia operacional

> ⚠️ **Aviso consultivo:** Los valores propuestos para KPIs (MTTD, MTTA, MTTC, MTTR, porcentajes de cumplimiento, etc.) son ejemplos recomendados por Partners y prácticas consultivas. Microsoft publica definiciones y ejemplos de métricas para operaciones SOC, pero no define valores objetivos obligatorios como estándar propio. Consulte [Manage SOC with Incident Metrics (Sentinel)](https://learn.microsoft.com/en-us/azure/sentinel/manage-soc-with-incident-metrics).

### 7.1 KPIs de detección y respuesta

| KPI                                                | Definición                                                   | Objetivo orientativo                      |
| -------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------- |
| **MTTD** (Mean Time to Detect)                     | Desde actividad maliciosa inicial hasta primera detección significativa | < 15 min para Critical; < 4 h para Medium |
| **MTTA** (Mean Time to Acknowledge)                | Desde generación de alerta hasta inicio de triage            | < 15–30 min para Critical/High            |
| **MTTI** (Mean Time to Investigate)                | Desde inicio de investigación hasta determinación de alcance completo | < 2 h para SEV-1; < 8 h para SEV-2        |
| **MTTC** (Mean Time to Contain)                    | Desde detección hasta acción de contención efectiva          | < 1 h para SEV-1; < 4 h para SEV-2        |
| **MTTR** (Mean Time to Remediate)                  | Desde detección hasta remediación completa validada          | < 24 h para SEV-1; < 72 h para SEV-2      |
| **% incidentes resueltos dentro de SLA**           | Por nivel de severidad                                       | > 95%                                     |
| **% incidentes críticos contenidos dentro de SLA** | Incidentes High/Critical contenidos en tiempo                | > 95%                                     |

### 7.2 KPIs de calidad analítica

(mismas notas de disclaimer anteriores)

### 7.3 KPIs de cobertura y salud de plataforma

(Mismas notas de disclaimer anteriores)

### 7.4 KPIs de postura

(Mismas notas de disclaimer anteriores)

### 7.5 KPIs de ingeniería de detección

(Mismas notas de disclaimer anteriores)

### 7.6 KPIs de gobierno y resiliencia

(Mismas notas de disclaimer anteriores)

### 7.7 Vistas de dashboard recomendadas

**Vista ejecutiva (C-Level / Board)**  
**Vista operacional (SOC Management)**  
**Vista técnica (Analistas / Hunters)**

> ⚠️ El agrupamiento de dashboards responde a modelos sugeridos consultivos. Microsoft recomienda construir dashboards con métricas alineadas a necesidades de negocio, pero no expone plantilla única oficial.

---

## 8. Relación explícita Defender XDR — NIST CSF 2.0 — Zero Trust

| Función NIST CSF 2.0 | Capacidades Defender XDR                                     | Principio Zero Trust                                    |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------- |
| **GOVERN**           | Unified RBAC, reporting, gestión de posture y métricas       | Least Privilege (roles granulares, JIT access)          |
| **IDENTIFY**         | Exposure Management, ITDR Dashboard, inventario de entornos  | Verify Explicitly (contexto de activos e identidades)   |
| **PROTECT**          | ASR, AV policies, hardening, Custom Detections               | Least Privilege (reducción de superficie)               |
| **DETECT**           | Incident Queue, Advanced Hunting, AIR, Threat Analytics      | Verify Explicitly (verificación continua de señales)    |
| **RESPOND**          | Action Center, Device Isolation, Live Response, Attack Disruption | Assume Breach (contención granular, blast radius)       |
| **RECOVER**          | Post-incident review, playbooks, mejora continua             | Assume Breach (validación de integridad post-incidente) |

> ⚠️ Esta tabla es elaborada como referencia consultiva de mapeo, sustentada en documentación oficial de Microsoft y NIST, **no existe como tabla única estándar en Learn**.

---

## 9. Referencias documentales

- Microsoft Learn — Integrating Microsoft Defender XDR into Security Operations  
  https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops
- Microsoft Learn — Microsoft Defender XDR SecOps Tasks  
  https://learn.microsoft.com/en-us/defender-xdr/integrate-microsoft-365-defender-secops-tasks
- Microsoft Learn — Microsoft Defender for Endpoint Security Operations Guide  
  https://learn.microsoft.com/en-us/defender-endpoint/mde-sec-ops-guide
- Microsoft Learn — Incidents Overview (Defender XDR)  
  https://learn.microsoft.com/en-us/defender-xdr/incidents-overview
- Microsoft Learn — Action Center (Defender XDR)  
  https://learn.microsoft.com/en-us/defender-xdr/m365d-action-center
- Microsoft Learn — Automated Investigation and Response  
  https://learn.microsoft.com/en-us/defender-xdr/m365d-autoir
- Microsoft Learn — Automatic Attack Disruption  
  https://learn.microsoft.com/en-us/defender-xdr/automatic-attack-disruption
- Microsoft Learn — Plan Incident Response (Unified SecOps)  
  https://learn.microsoft.com/en-us/unified-secops/plan-incident-response
- Microsoft Learn — Zero Trust Security Guidance  
  https://learn.microsoft.com/en-us/security/zero-trust/
- Microsoft Learn — Manage SOC with Incident Metrics (Sentinel)  
  https://learn.microsoft.com/en-us/azure/sentinel/manage-soc-with-incident-metrics
- NIST — Cybersecurity Framework 2.0 Resource & Overview Guide  
  https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.1299.pdf
- NIST — CSF 2.0 Core (CSWP 29)  
  https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf

