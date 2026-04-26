---
layout: page
title: "MS_Purview_M365E5-SecOps_e03v01_Art06-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e03/ms-purview-m365e5-secops-e03v01-art06-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e03v01_Art06-MS_CSU_Security_MCSA

El documento describe un roadmap de implementación por niveles de madurez para Microsoft Purview Suite en entornos Microsoft 365. Integra capacidades de clasificación y protección de información, prevención de fuga de datos, auditoría avanzada y gestión del ciclo de vida. Suma herramientas de riesgo interno, cumplimiento normativo, soberanía de claves, gobierno analítico y adopción segura de inteligencia artificial y Copilot. La guía propone tres niveles temporales, de 0 a 90, 90 a 180 y más de 180 días. Cada nivel incluye hitos, principios, métricas y dependencias técnicas, además de recomendaciones de priorización y narrativa para dirección y auditores.

El Nivel 1 se centra en ver antes de proteger y construir la base mínima de control. Incluye Information Protection con etiquetado manual, D L P básico en correos y archivos, y Audit Premium. Se añade Compliance Manager para un puntaje inicial de cumplimiento y evaluaciones alineadas con NIST y la norma ISO 27001. Un inventario básico de postura de datos detecta sobreexposición antes de Copilot, y se definen métricas de cobertura.

El Nivel 2 lleva los controles desde la acción manual hacia la automatización y la inteligencia operativa sobre los datos. Activa auto etiquetado con modelos de aprendizaje automático y datos estructurados, ampliando la clasificación iniciada en el Nivel 1. Se despliega Endpoint D L P en dispositivos, Insider Risk Management para analizar comportamiento, y Communication Compliance para supervisar comunicaciones críticas. Data Lifecycle Management se vuelve avanzado, con ámbitos adaptativos, retención por eventos y KPIs de clasificación automática y cobertura.

El Nivel 3 explota todo el potencial de Purview cuando los niveles anteriores ya son estables y generan datos de calidad. Introduce Adaptive Protection, que ajusta restricciones según riesgo, e Information Barriers, que separa grupos para cumplir segregación de funciones. Customer Key y Lockbox refuerzan la soberanía de datos, mientras DSPM para inteligencia artificial asegura Copilot antes de producción. Security Copilot se integra con Purview para investigación forense rápida, y Fabric aporta gobierno unificado sobre datos analíticos y colaborativos.

El mapa de dependencias muestra cómo ciertas capacidades básicas habilitan cadenas completas de funciones más avanzadas dentro del roadmap. Information Protection alimenta D L P, auto etiquetado y gobierno del ciclo de vida; Audit Premium sostiene riesgo interno y comunicaciones. D L P básico precede a Endpoint D L P y a Adaptive Protection, que combinan contexto de usuario y dispositivo. Por eso, con recursos limitados, la secuencia prioritaria es implementar primero Information Protection, luego Audit Premium y después D L P básico.

El último concepto traduce la madurez técnica en indicadores claros, con KPIs distintos para visibilidad, control y optimización avanzada. Se definen métricas, fuentes y frecuencias, usando paneles de Purview para clasificación, D L P, riesgo interno y postura de datos. El Compliance Score ofrece a seguridad y finanzas evidencia cuantificable, alineada con marcos reconocidos como NIST y la norma ISO 27001. Las notas finales aclaran alcance consultivo, particularidades reguladas en Latinoamérica, aceleradores como Defender para endpoint y la narrativa por nivel hacia el directorio.





# Roadmap de Implementación de Microsoft Purview Suite por Madurez

## Nivel 2 - Conceptos Clave

### Concepto 1 - Nivel 1: Fundamentos (Días 0-90)

**Ver antes de proteger**

El Nivel 1 establece la infraestructura de visibilidad y control básico. Sin estos fundamentos, las capacidades avanzadas no operan de forma fiable. Algunas piezas pueden ejecutarse con M365 E3, pero el potencial completo se alcanza con Purview Suite.

#### Hitos del Nivel 1

| #    | Hito                                     | Capacidad activada                              | Dependencia técnica                          | Impacto inmediato                                 |
| ---- | ---------------------------------------- | ----------------------------------------------- | -------------------------------------------- | ------------------------------------------------- |
| 1.1  | Activar Information Protection           | Etiquetado manual en aplicaciones Office        | Ninguna, punto de partida                    | Los usuarios clasifican documentos desde el día 1 |
| 1.2  | Configurar DLP básico                    | Políticas DLP en Exchange, SharePoint, OneDrive | Etiquetas de sensibilidad configuradas (1.1) | Prevención de fuga en canales principales         |
| 1.3  | Habilitar Audit Premium                  | Retención de logs 1 año y eventos avanzados     | Licencia Purview Suite activa                | Visibilidad forense desde la activación           |
| 1.4  | Crear evaluaciones en Compliance Manager | Compliance Score inicial                        | Audit Premium activo (1.3)                   | Línea base de cumplimiento cuantificada           |
| 1.5  | Inventario con DSPM básico               | Mapa inicial de datos sobrexpuestos             | Information Protection activo (1.1)          | Identificación de riesgo previo a Copilot         |

#### Principio del Nivel 1

```text
VER → CLASIFICAR → REGISTRAR → MEDIR
 │         │           │          │
Data     Etiquetas    Audit    Compliance
Map     manuales     Premium    Score
```

#### Por qué el orden importa

- Audit Premium debe activarse antes de necesitarlo, porque los logs no se generan de forma retroactiva.
- El Compliance Score inicial es la línea base con la que se demostrará mejora a auditorías y dirección.
- El inventario DSPM básico debe ejecutarse antes de activar Copilot para validar el estado de exposición de datos.

#### Métricas de éxito del Nivel 1

- Al menos 80 % de los documentos críticos con etiqueta de sensibilidad aplicada.
- Compliance Score inicial documentado en Compliance Manager.
- Audit Premium activo con retención mínima de 1 año configurada.
- Al menos 2 evaluaciones de cumplimiento creadas (por ejemplo, NIST CSF e ISO 27001).
- Mapa de datos sobrexpuestos generado con DSPM y registrado como línea base de riesgo.


---

### Concepto 2 - Nivel 2: Automatización (Días 90-180)

**De control manual a inteligencia operativa**

El Nivel 2 transforma controles manuales en un sistema automatizado que amplía superficie protegida y anticipa riesgos. Cada hito depende de componentes ya desplegados en el Nivel 1.

#### Hitos del Nivel 2

| #    | Hito                               | Capacidad activada                                           | Dependencia del Nivel 1                                   | Salto de madurez                                        |
| ---- | ---------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------- |
| 2.1  | Auto-labeling con ML               | Clasificación automática con clasificadores entrenables y EDM | Information Protection manual activo (1.1)                | De etiquetado por usuario a etiquetado por política     |
| 2.2  | Endpoint DLP                       | Control en dispositivos Windows 10/11 y macOS                | DLP básico en Exchange y SharePoint activo (1.2)          | Cierra exfiltración física (USB, impresión, portátiles) |
| 2.3  | Insider Risk Management            | Análisis de comportamiento de usuarios                       | Audit Premium activo (1.3), IRM consume logs de auditoría | De respuesta reactiva a anticipación del riesgo interno |
| 2.4  | Communication Compliance           | Supervisión de comunicaciones asistida por ML                | Audit Premium activo (1.3)                                | Visibilidad sobre canales de comunicación clave         |
| 2.5  | Data Lifecycle Management avanzado | Ámbitos adaptativos y retención basada en eventos            | DLP y etiquetas activos (1.1, 1.2)                        | Gobierno del ciclo de vida completo del contenido       |

#### Principio del Nivel 2

```text
AUTOMATIZAR → EXPANDIR → ANTICIPAR → GOBERNAR
      │            │          │           │
   Auto-        Endpoint   Insider      Data
  labeling        DLP       Risk      Lifecycle
```

#### Dependencia crítica del Nivel 2

Insider Risk Management requiere Audit Premium activo desde el Nivel 1. IRM usa el registro de actividad de Audit para construir el comportamiento normal de cada usuario. Si Audit Premium se activa al mismo tiempo que IRM, el sistema no tiene histórico suficiente y las primeras semanas de IRM funcionan casi sin contexto. Esta es la principal razón para activar Audit Premium desde el día 1.

#### Métricas de éxito del Nivel 2

- Al menos 95 % de los datos sensibles clasificados automáticamente (sin intervención del usuario).
- Endpoint DLP activo en al menos 90 % de los dispositivos gestionados.
- Políticas de IRM configuradas para al menos 3 escenarios de riesgo (exfiltración, offboarding, error accidental).
- Communication Compliance activo en canales regulados (Teams, Exchange).
- Retención automatizada configurada para al menos 80 % del contenido con requisitos regulatorios.


---

### Concepto 3 - Nivel 3: Optimización avanzada (Días 180+)

**El sistema aprende, se adapta y demuestra**

El Nivel 3 activa capacidades que requieren que los niveles anteriores estén no solo desplegados, sino maduros. El enfoque cambia de simple cumplimiento a ventaja operativa basada en datos.

#### Hitos del Nivel 3

| #    | Hito                         | Capacidad activada                                           | Dependencia del Nivel 2                               | Diferenciador estratégico                                 |
| ---- | ---------------------------- | ------------------------------------------------------------ | ----------------------------------------------------- | --------------------------------------------------------- |
| 3.1  | Adaptive Protection          | IRM y DLP integrados, restricciones dinámicas por nivel de riesgo | IRM con baseline (2.3) y Endpoint DLP (2.2)           | Controles que se ajustan al riesgo sin intervención       |
| 3.2  | Information Barriers         | Separación técnica regulatoria en Teams, SharePoint y Exchange | Communication Compliance activo (2.4)                 | Cumplimiento de separación de funciones y conflictos      |
| 3.3  | Customer Key y Lockbox       | Soberanía de datos con claves en Azure Key Vault del cliente | Data Lifecycle Management avanzado (2.5)              | Respuesta sólida a objeciones de soberanía y jurisdicción |
| 3.4  | DSPM for AI completo         | Postura de seguridad para Copilot y herramientas de IA       | Auto-labeling activo (2.1) y clasificación madura     | Habilita Copilot en producción con riesgo controlado      |
| 3.5  | Security Copilot con Purview | Investigación forense asistida por IA                        | Audit Premium con al menos 6 meses de histórico (1.3) | Multiplicador de productividad del equipo de seguridad    |
| 3.6  | Purview con Microsoft Fabric | Gobernanza del dato analítico y estructurado                 | Data Map activo con clasificación madura              | Vista unificada M365, Azure y Fabric                      |

#### Principio del Nivel 3

```text
ADAPTAR → AISLAR → SOBERANÍA → IA SEGURA → INVESTIGAR → UNIFICAR
    │        │          │           │            │           │
Adaptive  Info.     Customer    DSPM for    Security    Purview
Protect. Barriers    Key+LB       AI        Copilot     + Fabric
```

#### Por qué el Nivel 3 no puede adelantarse

Adaptive Protection (3.1) sin un IRM maduro genera muchos falsos positivos, porque el sistema no conoce aún el comportamiento normal de los usuarios. DSPM for AI (3.4) sin auto-labeling maduro no puede evaluar el riesgo real, ya que ve datos sin clasificar. La precisión de las capacidades del Nivel 3 depende directamente de la calidad de implementación de los niveles previos.

#### Métricas de éxito del Nivel 3

- Adaptive Protection activo con una tasa de falsos positivos por debajo del 5 %.
- Customer Key configurado para Exchange Online, SharePoint y OneDrive.
- DSPM for AI ejecutado y con remediación de oversharing completada antes de habilitar Copilot.
- Security Copilot integrado con Purview para investigaciones de incidentes DLP e IRM.
- Compliance Score de al menos 70 % en evaluaciones NIST CSF e ISO 27001.
- Reporte de postura de datos unificado (M365 y Fabric) disponible para auditoría interna y externa.


---

### Concepto 4 - Mapa de dependencias: Arquitectura del roadmap

El valor principal del roadmap es explicitar las dependencias técnicas entre capacidades. Esta arquitectura ayuda a justificar la secuencia de inversión y los plazos.

#### Cadena de dependencias críticas

```text
Information Protection (1.1)
    │
    ├──► DLP básico (1.2) ──────────────► Endpoint DLP (2.2) ──────────────► Adaptive Protection (3.1)
    │                                                                                    ▲
    ├──► Auto-labeling ML (2.1) ──────────────────────────────────────────► DSPM for AI (3.4)
    │
    └──► Data Lifecycle Management (2.5) ──────────────────────────────► Customer Key (3.3)

Audit Premium (1.3)
    │
    ├──► Insider Risk Management (2.3) ─────────────────────────────────► Adaptive Protection (3.1)
    │
    ├──► Communication Compliance (2.4) ──────────────────────────────► Information Barriers (3.2)
    │
    └──► Security Copilot + Purview (3.5) [requiere ≥6 meses de histórico]

Compliance Manager (1.4)
    │
    └──► Evaluaciones regulatorias continuas (todos los niveles)
```

#### Regla de las tres dependencias universales

Tres hitos del Nivel 1 habilitan la mayoría del roadmap:

1. **Information Protection (1.1)** habilita gran parte de las capacidades posteriores, incluida la clasificación automática y DSPM.
2. **Audit Premium (1.3)** habilita Insider Risk, Communication Compliance, Security Copilot y todas las capacidades que dependen de logs ricos y persistentes.
3. **DLP básico (1.2)** habilita Endpoint DLP y los controles que se basan en políticas de protección de información.

**Implicación para la priorización**

Si hay recursos limitados en el Nivel 1, la secuencia recomendada es:

1. Information Protection.
2. Audit Premium.
3. DLP básico.

Estos tres hitos desbloquean la mayoría del valor del roadmap y reducen el riesgo de re-trabajo en niveles posteriores.


---

### Concepto 5 - KPIs de madurez: de la implementación a la evidencia

Un roadmap solo tiene impacto cuando se traduce en métricas que puedan defenderse ante CISO, CFO y auditores. Los KPIs convierten cada nivel de madurez en evidencia cuantificable.

#### Nivel 1 - KPIs de visibilidad

| KPI                        | Métrica                                  | Fuente                           | Frecuencia      |
| -------------------------- | ---------------------------------------- | -------------------------------- | --------------- |
| Cobertura de clasificación | Porcentaje de documentos con etiqueta    | Information Protection Dashboard | Semanal         |
| Compliance Score baseline  | Puntuación inicial en Compliance Manager | Compliance Manager               | Una vez (mes 1) |
| Cobertura de auditoría     | Porcentaje de actividades registradas    | Audit Premium Dashboard          | Mensual         |
| Datos sobrexpuestos        | Número de activos con acceso excesivo    | DSPM Report                      | Una vez (mes 2) |

#### Nivel 2 - KPIs de control

| KPI                        | Métrica                                              | Fuente                           | Frecuencia |
| -------------------------- | ---------------------------------------------------- | -------------------------------- | ---------- |
| Tasa de auto-clasificación | Porcentaje de documentos clasificados auto vs manual | Information Protection Analytics | Mensual    |
| Incidentes DLP             | Número de alertas DLP por superficie                 | DLP Reports                      | Semanal    |
| Usuarios de alto riesgo    | Número de usuarios con score IRM elevado o crítico   | IRM Dashboard                    | Diario     |
| Cobertura Endpoint DLP     | Porcentaje de dispositivos con Endpoint DLP activo   | Intune y Purview                 | Mensual    |

#### Nivel 3 - KPIs de optimización

| KPI                             | Métrica                                                      | Fuente                     | Frecuencia               |
| ------------------------------- | ------------------------------------------------------------ | -------------------------- | ------------------------ |
| Efectividad Adaptive Protection | Reducción porcentual de incidentes DLP tras la activación    | DLP e IRM Reports          | Mensual                  |
| Compliance Score objetivo       | Puntuación igual o superior a 70 % en NIST CSF e ISO 27001   | Compliance Manager         | Trimestral               |
| Tiempo de investigación         | Reducción de MTTR en incidentes DLP o IRM con Security Copilot | Security Copilot Analytics | Por incidente            |
| Postura DSPM for AI             | Porcentaje de datos sobrexpuestos remediados antes de Copilot | DSPM Dashboard             | Antes de activar Copilot |

#### Uso del Compliance Score para el CFO

El Compliance Score de Purview cuantifica el estado de cumplimiento en tiempo casi real, lo asocia a controles específicos y prioriza las acciones con mayor impacto. Para el CFO, permite pasar de mensajes cualitativos del tipo "estamos trabajando en cumplimiento" a cifras concretas, por ejemplo: "estamos al 67 % de cumplimiento con NIST CSF y estas son las 5 acciones priorizadas para alcanzar el 80 % en los próximos 90 días".


---

## Nivel 3 - Notas de soporte e insights

### Alcance y naturaleza del roadmap

Este roadmap es consultivo y refleja una secuencia de mejores prácticas basada en dependencias técnicas documentadas. Los plazos de 90 y 180 días son referencias orientativas que variarán según:

- Tamaño de la organización.
- Madurez previa de seguridad y cumplimiento.
- Recursos dedicados al proyecto.

Para un diseño oficial y detallado de proyecto se deben consultar las guías de despliegue de Microsoft Learn y la documentación de producto específica.

### Sectores regulados en Latinoamérica

En banca y salud (por ejemplo, BCRA, CMF, CNBV, SBS), la organización debe considerar el complemento de Audit con retención de 10 años como parte del Nivel 1. Estos sectores suelen requerir períodos de retención superiores al año estándar de Audit Premium. Esta decisión de licenciamiento debe tomarse al inicio del roadmap, no en etapas avanzadas.

### Aceleradores de implementación

Organizaciones que ya tienen Microsoft Defender for Endpoint (MDE) operativo pueden acelerar sensiblemente el despliegue de Endpoint DLP, ya que ambos comparten agente en el dispositivo. Si MDE está activo, Endpoint DLP se habilita mediante políticas, sin requerir un nuevo agente ni grandes cambios en el endpoint.

### Narrativa con el negocio y el board

Cada nivel de madurez habilita una conversación distinta con la alta dirección:

- **Nivel 1**: "Ahora vemos nuestros datos" y podemos cuantificar la exposición.
- **Nivel 2**: "Ahora controlamos nuestros datos" con automatización y cobertura ampliada.
- **Nivel 3**: "Ahora demostramos cumplimiento y habilitamos IA de forma segura" con métricas y evidencia para auditores y reguladores.

La narrativa de madurez técnica se alinea así con una narrativa de creación de valor de negocio progresivo.
