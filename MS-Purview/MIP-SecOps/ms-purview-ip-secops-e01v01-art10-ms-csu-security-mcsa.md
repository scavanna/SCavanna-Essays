---
layout: page
title: "MS_Purview_IP-SecOps_e01v01_Art10-MS_CSU_Security_MCSA"
permalink: /ms-purview/mip-secops/ms-purview-ip-secops-e01v01-art10-ms-csu-security-mcsa/
---

# MS_Purview_IP-SecOps_e01v01_Art10-MS_CSU_Security_MCSA



Este documento presenta un programa de formación para analistas de ciberseguridad, diseñado en tres niveles de desarrollo profesional: junior, intermedio y liderazgo operativo. Se estructura en módulos alineados con tecnologías y recursos oficiales de Microsoft, como Microsoft Information Protection, Azure Rights Management y Microsoft Sentinel. La finalidad es guiar una progresión clara, verificable y auditable, desde la comprensión básica hasta la autonomía operativa y la capacidad de liderazgo en gestión de incidentes y cumplimiento. Se apoya en certificaciones, entrenamientos comunitarios y documentación técnica actualizada, generando evidencia efectiva para auditorías y mejorando la resiliencia y retención de talento.

El nivel inicial, dirigido a analistas junior, se enfoca en que los participantes logren una comprensión práctica de los fundamentos de protección de información en el entorno Microsoft, incluyendo taxonomía de etiquetas, monitoreo de logs y gestión básica de incidentes, todo bajo supervisión. Se utilizan certificaciones, documentación oficial y materiales audiovisuales, enfatizando conocimiento funcional más que autonomía. En el segundo nivel, para analistas intermedios, el trayecto intensifica el aprendizaje sobre operaciones autónomas. Aquí, se suman recursos avanzados como guías operativas de Sentinel, enfoques de correlación, autoetiquetado y búsqueda avanzada de logs. Al completarlo, los analistas pueden gestionar el ciclo completo diario y semanal y realizar investigaciones iniciales de incidentes con mínima supervisión. El tercer nivel está orientado al liderazgo y cubre habilidades de diseño de reglas, ejercicios de simulación, elaboración de reportes y responsabilidad ante auditorías. Se introducen entrenamientos comunitarios avanzados, guías de optimización y alineamiento consultivo con estándares internacionales como NIST CSF. A lo largo de los tres niveles, el programa se alimenta de recursos verificables, desde la documentación técnica de Microsoft hasta guías externas, asegurando la trazabilidad y auditabilidad con badges, transcripciones y registros firmados. Además, se detallan las conexiones entre cada nivel, los recursos clave y cómo cada etapa habilita nuevas capacidades operativas dentro del equipo. Finalmente, el enfoque del programa previene la dependencia del conocimiento no documentado y prioriza la formación mapeada a actividades reales del puesto, lo que facilita la medición de madurez y aporta sustentabilidad y valor organizacional.



# Programa de Formación: De Junior a Lead en 3 Niveles

## Nivel 2 — Conceptos Clave

### Concepto 1: Nivel 1 — Analista Junior: Los Fundamentos (Semanas 1–4)

El Nivel 1 tiene como objetivo que el analista junior comprenda los fundamentos de Microsoft Information Protection (MIP) y ejecute actividades de monitoreo diario con supervisión. No se busca autonomía, sino competencia funcional rápida para contribuir al ciclo operativo.

#### Ruta de formación

| Prioridad | Recurso                                              | Tipo                    | Tiempo | Qué Habilita                                           |
| --------- | ---------------------------------------------------- | ----------------------- | ------ | ------------------------------------------------------ |
| 1         | SC-900: Security, Compliance & Identity Fundamentals | Certificación Microsoft | 6–8h   | Base conceptual: identidad, compliance, seguridad M365 |
| 2         | Learn about sensitivity labels                       | Documentación oficial   | 2h     | Comprensión de taxonomía y etiquetado                  |
| 3         | Audit log activities in Microsoft Purview            | Documentación oficial   | 2h     | Navegación y filtro del Audit Log                      |
| 4         | Microsoft Mechanics – Information Protection         | Video YouTube oficial   | 30min  | Stack MIP en formato accesible                         |
| 5         | Sensitivity labels for Teams, Groups and Sites       | Documentación oficial   | 1.5h   | Oversharing y container labels                         |
| 6         | Rights Management usage rights                       | Documentación oficial   | 1.5h   | Cifrado y derechos de uso                              |

**Tiempo estimado:** 14–16 horas (2 semanas).

**Competencias verificables:** Ejecutar actividades de monitoreo básico: revisión de alertas, monitoreo de audit log, eventos de oversharing, seguimiento de incidentes, accesos fallidos. El analista actúa bajo supervisión.

**Limitaciones:** No cubre correlación en Sentinel, auto-labeling, forense ni liderazgo de incidentes. Estos requieren la base conceptual de Nivel 1.

---

### Concepto 2: Nivel 2 — Analista Mid-Level: Operación Autónoma (Meses 2–6)

El Nivel 2 permite operar el ciclo diario completo y participar en el ciclo semanal y respuesta a incidentes. El analista detecta, interpreta y actúa de forma autónoma.

#### Ruta de formación

| Prioridad | Recurso                                     | Tipo                  | Tiempo | Qué Habilita                    |
| --------- | ------------------------------------------- | --------------------- | ------ | ------------------------------- |
| 1         | Azure Rights Management service             | Documentación oficial | 3h     | Cifrado, revocación, tracking   |
| 2         | Microsoft Sentinel operational guide        | Documentación oficial | 4h     | Correlación, incidentes, reglas |
| 3         | Automatically apply sensitivity labels      | Documentación oficial | 2h     | Validar y ajustar políticas     |
| 4         | Search the audit log in Microsoft Purview   | Documentación oficial | 2h     | Búsqueda avanzada               |
| 5         | Best practices for Microsoft Sentinel       | Documentación oficial | 2h     | Tuning de reglas, optimización  |
| 6         | Microsoft Sentinel deep dive                | Video YouTube oficial | 1h     | Refuerzo de Sentinel            |
| 7         | Streamlining Compliance with Purview Alerts | Blog técnico          | 45min  | Alertas de compliance           |

**Tiempo estimado:** 15–16 horas (4–6 semanas).

**Competencias verificables:** Ejecutar todas las actividades diarias y semanales de forma autónoma. Ejecutar investigación y contención inicial con supervisión.

**Diferencia clave:** Correlación multi-fuente y síntesis de señales anómalas para identificar patrones avanzados.

---

### Concepto 3: Nivel 3 — Analista Senior / Lead SOC: Liderazgo Operativo (Mes 6+)

El Nivel 3 transforma al analista en líder operativo y estratégico. Dirige el diseño de reglas, ejercicios de tabletop, reportes ejecutivos, escalamiento legal y representa ante auditoría.

#### Ruta de formación

| Prioridad | Recurso                                             | Tipo                                | Tiempo | Qué habilita                                 |
| --------- | --------------------------------------------------- | ----------------------------------- | ------ | -------------------------------------------- |
| 1         | Microsoft Sentinel Ninja Training (Community Route) | Entrenamiento comunitario Microsoft | 8–10h  | Sentinel avanzado (no certificación oficial) |
| 2         | SOC optimization reference                          | Documentación oficial               | 2h     | Optimización del programa de métricas        |
| 3         | Deploy the Information Protection scanner           | Documentación oficial               | 3h     | Expansión de cobertura on-premises           |
| 4         | Microsoft Expanded Cloud Logs Playbook (CISA)       | Guía gubernamental                  | 2h     | Logging avanzado, retención, forense         |
| 5         | Microsoft Purview Information Protection overview   | Documentación oficial               | 2h     | Visión integral para reportes ejecutivos     |
| 6         | How to audit Microsoft 365 with Purview             | Blog técnico                        | 45min  | Perspectiva práctica de auditoría            |

**Tiempo estimado:** 18–20 horas (4–8 semanas).

**Competencias verificables:** Liderar todas las actividades del modelo, preparar reportes ejecutivos, liderar tabletop, escalar legalmente, mapear controles consultivamente a NIST CSF 2.0, coordinar análisis forense, optimizar correlación avanzada en Sentinel. El mapeo a NIST CSF es consultivo, no estándar Microsoft.

---

### Concepto 4: Ecosistema de Recursos — 20 Fuentes Verificables

La formación está basada en recursos oficiales y verificables: documentación Microsoft, certificaciones, entrenamiento comunitario, guías de CISA y blogs técnicos. Cada recurso está mapeado a actividades operativas concretas.

#### Distribución por tipo

| Tipo de Recurso                    | Cantidad | Ejemplos                                                     | Valor Diferencial                                       |
| ---------------------------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------- |
| Documentación oficial Microsoft    | 11       | Audit log, Azure RMS, Sentinel ops guide                     | Actualizada, verificable                                |
| Entrenamiento formal y comunitario | 2        | SC-900, Sentinel Ninja Training (community, no certificación) | Badge/transcripción oficial, reconocimiento comunitario |
| Videos oficiales Microsoft         | 2        | Mechanics MIP, Sentinel deep dive                            | Onboarding visual                                       |
| Guías gubernamentales              | 1        | CISA Expanded Cloud Logs Playbook                            | Seguridad nacional                                      |
| Blogs técnicos                     | 2        | Zure.com, M365.fm                                            | Implementación práctica                                 |

#### Mapeo a horizontes operativos

- **Actividades diarias:** Recursos base y monitoreo (Audit log, Sentinel ops, Azure RMS)
- **Actividades semanales:** Recursos de optimización (Best practices, auto-labeling, CISA Playbook)
- **Actividades mensuales:** Recursos de liderazgo (Purview IP overview, mapeo consultivo a NIST CSF 2.0)
- **Procedimientos ad-hoc:** Recursos de respuesta (Audit log search, Azure RMS, CISA Playbook)

---

### Concepto 5: Evidencia de Formación Auditable

El programa genera evidencia verificable para auditoría: badges, transcripciones, rutas comunitarias, registros internos, y actas de ejercicios prácticos.

#### Tipos de evidencia

| Tipo                                      | Qué demuestra        | Cómo se obtiene            | Audiencia                                        |
| ----------------------------------------- | -------------------- | -------------------------- | ------------------------------------------------ |
| Microsoft Learn badges                    | Completar módulos    | Microsoft Learn            | Auditores, SOC2                                  |
| Transcripción SC-900                      | Certificación formal | Microsoft Certification    | Reguladores, contratación                        |
| Sentinel Ninja Training (Community Route) | Dominio en Sentinel  | Blog oficial               | Técnicos, comunidad (no certificación Microsoft) |
| Registro interno                          | Lectura y progreso   | Documento firmado          | Auditoría interna                                |
| Acta de tabletop                          | Ejercicio práctico   | Documento firmado por Lead | SOC2, resiliencia                                |

#### Cadena de evidencia por nivel

```
Nivel 1: SC-900 transcripción, badges, registro interno
Nivel 2: Badges extra, Sentinel Ninja Community, registro interno, actas tabletop
Nivel 3: Sentinel Ninja Community nivel Expert, registro avanzado, certificados de liderazgo, reportes ejecutivos, mapeo consultivo a NIST CSF 2.0
```

La evidencia está alineada con auditoría y resiliencia operacional.

---

## Nivel 3 — Notas de Soporte e Insights

- **Inversión de tiempo:** 48–52 horas/analista en 6 meses para competencia completa; valor superior a depender de conocimiento tribal.
- **Recurso pivotal:** Sentinel Ninja Training (Community Route) transfiere dominio avanzado de Sentinel; reconocimiento comunitario, no certificación oficial [Referencia: Blog Sentinel Ninja Training Community](https://techcommunity.microsoft.com/t5/microsoft-sentinel-blog/microsoft-sentinel-ninja-training/ba-p/2119327).
- **Retención de talento:** Formación estructurada con certificaciones y rutas comunitarias aporta valor organizacional y profesional.
- **Anti-patrón:** No priorizar cursos genéricos, sino formación directamente mapeada a actividades operativas.
- **Progresión del equipo:** Madurez consultiva medida por niveles alcanzados en el equipo y alineada con fases de implementación (no estándar Microsoft).

---

## Conexiones Externas

| Conexión                                                     | Destino         |
| ------------------------------------------------------------ | --------------- |
| Nivel 1 habilita actividades diarias básicas                 | Bloque 4        |
| Nivel 2 habilita ciclo completo diario/semana y respuesta inicial | Bloques 4, 5, 7 |
| Nivel 3 habilita gobernanza mensual y liderazgo ad-hoc       | Bloques 6, 7    |
| Recursos mapeados a stack                                    | Bloque 3        |
| Competencias requeridas por principios operativos            | Bloque 2        |
| KPI consultivo de formación                                  | Bloque 8        |
| Mapeo consultivo a NIST CSF 2.0                              | Bloque 9        |
| Fases de implementación requieren por nivel                  | Bloque 11       |

---



