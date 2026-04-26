---
layout: page
title: "MS_Purview_IP-SecOps_e01v01_Art08-MS_CSU_Security_MCSA"
permalink: /ms-purview/mip-secops/ms-purview-ip-secops-e01v01-art08-ms-csu-security-mcsa/
---

# MS_Purview_IP-SecOps_e01v01_Art08-MS_CSU_Security_MCSA

## 

Este documento aborda la estructura y finalidad de un tablero de control diseñado específicamente para un Centro de Operaciones de Seguridad (SOC). El enfoque central radica en la integración de indicadores clave de desempeño (KPIs) y señales de degradación. Las tecnologías involucradas incluyen Microsoft Sentinel, Purview, y Defender, con énfasis en prácticas de protección de la información, monitoreo operativo y gobernanza. El documento busca facilitar la gestión proactiva y reactiva de la seguridad, destacando la importancia de medir tanto el rendimiento actual como posibles fallos futuros. Está dirigido a equipos de seguridad que necesitan reportar a partes interesadas y anticipar problemas, manteniendo la excelencia operacional del SOC.

El tablero de control propuesto combina once KPIs principales, que permiten evaluar y reportar el estado actual del SOC, con seis señales de degradación que anticipan puntos de falla. Los KPIs de velocidad, como el tiempo medio para detectar y contener incidentes (MTTA y MTTR), muestran qué tan rápido reacciona el equipo. Además, se mide cobertura de clasificación de documentos, ratio de verdaderos positivos en alertas, incidentes contenidos sin pérdida de datos, cambios administrativos auditados, uptime de conectores, porcentaje de auto-etiquetado y formación del personal. Estas métricas ayudan tanto en la gestión diaria como en la toma de decisiones a nivel ejecutivo. Por otro lado, las señales de degradación permiten detectar problemas emergentes, como caída significativa en la cobertura de clasificación, aumento en falsos positivos, baja en el volumen de logs de auditoría, incremento sostenido en el tiempo de detección, reincidencia de incidentes iguales, o ausencia de revocaciones tras incidentes. Los umbrales sugeridos para activar estas alertas deben adaptarse al contexto y no son estándares rígidos. La lectura integrada del tablero permite un diagnóstico cruzado, identificando patrones que definen el nivel de madurez del SOC. Se enfatiza que la segmentación de KPIs según audiencia, el volumen de logs como indicador vital, la revisión adaptada a cada empresa y el cálculo consultivo del retorno de inversión son buenas prácticas para asegurar una gestión eficaz y responsable. El tablero se conecta con otros bloques del marco consultivo del SOC, garantizando alineación operativa, evidencia auditable y reporting consolidado para la toma de decisiones estratégicas y validación de progresos. Todo el sistema, basado en tecnologías Microsoft, busca prevenir incidentes y mejorar continuamente la postura de seguridad del entorno operativo.





# Bloque 08/11 — Tablero de Control: KPIs y Señales de Degradación

## Jerarquización Interna

### El Core (Insight Dominante)

Lo que no se mide no se gestiona, pero lo que solo se mide retrospectivamente se gestiona tarde. Este tablero opera en dos horizontes: los 11 KPIs primarios miden la excelencia operacional del SOC en el presente y responden "¿cómo estamos hoy?", mientras que las 6 señales de degradación anticipan problemas antes de que se conviertan en incidentes, respondiendo "¿qué está a punto de fallar?". Un SOC que solo monitorea KPIs reacciona; uno que también observa señales de degradación puede prevenir.

Los KPIs sirven como mecanismo de reporte para stakeholders externos, mientras que las señales de degradación son herramientas internas de monitoreo proactivo, permitiendo intervención temprana.

---

### Conceptos Clave

#### KPIs de Velocidad — ¿Qué Tan Rápido Detectamos y Contenemos? (MTTA + MTTR)

La velocidad de detección y respuesta es uno de los principales indicadores observados. Se recomienda el seguimiento de cuatro KPIs:

| KPI             | Definición Operativa                                         | Objetivo de Madurez (Recomendado) | Frecuencia |
| --------------- | ------------------------------------------------------------ | --------------------------------- | ---------- |
| MTTA — Critical | Desde el evento hasta la detección y reconocimiento por el SOC | < 1 hora                          | Mensual    |
| MTTA — High     | Mismo concepto, severidad High                               | < 4 horas                         | Mensual    |
| MTTR — Critical | Desde la detección hasta la contención efectiva confirmada   | < 4 horas                         | Mensual    |
| MTTR — High     | Mismo concepto, severidad High                               | < 8 horas                         | Mensual    |

[Referencia general: Microsoft Sentinel — Detección y respuesta SOC](https://learn.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats)

- MTTA refleja eficiencia de detección (reglas, conectores, calibración de alertas).
- MTTR mide la rapidez y efectividad operativa en la contención.

La tendencia trimestral es más significativa que el valor puntual. Una degradación sostenida, incluso "dentro de objetivo", es señal de erosión sistemática.

---

#### KPIs de Cobertura y Calidad — ¿Cuánto Protegemos y Cuán Bien Detectamos?

- **Cobertura de clasificación** (% de documentos clasificados)
  - Documentos con etiqueta de sensibilidad dividido por total de documentos activos.
  - Objetivo recomendado: >85% (E5), >70% (E3), según modelo madurez y contexto.
  - [Microsoft Purview Information Protection Overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/information-protection-overview)

- **Ratio de verdaderos positivos (VP) en alertas**
  - Porcentaje de alertas reales sobre el total de alertas detectadas.
  - Objetivo recomendable: >70%.
  - [Mejora de precisión de alertas](https://learn.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats)

- **Incidentes contenidos sin pérdida confirmada**
  - Incidentes donde el cifrado persistente (Azure RMS) evitó exposición de datos sobre el total de incidentes de datos.
  - Objetivo recomendable: >95%.
  - [Encryption in Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365/compliance/encryption-of-content-in-microsoft-365)

---

#### KPIs de Gobernanza y Continuidad — ¿Los Controles Están Gobernados y la Infraestructura Es Confiable?

- **Cambios administrativos con ticket aprobado**
  - Cambios en etiquetas, políticas y configuración con aprobación documentada sobre el total de cambios.
  - Objetivo recomendable: 100%.
  - [Change management guidance](https://learn.microsoft.com/en-us/azure/security/fundamentals/change-management)

- **Uptime de conectores**
  - Disponibilidad de conectores Purview/M365/Defender activos vs. horas totales del periodo.
  - Objetivo recomendado: >99.5%.
  - [Sentinel data connectors](https://learn.microsoft.com/en-us/azure/sentinel/connect-data-sources)

- **Porcentaje de auto-labeling (tendencia)**
  - Documentos etiquetados automáticamente sobre el total etiquetado.
  - Debe monitorearse la tendencia; no existe umbral universal.
  - [Auto-labeling overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/auto-labeling-overview)

---

#### KPI de Formación — ¿El Equipo Está Preparado? + Sistema de Alerta Temprana

- **Porcentaje de equipo con formación completada**
  - Analistas con capacitación cumplida respecto al total.
  - Objetivo recomendado: >90%.
  - [Security training guidance](https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/security-training-awareness)

**Sistema de Alerta Temprana: Las 6 señales del modelo**
- Señales y umbrales sugeridos son recomendaciones de monitoreo, no estándares oficiales.
- Ejemplos:
  - Caída de cobertura de clasificación sustancial.
  - Aumento en falsos positivos.
  - Disminución en el volumen de Audit Log.
  - Aumento sostenido en MTTA.
  - Recurrencia de incidentes similares.
  - Ausencia de revocaciones pese a incidentes activos.

Los umbrales sugeridos para activar señales (ej. >5% de caída, >10% de aumento) son orientativos y deben adaptarse al entorno operativo.

---

#### Lectura Integrada del Tablero — ¿Cómo Se Lee Todo Junto?

La interpretación cruzada de los KPIs y las señales permite identificar patrones como: "SOC rápido pero ciego", "SOC exhausto", "SOC desarmado", "SOC sin fundamentos", y "SOC maduro". Estos fundamentos se basan en benchmarking consultivo y no corresponden a designaciones oficiales.

---

### Notas de Soporte e Insights

- **Insight A:** Los KPIs segmentados según audiencia sirven para ajuste organizacional y cumplimiento, no como norma oficial.
- **Insight B:** El descenso del volumen de Audit Log es una señal crítica que afecta la confiabilidad de todos los KPIs.
- **Insight C:** No se deben medir ciertos indicadores que pueden incentivar malas prácticas.
- **Insight D:** La frecuencia de revisión de cada KPI debe alinearse a visibilidad y régimen de cada organización.
- **Insight E:** Calcular el ROI basándose en KPIs históricos y costos es una práctica consultiva sin fórmula de Microsoft.

---

## Conexiones Externas

| Conexión                                            | Destino         | Naturaleza                                            |
| --------------------------------------------------- | --------------- | ----------------------------------------------------- |
| KPIs se alimentan de actividades específicas        | Bloques 4, 5, 6 | Enlace consultivo entre operación y monitoreo         |
| KPIs de velocidad aplican a respuesta ad-hoc        | Bloque 7        | Aplicación operacional de procedimientos              |
| Principios medidos de forma transversal             | Bloque 2        | Telemetría y evidencia auditable como foco de control |
| Stack tecnológico monitoreado integralmente         | Bloque 3        | Medición de uptime, ingesta e integridad              |
| Señales de degradación disparan actividades         | Bloques 4, 5    | Conexión ante eventos de riesgo                       |
| Reportes ejecutivos y scorecards se construyen aquí | Bloque 6        | Consolidación para reporting                          |
| Mapa NIST CSF organiza KPIs                         | Bloque 9        | Alineación consultiva de función/KPI                  |
| KPI de formación enlazado a competencias            | Bloque 10       | KPI vinculado a madurez por nivel del equipo          |
| KPIs históricos validan plan progresivo             | Bloque 11       | Uso de métricas para validación de madurez            |
