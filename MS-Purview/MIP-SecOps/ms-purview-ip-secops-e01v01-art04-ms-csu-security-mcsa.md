---
layout: page
title: "MS_Purview_IP-SecOps_e01v01_Art04-MS_CSU_Security_MCSA"
permalink: /ms-purview/mip-secops/ms-purview-ip-secops-e01v01-art04-ms-csu-security-mcsa/
---

# MS_Purview_IP-SecOps_e01v01_Art04-MS_CSU_Security_MCSA



Este documento describe un ciclo operativo diario que ejecuta un Centro de Operaciones de Seguridad, conocido como SOC, enfocado en la protección de datos sensibles con tecnologías de Microsoft. Involucra herramientas como Microsoft Sentinel, Microsoft Purview y Azure RMS, centrándose en la gestión de telemetría, análisis de incidentes y validación de controles de seguridad. Su finalidad es asegurar que la infraestructura de monitoreo esté funcional, identificar incidentes relevantes, detectar anomalías en el manejo de información crítica y garantizar que los mecanismos de cifrado siguen protegiendo los activos. Además, establece un método estructurado para la revisión, correlación y reporte de eventos, vinculado al cumplimiento con marcos normativos internacionales como NIST CSF.

La rutina diaria del SOC sigue una secuencia lógica para no caer en análisis erróneos. El día comienza con la verificación de que todos los conectores de datos funcionan, asegurando la integridad de la telemetría. Luego, se valida que no haya cambios administrativos no autorizados en Purview, pues cualquier alteración sin control puede exponer datos. Si los cimientos son válidos, se inicia la detección principal. Por un lado, se revisan automáticamente las alertas de Purview según las políticas configuradas, y se documentan tanto verdaderos positivos como falsos. Por otro, se realiza hunting proactivo en los registros de audición, buscando eventos críticos incluso si no activaron alertas, elevando incidentes cuando corresponde.

Posteriormente, la correlación se vuelve clave. Se entrelazan señales provenientes de Purview, Sentinel, identidades y dispositivos, para identificar incidentes que podrían ser más graves al combinarse. Además, se revisan eventos de oversharing, es decir, si documentos sensibles quedaron expuestos en lugares inseguros, gestionando cada caso con responsables. La validación de controles activos es otro paso esencial, donde se audita si el cifrado de archivos sigue funcionando adecuadamente y se detectan intentos fallidos de acceso, lo cual puede indicar amenazas o problemas técnicos. El ciclo cierra evaluando el estado de incidentes abiertos, asegurando cumplimiento de tiempos de respuesta y documentando el estado diario en el dashboard de operaciones, con indicadores claros y listos para comunicación interna o para ser escalados. Todo este flujo no solo depende de la automatización, sino también de la revisión humana, especialmente para interpretar contextos complejos y tomar acciones inmediatas en caso de riesgo. Los resultados y hallazgos de cada día alimentan procesos de mejora semanal y permiten cumplir con buenas prácticas internacionales de detección y respuesta, según los marcos de referencia más reconocidos.





# El Ciclo de 24 Horas: 10 Actividades Diarias del SOC

## Nivel 2 — Conceptos Clave

### Concepto 1: Verificación de Cimientos — "¿Puedo Confiar en lo que Veo?" (D-07 + D-08)

Antes de analizar la telemetría, el SOC valida dos factores fundamentales: (1) que todos los conectores de datos están activos y la información ingresa correctamente a Sentinel, y (2) que no hubo cambios administrativos no autorizados en Purview.

**D-07 · Verificación de Integridad de Conectores**
El analista revisa en Sentinel que los conectores clave (ej. Microsoft 365, Defender XDR) estén en estado "Connected", y que el volumen de datos coincida con la línea base semanal. Caídas superiores al 20% deben alertar, pues un conector inactivo puede dejar al SOC sin visibilidad crítica [Sentinel Data Connectors](https://learn.microsoft.com/en-us/azure/sentinel/connect-data-sources).

**D-08 · Validación de Cambios Administrativos**
Se revisan los cambios administrativos recientes en Purview (creación/modificación/eliminación de etiquetas y políticas) vía Audit Log. Todo cambio debe contar con ticket de autorización; lo contrario puede indicar riesgo grave de exposición masiva [Search the audit log in the compliance portal](https://learn.microsoft.com/en-us/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide).

*D-07 antecede a D-08: sin integridad en la telemetría, la revisión de logs puede ser inválida.*

---

### Concepto 2: Detección Primaria — "¿Qué Ocurrió con los Datos Sensibles?" (D-01 + D-02)

Con los cimientos asegurados, el SOC ejecuta la detección principal: revisión de alertas y hunting proactivo en logs.

**D-01 · Revisión de Alertas de Microsoft Purview**
Se aplican filtros por severidad sobre las alertas generadas por Purview (portal o dashboard), clasificándolas como verdadero/falso positivo o bajo investigación. Los falsos positivos documentados se usan para ajuste posterior; los verdaderos se convierten en tickets para investigación [Microsoft Purview DLP Alert Management](https://learn.microsoft.com/en-us/microsoft-365/compliance/dlp-alert-management?view=o365-worldwide).

**D-02 · Monitoreo del Audit Log — Information Protection**
El analista busca eventos críticos como `SensitivityLabelRemoved`, `RMSProtectionDisabled` o `FileSensitivityLabelMismatch`, independientemente de configuraciones previas de alerta. Los hallazgos se exportan para correlación y anomalías de volumen a futuro [Audit Log Activities](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-activities?view=o365-worldwide).

*D-01 se basa en políticas configuradas; D-02 es hunting proactivo. Ambas son obligatorias y complementarias.*

---

### Concepto 3: Correlación y Contexto — "¿Qué Historia Cuentan las Señales en Conjunto?" (D-03 + D-05)

La combinación de señales incrementa el valor de la detección.

**D-03 · Correlación de Eventos en Microsoft Sentinel**
Revisión de incidentes en Sentinel que fusionen señales de Purview con identidad y dispositivos. Cada incidente se categoriza por severidad, usuarios implicados y táctica MITRE asociada [Microsoft Sentinel documentation](https://learn.microsoft.com/en-us/azure/sentinel/).

**D-05 · Revisión de Eventos de Oversharing**
Se detectan eventos donde documentos de alta sensibilidad están en repositorios con menor protección (`FileSensitivityLabelMismatch`). Cada caso se revisa y se notifica al responsable para remediar [Sensitivity labels for SharePoint, OneDrive, and Teams](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-sharepoint-onedrive-files?view=o365-worldwide).

---

### Concepto 4: Validación de Controles Activos — "¿Los Mecanismos de Protección Siguen Funcionando?" (D-04 + D-09)

Se comprueba que el cifrado, el principal control técnico, permanezca activo y eficaz.

**D-04 · Validación de Accesos a Contenido Cifrado**
Se inspeccionan logs de Azure RMS (`RMSProtect`, `RMSUnprotect`, etc.) y se valida en Azure Portal que el servicio esté funcional y que los usuarios no reporten problemas de acceso [What is Azure Rights Management?](https://learn.microsoft.com/en-us/azure/information-protection/what-is-azure-rms).

**D-09 · Revisión de Accesos Fallidos**
El SOC analiza intentos de acceso denegado a archivos protegidos, identificando patrones inusuales (ej. múltiples intentos fallidos por usuario/documento) que puedan indicar tanto amenazas como problemas operativos.

---

### Concepto 5: Cierre de Ciclo — "¿En Qué Estado Termina el Día?" (D-06 + D-10)

El ciclo termina con la revisión de incidentes y la consolidación del estado en el dashboard.

**D-06 · Seguimiento de Incidentes Activos**
Se revisan todos los incidentes de datos abiertos, evaluando el cumplimiento de los SLA (ejemplo consultivo: críticos <4h para cierre, altos <8h, etc.). Los incidentes en riesgo de incumplimiento se escalan según la severidad.

| Severidad | MTTA objetivo | MTTR objetivo | Escalamiento si breach |
| --------- | ------------- | ------------- | ---------------------- |
| Critical  | < 1 hora      | < 4 horas     | CISO + Legal inmediato |
| High      | < 4 horas     | < 8 horas     | Lead SOC               |
| Medium    | < 8 horas     | < 24 horas    | Supervisor de turno    |
| Low       | < 24 horas    | < 72 horas    | Analista asignado      |

**D-10 · Actualización del Tablero Operativo**
El analista verifica en el dashboard que todos los KPIs diarios estén en umbral aceptable: alertas pendientes, oversharing detectado, incidentes en SLA, cambios no autorizados, estado de conectores y volumen de accesos fallidos. Se prepara un resumen RAG para comunicación y escalamiento según se requiera.

---

## Nivel 3 — Notas de Soporte e Insights

### La Secuencia Importa: No Es una Checklist Plana

El orden es crítico para garantizar resultados fiables:
- D-07 → D-08 → D-01/D-02 → D-03/D-05 → D-04/D-09 → D-06 → D-10.

Omitir pasos o alterar el flujo puede provocar análisis sobre datos incompletos y errores graves de gestión.

### 60–90 Minutos Solo Son Viables con Automatización

Estos tiempos consideran ciertas tareas automatizadas (alertas de conectores, generación de dashboard), pero siempre requieren supervisión humana para interpretación y escalamiento.

### Los Outputs del Día Alimentan la Mejora Semanal

Falsos positivos, tendencias y anomalías detectadas a diario, conforman los insumos del proceso semanal de tuning, análisis y corrección continua.

### Dominancia de NIST CSF 2.0 en DETECT y RESPOND

El ciclo diario cubre principalmente competencias de detección y respuesta (DE.CM, RS.CO), siguiendo el marco NIST y estándares consultivos de Microsoft [NIST Cybersecurity Reference Architecture](https://learn.microsoft.com/en-us/security/compass/nist-cybersecurity-reference).

### Pregunta Esencial de Cada Actividad

| Actividad | Pregunta Esencial                                        |
| --------- | -------------------------------------------------------- |
| D-07      | ¿Estoy viendo todo lo que debería ver?                   |
| D-08      | ¿Alguien cambió los controles sin permiso?               |
| D-01      | ¿Qué alertó Purview hoy?                                 |
| D-02      | ¿Qué más ocurrió que Purview no alertó?                  |
| D-03      | ¿Las señales combinadas revelan algo más grave?          |
| D-05      | ¿Hay datos sensibles en lugares donde no deberían estar? |
| D-04      | ¿El cifrado sigue funcionando correctamente?             |
| D-09      | ¿Alguien está golpeando las puertas cerradas?            |
| D-06      | ¿Hay incidentes que se están demorando demasiado?        |
| D-10      | ¿Cómo cierra el día y qué debo comunicar?                |

---

## Conexiones Externas (hacia otros Bloques)

| Conexión                                           | Destino   | Naturaleza                                            |
| -------------------------------------------------- | --------- | ----------------------------------------------------- |
| El ciclo diario opera sobre el stack tecnológico   | Bloque 3  | Uso de componentes del stack                          |
| Principios operativos se reflejan en actividades   | Bloque 2  | Telemetría, correlación, auditoría evidenciable       |
| Outputs diarios alimentan el ciclo semanal         | Bloque 5  | FP → S-01; tendencias → S-02; correlaciones → S-07    |
| Actividades pueden escalar a procedimientos ad-hoc | Bloque 7  | Las detecciones pueden activar respuesta inmediata    |
| KPIs reportados en dashboard maestro               | Bloque 8  | Indicadores consolidan el estado de control           |
| Formación de Nivel 1 habilita las actividades      | Bloque 10 | Capacitación previa requerida para ejecución correcta |
| Mapeo a NIST CSF 2.0                               | Bloque 9  | Clasificación y cumplimiento normativo                |
