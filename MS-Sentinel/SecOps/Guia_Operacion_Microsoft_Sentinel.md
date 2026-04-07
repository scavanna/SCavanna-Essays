# Modelo de Operación SecOps para Microsoft Sentinel
## Guía Consolidada: Actividades, RACI, KPIs y Alineación con Marcos de Referencia

> **Nota metodológica**: Este documento consolida dos segmentos complementarios sobre el
> mismo modelo operativo. El contenido normativo y las referencias a documentación oficial
> de Microsoft están respaldados por:
> - Guía operativa oficial: [`learn.microsoft.com/azure/sentinel/ops-guide`](https://learn.microsoft.com/azure/sentinel/ops-guide)
> - Guía SIEM-XDR: [`learn.microsoft.com/security/operations/siem-xdr-overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview)
> - Página MITRE ATT&CK de Sentinel: [`learn.microsoft.com/azure/sentinel/mitre-coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage)
> - Gobernanza NIST CSF 2.0 (Iniciativa Secure Future): [`learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance`](https://learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance)
> - Guía de respuesta a incidentes: [`learn.microsoft.com/security/operations/incident-response-overview`](https://learn.microsoft.com/security/operations/incident-response-overview)

---

## Índice

1. [Descripción general del modelo](#1-descripción-general-del-modelo)
2. [Principios operativos](#2-principios-operativos)
3. [Modelo de roles](#3-modelo-de-roles)
4. [Actividades diarias](#4-actividades-diarias)
5. [Actividades semanales](#5-actividades-semanales)
6. [Actividades mensuales](#6-actividades-mensuales)
7. [Actividades semestrales y anuales](#7-actividades-semestrales-y-anuales)
8. [Actividades ad-hoc: triage y respuesta a incidentes](#8-actividades-ad-hoc-triage-y-respuesta-a-incidentes)
9. [Indicadores clave de desempeño (KPIs)](#9-indicadores-clave-de-desempeño-kpis)
10. [Alineación con marcos de referencia](#10-alineación-con-marcos-de-referencia)
11. [Referencias oficiales](#11-referencias-oficiales)

---

## 1. Descripción general del modelo

Microsoft Sentinel es la solución **SIEM** (Security Information and Event Management)
y **SOAR** (Security Orchestration, Automation and Response) nativa de la nube de
Microsoft [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide). Se integra de forma nativa con Microsoft Defender XDR, Microsoft Entra ID,
Azure y el ecosistema complementario de seguridad de Microsoft [`SIEM-XDR Overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview).

La guía operativa oficial establece que los equipos de operaciones de seguridad (SOC)
y administradores de seguridad deben planificar y ejecutar rutinas estructuradas como
parte de sus actividades periódicas. El modelo aplica tanto a **Microsoft Sentinel en
el portal de Microsoft Defender** como a **Microsoft Sentinel en Azure Portal**
[`Feature Availability`](https://learn.microsoft.com/azure/sentinel/feature-availability).

El modelo se organiza en cinco cadencias:

| Cadencia            | Actividades principales                                      |
| ------------------- | ------------------------------------------------------------ |
| **Diaria**          | Triage e investigación de incidentes; hunting; gestión de reglas analíticas; monitoreo de conectores; verificación del AMA; revisión de fallos en playbooks |
| **Semanal**         | Revisión de actualizaciones del Content Hub; auditoría de cambios en recursos de Sentinel; reunión de control operacional |
| **Mensual**         | Revisión de permisos de usuarios e inactivos; verificación de política de retención del workspace; revisión de ingeniería de detecciones |
| **Semestral/Anual** | Análisis de cobertura MITRE ATT&CK; autoevaluación NIST CSF 2.0; simulacros de respuesta a incidentes; revisión de arquitectura y estrategia Zero Trust |
| **Ad-hoc**          | Triage acelerado; investigación con Advanced Hunting; contención; erradicación; análisis post-incidente |

> ⚠️ **Nota de alcance**: La guía operativa oficial de Microsoft Sentinel
> ([`learn.microsoft.com/azure/sentinel/ops-guide`](https://learn.microsoft.com/azure/sentinel/ops-guide)) cubre actividades hasta frecuencia
> mensual. Las actividades semestrales y anuales se derivan de guías complementarias
> de Microsoft sobre gobernanza NIST CSF 2.0 y cobertura MITRE ATT&CK, y representan
> prácticas esenciales para la madurez del programa SecOps, pero no forman parte del
> `ops-guide` directamente.

---

## 2. Principios operativos

Microsoft documenta los siguientes principios que sustentan el modelo:

- **Incidents-first**: el incidente es la unidad central de operación.
- **Automation-assisted SOC**: el analista es aumentado por automatización, no reemplazado.
- **Metrics-driven operations**: toda actividad es medible; los KPIs guían la mejora continua.
- **Threat-informed defense**: MITRE ATT&CK como taxonomía común de tácticas y técnicas (versión 18) [`MITRE Coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage).
- **Continuous optimization**: reglas, ingestión de datos y costos se optimizan de forma iterativa.

Estos principios se implementan mediante la combinación operativa de Sentinel y
Defender XDR, y se alinean con los tres principios de **Zero Trust** [`SIEM-XDR Overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview):

| Principio Zero Trust               | Implementación en Sentinel + Defender XDR                    |
| ---------------------------------- | ------------------------------------------------------------ |
| **Verificación explícita**         | Sentinel recopila datos de todo el entorno y analiza amenazas y anomalías; Defender XDR provee detección y respuesta extendidas. Se configuran automatizaciones para actuar sobre señales de riesgo capturadas por Defender XDR. |
| **Acceso con privilegios mínimos** | Sentinel detecta actividades anómalas mediante UEBA. Microsoft Entra ID Protection bloquea usuarios según riesgo de identidad y alimenta datos a Sentinel para análisis y automatización adicionales. |
| **Asumir la intrusión**            | Defender XDR examina continuamente el entorno; Sentinel analiza tendencias de comportamiento para detectar amenazas de varias fases (APT). Ambos implementan tareas de corrección automatizadas. El riesgo de dispositivo se utiliza como señal para Microsoft Entra Conditional Access. |

---

## 3. Modelo de roles

**Notación RACI**: R = Responsible (ejecuta) · A = Accountable (responde por el resultado; único por actividad) · C = Consulted (aporta input) · I = Informed (recibe notificación)

| Rol                                    | Descripción operacional                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| **SOC L1**                             | Primera línea: monitoreo, triage inicial, enriquecimiento, escalamiento |
| **SOC L2**                             | Investigación profunda, coordinación de contención, retroalimentación de tuning |
| **SOC L3 / Detection Engineer**        | Investigación avanzada, ingeniería de detecciones, optimización KQL, automatización |
| **SOC Manager**                        | Gobernanza, ownership de SLAs, gestión de personal, reportes ejecutivos |
| **Platform Engineer**                  | Salud de la plataforma, conectores, AMA, workspace, ciclo de vida de contenido |
| **Threat Hunter**                      | Hunts proactivos, análisis por hipótesis, retroalimentación purple team |
| **Identity / Entra Admin**             | Controles de identidad, revocación de tokens, remediación de cuentas privilegiadas |
| **Endpoint / Intune / Defender Admin** | Aislamiento de endpoints, remediación, postura de dispositivos |
| **Cloud / Infrastructure Ops**         | Remediación en servidores, redes, Azure, aplicaciones        |
| **GRC / Risk / Compliance**            | Mapeo de controles, evidencia regulatoria, soporte a auditoría |
| **Business / Asset Owner**             | Validación de impacto en negocio, aprobación de decisiones de servicio, coordinación de remediación |

**Patrón de asignación de responsabilidades por dimensión**:

| Dimensión                                             | Rol accountable                             |
| ----------------------------------------------------- | ------------------------------------------- |
| Velocidad (triage y primer contacto)                  | L1                                          |
| Calidad de investigación y coordinación de contención | L2                                          |
| Calidad de detecciones y resolución técnica profunda  | L3 / Detection Engineer                     |
| Continuidad de telemetría y salud de plataforma       | Platform Engineer                           |
| SLAs, gobernanza y mejora continua                    | SOC Manager                                 |
| Remediación de dominio (identidad / endpoint / cloud) | Identity Admin / Endpoint Admin / Cloud Ops |
| Descubrimiento proactivo y cobertura ATT&CK           | Threat Hunter                               |
| Evidencia regulatoria y mapeo de controles a CSF      | GRC                                         |

---

## 4. Actividades diarias

La guía operativa oficial establece seis categorías de tareas diarias obligatorias.

### 4.1 Triage e investigación de incidentes

**Objetivo**: detectar, priorizar y determinar acción sobre los incidentes generados
por las reglas analíticas configuradas.

**Procedimiento**:
1. Acceder a la página **Incidents** de Sentinel; ordenar por severidad (`Critical > High > Medium > Low > Informational`) y tiempo de creación.
2. Para cada incidente: revisar título, descripción, alertas correlacionadas y entidades involucradas (cuentas, IPs, hosts, archivos, URLs).
3. Visualizar el alcance mediante el **grafo de incidentes** y construir la cronología de eventos.
4. Evaluar validez: determinar si es **verdadero positivo (TP)**, **falso positivo (FP)** o **benigno positivo (BP)**. Los FP se cierran con clasificación documentada.
5. Investigar con herramientas contextuales: paneles de información general de Sentinel y workbooks como **Investigation Insights**.
6. Ejecutar consultas de hunting para buscar indicadores de riesgo adicionales y la causa raíz.
7. Analizar comportamiento de entidades con **UEBA** (usuarios, hosts, IPs, aplicaciones).
8. Consultar **watchlists** para enriquecer la investigación (rangos IP corporativos, usuarios VIP, empleados desvinculados).
9. Asignar propietario, cambiar estado a `Active` y documentar hallazgos con comentarios detallados.

**RACI — Triage e investigación**:
[Referencias inline por cada actividad: [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide)]

| Actividad                                     | L1   | L2   | L3   | SOC Mgr | Platform Eng | Identity | Endpoint | Cloud | Threat Hunter | Biz Owner |
| --------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | -------- | -------- | ----- | ------------- | --------- |
| Monitorear cola de incidentes                 | R    | C    | I    | A       | I            | I        | I        | I     | I             | I         |
| Asignar ownership del incidente               | R    | C    | I    | A       | I            | I        | I        | I     | I             | I         |
| Triage inicial y revisión de falsos positivos | R    | A    | C    | I       | I            | I        | I        | I     | C             | I         |
| Revisar grafo, entidades, alertas, logs       | R    | A    | C    | I       | I            | C        | C        | C     | C             | I         |
| Escalar incidentes sospechosos o confirmados  | R    | A    | C    | I       | I            | C        | C        | C     | C             | I         |
| Documentar evidencia y notas del caso         | R    | A    | C    | I       | I            | I        | I        | I     | I             | I         |

### 4.2 Hunting y bookmarks

**Objetivo**: garantizar que el SOC no dependa exclusivamente de alertas predefinidas
y pueda descubrir amenazas emergentes de forma proactiva [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide).

**Procedimiento**:
1. Acceder a la sección **Hunting** de Sentinel y ejecutar las consultas predefinidas organizadas por táctica MITRE ATT&CK [`MITRE Coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage).
2. Analizar resultados para identificar eventos sospechosos no cubiertos por reglas analíticas.
3. Crear **bookmarks** para hallazgos notables.
4. Generar incidentes manuales o actualizar incidentes existentes si la evidencia confirma actividad maliciosa.

**RACI — Hunting y bookmarks**:

| Actividad                                       | L1   | L2   | L3   | SOC Mgr | Platform Eng | Threat Hunter |
| ----------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | ------------- |
| Ejecutar consultas de hunting predefinidas      | C    | R    | C    | I       | I            | A             |
| Validar resultados de hunting                   | C    | R    | C    | I       | I            | A             |
| Crear / actualizar bookmarks                    | C    | R    | C    | I       | I            | A             |
| Convertir hallazgos en detecciones o incidentes | I    | R    | A    | I       | C            | C             |

### 4.3 Gestión de reglas analíticas y tuning

**Objetivo**: asegurar que las detecciones cubran las amenazas actuales y operen sin errores.

Sentinel ofrece dos tipos de reglas de detección:
- **Reglas programadas (Scheduled)**: escritas por expertos de seguridad de Microsoft, buscan cadenas de actividad sospechosa en los logs recopilados [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide).
- **Reglas casi en tiempo real (NRT)**: diseñadas para ejecutarse cada minuto, proporcionan información lo más actualizada posible.

**Procedimiento**:
1. Revisar nuevas plantillas de reglas disponibles y habilitarlas si añaden cobertura útil.
2. Monitorear el estado de salud y optimizar la ejecución de reglas existentes.
3. Ajustar umbrales, ventanas de tiempo o condiciones de correlación en reglas que generen falsos positivos recurrentes.
4. Confirmar que las reglas NRT se ejecutan puntualmente sin errores.

**RACI — Gestión de reglas analíticas**:

| Actividad                                          | L1   | L2   | L3   | SOC Mgr | Platform Eng |
| -------------------------------------------------- | ---- | ---- | ---- | ------- | ------------ |
| Identificar reglas ruidosas desde el triage diario | R    | A    | C    | I       | I            |
| Revisar integridad y fallos de reglas              | I    | C    | A    | I       | R            |
| Recomendar cambios de tuning                       | C    | R    | A    | I       | C            |
| Habilitar nuevas reglas relevantes                 | I    | C    | A    | I       | R            |

### 4.4 Monitoreo de conectores de datos, AMA y playbooks

**Objetivo**: garantizar que el flujo de ingestión de logs esté operativo, la conectividad de endpoints al workspace sea correcta y las automatizaciones de respuesta funcionen sin errores [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide).

**Procedimiento — Conectores**:
1. Verificar el estado de cada conector en **Data connectors**. Los conectores críticos (Entra ID, Microsoft 365 Defender, Azure Activity, firewalls, proxies) deben mostrar `Connected` con ingestión reciente.
2. Identificar conectores en estado `Disconnected` o con caída en volumen de datos (variaciones >20–30% respecto a la línea base) e investigar la causa.
3. Evaluar nuevos conectores disponibles para ampliar la visibilidad del entorno.

**Procedimiento — AMA**:
1. Comprobar que los hosts están conectados activamente al workspace de Log Analytics [`Azure Monitor Agent`](https://learn.microsoft.com/azure/azure-monitor/agents/azure-monitor-agent-overview).
2. Identificar y corregir conexiones fallidas del AMA.

**Procedimiento — Playbooks**:

Sentinel utiliza reglas de automatización y playbooks (Logic Apps) para gestionar la respuesta automatizada a amenazas, aumentando la eficacia del SOC y reduciendo el tiempo dedicado a tareas repetitivas [`Incident Response Overview`](https://learn.microsoft.com/security/operations/incident-response-overview).

1. Verificar los estados de ejecución de cada playbook activo.
2. Detectar y corregir errores (fallos de autenticación, tiempos de espera expirados, conectores inválidos).
3. Re-ejecutar manualmente playbooks críticos que hubieran fallado durante el turno.

**RACI — Conectores, AMA y playbooks**:

| Actividad                                         | L1   | L2   | L3   | SOC Mgr | Platform Eng | Cloud Ops |
| ------------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | --------- |
| Verificar salud de conectores de datos            | I    | C    | C    | I       | A/R          | C         |
| Verificar continuidad de ingestión                | I    | C    | C    | I       | A/R          | C         |
| Verificar salud del Azure Monitor Agent           | I    | I    | C    | I       | A            | R         |
| Revisar ejecuciones fallidas de playbooks         | I    | C    | R    | I       | A            | C         |
| Restaurar dependencias de automatización fallidas | I    | C    | R    | I       | A            | C         |

---

## 5. Actividades semanales

### 5.1 Ciclo de vida del contenido (Content Hub)

**Objetivo**: mantener actualizado el catálogo de detecciones, workbooks y automatizaciones [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide).

**Procedimiento**:
1. Consultar el **Content Hub** por nuevas versiones de soluciones instaladas y aplicar actualizaciones disponibles.
2. Explorar nuevas soluciones, plantillas de reglas analíticas, consultas de hunting, workbooks y playbooks relevantes para amenazas emergentes.
3. Evaluar aplicabilidad al entorno; probar e implementar contenido adicional documentando los cambios.
4. Crear backlog de adopción de contenido priorizado.

> Ejemplo relevante: la **solución Zero Trust (TIC 3.0)** del Content Hub incluye un
> workbook, reglas analíticas y un playbook con visualización automatizada de principios
> de confianza cero cruzada con el marco Trust Internet Connections.

**RACI — Content Hub**:

| Actividad                                         | L1   | L2   | L3   | SOC Mgr | Platform Eng | Threat Hunter |
| ------------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | ------------- |
| Revisar actualizaciones del Content Hub           | I    | C    | A    | I       | R            | C             |
| Evaluar nuevas soluciones y paquetes de contenido | I    | C    | A    | I       | R            | C             |
| Evaluar nuevas reglas, workbooks y playbooks      | I    | C    | A    | I       | R            | C             |
| Crear backlog de adopción de contenido            | I    | C    | R    | A       | R            | C             |

### 5.2 Auditoría y gobernanza de cambios

**Objetivo**: identificar cambios no autorizados o inesperados en configuraciones de la plataforma.

**Procedimiento**:
1. Extraer logs de auditoría para listar eventos de creación, actualización o eliminación de recursos (reglas analíticas, bookmarks, conectores, playbooks).
2. Identificar cambios no autorizados que puedan comprometer la cobertura de seguridad.
3. Verificar conformidad con el proceso de control de cambios y revertir modificaciones problemáticas.
4. Preservar evidencia de auditoría.

**RACI — Auditoría de cambios**:

| Actividad                                                 | L1   | L2   | L3   | SOC Mgr | Platform Eng | GRC  |
| --------------------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | ---- |
| Revisar cambios de recursos en Sentinel                   | I    | C    | C    | A       | R            | C    |
| Validar cambios contra solicitudes aprobadas              | I    | I    | C    | A       | R            | C    |
| Investigar actualizaciones o eliminaciones no autorizadas | I    | C    | C    | A       | R            | C    |
| Preservar evidencia de auditoría                          | I    | I    | I    | A       | R            | C    |

### 5.3 Reunión de control operacional semanal

| Actividad                                         | L1   | L2   | L3   | SOC Mgr | Platform Eng | GRC  | Biz Owner |
| ------------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | ---- | --------- |
| Revisar incumplimientos de SLA y backlog          | I    | C    | C    | A       | I            | C    | I         |
| Revisar incidentes mayores y tendencias           | I    | R    | C    | A       | I            | C    | I         |
| Priorizar tuning y correcciones de automatización | I    | C    | R    | A       | R            | I    | I         |
| Revisar riesgos sobre servicios críticos          | I    | C    | C    | A       | I            | C    | C         |

---

## 6. Actividades mensuales

### 6.1 Revisión del acceso de usuarios

**Objetivo**: implementar el principio de acceso con privilegios mínimos (Zero Trust) [`SIEM-XDR Overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview).

**Procedimiento**:
1. Listar los roles asignados en el workspace de Sentinel (Analyst, Responder, Contributor, etc.).
2. Detectar cuentas sin uso en el período (usuarios que ya no pertenecen al SOC o cuentas de servicio obsoletas).
3. Revocar accesos innecesarios o reducir privilegios excesivos.
4. Registrar evidencia del proceso para auditoría.

**RACI — Revisión de acceso**:

| Actividad                                 | L1   | L2   | L3   | SOC Mgr | Platform Eng | Identity Admin | GRC  |
| ----------------------------------------- | ---- | ---- | ---- | ------- | ------------ | -------------- | ---- |
| Revisar asignaciones de roles en Sentinel | I    | I    | C    | A       | R            | C              | C    |
| Identificar accesos inactivos o excesivos | I    | I    | C    | A       | R            | C              | C    |
| Eliminar permisos innecesarios            | I    | I    | I    | A       | R            | C              | C    |
| Registrar evidencia para auditoría        | I    | I    | I    | A       | R            | I              | C    |

### 6.2 Revisión del workspace de Log Analytics

**Objetivo**: asegurar alineación entre la política de retención de datos y los requisitos regulatorios; controlar costos de ingestión [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide).

**Procedimiento**:
1. Confirmar que la política de retención almacena los logs por el período requerido según normativas aplicables.
2. Para marcos que exigen conservar registros por períodos extendidos, configurar exportación a **Azure Data Explorer** u otros repositorios de largo plazo [`Azure Data Explorer`](https://learn.microsoft.com/azure/data-explorer/data-explorer-overview).
3. Revisar el volumen de datos ingeridos por fuente; identificar tablas de alto volumen y bajo valor de seguridad para ajustar ingestión o retención.

**RACI — Revisión de retención y costos**:

| Actividad                                                 | L1   | L2   | L3   | SOC Mgr | Platform Eng | Cloud Ops | GRC  |
| --------------------------------------------------------- | ---- | ---- | ---- | ------- | ------------ | --------- | ---- |
| Revisar configuración de retención                        | I    | I    | C    | A       | R            | C         | C    |
| Validar retención contra política                         | I    | I    | I    | A       | C            | I         | R    |
| Revisar crecimiento de ingestión y tablas de alto volumen | I    | I    | C    | A       | R            | C         | I    |
| Recomendar estrategia de optimización / archivo           | I    | I    | C    | A       | R            | C         | C    |

### 6.3 Revisión de ingeniería de detecciones

| Actividad                                        | L1   | L2   | L3   | SOC Mgr | Platform Eng | Threat Hunter |
| ------------------------------------------------ | ---- | ---- | ---- | ------- | ------------ | ------------- |
| Revisar detecciones más ruidosas / de bajo valor | C    | R    | A    | I       | C            | C             |
| Aprobar plan de tuning de reglas                 | I    | C    | R    | A       | C            | C             |
| Revisar problemas de performance de KQL          | I    | I    | A    | I       | R            | I             |
| Rastrear brechas de cobertura MITRE              | I    | C    | R    | I       | I            | A             |

---

## 7. Actividades semestrales y anuales

> ⚠️ **Nota de alcance**: Las actividades de esta sección se derivan de guías
> complementarias de Microsoft (gobernanza NIST CSF 2.0 e integración MITRE ATT&CK),
> no de la guía operativa de Sentinel directamente. Representan prácticas esenciales
> para la madurez del programa SecOps [`Integración NIST`](https://learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance).

### 7.1 Autoevaluación anual de madurez según NIST CSF 2.0

Microsoft recomienda una autoevaluación anual alineada con NIST CSF 2.0. Los componentes del proceso incluyen: alcance, estructura de evaluación, integración con gestión de riesgos, mejora continua [`Integración NIST`](https://learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance).

- **Alcance (Scoping)**: seleccionar unidades organizativas y servicios mediante un proceso de admisión basado en riesgo; revisar el alcance anualmente para reflejar cambios en el negocio y el panorama de amenazas.
- **Estructura de evaluación**: cuestionario estandarizado que evalúa madurez en las seis funciones del CSF: Govern, Identify, Protect, Detect, Respond y Recover.
- **Puntuación**: basada en los *implementation tiers* de NIST, que permiten benchmarking objetivo.
- **Integración con gestión de riesgos**: registrar riesgos y oportunidades clave en el Risk Register para priorización y seguimiento.
- **Mejora continua**: refinar la metodología para mejorar objetividad y reducir complejidad.

Acciones recomendadas por caso de uso:

| Caso de uso                  | Acción recomendada                                           |
| ---------------------------- | ------------------------------------------------------------ |
| Evaluación NIST CSF 2.0      | Definir alcance basado en prioridades organizacionales; usar ejemplos de implementación para establecer línea base de procesos |
| Integrar riesgo cyber en ERM | Agregar riesgos cibernéticos junto con riesgos financieros, operacionales y reputacionales; asignar gestores de riesgo; establecer criterios de escalamiento |
| Línea base de controles      | Mapear controles existentes a funciones del CSF; usar NIST SP 800-53 para líneas base; asignar responsables para remediar brechas |
| Proteger activos críticos    | Realizar análisis de impacto al negocio; identificar *crown jewels*; definir objetivos RTO/RPO; usar *tabletop exercises* para validar resiliencia |
| Fortalecer gobernanza        | Definir roles y responsabilidades; revisar y alinear políticas con requisitos legales y regulatorios |

### 7.2 Revisión semestral de cobertura MITRE ATT&CK

Sentinel proporciona una página MITRE ATT&CK nativa para visualizar la cobertura de detecciones activas. Sentinel está alineado con la **versión 18 de MITRE ATT&CK** [`MITRE Coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage).

**Procedimiento**:
1. Verificar la cobertura actual: las reglas Scheduled y NRT activas se indican en la matriz.
2. Seleccionar una técnica para ver detalles en el panel lateral: enlaces a reglas analíticas, consultas de hunting y descripción de la técnica. En el portal Defender, el panel también muestra la proporción de detecciones activas y servicios recomendados.
3. Simular la cobertura posible: la cobertura *simulada* muestra las detecciones disponibles pero no configuradas, revelando el estado de seguridad potencial si se activaran todas.
4. Identificar brechas (*gap analysis*): comparar cobertura real vs. simulada para priorizar técnicas con baja cobertura.
5. Implementar nuevas reglas analíticas o consultas de hunting enfocadas en las brechas identificadas.
6. Al crear consultas de hunting, seleccionar las tácticas y técnicas específicas para que los marcadores hereden el mapeo MITRE correctamente.

> Tener reglas Scheduled con técnicas MITRE aplicadas ejecutándose regularmente en el
> workspace mejora el estado de seguridad mostrado en la matriz de cobertura MITRE de
> la organización.

Áreas ATT&CK de alta prioridad para operaciones con Sentinel:
Initial Access · Credential Access · Persistence · Defense Evasion · Lateral Movement · Command and Control · Exfiltration [`MITRE Coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage).

**RACI — Revisión estratégica y cobertura MITRE**:

| Actividad                                       | L3   | Threat Hunter | SOC Mgr | GRC  | Identity | Endpoint | Cloud |
| ----------------------------------------------- | ---- | ------------- | ------- | ---- | -------- | -------- | ----- |
| Revisar inventario de casos de uso              | A    | R             | C       | C    | C        | C        | C     |
| Mapear detecciones a MITRE ATT&CK               | R    | A             | I       | C    | I        | I        | I     |
| Mapear controles a NIST CSF 2.0                 | C    | C             | I       | A    | C        | C        | C     |
| Revisar brechas de alineación Zero Trust        | C    | C             | A       | R    | R        | R        | R     |
| Planificar nuevas detecciones de alta prioridad | A    | R             | C       | I    | C        | C        | C     |

### 7.3 Simulacros de respuesta a incidentes

Microsoft recomienda estas pruebas como parte de la protección de activos críticos [`Incident Response Overview`](https://learn.microsoft.com/security/operations/incident-response-overview).

**Procedimiento**:
1. Diseñar escenarios realistas (ransomware, compromiso de identidades, exfiltración de datos) que ejerciten los playbooks configurados.
2. Ejecutar la simulación con participación de equipos SecOps, TI y liderazgo.
3. Verificar paso a paso cómo se detectaría y respondería usando Sentinel y las herramientas complementarias.
4. Documentar hallazgos y actualizar procedimientos, playbooks y reglas de detección según las brechas identificadas.

**RACI — Simulacros y validación de resiliencia**:

| Actividad                                     | L2   | L3   | SOC Mgr | Platform Eng | Cloud Ops | Identity | GRC  | Biz Owner |
| --------------------------------------------- | ---- | ---- | ------- | ------------ | --------- | -------- | ---- | --------- |
| Ejecutar tabletop exercises                   | C    | C    | A       | I            | C         | C        | R    | C         |
| Ejecutar simulación técnica / purple team     | R    | A    | C       | C            | C         | C        | I    | I         |
| Validar comunicaciones del plan de respuesta  | I    | I    | A       | I            | I         | I        | R    | C         |
| Rastrear lecciones aprendidas hasta su cierre | C    | C    | A       | C            | C         | C        | R    | I         |

### 7.4 Revisión anual de arquitectura y estrategia Zero Trust

**Componentes de la revisión**:
- Evaluar si todas las fuentes de datos relevantes están integradas; considerar nuevas fuentes por cambios en la infraestructura o adopción de tecnologías.
- Revisar si la estructura del workspace de Log Analytics es óptima para el tamaño y la gobernanza de la organización.
- Evaluar la efectividad de integraciones con EDR, NDR, CASB e IAM y planificar integraciones adicionales [`SIEM-XDR Overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview).
- Proyectar crecimiento de ingestión de datos y planificar recursos apropiados.
- Planificar la transición hacia el **portal de Microsoft Defender** como interfaz unificada de operaciones (soporte de Sentinel en Azure Portal concluye después del 31 de marzo de 2027) [`Feature Availability`](https://learn.microsoft.com/azure/sentinel/feature-availability).

**RACI — Revisión estratégica de arquitectura**:

| Actividad                                       | SOC Mgr | Platform Eng | L3   | Cloud Ops | Identity Admin | GRC  |
| ----------------------------------------------- | ------- | ------------ | ---- | --------- | -------------- | ---- |
| Revisar arquitectura de Sentinel y roadmap      | A       | R            | C    | C         | C              | C    |
| Planificar transición al portal de Defender     | A       | R            | C    | I         | I              | I    |
| Revisar integración con Defender XDR            | A       | R            | C    | C         | C              | I    |
| Aprobar estrategia de telemetría de largo plazo | A       | R            | C    | C         | C              | C    |

---

## 8. Actividades ad-hoc: triage y respuesta a incidentes

### 8.1 Triage acelerado

**Procedimiento**:
1. Confirmar la severidad y validar que la alerta es verosímil.
2. Evaluar el impacto potencial: sistemas afectados, datos sensibles en riesgo, servicios de negocio impactados.
3. Si la alerta forma parte de un incidente con múltiples alertas correlacionadas, examinar todo el contexto en Sentinel.
4. Notificar a los miembros clave del SOC y stakeholders relevantes.
5. Determinar si es **falso positivo** o **verdadero positivo** y decidir la ruta de escalamiento.

**RACI — Triage ad-hoc**:

| Actividad                                        | L1   | L2   | L3   | SOC Mgr | Threat Hunter |
| ------------------------------------------------ | ---- | ---- | ---- | ------- | ------------- |
| Recibir y clasificar incidente                   | R    | A    | C    | I       | I             |
| Validar severidad e impacto en negocio           | R    | A    | C    | I       | C             |
| Determinar falso positivo vs. verdadero positivo | R    | A    | C    | I       | C             |
| Decidir ruta de escalamiento                     | R    | A    | C    | I       | I             |

### 8.2 Investigación exhaustiva

**Procedimiento**:
1. **Advanced Hunting**: explorar hasta 30 días de datos sin procesar con consultas KQL para búsqueda sin restricciones de amenazas conocidas y potenciales [`Ops Guide`](https://learn.microsoft.com/azure/sentinel/ops-guide).
2. **UEBA**: investigar el historial de comportamiento organizacional de las entidades involucradas (cuentas, hosts, IPs, aplicaciones).
3. **Motor Fusion**: verificar si el incidente forma parte de una cadena de ataque multi-fase detectada por el motor de correlación basado en machine learning.
4. **Threat Intelligence**: correlacionar IoCs (hashes, dominios, IPs) con la inteligencia importada de Microsoft y proveedores de terceros.
5. Construir la línea de tiempo completa del ataque: punto de entrada inicial, movimiento lateral, acciones del adversario.
6. Expandir el alcance a entidades relacionadas para identificar actividad maliciosa adicional no detectada inicialmente.

**RACI — Investigación**:

| Actividad                                          | L1   | L2   | L3   | Identity | Endpoint | Cloud | Threat Hunter |
| -------------------------------------------------- | ---- | ---- | ---- | -------- | -------- | ----- | ------------- |
| Pivotar entre logs y entidades                     | C    | A    | R    | C        | C        | C     | C             |
| Construir línea de tiempo del ataque               | I    | A    | R    | C        | C        | C     | C             |
| Expandir alcance a cuentas/hosts/apps relacionadas | I    | A    | R    | C        | C        | C     | C             |
| Identificar hipótesis de causa raíz                | I    | A    | R    | C        | C        | C     | C             |

### 8.3 Contención y respuesta inmediata

Capacidades disponibles en Sentinel + Defender XDR [`Incident Response Overview`](https://learn.microsoft.com/security/operations/incident-response-overview):

| Capacidad                                      | Descripción                                                  |
| ---------------------------------------------- | ------------------------------------------------------------ |
| **AIR (Automated Investigation and Response)** | Examina alertas y toma acción inmediata para resolver brechas; reduce el volumen de alertas para que el SOC se enfoque en amenazas más sofisticadas |
| **Aislamiento de dispositivos**                | Contención mediante aislamiento de red del dispositivo comprometido |
| **Live Response**                              | Acceso instantáneo a un dispositivo mediante conexión de shell remoto; permite investigación en profundidad y acciones de respuesta en tiempo real |
| **EDR block mode**                             | Protección adicional contra artefactos maliciosos cuando MDAV opera en modo pasivo |
| **Indicadores personalizados de archivos**     | Prevención de propagación prohibiendo archivos potencialmente maliciosos |
| **Indicadores personalizados de red**          | Bloqueo de IPs, URLs y dominios basado en inteligencia de amenazas propia |
| **Playbooks de respuesta automática**          | Reglas de automatización y playbooks que aumentan la eficacia del SOC y reducen tiempo y recursos |

Acciones de contención por vector:
- **Identidad comprometida**: deshabilitar cuenta en Entra ID, revocar tokens de sesión, restablecer MFA, validar detalles de MFA del usuario.
- **Endpoint comprometido**: aislar dispositivo mediante Defender for Endpoint.
- **Email malicioso**: eliminar artefactos maliciosos de buzones de correo.
- **Infraestructura cloud**: remediar compromisos de servidor o aplicación en coordinación con Cloud Ops.

**RACI — Contención y erradicación**:

| Actividad                                                  | L1   | L2   | L3   | SOC Mgr | Identity | Endpoint | Cloud | Biz Owner |
| ---------------------------------------------------------- | ---- | ---- | ---- | ------- | -------- | -------- | ----- | --------- |
| Recomendar estrategia de contención                        | I    | A    | R    | C       | C        | C        | C     | I         |
| Deshabilitar cuentas comprometidas / resetear credenciales | I    | C    | I    | I       | A/R      | I        | I     | I         |
| Revocar tokens / validar detalles de MFA                   | I    | C    | I    | I       | A/R      | I        | I     | I         |
| Aislar endpoint                                            | I    | C    | I    | I       | I        | A/R      | I     | I         |
| Remediar compromiso de servidor / app                      | I    | C    | C    | I       | I        | I        | A/R   | C         |
| Eliminar artefactos maliciosos de email                    | I    | R    | C    | I       | I        | I        | I     | I         |

> Microsoft describe los procedimientos de respuesta para endpoints, servidores,
> cuentas de usuario, cuentas de servicio y emails maliciosos, y enfatiza la elección
> entre el enfoque "clean as you go" y el enfoque "big bang" según la persistencia del
> atacante y el riesgo operacional [`Incident Response Overview`](https://learn.microsoft.com/security/operations/incident-response-overview).

### 8.4 Erradicación y recuperación

1. Eliminar artefactos maliciosos de los sistemas infectados.
2. Aplicar parches a vulnerabilidades explotadas y fortalecer configuraciones débiles detectadas.
3. Restaurar servicios afectados; re-incorporar equipos aislados tras verificar su limpieza.
4. Implementar monitoreo intensivo de los activos involucrados para detectar recurrencia.

### 8.5 Análisis posterior al incidente

1. Consolidar una cronología detallada: vector de ataque, técnicas (mapeadas a MITRE ATT&CK), sistemas afectados, acciones de respuesta e impacto.
2. Identificar la causa raíz: vulnerabilidades, debilidades de proceso o brechas de detección.
3. Implementar mejoras: agregar IoCs a la plataforma de Threat Intelligence, ajustar reglas de Sentinel, actualizar watchlists, corregir logging gaps.
4. Actualizar runbooks y procedimientos operativos.
5. Presentar un informe de lecciones aprendidas al management.

**RACI — Mejora post-incidente**:

| Actividad                                          | L2   | L3   | SOC Mgr | Platform Eng | Threat Hunter | GRC  |
| -------------------------------------------------- | ---- | ---- | ------- | ------------ | ------------- | ---- |
| Registrar IoCs en Threat Intelligence / watchlists | R    | A    | I       | C            | C             | I    |
| Identificar brechas de logging                     | C    | A    | I       | R            | C             | I    |
| Hacer tuning o crear nuevas detecciones            | C    | A    | I       | C            | C             | I    |
| Actualizar runbooks y procedimientos               | C    | R    | A       | C            | C             | I    |
| Documentar lecciones aprendidas                    | C    | C    | A       | I            | I             | R    |

---

## 9. Indicadores clave de desempeño (KPIs)

### 9.1 KPIs de eficiencia operacional del SOC

| KPI                                              | Descripción                                                  | Responsible | Accountable |
| ------------------------------------------------ | ------------------------------------------------------------ | ----------- | ----------- |
| **MTTA** (Mean Time to Triage)                   | Tiempo promedio desde la creación del incidente hasta su primera evaluación por un analista. Mide la velocidad de priorización inicial. Microsoft lo cita como medición clave de responsiveness del SOC. | L1 / L2     | SOC Manager |
| **MTTR / MTTC** (Mean Time to Respond / Closure) | Tiempo promedio desde la creación hasta el cierre del incidente. Mide la eficacia integral del proceso de investigación y respuesta. | L2          | SOC Manager |
| **Tasa de falsos positivos**                     | Porcentaje de incidentes clasificados como no amenazas reales. Una tasa alta indica necesidad de ajuste en reglas de detección. | L2 / L3     | L3          |
| **Distribución por severidad**                   | Proporción de incidentes por nivel (Alta, Media, Baja, Informacional). | L1 / L2     | SOC Manager |
| **Incidentes por clasificación de cierre**       | Resultado de incidentes investigados: TP / FP / BP. Indica la calidad de las detecciones. | L1 / L2     | SOC Manager |
| **Incidentes por táctica MITRE**                 | Distribución según tácticas ATT&CK para priorizar mejoras en áreas con mayor actividad adversaria. | L2 / L3     | SOC Manager |
| **Cumplimiento de SLA de incidentes**            | Porcentaje de incidentes resueltos dentro del SLA definido por severidad. | L1 / L2     | SOC Manager |

> ⚠️ **Nota editorial — Conflicto C1**: MTTD (Mean Time to Detect) no aparece documentado como KPI central en la documentación oficial de Microsoft. Puede usarse como perspectiva consultiva, pero no como benchmark normativo [`Manage SOC w/ Metrics`](https://learn.microsoft.com/azure/sentinel/manage-soc-with-incident-metrics).

> ⚠️ **Nota editorial — Conflicto C2**: MTTR es medido por Microsoft como MTTR y MTTC; así como MTTR-Complete para el tiempo de resolución total incluyendo erradicación y recuperación. Todas las denominaciones se preservan.

### 9.2 KPIs de cobertura de detección

| KPI                                   | Descripción                                                  | Responsible        | Accountable |
| ------------------------------------- | ------------------------------------------------------------ | ------------------ | ----------- |
| **Cobertura MITRE ATT&CK**            | Proporción de tácticas y técnicas del framework v18 cubiertas por detecciones activas vs. disponibles en el workspace | Threat Hunter / L3 | SOC Manager |
| **Alert-to-Incident conversion rate** | Tasa de conversión de alertas en incidentes; indica la eficacia de las reglas de correlación | L2 / L3            | SOC Manager |
| **Reglas activas vs. deshabilitadas** | Estado del catálogo de detecciones                           | Platform Engineer  | SOC Manager |

### 9.3 KPIs de automatización

| KPI                                        | Descripción                                                  | Responsible            | Accountable |
| ------------------------------------------ | ------------------------------------------------------------ | ---------------------- | ----------- |
| **Porcentaje de incidentes auto-triados**  | Incidentes procesados sin intervención manual inicial        | Platform Engineer / L3 | SOC Manager |
| **Porcentaje de incidentes auto-cerrados** | Incidentes resueltos completamente por automatización        | Platform Engineer / L3 | SOC Manager |
| **Playbook success rate**                  | Porcentaje de ejecuciones exitosas de playbooks. Target referencial: >95% | Platform Engineer / L3 | SOC Manager |

### 9.4 KPIs de salud de la plataforma

| KPI                             | Descripción                                                  | Responsible       | Accountable |
| ------------------------------- | ------------------------------------------------------------ | ----------------- | ----------- |
| **Data Connector availability** | Porcentaje de uptime de conectores críticos. Target referencial: >99.5% | Platform Engineer | SOC Manager |
| **Data Ingestion Completeness** | Porcentaje de logs esperados que se ingieren exitosamente. Target referencial: >99% | Platform Engineer | SOC Manager |

### 9.5 KPIs de gobernanza (alineados a NIST CSF 2.0)

La guía de Microsoft sobre integración de NIST CSF 2.0 propone los siguientes
indicadores de madurez para la Iniciativa Secure Future [`Integración NIST`](https://learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance):

| Indicador                                          | Descripción                                                  | Responsible                        | Accountable |
| -------------------------------------------------- | ------------------------------------------------------------ | ---------------------------------- | ----------- |
| **Desviaciones de política aceptadas como riesgo** | Número de excepciones de seguridad formalmente registradas   | GRC                                | SOC Manager |
| **Activos ICT en inventario**                      | Porcentaje de activos de TI en inventario conforme a política organizacional | Platform Engineer / GRC            | SOC Manager |
| **Incidentes por deficiencias de control**         | Porcentaje de incidentes relacionados con deficiencias de control existentes | L3 / GRC                           | SOC Manager |
| **Conexiones de terceros conformes**               | Porcentaje de conexiones de terceros clave que cumplen requisitos de conformidad definidos | GRC                                | SOC Manager |
| **Completitud de revisión de accesos**             | Porcentaje de revisiones de acceso completadas en plazo      | Platform Engineer / Identity Admin | SOC Manager |
| **Cumplimiento de política de retención**          | Porcentaje de workspace configurado conforme a política de retención | Platform Engineer / GRC            | SOC Manager |
| **Tasa de cierre de lecciones aprendidas**         | Porcentaje de lecciones aprendidas cerradas con acción implementada | L3 / GRC                           | SOC Manager |

> ⚠️ **Nota editorial — Conflicto C4**: Benchmarks numéricos como "True Positive Rate >40%" no están respaldados por documentación oficial Microsoft. Puede mencionarse solo como referencia de la industria si se desea, con disclaimer explicitando carácter consultivo.

---

## 10. Alineación con marcos de referencia

### NIST CSF 2.0

Las actividades de Sentinel abarcan las seis funciones del framework [`Integración NIST`](https://learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance):

| Función CSF 2.0 | Actividades de Sentinel alineadas                            |
| --------------- | ------------------------------------------------------------ |
| **Govern**      | Revisión de accesos, auditoría de cambios, control de políticas de retención, reportes ejecutivos, evaluación anual de madurez |
| **Identify**    | Inventario de permisos, catálogo de casos de uso, revisión de arquitectura, inventario de fuentes de datos |
| **Protect**     | Control de acceso con privilegios mínimos, retención de datos conforme a política, guardrails en playbooks |
| **Detect**      | Reglas analíticas (Scheduled/NRT), hunting proactivo, monitoreo de conectores, UEBA, enriquecimiento con Threat Intelligence, watchlists, motor Fusion |
| **Respond**     | Triage de incidentes, investigación con Advanced Hunting, contención (aislamiento de dispositivos, bloqueo de cuentas), playbooks de respuesta automatizada |
| **Recover**     | Restauración de servicios, validación de integridad post-incidente, lecciones aprendidas, tabletop exercises, actualización de runbooks |

El mapeo inicial de controles a categorías del CSF puede ser intensivo en recursos, y
la naturaleza no prescriptiva del framework requiere gobernanza sólida para evitar
inconsistencias. Los beneficios incluyen compatibilidad con múltiples estándares
(ISO 27001, NIST SP 800-53), mejora de la gestión basada en riesgo y fortalecimiento
de la postura regulatoria.

### MITRE ATT&CK

Sentinel integra de forma nativa la taxonomía de tácticas y técnicas del framework en
sus reglas analíticas, consultas de hunting e incidentes [`MITRE Coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage). Cuando se crean incidentes
a partir de alertas generadas por reglas con técnicas MITRE configuradas, las técnicas
se agregan automáticamente a los incidentes. La revisión semestral de la matriz de
cobertura garantiza el cierre progresivo de brechas de detección.

### Zero Trust

La implementación de los tres principios de Zero Trust se describe en la sección 2 de
este documento. La guía de implementación de Sentinel y Defender XDR para Zero Trust
está referenciada directamente desde la guía operativa de Sentinel, confirmando la
vinculación intencional de Microsoft entre las operaciones diarias y la estrategia Zero Trust [`SIEM-XDR Overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview).

---

## 11. Referencias oficiales

| Documento                                          | URL                                                          |
| -------------------------------------------------- | ------------------------------------------------------------ |
| Guía operativa de Microsoft Sentinel               | [`learn.microsoft.com/azure/sentinel/ops-guide`](https://learn.microsoft.com/azure/sentinel/ops-guide) |
| Guía SIEM-XDR integrado (Sentinel + Defender XDR)  | [`learn.microsoft.com/security/operations/siem-xdr-overview`](https://learn.microsoft.com/security/operations/siem-xdr-overview) |
| Página MITRE ATT&CK de Sentinel                    | [`learn.microsoft.com/azure/sentinel/mitre-coverage`](https://learn.microsoft.com/azure/sentinel/mitre-coverage) |
| Gobernanza NIST CSF 2.0 — Iniciativa Secure Future | [`learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance`](https://learn.microsoft.com/security/zero-trust/sfi/integrate-nist-2-governance) |
| Mejores prácticas de Microsoft Sentinel            | [`learn.microsoft.com/azure/sentinel/best-practices`](https://learn.microsoft.com/azure/sentinel/best-practices) |
| Gestión de incidentes con métricas del SOC         | [`learn.microsoft.com/azure/sentinel/manage-soc-with-incident-metrics`](https://learn.microsoft.com/azure/sentinel/manage-soc-with-incident-metrics) |
| Referencia de optimización del SOC                 | [`learn.microsoft.com/azure/sentinel/soc-optimization/soc-optimization-reference`](https://learn.microsoft.com/azure/sentinel/soc-optimization/soc-optimization-reference) |
| Acceso a optimización del SOC                      | [`learn.microsoft.com/azure/sentinel/soc-optimization/soc-optimization-access`](https://learn.microsoft.com/azure/sentinel/soc-optimization/soc-optimization-access) |
| Guía de respuesta a incidentes                     | [`learn.microsoft.com/security/operations/incident-response-overview`](https://learn.microsoft.com/security/operations/incident-response-overview) |
| Responder a incidentes desde Azure Portal          | [`learn.microsoft.com/security/zero-trust/respond-incident-azure`](https://learn.microsoft.com/security/zero-trust/respond-incident-azure) |
| Disponibilidad de features de Sentinel por región  | [`docs.azure.cn/en-us/sentinel/feature-availability`](https://docs.azure.cn/en-us/sentinel/feature-availability) |

---

**Disclaimer final sobre benchmarks y métricas normativas:**  
Benchmarks numéricos, como tasas de True Positive Rate o valores de MTTD, deben considerarse solo como ejemplos de la industria en ausencia de referencia explícita en Microsoft Docs. Para decisiones normativas y de madurez, referirse solo a los KPIs, métricas y targets oficialmente documentados por Microsoft.