---
layout: page
title: "MS_Purview_IP-SecOps_e01v01_Art03-MS_CSU_Security_MCSA"
permalink: /ms-purview/mip-secops/ms-purview-ip-secops-e01v01-art03-ms-csu-security-mcsa/
---

# MS_Purview_IP-SecOps_e01v01_Art03-MS_CSU_Security_MCSA

## 

El texto describe el funcionamiento de una arquitectura de seguridad centrada en la detección y respuesta ante incidentes mediante telemetría en el ecosistema Microsoft. Involucra tecnologías como Microsoft Information Protection, el portal Purview, Microsoft 365 Audit Log y Microsoft Sentinel. La finalidad del documento es guiar la comprensión de cómo fluye la información desde la generación de una señal (evento de seguridad), pasando por su correlación e interpretación, hasta la toma de decisión y ejecución de controles de respuesta automatizados o manuales. Está enfocado en equipos de seguridad que deben monitorear, correlacionar y responder ante riesgos en datos sensibles y actividades anómalas.

El proceso comienza en la capa de generación de señal, donde diversas fuentes como Microsoft Purview Portal, los registros unificados de auditoría de Microsoft 365 y Microsoft Sentinel generan y centralizan información sobre eventos relacionados con la protección de datos. Estos eventos incluyen desde la aplicación o remoción de etiquetas de sensibilidad, cambios en la protección RMS, hasta señales de exposición accidental de datos. No todos los eventos tienen el mismo valor: se identifican especialmente aquellos que indican degradación de protección, eliminación de cifrado, acceso inusual o acciones fuera del horario normal. Estos eventos pasan a la capa de correlación, donde Microsoft Sentinel actúa como centro nervioso. Sentinel ingiere telemetría, correlaciona eventos mediante reglas de análisis y alimenta dashboards y playbooks de respuesta. Un incidente típico sigue un flujo estándar: el evento es detectado y registrado, Sentinel lo ingiere y lo analiza, genera un incidente visualizable y procesable, y un analista ejecuta el triage. Si se confirma el incidente, se aplican controles de mitigación como revocar acceso, bloquear cuentas, ajustar permisos o ejecutar preservación de evidencia. El tiempo total desde la aparición del evento hasta la acción debe mantenerse por debajo de una hora para maximizar la efectividad. Es clave, además, la salud de los conectores de datos, la correcta configuración del almacenamiento de logs y la adecuada retención de registros según la suscripción contratada. Todo el ciclo se sostiene sobre una operación diaria rigurosa, integrando insights sobre puntos vulnerables del stack técnico, y está enmarcado por conexiones con procedimientos de operación, métricas de salud y rutinas de formación continua. Este modelo busca asegurar una defensa robusta, centralizada y basada en evidencias, adaptable a diferentes grados de adopción tecnológica y madurez operativa.



# Anatomía de la Máquina de Detección: Cómo Fluye la Telemetría desde el Dato hasta la Decisión

## NIVEL 2 — Conceptos Clave

### Concepto 1: Capa de Generación de Señal — Las Fuentes de Telemetría

La telemetría de Microsoft Information Protection (MIP) se origina en tres fuentes principales:

**1. Microsoft Purview Portal:**  
- **Alert Policies:** Alertas de cumplimiento y clasificación en tiempo real ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/data-loss-prevention-policies?view=o365-worldwide)).
- **Content Explorer:** Inventario de datos sensibles actualizado, basado en etiquetas y tipos de información sensible ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/data-classification-content-explorer?view=o365-worldwide)).
- **Activity Explorer:** Registro de actividades de etiquetado, consultable por usuario, workload y periodo ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/activity-explorer?view=o365-worldwide)).

**2. M365 Unified Audit Log:**  
Centraliza auditoría relevante:
- **Eventos de etiquetas:** Aplicación, cambio, remoción, degradación.
- **Eventos RMS:** Protección, desprotección, acceso denegado, descifrado ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-search?view=o365-worldwide)).
- **Eventos de oversharing:** Señalización de protección inferior al contenedor ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-label-events-audit-logs?view=o365-worldwide)).

**3. Microsoft Sentinel**  
Ingiere los logs de las dos fuentes anteriores y los transforma en datos consultables, correlacionables y accionables ([Fuente oficial](https://learn.microsoft.com/en-us/azure/sentinel/connectors)).

---

### Concepto 2: Los Eventos Críticos — El Vocabulario de Riesgo del Audit Log

No todos los eventos del Audit Log tienen mismo peso. El modelo identifica eventos clave con nivel de riesgo y respuesta asociada ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-label-events-audit-logs?view=o365-worldwide)):

| Evento                              | Significado             | Nivel de Riesgo | Respuesta Esperada                    |
| ----------------------------------- | ----------------------- | --------------- | ------------------------------------- |
| SensitivityLabelRemoved             | Dato desprotegido       | Alto            | Investigar usuario y contexto         |
| SensitivityLabelChanged (downgrade) | Protección degradada    | Alto            | Verificar justificación y correlación |
| RMSProtectionDisabled               | Cifrado eliminado       | Crítico         | Contención inmediata                  |
| FileSensitivityLabelMismatch        | Oversharing             | Alto            | Notificar propietario, remediar       |
| Accesos fuera de horario/destino    | Posible exfiltración    | Alto            | Correlacionar accesos y alertas       |
| RMSGetServersideDecryptedContent    | Descifrado por servidor | Medio-Alto      | Verificar legitimidad                 |

La ausencia anómala de estos eventos puede indicar fallas de logging o conectores, así como baja actividad.

---

### Concepto 3: Capa de Correlación e Inteligencia — Microsoft Sentinel como Sistema Nervioso

Sentinel es el componente donde la señal se transforma en inteligencia operativa, mediante:

- **Data Connectors:** Ingestan telemetría y su estado debe verificarse diariamente ([Fuente oficial](https://learn.microsoft.com/en-us/azure/sentinel/connectors)).
- **Analytics Rules:** Correlacionan señales múltiples para crear incidentes, deben ser afinadas periódicamente ([Fuente oficial](https://learn.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats-custom)).
- **Workbooks:** Dashboards para visibilidad centralizada, esenciales para la operación diaria ([Fuente oficial](https://learn.microsoft.com/en-us/azure/sentinel/workbooks)).
- **Playbooks (Logic Apps):** Automatizan respuestas para incidentes ([Fuente oficial](https://learn.microsoft.com/en-us/azure/sentinel/tutorial-respond-threats-playbook)).

---

### Concepto 4: Capa de Mitigación Activa — Los Controles de Respuesta

Controles disponibles cuando la detección confirma un incidente real ([Fuente oficial](https://learn.microsoft.com/en-us/security/compass/defense-in-depth#security-controls-and-defense-in-depth)):

- **Revocación de acceso en Purview (Document Tracking):** Corta acceso a documentos protegidos ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/dlp-offboarding-scenario?view=o365-worldwide)).
- **Bloqueo de cuenta y revocación de tokens en Entra ID:** Previene el uso indebido de identidades comprometidas ([Fuente oficial](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-session-lifetime)).
- **Modificación de usage rights en Azure RMS:** Ajusta permisos sobre documentos protegidos ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/assign-usage-rights-to-information-protection-labels?view=o365-worldwide)).
- **Hold de preservación de evidencia en eDiscovery:** Conserva datos relevantes para investigaciones legales ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-cases?view=o365-worldwide)).

Estos controles pueden aplicarse en secuencia rápida en incidentes críticos, pero su plazo de ejecución depende del contexto operativo.

---

### Concepto 5: El Flujo End-to-End — De la Señal a la Decisión

Ejemplo de flujo real:

1. **Generación:** Evento `SensitivityLabelRemoved` detectado por Audit Log.
2. **Ingesta:** Data connector ingesta el evento en Sentinel (latencia 5–15 min).
3. **Correlación:** Analytics rule detecta sesión anómala y genera incidente High.
4. **Visualización:** El incidente aparece en el workbook y la cola de incidentes.
5. **Triage:** Analista evalúa contexto y clasifica el incidente.
6. **Acción:** Se activa la secuencia de mitigación: revocación de acceso, bloqueo de cuenta, revocación de tokens, hold de eDiscovery.
7. **Evidencia:** Todo queda documentado, con timestamps y confirmación de efectividad.

Tiempo recomendado de extremo a extremo: menos de 60 minutos, según prácticas consultivas ([Referencia](https://learn.microsoft.com/en-us/azure/sentinel/respond-incident)).

---

## NIVEL 3 — Notas de Soporte e Insights

**Insight A:**  
El conector de datos es el punto más frágil; si deja de funcionar sin alarma, la ausencia de eventos puede confundirse con baja actividad. Se recomienda la verificación diaria y activar investigación si la caída es mayor a 20% respecto al promedio semanal ([Fuente oficial](https://learn.microsoft.com/en-us/azure/sentinel/connectors)).

**Insight B:**  
Sentinel tiene dos tipos de almacenamiento: Analytic Logs y Basic Logs. Los datos críticos deben mantenerse en Analytic Logs para asegurar capacidad de correlación y análisis forense ([Información sobre logs](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-overview)).

**Insight C:**  
La retención de logs depende del SKU: Audit Standard (90/180 días), Audit Premium (hasta un año) ([Fuente oficial](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-retention?view=o365-worldwide)). Incidentes detectados fuera de esa ventana pueden quedar sin evidencia si no se cuenta con Audit Premium.

**Insight D:**  
El stack técnico es prerrequisito del modelo operativo recomendado. Deben documentarse los planes de mitigación para cada componente ausente, asumiendo una adopción progresiva.

---

## Conexiones Externas

| Conexión                                                   | Destino   | Naturaleza                 |
| ---------------------------------------------------------- | --------- | -------------------------- |
| Las fuentes de telemetría alimentan actividades diarias    | Bloque 4  | D-01, D-02, D-07           |
| La capa de correlación refina tuning semanal               | Bloque 5  | S-01, S-07, S-08           |
| Los controles de mitigación soportan procedimientos ad-hoc | Bloque 7  | AH-B                       |
| Salud del stack medida por KPIs específicos                | Bloque 8  | Uptime, ingesta, retención |
| Integración end-to-end validada mensualmente               | Bloque 6  | M-08                       |
| Principios operativos se materializan aquí                 | Bloque 2  | Correlación y telemetría   |
| El programa de formación cubre cada capa                   | Bloque 10 | Nivel 1, 2 y 3             |
