---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art10-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art10-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art10-MS_CSU_Security_MCSA

Este documento explica cómo Purview evoluciona en la era de la inteligencia artificial generativa. Describe capacidades de seguridad y gobernanza sobre datos usados por asistentes como Microsoft 365 Copilot y Security Copilot. También cubre agentes autónomos que operan con identidades de servicio en Microsoft Entra y servicios analíticos en Microsoft Fabric. El texto detalla cómo Purview aplica etiquetas, políticas de protección, linaje y auditoría a estos nuevos actores. Además presenta escenarios de uso e integración con Defender for Cloud Apps y funciones de cumplimiento.

En primer lugar, el texto presenta Data Security Posture Management para inteligencia artificial dentro de Purview. Esta capacidad muestra qué hacen los modelos con datos corporativos, tanto en servicios aprobados como en herramientas externas. Detecta oversharing cuando Copilot usa documentos innecesarios para el rol, analiza prompts con datos sensibles y puede alertar, bloquear o sanitizar. Por otro lado, mediante Defender for Cloud Apps, identifica Shadow AI y cuantifica el uso de servicios no autorizados.

En segundo lugar, se describe la gobernanza de agentes autónomos que ejecutan procesos completos, como ventas, sin supervisión. Estos agentes usan identidades de servicio en Microsoft Entra, consultan sistemas empresariales y actúan como aplicaciones, no como personas. Purview aplica herencia de políticas para que el agente solo acceda a datos permitidos, respete etiquetas y límites de protección. Las acciones quedan registradas en Auditoría con identidad propia del agente, facilitando trazabilidad y responsabilidades en caso de incidentes.

En tercer lugar, se presenta Security Copilot, asistente de seguridad que se integra con Purview para investigar riesgos sobre datos. Antes, las investigaciones exigían recorrer varios portales y construir consultas avanzadas para Auditoría, riesgos internos y descubrimiento electrónico. Ahora el analista formula preguntas en lenguaje natural; Security Copilot genera búsquedas, resume resultados y acelera la comprensión de incidentes complejos. Su uso aporta velocidad, pero requiere licencias específicas basadas en unidades de cómputo de seguridad, separadas de Microsoft 365 E5.

En cuarto lugar, se aborda la integración de Microsoft Fabric con Purview para gobernar datos analíticos, pipelines y modelos. Fabric unifica ingeniería y análisis; Purview añade catálogo, clasificación y linaje, mostrando el recorrido del dato desde origen hasta informes. Las etiquetas de sensibilidad pueden viajar desde Microsoft 365 hacia Fabric y heredarse en conjuntos derivados, aunque algunas transformaciones requieren comprobaciones manuales. Además, la integración facilita aplicar retención, localizar datos personales y asegurar que paneles analíticos cumplen normas de privacidad corporativa.

Por último, el documento ofrece hoja de ruta para 2026 y sitúa las capacidades en su nivel de madurez. Diferencia funciones ya disponibles, características en expansión y elementos en preview, desde DSPM para IA hasta clasificación multimodal. Advierte que habilitar Copilot sin ordenar permisos, etiquetas y controles de fuga aumenta el riesgo regulatorio y de exposición interna. Propone al CISO como arquitecto de la gobernanza y presenta a Purview como plataforma de IA que también audita IA y datos.





# Microsoft Purview en la era de la IA generativa, los agentes autónomos y DSPM

---

## 1. DSPM para IA: ver qué hace la IA con tus datos

Data Security Posture Management (DSPM) para IA es la capacidad de Microsoft Purview que proporciona **visibilidad sobre cómo los modelos de inteligencia artificial interactúan con los datos de la organización**, tanto modelos corporativos aprobados como servicios externos que los empleados usan por su cuenta.

DSPM para IA aborda dos ejes principales de riesgo.

### 1.1 Oversharing: datos que la IA puede ver pero no debería

M365 Copilot accede a los datos a los que el usuario tiene permiso técnico. En muchas organizaciones, los permisos históricos en SharePoint y OneDrive son demasiado amplios respecto al principio de mínimo privilegio.

Ejemplo típico: una persona de Marketing consulta a Copilot sobre estrategia de producto y recibe información procedente de un documento de roadmap técnico al que tiene acceso por configuración, pero que no es relevante para sus funciones.

DSPM para IA ayuda a identificar estos escenarios de **oversharing** mediante señales como:

- Etiquetas de sensibilidad de los documentos usados por Copilot en sus respuestas.  
- Frecuencia con la que Copilot accede a documentos fuera del área funcional habitual del usuario.  
- Volumen de datos procesados por consulta comparado con el patrón histórico del usuario.

Con esta información, el equipo de seguridad puede ajustar permisos, reclasificar contenido o reforzar políticas de acceso para reducir el riesgo de exposición innecesaria.

### 1.2 Prompts con datos sensibles: lo que el usuario envía a la IA

El segundo eje de riesgo está en el contenido que el usuario introduce en los prompts. Es habitual que empleados incluyan:

- Fragmentos de contratos.  
- Datos de clientes.  
- Información financiera interna.  
- Código propietario u otros activos sensibles.

DSPM para IA puede analizar el contenido de algunos prompts dirigidos a Copilot usando clasificadores de información configurados. Cuando un prompt coincide con tipos de información sensible, el sistema puede, según la política:

- Generar una alerta para el equipo de seguridad.  
- Notificar al usuario y advertirle del riesgo.  
- Bloquear el envío del prompt.  
- Sanitizar o eliminar el contenido sensible antes de que llegue al modelo.

### 1.3 Shadow AI: visibilidad sobre IA no autorizada

DSPM para IA se apoya en Microsoft Defender for Cloud Apps (MDCA) para detectar el uso de herramientas de IA no autorizadas, lo que suele denominarse **Shadow AI**.

Mediante App Discovery, MDCA puede identificar:

- Qué servicios de IA externos se utilizan desde dispositivos y redes corporativas.  
- Cuántos usuarios los usan y con qué frecuencia.  
- Volumen estimado de datos transferidos a esos servicios.

Esta visibilidad permite definir una política de Shadow AI basada en datos: distinguir entre herramientas aprobadas con controles adecuados y servicios que deben restringirse o bloquearse por su riesgo de exfiltración.

---

## 2. Gobernanza de agentes autónomos: políticas para actores que no duermen

Tras la primera ola de IA centrada en asistentes como M365 Copilot, surge una segunda generación de capacidades: **agentes autónomos** capaces de ejecutar flujos completos sin intervención humana en cada paso. Un agente de ventas, por ejemplo, podría:

- Consultar CRM.  
- Leer inventarios en SharePoint.  
- Revisar condiciones de contrato en OneDrive.  
- Generar y enviar una propuesta personalizada.

Estos agentes operan como aplicaciones con identidades de servicio en Microsoft Entra ID, no como usuarios humanos. Esto plantea cuatro preguntas clave de gobernanza:

1. **Identidad del agente**  
   Con qué identidad actúa, qué permisos tiene y si esos permisos son más amplios de lo que tendría un usuario individual.

2. **Ámbito de datos que puede procesar**  
   Si puede acceder a documentos Altamente Confidenciales o limitados a ciertos equipos y si debe respetar las etiquetas de sensibilidad en sus outputs.

3. **Qué puede hacer con los datos**  
   Si puede incluir fragmentos de documentos sensibles en correos a externos o exportar datos a sistemas fuera del control de la organización.

4. **Responsabilidad y trazabilidad**  
   Cómo se registra la actividad del agente y quién es responsable ante una violación de política.

### 2.1 Respuesta de Purview: herencia de políticas

Microsoft aborda este problema aplicando el principio de **herencia de políticas** a los agentes de IA que operan con identidades de servicio dentro del ecosistema:

- Un agente **no puede acceder** a documentos a los que el usuario en cuyo contexto opera no tenga permiso.  
- Un agente **no puede redistribuir** contenido protegido por etiquetas de sensibilidad y cifrado de Information Protection si la política lo prohíbe.  
- Las **acciones del agente se registran en Audit** con su propia identidad, lo que permite trazar qué documentos accedió, qué datos procesó y qué outputs generó.

Este enfoque no elimina todos los riesgos, especialmente en escenarios con identidades de servicio muy amplias o agentes que operan fuera de M365, pero proporciona un marco coherente para extender la gobernanza existente a actores no humanos sin rediseñar todas las políticas.

---

## 3. Security Copilot + Purview: investigación forense en lenguaje natural

Microsoft Security Copilot es el asistente de IA generativa orientado a equipos de seguridad, respuesta a incidentes y cumplimiento. Su integración con Purview transforma la forma de investigar incidentes y riesgos relacionados con datos.

### 3.1 Problema que resuelve

Las investigaciones basadas en Purview suelen requerir:

- Navegar múltiples superficies del portal de Purview.  
- Construir consultas complejas para Insider Risk, DLP, Audit Premium o eDiscovery.  
- Filtrar por usuarios, departamentos, tipos de etiqueta y rangos de fechas.  

Esto demanda tiempo y habilidades técnicas específicas.

Con Security Copilot integrado, el analista puede formular preguntas en lenguaje natural, por ejemplo:

> "Qué archivos etiquetados como Confidencial compartieron externamente los usuarios del área financiera esta semana"

Security Copilot traduce esa pregunta en consultas sobre los datos de Purview, ejecuta las búsquedas necesarias y devuelve un resumen estructurado con los hallazgos relevantes. El beneficio principal es la reducción de fricción técnica y el aumento de velocidad y consistencia en las investigaciones.

### 3.2 Casos de uso típicos

- **Insider Risk Management**: contextualizar y ampliar una alerta, identificando contenido, patrones de tiempo y posibles correlaciones con otros eventos.  
- **Respuesta a incidentes con Audit Premium**: reconstruir la secuencia de acciones de un usuario o agente sobre documentos sensibles.  
- **eDiscovery**: definir criterios de búsqueda, refinar colecciones de documentos y sintetizar hallazgos iniciales.  
- **Correlación entre pilares**: cruzar señales de DLP, IRM, Audit y otras fuentes para entender un incidente de forma integral.

### 3.3 Licenciamiento

Security Copilot requiere un modelo de licenciamiento específico basado en **Security Compute Units (SCU)** y **no está incluido** automáticamente en Microsoft 365 E5. Esta consideración es clave al planificar su adopción como parte del modelo de gobernanza.

---

## 4. Microsoft Fabric + Purview: gobernanza del dato analítico

Además de los datos no estructurados de M365, muchas organizaciones manejan un volumen crítico de **datos analíticos en movimiento**: pipelines de ingesta y transformación, lakehouses, data warehouses, dataframes de análisis y modelos de machine learning.

**Microsoft Fabric** unifica ingeniería de datos, ciencia de datos, almacenamiento analítico y BI en una sola plataforma, e integra Microsoft Purview como sistema de gobernanza nativo. Esto amplía la cobertura de Purview al dominio analítico.

### 4.1 Etiquetas de sensibilidad en la cadena analítica

Cuando un dataset etiquetado, por ejemplo como **Confidencial**, se importa desde M365 a un workspace de Fabric:

- La etiqueta viaja con el dato a medida que se mueve por el pipeline.  
- Los datasets derivados heredan, por defecto, la etiqueta de mayor sensibilidad de sus fuentes.  

La propagación de etiquetas sigue evolucionando. Algunas transformaciones complejas aún no transmiten las etiquetas de forma automática, lo que exige validaciones adicionales por parte del equipo de datos.

### 4.2 Linaje de datos como funcionalidad de gobernanza

Fabric genera **linaje de datos** de forma automática e integrada con el Purview Data Map. Esto permite:

- Ver el recorrido completo de un dato desde su origen hasta los informes o modelos que lo consumen.  
- Evaluar el impacto de cambios en datos fuente sobre informes y modelos derivados.  
- Demostrar a auditores cómo se transforman y utilizan los datos sensibles a lo largo de todo el pipeline.

### 4.3 Cumplimiento sobre datos analíticos

Los datos personales tratados en pipelines analíticos están sujetos a las mismas obligaciones de privacidad y cumplimiento que los documentos de M365. La integración Fabric + Purview facilita:

- Aplicar políticas de retención y eliminación a datasets analíticos.  
- Localizar datos personales asociados a un sujeto para responder solicitudes de acceso o borrado.  
- Garantizar que los dashboards y modelos no expongan información en violación de políticas o regulaciones.

---

## 5. Hoja de ruta 2025-2026: estados de madurez y dirección de viaje

Las capacidades descritas en este bloque se encuentran en distintos estados de disponibilidad. Para diseño de arquitecturas de producción es importante distinguir entre lo que ya está generalmente disponible y lo que se encuentra en preview o en fase de hoja de ruta.

### 5.1 Estado actual por capacidad

- **DSPM para IA**  
  - Visibilidad básica sobre uso de Copilot y detección de prompts con contenido sensible: disponible y en expansión.  
  - Detección de Shadow AI mediante Defender for Cloud Apps: disponible.  
  - Capacidades avanzadas de detección de oversharing y señales adicionales: en evolución, con nuevas funciones incorporándose de forma continua.

- **Gobernanza de agentes autónomos**  
  - Herencia de políticas para agentes que operan con identidades de servicio: disponible en escenarios soportados, con limitaciones en contextos complejos (múltiples principals, multi-tenant, agentes fuera de M365).  
  - La madurez funcional seguirá aumentando a medida que crezcan los escenarios oficiales de agentes.

- **Security Copilot + Purview**  
  - Integración con Insider Risk Management, Audit Premium y eDiscovery Premium: disponible.  
  - Cobertura para otros componentes de cumplimiento, como Communication Compliance y DLP: en expansión activa.  
  - Licenciamiento separado por SCU.

- **Fabric + Purview**  
  - Integración de Purview como capa de clasificación, linaje y catálogo para Fabric: disponible.  
  - Propagación completa de etiquetas en todas las transformaciones: en desarrollo, con mejoras graduales.

- **Clasificación multimodal**  
  - Primeras capacidades para clasificar y proteger imágenes: en preview limitada.  
  - Cobertura extendida para imagen, audio y video: objetivo de hoja de ruta hacia 2026.

### 5.2 Tendencia unificadora

Más allá de cada funcionalidad concreta, la dirección de Purview para 2025-2026 apunta a una **gobernanza continua, automatizada y agnóstica respecto al tipo de actor**, donde:

- Las mismas políticas de clasificación y protección se aplican a usuarios, agentes de IA, pipelines de datos y modelos.  
- El linaje, la auditoría y las etiquetas permiten seguir el dato a través de todo su ciclo de vida, en tiempo real o casi real.  
- El modelo de Zero Trust para datos se extiende a contextos que hace pocos años no existían en el perímetro tradicional.

---

## 6. Notas de soporte e insights de profundidad

### 6.1 Riesgo de habilitar Copilot sin gobernanza previa

Activar M365 Copilot sobre un entorno con permisos desordenados, sin etiquetas coherentes ni políticas de DLP consolidadas, aumenta significativamente el riesgo de:

- Exposición de información sensible a usuarios que no deberían verla.  
- Violaciones de obligaciones regulatorias por accesos o usos indebidos de datos personales.  

La recomendación técnica es, al menos, contar con un **nivel fundacional de gobernanza** operativo (clasificación y etiquetado básico, permisos revisados en repositorios críticos y DLP en superficies clave) antes de una habilitación amplia de Copilot. Idealmente, los niveles de automatización y monitoreo continuo deberían estar ya en marcha.

### 6.2 El CISO como arquitecto de la gobernanza de IA

La adopción de IA generativa reposiciona el rol del CISO y de los líderes de seguridad:

- Entender DSPM para IA, gobernanza de agentes y clasificación de prompts permite liderar la conversación de gobernanza de IA desde una perspectiva técnica y de negocio.  
- El CISO puede pasar de ser percibido como freno a la adopción a ser el habilitador de un uso seguro y responsable de IA, definiendo patrones y controles que permitan a las áreas de negocio innovar con menor riesgo.

### 6.3 IA que audita IA: reducción de fricción con responsabilidad humana

La combinación de Purview con Security Copilot ilustra un patrón que se repetirá en otros ámbitos: **usar IA para gobernar sistemas que también incorporan IA**. Esto implica:

- Delegar en modelos de lenguaje parte del trabajo de búsqueda, correlación y síntesis de evidencias.  
- Mantener en manos humanas la interpretación final, la toma de decisiones y la responsabilidad sobre los resultados.

El diseño de procesos debe reflejar explícitamente esta combinación de automatización y supervisión humana.

### 6.4 Extensión de Purview a una nueva generación de problemas

Los mismos mecanismos fundamentales de Purview siguen siendo el núcleo de la solución:

- **Etiquetas de sensibilidad** como contrato técnico persistente sobre el dato.  
- **Políticas de protección y DLP** que actúan sobre esas etiquetas y otros metadatos.  
- **Clasificación automática** para escalar la gobernanza.  
- **Audit y eDiscovery** como infraestructura de verdad y memoria forense.

La diferencia es el tipo de actor al que se aplican. Purview deja de gobernar solo usuarios y dispositivos para extenderse a agentes, modelos y plataformas analíticas, conservando los mismos principios técnicos y ampliando su alcance.
