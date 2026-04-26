---
layout: page
title: "MS_Purview_IP-SecOps_e01v01_Art07-MS_CSU_Security_MCSA"
permalink: /ms-purview/mip-secops/ms-purview-ip-secops-e01v01-art07-ms-csu-security-mcsa/
---

# MS_Purview_IP-SecOps_e01v01_Art07-MS_CSU_Security_MCSA

Este documento aborda cinco conceptos clave sobre respuesta a incidentes en entornos Microsoft 365, orientados a la gestión de alertas críticas y su consecuente manejo según mejores prácticas de seguridad. Utiliza como núcleo tecnológico Microsoft Purview Portal, Sentinel, Entra ID, Defender for Endpoint y eDiscovery para la preservación de evidencia, junto con lineamientos regulatorios como GDPR. Su finalidad es guiar a equipos de operaciones de seguridad en un flujo estructurado, integrando investigación, contención, análisis forense, escalamiento legal y comunicación post-incidente, promoviendo rapidez, documentación sólida y cumplimiento normativo.

El primer concepto, llamado “Investigación de Alerta Crítica” (AH-A), establece el arranque del proceso ante alertas de alta o crítica severidad. Este paso comprende un triage inicial donde se identifican entidades, documentos y etiquetas, y se clasifica la alerta en verdadero positivo, falso positivo o en revisión. Si la alerta es confirmada, se pasa de inmediato al procedimiento de contención. El segundo concepto, “Contención Inmediata” (AH-B), se activa tras la validación de acceso no autorizado o exfiltración de datos sensibles. Incluye revocar accesos usando Purview, bloquear cuentas en Entra ID, preservar evidencia exportando registros, y notificar a los responsables legales en cumplimiento con GDPR, que exige una notificación dentro de 72 horas.

El tercer concepto, “Análisis Forense” (AH-C), se ejecuta cuando es necesario reconstruir toda la línea de tiempo relacionada al acceso y uso de datos afectados. Esto implica consultar logs, activar retenciones legales y enumerar, paso a paso, acciones como accesos, modificaciones o exfiltraciones de información. El cuarto concepto, “Escalamiento Legal y Compliance” (AH-D), se aplica si están involucrados datos bajo regulación especial, coordinando directamente con el área legal, el oficial de protección de datos y compliance. Contempla el envío de notificaciones regulatorias, preparación de documentación técnica y la retención de información relevante para auditoría o proceso legal.

Por último, el concepto “Comunicación Ejecutiva y Post-Mortem” (AH-E) marca el cierre formal del incidente. Se comunica a los ejecutivos la naturaleza del incidente, los datos afectados y el impacto, junto con las acciones tomadas. Además, se realiza una revisión post-mortem donde se identifica la causa raíz, se evalúa la efectividad de los controles aplicados y se definen acciones correctivas, las cuales quedan plasmadas con responsables y fechas de cumplimiento. Todos estos pasos están alineados a un flujo binario estructurado, donde cada decisión orienta el paso siguiente, y el tiempo máximo para la ejecución de todo el proceso queda condicionado por el marco regulatorio, especialmente por el plazo de notificación de GDPR.



# Cuando la Alarma Suena: 5 Procedimientos de Respuesta que Separan la Contención del Caos

## Nivel 2 — Conceptos Clave (5 Conceptos Articulados)

### Concepto 1: AH-A — Investigación de Alerta Crítica

AH-A es la entrada inicial al flujo de respuesta, activándose ante alertas de alta o crítica severidad en Purview Portal o incidentes generados por Sentinel para correlaciones múltiples ([Investigating alerts in Microsoft Purview](https://learn.microsoft.com/en-us/microsoft-365/compliance/incident-management-overview); [Incident handling in Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/incident-management)).  
El procedimiento recomendado incluye:

- **Triage Inicial (0–15 min):** Identificación de entidad, documento, etiqueta, workload y timestamp. Clasificación binaria como VP, FP o RIA. VP activa AH-B de inmediato, FP documentado en logs, RIA con deadline sugerido de 4 horas.
- **Análisis de Contexto (15–30 min):** Consulta de Audit Log, Entra ID, Defender for Endpoint y Sentinel para correlacionar señales ([Audit Log](https://learn.microsoft.com/en-us/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance), [Identity Protection in Microsoft Entra ID](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/), [Defender for Endpoint Overview](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint), [Microsoft Sentinel Overview](https://learn.microsoft.com/en-us/azure/sentinel/azure-sentinel-overview)).
- **Determinación de Alcance (30–60 min):** Cuantificación de documentos afectados, exfiltración, ubicación de acceso y posible compromiso externo.
- **Escalamiento:** Ante exfiltración o compromiso, notificación a CISO en menos de 1 hora. Si impacto es limitado, cierre con documentación.

  
### Concepto 2: AH-B — Contención Inmediata

AH-B es el procedimiento de mayor urgencia, iniciando tras confirmación de acceso no autorizado o exfiltración. Su secuencia es:

- **Revocar acceso:** Usar Purview Portal y ajustar políticas según volumen ([Revoke access to files protected by sensitivity labels](https://learn.microsoft.com/en-us/microsoft-365/compliance/revoke-access-to-files-protected-by-sensitivity-labels)). Registrar la efectividad vía Audit Log.
- **Bloquear cuenta comprometida:** Entra ID, revocación de sesiones, y restablecimiento de MFA ([Block sign-ins for user accounts](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/how-to-block-or-unblock-sign-in), [Revoke user sign-in sessions](https://learn.microsoft.com/en-us/azure/active-directory/manage-apps/revoke-user-access)).
- **Preservar evidencia:** Exportar logs, correlacionar eventos en Sentinel y activar eDiscovery hold si procede ([eDiscovery solution in Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-overview), [eDiscovery and chain of custody in Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-best-practices)).
- **Notificación inicial:** Informar a CISO y Legal, activar proceso regulatorio si es una brecha (GDPR: 72 horas desde conocimiento) ([GDPR Notification Requirements](https://learn.microsoft.com/en-us/microsoft-365/compliance/gdpr-dpa-summary)).


### Concepto 3: AH-C — Análisis Forense

Se realiza al requerir reconstrucción total de línea de tiempo de acceso/uso de datos sensibles, con pasos:

1. Definir alcance de tiempo y entidades involucradas.
2. Consultar Audit Log de M365 ([Search the audit log](https://learn.microsoft.com/en-us/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance)).
3. Uso de eDiscovery en Purview ([eDiscovery solution](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-overview)).
4. Activar eDiscovery hold ([eDiscovery hold](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-best-practices)).
5. Reconstrucción secuencial de eventos: acceso, modificación, compartición, exfiltración.
6. Documentación del informe forense.


### Concepto 4: AH-D — Escalamiento Legal y Compliance

Escala al involucrar datos regulados, coordinando con Legal, DPO y Compliance.

- Notificación inmediata a Legal y DPO.
- Clasificación regulatoria bajo GDPR, CCPA, LGPD, etc. ([GDPR Notification Requirements](https://learn.microsoft.com/en-us/microsoft-365/compliance/gdpr-dpa-summary)).
- Activar notificación regulatoria (plazo GDPR: 72 horas).
- Evaluar notificación a afectados.
- Preparar documentación técnica.
- Activar retención legal (eDiscovery hold).

La decisión regulatoria es jurídica, dependiendo de la existencia y velocidad de documentación técnica generada por el SOC.


### Concepto 5: AH-E — Comunicación Ejecutiva y Post-Mortem

Se activa al cerrar formalmente un incidente High/Critical, con dos componentes principales:

**Comunicación Ejecutiva (dentro de 24h):**

| Pregunta que Responde           | Contenido                                  |
| ------------------------------- | ------------------------------------------ |
| ¿Qué ocurrió?                   | Descripción ejecutiva                      |
| ¿Qué datos se vieron afectados? | Tipo, volumen, clasificación               |
| ¿Cuándo ocurrió?                | Línea de tiempo resumida                   |
| ¿Cuál fue el impacto?           | Usuarios, sistemas, exposición regulatoria |
| ¿Cómo se respondió?             | Acciones y ventana temporal                |
| ¿Está contenido?                | Estado actual y monitoreo                  |

**Post-Mortem (dentro de 72h):**

| Sección                   | Lo que Resuelve            |
| ------------------------- | -------------------------- |
| Causa raíz                | Evidencia específica       |
| Línea de tiempo           | Secuencia exacta           |
| Efectividad controles MIP | Cifrado, revocación, logs  |
| Brechas identificadas     | Controles faltantes        |
| Acciones correctivas      | Cambio, responsable, fecha |
| Seguimiento               | Fecha de verificación      |

Las acciones resultantes tienen responsables y fechas, gestionándose como riesgos si no se cumplen.

---

## Nivel 3 — Notas de Soporte e Insights

### Insight A — El flujo es un algoritmo binario
El valor del conjunto está en cómo las decisiones binarios encadenan las fases. Ejemplo:

```
TRIGGER: Alerta High/Critical
    │
  AH-A: INVESTIGACIÓN
    │
    ├─── ¿VP confirmado? ─── NO → Documentar FP → Cerrar
    │
    YES
    │
  AH-B: CONTENCIÓN
    │
    ├─── ¿Exfiltración o potencial legal? NO → Cierre → AH-E
    │
    YES
    │
  AH-C: FORENSE
    │
    ├─── ¿Datos regulados? ─── NO → AH-E
    │
    YES
    │
  AH-D: ESCALAMIENTO LEGAL
    │
  AH-E: POST-MORTEM + COMUNICACIÓN
```

### Insight B — Ventana de 72 Horas
El diseño del flujo usa el plazo de 72 horas de GDPR como restricción temporal a nivel industrial ([GDPR Notification Requirements](https://learn.microsoft.com/en-us/microsoft-365/compliance/gdpr-dpa-summary)).

```
Hora 0: Trigger
Hora 1: AH-A y AH-B en ejecución
Hora 4: AH-B completado
Hora 8–24: AH-C en curso
Hora 24: Comunicación Ejecutiva
Hora 72: Límite para notificación regulatoria
```

### Insight C — AH-B marca la cultura SOC

La contención proactiva, incluso antes de diagnóstico completo, refleja un SOC orientado a protección ([Revoke access to files protected by sensitivity labels](https://learn.microsoft.com/en-us/microsoft-365/compliance/revoke-access-to-files-protected-by-sensitivity-labels)).

### Insight D — El riesgo del procedimiento no practicado

La madurez reduce la frecuencia de uso, pero requiere simulacros periódicos para mantener competencia.

### Insight E — Urgencia de preservación de evidencia en AH-B

Preservar logs y cadenas de custodia antes de expiración es crítico ([Audit Log Retention](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-manage)).

---

## Conexiones Externas

| Conexión                            | Destino         | Naturaleza                                                   |
| ----------------------------------- | --------------- | ------------------------------------------------------------ |
| Activación desde ciclos recurrentes | Bloques 4, 5, 6 | Un VP detectado activa procedimientos AH                     |
| Ejecución de controles técnicos     | Bloque 3        | Revocación, bloqueo, RMS, eDiscovery                         |
| Principios operativos implementados | Bloque 2        | Evidencia auditable, defensa en profundidad                  |
| Retorno del post-mortem             | Bloques 4, 5, 6 | Tuning, evaluación, ejercicios, validación de cambios        |
| Medición de KPIs                    | Bloque 8        | MTTA y MTTR definidos por AH-A y AH-B ([Security Operations KPIs](https://learn.microsoft.com/en-us/azure/sentinel/security-operations-kpis)) |
| Mapeo a NIST CSF 2.0                | Bloque 9        | Correspondencia de controles y procesos ([Mapping NIST CSF to Microsoft 365 controls](https://learn.microsoft.com/en-us/microsoft-365/compliance/nist-cybersecurity-framework)) |
| Simulación y entrenamiento          | Bloque 6        | Ejercicios tabletop validan competencia                      |
| Formación por Nivel                 | Bloque 10       | Estructuración interna consultiva                            |

---
