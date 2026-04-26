---
layout: page
title: "MS_Purview_IP-SecOps_e01v01_Art09-MS_CSU_Security_MCSA"
permalink: /ms-purview/mip-secops/ms-purview-ip-secops-e01v01-art09-ms-csu-security-mcsa/
---

# MS_Purview_IP-SecOps_e01v01_Art09-MS_CSU_Security_MCSA

## 

Este documento presenta un mapa técnico basado en el estándar NIST CSF 2.0, enfocado en la protección de datos mediante Microsoft Purview Information Protection y herramientas relacionadas. El contenido desarrolla una estructura de 35 actividades, distribuidas en las funciones clave del marco NIST: gobernanza, detección, protección, identificación, respuesta y recuperación. El propósito central es facilitar la implementación operativa y consultiva de controles de seguridad, alineando cada actividad con las categorías y valores del estándar CSF 2.0. Además, se incluyen notas de soporte para distinguir recomendaciones de prácticas oficiales, y se puntualizan criterios de madurez y cobertura consultiva, manteniendo claridad tanto en la adopción como en la adaptación de estos procedimientos [NIST CSF 2.0 Overview](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.28.pdf).

El documento estructura las actividades de protección de datos siguiendo el marco NIST CSF 2.0, e introduce la función GOVERN como elemento envolvente que contextualiza el resto de las funciones. Aquí, ocho actividades de gobernanza aseguran autorizaciones, visibilidad operativa y auditoría, usando Microsoft Purview para validar procesos, reportar posturas y optimizar costos de logging. En la función DETECT, doce actividades diarias, semanales y mensuales abordan la vigilancia operativa con herramientas como Sentinel, Purview y Defender. Se destacan revisiones de alertas, correlaciones, tuning de reglas, y validaciones de cobertura de logging para mantener la integridad de la detección de incidentes y anomalías. Las funciones PROTECT e IDENTIFY forman la base preventiva. Cinco actividades de protección cubren controles de cifrado, validaciones de auto-labeling, pruebas de acceso y evaluaciones de cifrado persistente, mientras que dos actividades de identificación se enfocan en el inventario de datos clasificados y on-premise, apoyando la gestión preventiva. La función RESPOND agrupa seis acciones enfocadas en la contención y gestión de incidentes, con métricas de respuesta, análisis forense y escalamiento legal para mejorar la coordinación y la mitigación. RECOVER, finalmente, promueve la mejora continua post-incidente, con actividades semanales, mensuales y ad-hoc, dedicadas a documentar lecciones aprendidas, ejercicios de respuesta y cierre formal de incidentes. Las notas de soporte aclaran que la distribución de actividades es un perfil consultivo, no estándar oficial. Hay actividades puente que aportan valor simultáneo a múltiples funciones, y la progresión del perfil de madurez es una recomendación interpretativa, no un reemplazo de auditoría formal. Todo el material está preparado para uso técnico, claro y consultivo, listo para guiar la implementación operativa en entornos Microsoft y alineado al marco NIST CSF 2.0 [NIST CSF 2.0 Overview](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.28.pdf).



# Mapa NIST CSF 2.0: 35 Actividades Alineadas

## NIVEL 2 — Conceptos Clave

### GOVERN — Función Envolvente (8 Actividades)

NIST CSF 2.0 introduce la función GOVERN, que envuelve y contextualiza las demás funciones. El modelo consultivo mapea 8 actividades de gobernanza relevantes para la protección de datos en Microsoft Purview Information Protection.

| ID   | Actividad                             | Cadencia | Categoría CSF 2.0 | Propósito de Gobernanza                                   |
| ---- | ------------------------------------- | -------- | ----------------- | --------------------------------------------------------- |
| D-08 | Validación de cambios administrativos | Diaria   | GV.PO             | Ningún cambio ocurre sin autorización documentada         |
| D-10 | Actualización del tablero operativo   | Diaria   | GV.OC             | Visibilidad continua del estado del programa              |
| S-10 | Reporte semanal de postura            | Semanal  | GV.OC             | Comunicación estructurada a stakeholders                  |
| M-01 | Revisión de taxonomía de etiquetas    | Mensual  | GV.OC             | Fundamentos alineados con realidad actual                 |
| M-03 | Auditoría de cambios del mes          | Mensual  | GV.PO             | Evidencia de control de cambios para auditorías           |
| M-07 | Optimización de costos de logging     | Mensual  | GV.RM             | Sostenibilidad financiera del programa                    |
| M-09 | Reporte ejecutivo mensual             | Mensual  | GV.OC             | Conexión del SOC con gestión de riesgo corporativo        |
| M-10 | Revisión de cumplimiento CSF 2.0      | Mensual  | GV.OC             | Autoevaluación normalizada de madurez (modelo consultivo) |

### DETECT — Núcleo Operativo (12 Actividades)

DETECT concentra la vigilancia continua a través de herramientas como Microsoft Sentinel, Purview y Defender.

| ID   | Actividad                                        | Cadencia | Categoría CSF 2.0 |
| ---- | ------------------------------------------------ | -------- | ----------------- |
| D-01 | Revisión de alertas de Purview                   | Diaria   | DE.CM             |
| D-02 | Monitoreo Audit Log — eventos MIP                | Diaria   | DE.CM             |
| D-03 | Correlación en Microsoft Sentinel                | Diaria   | DE.CM             |
| D-05 | Revisión de eventos de oversharing               | Diaria   | DE.CM             |
| D-07 | Integridad de conectores Purview→Sentinel        | Diaria   | DE.CM             |
| D-09 | Revisión de accesos fallidos                     | Diaria   | DE.CM             |
| S-01 | Tuning de falsos positivos                       | Semanal  | DE.CM             |
| S-07 | Tuning de reglas en Sentinel                     | Semanal  | DE.CM             |
| S-08 | Validación de cobertura de logging               | Semanal  | DE.CM             |
| M-08 | Validación integración Purview–Sentinel–Defender | Mensual  | DE.CM             |
| AH-A | Investigación de alerta crítica                  | Ad-hoc   | DE.CM             |

### PROTECT + IDENTIFY — Base Preventiva (6 Actividades)

PROTECT e IDENTIFY cubren controles de prevención y inventario usando Purview Information Protection.

#### PROTECT (5 Actividades)
| ID   | Actividad                           | Cadencia | Categoría CSF 2.0 | Qué Protege                     |
| ---- | ----------------------------------- | -------- | ----------------- | ------------------------------- |
| D-04 | Validación de controles Azure RMS   | Diaria   | PR.DS             | Integridad del cifrado          |
| S-03 | Validación de auto-labeling         | Semanal  | PR.IP             | Clasificación automatizada      |
| S-05 | Pruebas de acceso controlado        | Semanal  | PR.DS             | Uso real de usage rights        |
| S-09 | Revisión de cuentas privilegiadas   | Semanal  | PR.AC             | Control de acceso               |
| M-02 | Efectividad del cifrado persistente | Mensual  | PR.DS             | Evaluación integral del control |

#### IDENTIFY (2 Actividades)
| ID   | Actividad                   | Cadencia | Categoría CSF 2.0 | Qué Identifica                   |
| ---- | --------------------------- | -------- | ----------------- | -------------------------------- |
| S-02 | Tendencias de clasificación | Semanal  | ID.AM             | Inventario de datos clasificados |
| M-06 | Cobertura del Scanner       | Mensual  | ID.AM             | Inventario de datos on-premise   |

### RESPOND — Acción de Contención (6 Actividades)

RESPOND abarca la gestión y contención de incidentes de datos.

| ID   | Actividad                          | Cadencia | Categoría CSF 2.0 | Fase de Respuesta                  |
| ---- | ---------------------------------- | -------- | ----------------- | ---------------------------------- |
| D-06 | Seguimiento de incidentes abiertos | Diaria   | RS.CO             | Coordinación y gestión de SLA      |
| S-04 | Revisión de revocaciones de acceso | Semanal  | RS.MI             | Mitigación retrospectiva           |
| M-04 | Métricas MTTA/MTTR                 | Mensual  | RS.MI             | Medición de velocidad de respuesta |
| AH-B | Contención: revocación y bloqueo   | Ad-hoc   | RS.MI             | Mitigación inmediata               |
| AH-C | Análisis forense de actividades    | Ad-hoc   | RS.AN             | Reconstrucción de incidentes       |
| AH-D | Escalamiento legal/compliance      | Ad-hoc   | RS.CO             | Notificación regulatoria           |

### RECOVER — Mejora After Incident (3 Actividades)

RECOVER promueve la mejora continua post-incidente.

| ID   | Actividad                            | Cadencia | Categoría CSF 2.0 | Mecanismo de Recuperación           |
| ---- | ------------------------------------ | -------- | ----------------- | ----------------------------------- |
| S-06 | Lecciones aprendidas                 | Semanal  | RC.IM             | Acciones correctivas de incidentes  |
| M-05 | Ejercicio tabletop de fuga de datos  | Mensual  | RC.IM             | Entrenamiento de respuesta          |
| AH-E | Post-mortem y comunicación ejecutiva | Ad-hoc   | RC.IM             | Cierre formal y mejora comprometida |

## NIVEL 3 — Notas de Soporte

### Perfil Consultivo de Cobertura

La distribución por función (GOVERN: 8; IDENTIFY: 2; PROTECT: 5; DETECT: 12; RESPOND: 6; RECOVER: 3) es guía consultiva y no estándar oficial de Microsoft ni NIST.

### Actividades Puente

Algunas actividades aportan valor a más de una función. Ejemplo:
- D-07 (DETECT/GOVERN): Detecta brechas y valida gobernanza.
- S-04 (RESPOND/DETECT): Verifica efectividad de respuesta.
- M-08 (DETECT/PROTECT): Valida protección de datos y monitoreo.
- AH-E (RECOVER/GOVERN): Mejora y documenta gobernanza del proceso.

### Decisión de Exclusión de Categorías

No se cubren en profundidad ID.RA, PR.AT y RC.RP, priorizando actividades operativas de protección de datos.

### Scorecard de Evaluación Consultivo

M-10 y la escala de madurez (“Initial”, “Repeatable”, “Defined”, “Managed”, “Optimizing”) son recomendaciones consultivas y no instrumentos oficiales de Microsoft.

### Visualización del Perfil de Madurez

La progresión del perfil de cobertura como señal de madurez es una propuesta interpretativa, no sustituto de auditoría formal.

---

Referencias:
- [NIST CSF 2.0 Overview](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.28.pdf)

