# Nivel 2 — Conceptos Clave (5 Nodos del Ecosistema)

## Nodo Central: Microsoft Sentinel (SIEM/SOAR)

Sentinel es el punto de convergencia principal del ecosistema. Sus capacidades incluyen:

- Ingesta y correlación de telemetría desde múltiples fuentes a través de conectores de datos, empleando reglas analíticas avanzadas para generar incidentes priorizados.
- Automatización SOAR mediante playbooks implementados sobre Logic Apps para respuesta, aislamiento, bloqueo, notificación y enriquecimiento.
- Funcionalidades de threat hunting con KQL, notebooks Jupyter y búsquedas ad-hoc.
- Métricas personalizadas para MTTD y MTTR vía workbooks y recomendaciones automáticas para optimización  [Manage your SOC better with incident metrics in Microsoft Sentinel | Microsoft Learn](https://learn.microsoft.com/en-us/azure/sentinel/manage-soc-with-incident-metrics)
- Auditorías detalladas mediante tablas dedicadas y logs de acción de los analistas [Create Workbooks for Microsoft Sentinel Solutions | Microsoft Learn](https://learn.microsoft.com/en-us/azure/sentinel/sentinel-workbook-creation#create-a-workbook)  /  [Visualize your data using workbooks in Microsoft Sentinel | Microsoft Learn](https://learn.microsoft.com/en-us/azure/sentinel/monitor-your-data?tabs=defender-portal)

Actividades SOC soportadas: 20 de 40 actividades, incluyendo operación diaria, semanal, mensual y ad-hoc.

## Nodo de Gobierno: Microsoft Purview

Purview se compone de cuatro sub-capacidades estratégicas:

- Audit: Centraliza todas las acciones en Microsoft 365; versión premium amplía capacidad de retención y eventos para forense (https://learn.microsoft.com/en-us/purview/audit-solutions-overview).
- Data Lifecycle Management: Políticas para retención/eliminación automatizada de datos (https://learn.microsoft.com/en-us/purview/data-lifecycle-management).
- Records Management: Declaración de registros, disposición bajo aprobaciones formales y legal hold (https://learn.microsoft.com/en-us/purview/records-management).
- DLP: Prevención de pérdida de datos, integración con Sentinel mediante conexión explícita [Data Loss Prevention (DLP) scenarios for the Australian Government - PSPF | Microsoft Learn](https://learn.microsoft.com/en-us/compliance/anz/pspf-dlp-overview#dlp-deployment-considerations)

Actividades SOC soportadas: 14 de 40, destacando acciones de auditoría, gestión de evidencia y cumplimiento.

## Nodo de Detección Extendida: Microsoft Defender XDR

Defender XDR aporta telemetría contextual enriquecida y capacidades de contención:

- Defender for Endpoint: Detección y control en dispositivos (https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint?view=o365-worldwide).
- Defender for Identity: Señales sobre identidades y posibles desplazamientos laterales.
- Defender for Office 365: Protección contra amenazas en correo y archivos adjuntos.
- Defender for Cloud Apps: Visibilidad y control de apps en la nube.
- Ejecución directa de contención desde el portal unificado 
- Defiende la integración con Sentinel para flujos de datos y acciones de respuesta 

Soporta triage, gestión de incidentes y contención directa.

## Nodo de Identidad: Microsoft Entra ID

Entra ID es la plataforma central de identidad:

- Define y gestiona autenticaciones y accesos condicionales, alimentando tanto a Sentinel como a Defender for Identity.
- Implementa Privileged Identity Management (PIM) con registro de auditoría de accesos privilegiados [Plan a Privileged Identity Management deployment - Microsoft Entra ID Governance | Microsoft Learn](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-deployment-plan#overview)
- Define roles RBAC para Sentinel, Purview y Defender, con auditoría de todos los cambios [Roles and role groups in Microsoft Defender for Office 365 and Microsoft Purview - Microsoft Defender for Office 365 | Microsoft Learn](https://learn.microsoft.com/en-us/defender-office-365/scc-permissions)

No asigna actividades SOC directas, pero habilita trazabilidad y control de privilegios en todo el ecosistema.

## Nodo de Convergencia: Microsoft Defender Portal

Defender Portal es la interfaz unificada de operación:

- Consolida todos los incidentes de Sentinel y Defender XDR en una sola cola para investigación y respuesta (https://learn.microsoft.com/en-us/microsoft-365/security/defender/microsoft-365-defender-portal?view=o365-worldwide).
- Facilita investigación integrada y coordinada, combinando información de endpoints, identidad y automatización.
- Demanda actualización de los procedimientos y runbooks de SOC bajo su adopción.

Refuerza la integración operacional y elimina esfuerzo manual, pero no crea capacidades técnicas nuevas (https://learn.microsoft.com/en-us/microsoft-365/security/defender/microsoft-365-defender-portal?view=o365-worldwide).

# Nivel 3 — Notas de Soporte / Insights

## Mapa Completo de Actividades por Herramienta

**Microsoft Sentinel (20 actividades):**

| **Cadencia** | **Actividades**                          | **Foco**                                    |
| ------------ | ---------------------------------------- | ------------------------------------------- |
| Diaria       | D-01, D-02, D-04, D-05, D-08, D-09       | Incidentes, triage, ingesta, playbooks, SLA |
| Semanal      | S-01, S-02, S-04, S-05, S-06, S-07, S-10 | Reglas, umbrales, cobertura, métricas, SOAR |
| Mensual      | M-01, M-02, M-04, M-07                   | Postura, MITRE, auditoría, costos           |
| Ad-Hoc       | AH-08                                    | Contención y erradicación                   |

**Microsoft Purview (14 actividades):**

| **Cadencia** | **Actividades**                          | **Foco**                                                   |
| ------------ | ---------------------------------------- | ---------------------------------------------------------- |
| Diaria       | D-03, D-06, D-07                         | Auditoría, cambios admin, DLP                              |
| Semanal      | S-03, S-08                               | Eventos Purview, accesos privilegiados                     |
| Mensual      | M-03, M-04, M-08                         | Retención, auditoría, cumplimiento                         |
| Ad-Hoc       | AH-01, AH-02, AH-03, AH-04, AH-05, AH-07 | Forense, legal hold, records, políticas, auditoría externa |

**Ambas plataformas (6 actividades):**

| **Cadencia** | **Actividades**     | **Integración clave**                                 |
| ------------ | ------------------- | ----------------------------------------------------- |
| Diaria       | D-07                | DLP alert → Sentinel incident correlation             |
| Mensual      | M-06, M-10          | NIST assessment + Executive report                    |
| Ad-Hoc       | AH-04, AH-06, AH-09 | Exfiltración, escalamiento legal, evidencia combinada |

**Actividades organizacionales (4):**

| **Cadencia** | **Actividades** | **Naturaleza**                                 |
| ------------ | --------------- | ---------------------------------------------- |
| Diaria       | D-10            | Escalamiento al CISO (proceso, no herramienta) |
| Semanal      | S-09            | Documentación operativa (repositorio SOC)      |
| Mensual      | M-05, M-09      | PIR y tabletop (ejercicios de equipo)          |

## Flujos de Datos entre Nodos

| **Flujo**               | **Origen → Destino**                          | **Tipo**                   | **Actividades**  |
| ----------------------- | --------------------------------------------- | -------------------------- | ---------------- |
| Telemetría de endpoint  | Defender for Endpoint → Sentinel              | Detección                  | D-01, D-02, S-01 |
| Telemetría de identidad | Defender for Identity + Entra ID → Sentinel   | Acceso, señales            | D-01, D-02, S-08 |
| Telemetría de email     | Defender for Office 365 → Sentinel            | Alerta phishing            | D-01, D-02       |
| Alertas DLP             | Purview DLP → Sentinel                        | Incidentes correlacionados | D-07, AH-04      |
| Acciones de contención  | Sentinel (playbook) → Defender XDR            | Respuesta                  | D-05, AH-08      |
| Auditoría cruzada       | Unified Audit Log → Purview Audit/Sentinel    | Trazabilidad               | D-03, D-06, M-04 |
| Logs de identidad       | Entra ID → Unified Audit Log                  | Registro de accesos        | S-08, M-04       |
| Evidencia forense       | Purview Audit Premium → Equipo Legal          | Exportación evidencia      | AH-01, AH-07     |
| Métricas operacionales  | Sentinel Incident Metrics → Reporte Ejecutivo | Reporting                  | S-05, M-10       |
| Métricas de gobierno    | Purview Content Explorer → Reporte Ejecutivo  | Reporting                  | M-03, M-10       |

## Prerequisito de Licenciamiento Microsoft 365 E5

| **Componente**                  | **Incluido en E5**   | **Impacto si ausente**                              |
| ------------------------------- | -------------------- | --------------------------------------------------- |
| Microsoft Sentinel              | No (licencia aparte) | Sin SIEM/SOAR — modelo inviable                     |
| Defender XDR completo           | Sí                   | Sin telemetría rica, datos parciales                |
| Purview Audit Premium           | Sí                   | Sin retención extendida/forense de alto valor       |
| Purview DLM + Records Mgmt Adv. | Sí                   | Sin disposición defensible ni automación legal hold |
| Purview DLP                     | Sí                   | Sin alertas de exfiltración integradas              |
| Entra ID P2 (PIM)               | Sí                   | Sin acceso just-in-time/auditoría privilegios       |

Sin E5, el modelo se degrada: se limitan capacidades de gobierno, forense extendida y automatización avanzada (https://www.microsoft.com/en-us/microsoft-365/compare-microsoft-365-enterprise-plans).

## Evolución del Ecosistema — Tendencias Activas

- **Unificación Defender Portal:** Migración continua a experiencia integrada para analistas y runbooks.
- **Copilot for Security:** Integración futura de IA generativa en investigación y reporte.
- **DSPM:** Purview avanza hacia gestión integral de riesgo y gobierno del dato.

## Lectura del Mapa por Rol

- **Analista L1/L2:** Enfoque en Sentinel, Defender XDR y Portal.
- **Analista L2/L3/Purview:** Suma Purview para auditar y gestionar evidencia.
- **Team Lead:** Visión global enfocada en flujos y correcta integración.
- **CISO:** Evaluación de cobertura, madurez (NIST), métricas y valor de inversión.