> **Nota de Verificación Técnica:** Este documento ha sido auditado contra documentación oficial de Microsoft.
> - ✅ Claims verificados incluyen referencias inline a Microsoft Learn/Docs.
> - ⚠️ Contenido marcado como "análisis consultivo" no constituye estándar oficial Microsoft.
> - 📊 Métricas, scores o niveles sin referencia explícita han sido eliminados o reformulados.

## Índice

1. [Resumen ejecutivo y justificación del modelo](#1-resumen-ejecutivo)
2. [Principios rectores](#2-principios-rectores)
3. [Fundamento técnico: telemetría, logs y retención](#3-telemetría-logs-y-retención)
4. [Roles operativos y artefactos mínimos](#4-roles-operativos-y-artefactos)
5. [Cadencia Diaria](#5-cadencia-diaria)
6. [Cadencia Semanal](#6-cadencia-semanal)
7. [Cadencia Mensual](#7-cadencia-mensual)
8. [Cadencia Semestral / Anual](#8-cadencia-semestral--anual)
9. [Actividades Ad-Hoc: triage y respuesta a incidentes](#9-actividades-ad-hoc)
10. [KPIs para excelencia operacional](#10-kpis)
11. [Referencias oficiales Microsoft](#11-referencias)

---

## 1. Resumen ejecutivo

Microsoft Entra ID es el **plano de control de identidad** en entornos híbridos y multinube [Security operations introduction (Entra ID)](https://learn.microsoft.com/entra/architecture/security-operations-introduction). Comprometer una identidad privilegiada o una aplicación con permisos excesivos puede equivaler a comprometer el negocio entero [Security operations privileged accounts](https://learn.microsoft.com/entra/architecture/security-operations-privileged-accounts). Microsoft posiciona Entra ID dentro de la estrategia **Zero Trust** y **Defense in Depth** [Zero Trust](https://learn.microsoft.com/security/zero-trust/), [Defense in Depth](https://learn.microsoft.com/security/compass/defense-in-depth), enfatizando monitoreo continuo, políticas adaptativas y capacidad de detección y respuesta.

El modelo SecOps de Entra ID no se limita a administrar usuarios. Articula cinco dimensiones permanentes [Security operations introduction (Entra ID)](https://learn.microsoft.com/entra/architecture/security-operations-introduction):

1. **Protección preventiva** (controles, MFA, CA, PIM)
2. **Detección** (ID Protection, sign-in logs, audit logs, Entra Health)
3. **Triage e investigación** (cola de incidentes, correlación, clasificación)
4. **Contención y remediación** (runbooks, playbooks, acciones en portal)
5. **Mejora continua** (Secure Score, recomendaciones, access reviews, KPIs)

El modelo se apoya en tres capas técnicas [Security operations introduction (Entra ID)](https://learn.microsoft.com/entra/architecture/security-operations-introduction):

- **Telemetría y evidencia**: audit logs, sign-in logs, provisioning logs, ID Protection, integración con Azure Monitor / Sentinel / SIEM, retención.
- **Controles y gobierno**: Conditional Access, MFA, Identity Protection, PIM, Access Reviews, Identity Secure Score.
- **Respuesta operacional**: incidentes Defender XDR / Sentinel, playbooks (Logic Apps), Microsoft Graph API.

---

## 2. Principios rectores

| #    | Principio                         | Descripción                                                  |
| ---- | --------------------------------- | ------------------------------------------------------------ |
| 1    | **Zero Trust**                    | Verificar explícitamente, usar mínimo privilegio, asumir compromiso. Entra ID es el control plane [Zero Trust - Microsoft docs](https://learn.microsoft.com/security/zero-trust/). |
| 2    | **Least privilege + JIT**         | Privilegios mínimos activados solo cuando se necesitan (PIM) con revisiones frecuentes [PIM Best practices](https://learn.microsoft.com/entra/id-governance/best-practices-secure-id-governance). |
| 3    | **Deny by default**               | Acceso solo con autorización explícita; aplica especialmente en entitlement management y aprovisionamiento [Entitlement Management - Best practices](https://learn.microsoft.com/entra/id-governance/best-practices-secure-id-governance). |
| 4    | **Baseline primero**              | Definir normalidad antes de buscar anomalías, para reducir falsos positivos. |
| 5    | **Automatización y trazabilidad** | Todos los cambios críticos auditados y exportados al SIEM [Opciones de integración de logs](https://learn.microsoft.com/entra/identity/monitoring-health/concept-log-monitoring-integration-options-considerations). |
| 6    | **Resiliencia operativa**         | MFA universal, control de autenticación heredada, autenticación resistente a fallas on-premises [Ops guide auth](https://learn.microsoft.com/entra/architecture/ops-guide-auth). |
| 7    | **Medición continua**             | KPIs, Secure Score, tiempos de atención, calidad de triage, reducción sostenida del riesgo [Identity Secure Score](https://learn.microsoft.com/entra/identity/monitoring-health/concept-identity-secure-score). |

---

## 3. Telemetría, logs y retención

### 3.1 Tipos de logs en Entra ID

| Log                   | Descripción                                                  | Uso principal                                  |
| --------------------- | ------------------------------------------------------------ | ---------------------------------------------- |
| **Sign-in logs**      | Intentos de acceso (interactivos, no interactivos, service principals) | Patrones, errores, análisis de acceso, hunting |
| **Audit logs**        | Cambios y acciones administrativas / de configuración        | Cambios de roles, CA, apps, PIM, usuarios      |
| **Provisioning logs** | Actividades del servicio de aprovisionamiento                | Ciclo de vida, errores de sincronización       |

### 3.2 Acceso a logs (opciones)

(todos los modos de acceso validados en [Acceso a activity logs](https://learn.microsoft.com/entra/identity/monitoring-health/howto-access-activity-logs), [Integrar logs con Azure Monitor](https://learn.microsoft.com/entra/identity/monitoring-health/howto-integrate-activity-logs-with-azure-monitor-logs))

### 3.3 Retención por licencia (implicaciones operativas)

| Nivel de licencia       | Retención sign-in logs  | Retención risky sign-ins | Retención audit logs |
| ----------------------- | ----------------------- | ------------------------ | -------------------- |
| Free                    | 7 días                  | —                        | 7 días               |
| P1                      | 30 días                 | 30 días                  | 30 días              |
| P2                      | 30 días                 | 90 días                  | 30 días              |
| Azure Monitor / Storage | Configurable (>90 días) | Configurable             | Configurable         |

> **Implicación operativa**: Si SecOps requiere investigación forense más allá de los límites nativos, es necesario exportar o integrar los logs hacia Azure Monitor, SIEM o Event Hub antes del vencimiento de la retención nativa [Retención de datos Entra](https://learn.microsoft.com/entra/identity/monitoring-health/reference-reports-data-retention).

---

## 4. Roles operativos y artefactos mínimos

⚠️ **Nota consultiva**: La siguiente distribución de roles y artefactos mínimos es una propuesta basada en prácticas operativas comunes. No representa un estándar o requerimiento oficial de Microsoft, ni existe una tabla de roles mínima obligatoria [Security operations introduction (Entra ID)](https://learn.microsoft.com/entra/architecture/security-operations-introduction). Adaptar según la estructura y gobernanza interna.

---

## 5. Cadencia Diaria

### 5.1 Triage de incidentes en cola unificada (Defender XDR / Sentinel)

1. Abrir cola de incidentes en Defender portal o Sentinel.
2. Ordenar por **severidad** y última actualización; priorizar High/Medium activos.
3. Por cada incidente: revisar timeline, entidades afectadas (usuarios/apps/dispositivos) y evidencia.
4. Clasificar preliminarmente: TP sospechoso → L2 / FP → cerrar con documentación / Needs more data → enriquecer con playbook.
5. Asignar dueño y SLA.

### 5.2 Revisión del tablero de riesgo (ID Protection)

1. Abrir **Entra ID Protection > Risky users**.
2. Revisar summary y gráfico de nuevos usuarios riesgosos por día (tendencia).
3. Para usuarios con riesgo High: revisar timeline de eventos, historial de sign-ins, activos accedidos.
4. Ejecutar acción según permisos y evidencia: `Confirm compromise`, `Dismiss risk`, forzar reset de credencial, revocar sesiones, bloquear inicio de sesión.
5. Revisar **Risky sign-ins** y **Risk detections** para patrones: atypical travel, anonymous IP, password spray, token anomalies, malware-linked IP, suspicious inbox rules.
6. Documentar decisión y escalar si hay indicios de movimiento lateral.

⚠️ **Nota editorial**: La revisión del Identity Secure Score puede ser diaria (monitoreo de cambios del tablero) o semanal (gestión analítica de recomendaciones y backlog).

### 5.3 Revisión de sign-in logs (patrones y errores)
### 5.4 Revisión de audit logs (cambios sensibles de las últimas 24 h)
### 5.5 Revisión de PIM: alertas y activaciones
### 5.6 Revisión de Entra Health
### 5.7 Revisión diaria de Entra Recommendations e Identity Secure Score
### 5.8 Salud de componentes híbridos (si aplica)

(Todas las actividades validadas en documentación: [Entra ID Protection](https://learn.microsoft.com/entra/id-protection/), [Sign-in logs](https://learn.microsoft.com/entra/identity/monitoring-health/concept-sign-ins), [Audit logs](https://learn.microsoft.com/entra/identity/monitoring-health/concept-audit-logs))

---

## 6. Cadencia Semanal

### 6.1 Revalidación del baseline y ajuste de "noise"
### 6.2 Revisión de impacto de Conditional Access (Insights workbook)
### 6.3 Higiene semanal de PIM
### 6.4 Revisión de amenazas emergentes y adaptación de detecciones
### 6.5 Revisión semanal de Identity Secure Score y recomendaciones

(Todos los flujos son recomendaciones integradas y no constituyen requerimiento operativo estándar de Microsoft. Referencias: [Conditional Access insights workbook](https://learn.microsoft.com/entra/identity/conditional-access/howto-conditional-access-insights-reporting), [PIM security alerts](https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-how-to-configure-security-alerts))

---

## 7. Cadencia Mensual

### 7.1 Campañas de Access Reviews
### 7.2 Higiene de aplicaciones y consentimientos
### 7.3 Revisión de autenticación heredada
### 7.4 Revisión de alertas ajustadas y tuning de detecciones
### 7.5 Verificación de retención e integridad de logs
### 7.6 Revisión de métricas y reporte mensual de postura

⚠️ **Nota consultiva:** Los flujos y métricas sugeridas deben adaptarse, pues Microsoft muestra tendencias y porcentajes en sus dashboards, pero los valores no representan estándar oficial.

---

## 8. Cadencia Semestral / Anual

### 8.1 Revisión estratégica del modelo de acceso (semestral)
### 8.2 Revisión estructural de postura: Identity Secure Score (semestral)
### 8.3 Ejercicios de respuesta y tabletop (semestral)
### 8.4 Auditoría de configuraciones críticas (semestral)
### 8.5 Backup y recuperación de configuración (anual)
### 8.6 Penetration testing y red teaming (anual)
### 8.7 Planificación de capacidad y adopción de nuevas capacidades (anual)
### 8.8 Actualización de documentación y entrenamiento (anual)

⚠️ **Nota consultiva:** Los ejercicios Red Team/tabletop, backup y recuperación, y certificaciones recomendadas son buenas prácticas, pero no son requerimientos oficiales de Microsoft. [Learn certification docs](https://learn.microsoft.com/certifications/)

---

## 9. Actividades Ad-Hoc: triage y respuesta a incidentes

### 9.1 Flujo general de triage
### 9.2 Runbook: Usuario riesgoso (Risky User)
### 9.3 Runbook: Actividad privilegiada sospechosa (PIM / Privileged roles)
### 9.4 Runbook: Cambio no autorizado en configuración
### 9.5 Runbook: Degradación / anomalía de autenticación (Entra Health)
### 9.6 Automatización ad-hoc (Sentinel Playbooks / Logic Apps)
### 9.7 Cierre y aprendizaje post-incidente

⚠️ **Nota consultiva:** Los tiempos de ejecución, flujos de runbook, y distribución de responsabilidades son consultivos y deben ser definidos por la organización.

---

## 10. KPIs para excelencia operacional

⚠️ **Nota importante:** Los valores de KPIs, umbrales (">95% MFA", "<5 Global Admins permanentes") son recomendaciones consultivas, NO estándar oficial Microsoft. [Identity Secure Score](https://learn.microsoft.com/entra/identity/monitoring-health/concept-identity-secure-score). Microsoft recomienda tendencia positiva en Secure Score, amplia cobertura MFA y minimización de privilegios permanentes, pero no publica cortes o valores normativos. Adapte los valores a la realidad de la organización.

---

## 11. Referencias oficiales Microsoft

| Tema                                            | URL                                                          |
| ----------------------------------------------- | ------------------------------------------------------------ |
| Security operations introduction (Entra ID)     | https://learn.microsoft.com/entra/architecture/security-operations-introduction |
| SecOps: user accounts                           | https://learn.microsoft.com/entra/architecture/security-operations-user-accounts |
| SecOps: privileged accounts                     | https://learn.microsoft.com/entra/architecture/security-operations-privileged-accounts |
| SecOps: PIM                                     | https://learn.microsoft.com/entra/architecture/security-operations-privileged-identity-management |
| Entra ID Protection                             | https://learn.microsoft.com/entra/id-protection/             |
| Risky user report                               | https://learn.microsoft.com/entra/id-protection/concept-risky-user-report |
| Investigate risk (framework)                    | https://github.com/MicrosoftDocs/entra-docs/blob/main/docs/id-protection/howto-identity-protection-investigate-risk.md |
| Sign-in logs (concept)                          | https://learn.microsoft.com/entra/identity/monitoring-health/concept-sign-ins |
| Acceso a activity logs                          | https://learn.microsoft.com/entra/identity/monitoring-health/howto-access-activity-logs |
| Integrar logs con Azure Monitor                 | https://learn.microsoft.com/entra/identity/monitoring-health/howto-integrate-activity-logs-with-azure-monitor-logs |
| Stream a Event Hub                              | https://learn.microsoft.com/entra/identity/monitoring-health/howto-stream-logs-to-event-hub |
| Opciones de integración de logs                 | https://learn.microsoft.com/entra/identity/monitoring-health/concept-log-monitoring-integration-options-considerations |
| Retención de datos Entra                        | https://learn.microsoft.com/entra/identity/monitoring-health/reference-reports-data-retention |
| Conditional Access insights workbook            | https://learn.microsoft.com/entra/identity/conditional-access/howto-conditional-access-insights-reporting |
| Conditional Access report-only mode             | https://learn.microsoft.com/entra/identity/conditional-access/concept-conditional-access-report-only |
| Plan de despliegue de Conditional Access        | https://learn.microsoft.com/entra/identity/conditional-access/plan-conditional-access |
| PIM security alerts                             | https://learn.microsoft.com/entra/id-governance/privileged-identity-management/pim-how-to-configure-security-alerts |
| Access Reviews deployment                       | https://learn.microsoft.com/entra/id-governance/deploy-access-reviews |
| Identity Secure Score                           | https://learn.microsoft.com/entra/identity/monitoring-health/concept-identity-secure-score |
| Entra Recommendations                           | https://learn.microsoft.com/entra/identity/monitoring-health/overview-recommendations |
| Entra Health monitoring                         | https://learn.microsoft.com/entra/identity/monitoring-health/concept-microsoft-entra-health |
| Investigar alertas Entra Health                 | https://learn.microsoft.com/entra/identity/monitoring-health/howto-investigate-health-scenario-alerts |
| Sentinel playbooks (Logic Apps)                 | https://learn.microsoft.com/azure/sentinel/automation/logic-apps-playbooks |
| Incidents en Defender portal                    | https://learn.microsoft.com/defender-xdr/incidents-overview  |
| Defender XDR Incidents API                      | https://learn.microsoft.com/defender-xdr/api-incident        |
| Best practices seguridad identidad (Governance) | https://learn.microsoft.com/entra/id-governance/best-practices-secure-id-governance |
| Operational best practices (Azure Security)     | https://learn.microsoft.com/azure/security/fundamentals/operational-best-practices |
| Ops guide intro (Entra)                         | https://learn.microsoft.com/entra/architecture/ops-guide-intro |
| Ops guide auth                                  | https://learn.microsoft.com/entra/architecture/ops-guide-auth |

