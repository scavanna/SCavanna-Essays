---
layout: page
title: "MS_Purview_M365E5-SecOps_e03v01_Art05-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e03/ms-purview-m365e5-secops-e03v01-art05-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e03v01_Art05-MS_CSU_Security_MCSA



El documento describe una estrategia integral de gobernanza y protección de datos para organizaciones que adoptan inteligencia artificial generativa en el ecosistema de Microsoft. Explica cómo Microsoft 365 Copilot trabaja sobre el grafo de datos corporativo y por qué actúa como amplificador de riesgos ya existentes. Presenta a Microsoft Purview como la capa central de clasificación, prevención de pérdida de datos y gestión de postura, e incorpora a Microsoft Fabric y Security Copilot. Su finalidad es guiar a CISOs y responsables de datos en un despliegue seguro, medible y alineado con regulaciones.

El primer eje expone a Copilot como amplificador del riesgo de datos, no como origen. Si los repositorios contienen información sensible mal clasificada o con permisos excesivos, Copilot facilita que cualquier usuario la localice y sintetice en segundos. Esto acelera el oversharing y la exfiltración asistida por inteligencia artificial. Además, cuando los usuarios copian respuestas hacia servicios externos, surge la llamada Shadow AI. La ecuación central es simple: mala clasificación más Copilot equivale a riesgo exponencial.

El segundo eje presenta DSPM para IA como un radar continuo sobre el uso de inteligencia artificial. Purview detecta qué datos pasan por Copilot y otras herramientas, clasifica prompts con posible información sensible y genera paneles de postura de datos. A partir de ahí, orienta la remediación del oversharing antes de activar Copilot en producción. El flujo operativo sigue cinco pasos claros: descubrir, mapear, evaluar, remediar y monitorizar. Esto convierte la preparación en un proceso auditable.

El tercer eje profundiza en Shadow AI Protection como cierre práctico del perímetro de datos frente a servicios de inteligencia artificial externos. Purview DLP analiza el contenido generado o manejado por Copilot y lo compara con políticas específicas. Si detecta datos sensibles, puede bloquear, advertir o solo auditar cada intento de copia, pegado o carga hacia aplicaciones de IA públicas. Para lograr cobertura efectiva, combina controles en navegador, dispositivos, aplicaciones en la nube y canales de colaboración.

El cuarto eje describe la integración de Purview con Microsoft Fabric para extender la gobernanza al dato analítico y estructurado. Las etiquetas de sensibilidad nacidas en Microsoft 365 acompañan al dato cuando entra en modelos, almacenes o paneles analíticos. El linaje end‑to‑end permite rastrear origen, transformaciones y consumo. Además, las políticas de acceso se centralizan y los informes de postura incluyen tanto contenido colaborativo como entornos analíticos. Así, CISOs obtienen una visión unificada del riesgo.

El quinto eje integra Security Copilot con Purview y resume las implicancias estratégicas de madurez. Security Copilot acelera investigaciones DLP, análisis de riesgo interno y preparación de auditorías al correlacionar registros de Purview con identidades y eventos. Sobre esa base, el documento propone una secuencia: primero clasificación y etiquetado, luego políticas DLP, después DSPM y protección frente a Shadow AI. También aborda requisitos técnicos, particularidades regulatorias regionales y la importancia de alinear el navegador corporativo.



# Purview y Microsoft 365 Copilot: Gobernanza de IA y Protección de Datos

## Copilot como amplificador de riesgo de datos

Microsoft 365 Copilot opera sobre el grafo de datos del usuario: accede a todo lo que el usuario puede ver en M365 (SharePoint, OneDrive, Teams, Exchange). No crea riesgos nuevos, amplifica los existentes.  

Cuando los datos están mal clasificados o sobreexpuestos, Copilot hace que lleguen al usuario de forma más rápida, sintetizada y consumible.

### Vectores principales de riesgo

#### 1. Oversharing latente

- Muchas organizaciones tienen datos sensibles mal clasificados o con permisos excesivos desde antes de Copilot.  
- Copilot permite que un usuario encuentre y sintetice en segundos información que manualmente le habría llevado semanas o que quizá nunca habría localizado.  
- Un dato sobreexpuesto que antes estaba “perdido” en el ruido ahora aparece en una consulta de lenguaje natural.  

**Conclusión:** Copilot no crea el oversharing, lo hace eficiente.

#### 2. Exfiltración asistida por IA

- Un usuario malintencionado puede usar Copilot para localizar y consolidar datos sensibles dispersos en múltiples repositorios.  
- Copilot puede generar un documento de alto valor (resumen, lista, reporte) listo para ser exportado como un solo archivo.  
- La velocidad y el volumen de esta consolidación superan muchos modelos de detección basados únicamente en volumen de transferencia.

#### 3. Shadow AI

- Usuarios copian respuestas de Copilot u otros datos corporativos hacia herramientas de IA externas no gestionadas (ChatGPT, Claude, Gemini personales, etc.).  
- El dato sale del entorno corporativo como texto plano, sin etiquetas, sin cifrado y sin trazabilidad.  
- Purview DLP con Shadow AI Protection está diseñado para detectar y bloquear específicamente este patrón de uso.

### Ecuación del riesgo

```text
Datos mal clasificados  x  Copilot (amplificador)  =  Riesgo exponencial
Datos bien clasificados x  Copilot (amplificador)  =  Productividad segura
```


## DSPM for AI: Radar de gobernanza para Copilot

Data Security Posture Management for AI (DSPM for AI) es la capacidad de Purview para evaluar y gobernar el uso de IA generativa en la organización. Funciona como un radar que descubre, clasifica y reporta el riesgo de datos en el contexto de Copilot y otras IAs.

### Capacidades clave de DSPM for AI

| Capacidad                              | Descripción técnica                                          | Valor para el CISO                                           |
| -------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Detección de interacciones IA**      | Identifica qué datos están siendo procesados por Copilot y otras herramientas IA | Visibilidad sobre el uso real de IA en la organización       |
| **Clasificación de datos en prompts**  | Detecta cuando un prompt contiene o genera datos sensibles   | Prevención de exposición de datos sensibles en el flujo de IA |
| **Reporte de postura de datos**        | Dashboard de qué datos están sobrexpuestos y accesibles por Copilot | Priorización de remediación antes de activar Copilot         |
| **Detección de Shadow AI**             | Identifica herramientas de IA no autorizadas usadas en la organización | Visibilidad sobre el ecosistema de IA no gestionado          |
| **Alertas de riesgo de datos IA**      | Notifica cuando datos sensibles fluyen hacia herramientas IA | Respuesta proactiva para evitar exfiltración                 |
| **Integración con Compliance Manager** | Evaluaciones de cumplimiento específicas para uso de IA      | Marco regulatorio para adopción responsable de IA            |

### Flujo operativo de DSPM for AI

```text
1. DESCUBRIR  → ¿Qué datos tiene la organización y cómo están clasificados?
2. MAPEAR     → ¿Qué datos son accesibles por Copilot según permisos actuales?
3. EVALUAR    → ¿Cuál es el riesgo de exposición si Copilot accede a esos datos?
4. REMEDIAR   → Ajustar clasificación, permisos y políticas antes de activar Copilot
5. MONITOREAR → Supervisar de forma continua el flujo de datos en interacciones de IA
```

### Recomendación clave

Antes de activar Microsoft 365 Copilot en producción, es recomendable ejecutar DSPM for AI para identificar y remediar el oversharing latente. Activar Copilot sobre un entorno sin clasificación es equivalente a desplegar un motor de búsqueda interno sin controles de acceso efectivos.


## Shadow AI Protection: cierre del perímetro de IA

Shadow AI es el equivalente moderno del Shadow IT: uso de herramientas de IA sin aprobación ni gobierno de seguridad. Purview DLP con Shadow AI Protection actúa como control de cierre del perímetro de datos frente a estas herramientas.

### Problema de Shadow AI

- Los usuarios copian contenido generado con Copilot (o datos corporativos) hacia IAs personales.  
- El contenido sale como texto plano, sin etiquetas de sensibilidad, sin cifrado y sin registro de auditoría.  
- En el momento del copy-paste o del upload se pierde toda trazabilidad del dato.

### Cómo funciona Shadow AI Protection en Purview

```text
USUARIO                        PURVIEW DLP                          RESULTADO
   │                               │                                     │
   ├─ Genera respuesta con Copilot ─► Detecta contenido sensible         │
   │                               │ en clipboard o en el output         │
   │                               │                                     │
   ├─ Intenta copiar o subir       ─► Evalúa política DLP Shadow AI      │
   │   a una IA externa            │                                     │
   │                               │                                     │
   └─ Acción según riesgo      ◄───┤ • Bloquear                          │
                                   │ • Advertir y permitir               │
                                   │ • Auditar y permitir                │
                                   │ • Bloquear y notificar al CISO      │
```

### Superficies cubiertas por Shadow AI Protection

- **Browser DLP (Edge gestionado)**  
  Detecta y controla uploads de contenido sensible hacia dominios de IA externos.

- **Endpoint DLP**  
  Bloquea o controla el copy-paste de contenido clasificado hacia aplicaciones no autorizadas.

- **Defender for Cloud Apps (MDCA)**  
  Aplica control inline sobre aplicaciones SaaS de IA en el navegador.

- **Teams DLP**  
  Previene compartir outputs de Copilot con canales externos o destinatarios no autorizados.

La lista de aplicaciones de IA monitorizadas incluye ChatGPT (web), Claude (web), Gemini personal, Perplexity y otros dominios categorizados como “Generative AI” en el catálogo de Defender for Cloud Apps.


## Purview + Microsoft Fabric: gobernanza del dato analítico

Microsoft Fabric consolida Power BI, Azure Data Factory, Synapse Analytics y capacidades de IA analítica. La integración con Purview extiende la gobernanza y protección al dato estructurado y analítico, no solo al contenido de M365.

### Capacidades de integración Purview + Fabric

| Capacidad                            | Descripción                                                  | Impacto operativo                                            |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Propagación de etiquetas**         | Las etiquetas de sensibilidad aplicadas en M365 se propagan a datasets en Fabric | Una etiqueta "Confidencial" en SharePoint acompaña al dato cuando se importa a un dataset analítico |
| **Linaje end-to-end**                | El Data Map de Purview rastrea el linaje del dato desde su origen hasta su uso en Fabric | Auditoría completa: se puede saber de dónde viene un dato y quién lo ha procesado |
| **Clasificación de datos en Fabric** | Purview escanea y clasifica datos en Fabric (lakehouses, warehouses, etc.) | Visibilidad sobre datos sensibles en entornos analíticos     |
| **Políticas de acceso**              | Data Policy de Purview aplica controles de acceso sobre fuentes de datos en Fabric | Gobernanza centralizada sin duplicar políticas en cada herramienta |
| **Reporting unificado**              | Data Estate Insights incorpora datos de Fabric en el dashboard de postura de datos | Vista única del riesgo y la postura de datos en M365, Azure y Fabric |

### Relevancia para CISOs y responsables de datos

El dato más valioso suele residir en modelos analíticos, datasets de entrenamiento y dashboards de Power BI, no solo en el correo electrónico. Purview + Fabric permite aplicar una política y una consola unificadas para gobernar tanto datos no estructurados (M365) como estructurados (Fabric/Azure), con linaje y etiquetado coherentes en todo el ciclo de vida del dato.


## Security Copilot + Purview: investigación forense asistida por IA

Microsoft Security Copilot es el asistente de IA para operaciones de seguridad. Al integrarse con Purview, ofrece capacidades avanzadas de investigación y respuesta sobre incidentes relacionados con datos.

### Casos de uso principales

#### 1. Investigación de incidentes DLP

- El analista solicita un resumen de los últimos intentos de exfiltración bloqueados por DLP.  
- Security Copilot consulta Purview Audit, correlaciona con identidades en Entra ID y resalta patrones (usuarios, tipos de dato, vectores de salida).  
- El resultado es un resumen accionable generado en segundos, en lugar de horas de análisis manual de logs.

#### 2. Análisis de riesgo insider

- El analista pide identificar usuarios con score de riesgo elevado en Insider Risk Management que además tengan acceso a datos “Altamente confidenciales”.  
- Security Copilot cruza datos de IRM con clasificación de Information Protection y devuelve una lista priorizada de usuarios de alto riesgo, con detalle del tipo de información a la que acceden.

#### 3. Preparación para auditorías

- El analista solicita un resumen del cumplimiento de políticas DLP para un período específico.  
- Security Copilot consulta Audit Premium, consolida métricas y genera un reporte estructurado listo para revisión de auditores internos o externos.

### Impacto en productividad del equipo de seguridad

Tareas que antes requerían entre 4 y 8 horas de un analista senior (correlación de eventos en múltiples consolas, cruce manual de datos, elaboración de reportes) pueden completarse en minutos con Security Copilot + Purview. La función del analista pasa de trabajo manual de agregación a validación, interpretación y toma de decisiones.


## Notas de soporte e insights para decisión y diseño de estrategia

### Preparación para Copilot y madurez de gobernanza

La pregunta central no es si se debe activar Copilot, sino si la organización está preparada para hacerlo de forma segura. DSPM for AI permite convertir esa pregunta en un proceso medible y auditable mediante:

- Descubrimiento de datos y clasificación efectiva.  
- Evaluación de exposición ante Copilot.  
- Remediación guiada del oversharing latente.  
- Monitorización continua del uso de IA.

Esto facilita justificar la adopción de Copilot ante comités de riesgo y dirección, basándose en evidencias y métricas, no solo en criterios subjetivos.

### Consideraciones regulatorias para Latinoamérica

En sectores regulados (por ejemplo, servicios financieros en Latinoamérica), muchos supervisores (BCRA, CMF, CNBV, SBS Perú y otros) aún no cuentan con marcos específicos para IA generativa, pero están trabajando activamente en ellos.  

Organizaciones que implementen hoy capacidades como DSPM for AI y Shadow AI Protection estarán en mejor posición para:

- Demostrar diligencia y control proactivo sobre el uso de IA.  
- Alinear rápidamente sus controles cuando se publiquen normativas específicas.  
- Aportar evidencia de gobernanza de IA en inspecciones y auditorías.

### Requisitos técnicos previos

DSPM for AI depende de que Information Protection esté activo y de que existan etiquetas de sensibilidad configuradas y aplicadas de manera consistente. Sin clasificación previa, DSPM no puede evaluar adecuadamente el riesgo de los datos a los que accede Copilot.  

Esto refuerza una secuencia de madurez:

1. Clasificación y etiquetado (Information Protection).  
2. Políticas DLP básicas.  
3. DSPM for AI, Shadow AI Protection y casos avanzados de Copilot.

La madurez es secuencial: intentar avanzar en IA sin fundamentos de clasificación y DLP genera brechas de gobernanza.

### Integración e interoperabilidad como diferenciador

La combinación de:

- Plataforma de productividad (M365),  
- Herramientas de IA generativa (Copilot, Security Copilot), y  
- Suite de gobernanza y protección de datos (Purview),

bajo un modelo de políticas unificadas y una consola integrada, permite:

- Reducir complejidad operativa y solapamiento de herramientas.  
- Aplicar controles coherentes a lo largo de todo el ciclo de vida del dato.  
- Aumentar la eficacia de detección y respuesta al correlacionar señales de múltiples productos.

### Consideraciones de implementación para Shadow AI Protection

Shadow AI Protection en Browser DLP requiere Microsoft Edge gestionado con políticas de Intune o Group Policy. En entornos donde Chrome u otros navegadores son el estándar corporativo:

- La cobertura puede ser parcial si no se usan extensiones o mecanismos adicionales de control.  
- Es importante abordar este punto en las evaluaciones de postura de seguridad y en las discusiones con los equipos de infraestructura y endpoint.  

Alinear el navegador corporativo con las capacidades de DLP y Shadow AI Protection es un elemento crítico para cerrar efectivamente el perímetro de IA.
