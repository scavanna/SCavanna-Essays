---
layout: page
title: "MS_Purview_M365E5-SecOps_e03v01_Art01-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e03/ms-purview-m365e5-secops-e03v01-art01-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e03v01_Art01-MS_CSU_Security_MCSA

Este texto explica el porqué de Microsoft Purview como respuesta estratégica a la gobernanza de datos moderna. Describe cómo unifica seguridad, cumplimiento y gobierno sobre datos repartidos entre Microsoft 365, Azure, nubes públicas y entornos locales. Presenta los pilares funcionales de la plataforma y el rol central del Purview Data Map. Además, muestra el cambio de paradigma: las políticas dejan de vivir en el perímetro y pasan a viajar con el dato. El objetivo es ofrecer un marco mental claro para ejecutivos y equipos de seguridad. Se centra en conceptos, no en instrucciones técnicas detalladas ni guías paso a paso.

El primer eje describe el problema histórico que Purview viene a corregir. Las organizaciones vivían con cuatro fracturas: silos de políticas, visibilidad limitada, gobernanza fragmentada y riesgo normativo alto. Las reglas de seguridad se aplicaban de forma distinta en productividad y en datos de infraestructura. Además, era casi imposible seguir el viaje completo de un dato crítico. No era un fallo técnico puntual, sino el resultado de un modelo mental centrado en el perímetro clásico.

El segundo eje relata el punto de quiebre en abril de 2022. Microsoft unificó Azure Purview y el antiguo centro de cumplimiento de Microsoft 365 bajo la marca Purview. Esa decisión creó un portal administrativo y un modelo de permisos coherente. Lo importante es el efecto práctico. Una etiqueta de sensibilidad se respeta cuando el dato pasa de SharePoint a una base de datos en Azure SQL. También cuando llega a Microsoft Fabric, sin reconfiguración.

El tercer eje presenta los tres pilares funcionales de Purview. En gobernanza de datos se agrupan capacidades como el mapa de datos, el catálogo unificado y las vistas del patrimonio. El pilar de seguridad de datos orquesta protección de la información, prevención de pérdida y riesgos internos. Por otro lado, el pilar de riesgo y cumplimiento integra auditoría, eDiscovery y administración de registros. Todos comparten metadatos comunes y actúan sobre el mismo inventario de datos.

El cuarto eje se centra en el Purview Data Map, el corazón técnico compartido. No es un pilar más, sino la infraestructura que alimenta a todos. Registra cada activo con su nombre, ubicación, linaje, clasificación, nivel de sensibilidad y métricas de calidad. Además, se conecta con orígenes en nubes como Azure, Amazon Web Services y Google Cloud, y también con entornos locales mediante conectores. Su capacidad se gestiona en unidades escalables según la demanda real.

El quinto eje explica el cambio de paradigma: la política viaja con el dato. Antes la seguridad protegía el perímetro; ahora la protección se incrusta en etiquetas persistentes con cifrado, identidad y reglas asociadas. Así el control se mantiene aunque el archivo cambie de usuario, dispositivo o nube. Purview ofrece cientos de tipos de información sensible listos para usar. Para el CISO, la cuestión clave pasa a ser si sus políticas acompañan a los datos.

# De los Silos al Sistema Nervioso Central: Qué es Microsoft Purview y por qué existe

## Conceptos clave

### 1. El problema histórico: cuatro fracturas estructurales

Antes de Purview, las organizaciones operaban con cuatro deficiencias estructurales crónicas que hacían imposible una gobernanza real:

| Fractura                   | Síntoma operativo                                            |
| -------------------------- | ------------------------------------------------------------ |
| **Silos de políticas**     | Reglas de seguridad desconectadas entre la capa de productividad (M365) y la capa de datos (Azure) |
| **Visibilidad limitada**   | Imposibilidad de rastrear un dato desde su origen hasta su destino final |
| **Gobernanza fragmentada** | Múltiples consolas, roles y flujos de trabajo sin integración entre sí |
| **Riesgo normativo**       | Brechas inevitables al no existir una vista unificada del patrimonio de datos |

Estas fracturas no eran fallas de implementación, sino consecuencias del modelo mental de la época: la seguridad protegía el perímetro, no el dato.

### 2. El momento de quiebre: la gran unificación (abril de 2022)

En abril de 2022, Microsoft fusionó dos universos tecnológicos bajo una única marca:

| Origen                         | Dominio de datos                                             | Enfoque                       |
| ------------------------------ | ------------------------------------------------------------ | ----------------------------- |
| **Azure Purview** (anterior)   | Datos estructurados y semiestructurados en Azure, AWS, GCP y on-premises | Gobernanza de infraestructura |
| **M365 Compliance** (anterior) | Datos no estructurados de colaboración (Exchange, SharePoint, Teams) | Seguridad y cumplimiento      |

El resultado práctico e inmediato es que una etiqueta de sensibilidad aplicada a un documento en SharePoint es reconocida y respetada cuando ese mismo dato migra a una base de datos de Azure SQL o es procesado en Microsoft Fabric. La política viaja con el dato.

### 3. Los tres pilares funcionales: la anatomía de Purview

Purview opera sobre tres grandes áreas funcionales que comparten metadatos comunes pero tienen dominios de acción diferenciados:

| Pilar                 | Color sugerido    | Productos clave                                              |
| --------------------- | ----------------- | ------------------------------------------------------------ |
| **Data Governance**   | Azul corporativo  | Data Map, Unified Catalog, Data Estate Insights, Data Policy, Data Sharing |
| **Data Security**     | Violeta o magenta | Information Protection, DLP, Insider Risk, Communication Compliance, Customer Key |
| **Risk & Compliance** | Verde teal        | Compliance Manager, eDiscovery, Audit, Information Barriers, Records Management |

Los metadatos fluyen desde el Data Map hacia los tres pilares; ningún pilar opera de forma completamente aislada.

### 4. El Purview Data Map: el corazón técnico compartido

El Data Map no es uno de los tres pilares; es la infraestructura que los hace posibles a todos. Captura en tiempo real cinco dimensiones de cada activo de datos:

1. **Nombre y ubicación** del activo  
2. **Linaje**: origen, transformaciones y destino del dato  
3. **Clasificación** por tipo de contenido  
4. **Sensibilidad** en relación con privacidad y datos personales  
5. **Metadatos de calidad** sobre la salud del activo  

Opera en unidades de capacidad (CU) con almacenamiento en incrementos de 10 GB por unidad y conecta fuentes multicloud (AWS, GCP, Azure) y on-premises mediante conectores nativos.

### 5. La política viaja con el dato: el cambio de paradigma

Este es el cambio conceptual más profundo que introduce Purview y también el más difícil de comunicar a audiencias ejecutivas:

| Modelo anterior                            | Modelo Purview                             |
| ------------------------------------------ | ------------------------------------------ |
| La seguridad protege el **perímetro**      | La seguridad viaja **dentro del dato**     |
| Las políticas viven en el **sistema**      | Las políticas viven en la **etiqueta**     |
| El control termina cuando el dato **sale** | El control persiste **donde el dato vaya** |

Técnicamente, esto se logra mediante etiquetas de sensibilidad persistentes con cifrado, identidad y políticas embebidas en el contenido. El dato puede moverse entre dispositivos, nubes o usuarios, y el escudo no desaparece.

## Notas de soporte e insights

### Dato de profundidad

La unificación de marca no fue solo cosmética. El portal `purview.microsoft.com` reemplazó tanto al Microsoft 365 Compliance Center como a la consola de Azure Purview, dos interfaces separadas con modelos de RBAC distintos, en una única entrada administrativa.

### Cifra de credibilidad

Purview incluye más de **300 tipos de información sensible (SIT)** predefinidos disponibles desde el primer día de activación, sin configuración adicional. Este dato es útil para comunicar la velocidad de time-to-value en conversaciones con CISOs.

### Tensión estratégica para el CISO

La pregunta relevante no es “¿tenemos Purview?” sino “¿nuestras políticas viajan con los datos o se quedan en el perímetro?”. Purview resuelve estructuralmente esa brecha, pero requiere un cambio de modelo mental en el equipo de seguridad.
