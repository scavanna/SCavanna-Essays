---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art02-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art02-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art02-MS_CSU_Security_MCSA

Este documento explica los conceptos clave de Microsoft Purview como plataforma unificada de gobierno, seguridad y cumplimiento de datos. Describe la evolución desde herramientas separadas en Azure y Microsoft 365 hasta una arquitectura integrada basada en un mapa de datos común. Presenta los principales componentes técnicos, como el Purview Data Map, las etiquetas de sensibilidad y su persistencia entre servicios. También resume pilares de la plataforma, los modelos de licenciamiento Azure‑native y Microsoft 365‑native, la integración con Microsoft Fabric y impactos organizativos de esta convergencia. Está dirigido a equipos de datos, seguridad y cumplimiento que necesiten una visión amplia compartida.

Antes de Purview unificado existían dos mundos avanzados pero aislados. Por un lado, Azure Purview gobernaba datos estructurados y semiestructurados mediante escaneos automáticos, linaje y catálogo centralizado. Por otro, Microsoft 365 Compliance protegía contenido no estructurado en Exchange, SharePoint, Teams y OneDrive con etiquetas, prevención de pérdida de datos, auditoría y eDiscovery. La falta de integración hacía que una etiqueta aplicada en Microsoft 365 no viajara automáticamente hacia procesos analíticos y almacenamiento en Azure directamente.

El Purview Data Map actúa como corazón técnico compartido de la plataforma. Centraliza metadatos sobre el patrimonio de datos, como orígenes, propietarios, clasificaciones y linaje. Escala en unidades de capacidad de diez gigabytes y puede abarcar entornos híbridos, multicloud y locales mediante conectores compatibles. Sobre este mapa se apoyan las funciones de descubrimiento, catalogación, clasificación y gobierno. Gracias a él, los tres pilares de Purview trabajan con una visión coherente y consistente del entorno completo.

Las etiquetas de sensibilidad son un ejemplo claro de integración práctica. Una etiqueta aplicada en SharePoint, Excel o Teams puede conservarse en servicios analíticos como Power BI o Microsoft Fabric, cuando ambos extremos son compatibles. La protección viaja con el dato gracias al modelo de metadatos compartido y, en muchos casos, al cifrado persistente. Sin embargo, no todos los formatos, servicios o bases de datos de Azure soportan el mismo nivel de interoperabilidad ni automatismo.

La arquitectura funcional de Purview se apoya en tres pilares. Data Governance ofrece visibilidad, descubrimiento, catalogación, clasificación y linaje del patrimonio de datos. Data Security aplica la protección activa mediante etiquetas, cifrado y prevención de pérdida de información, reduciendo riesgos de fuga. Risk and Compliance cubre auditoría, eDiscovery y gestión de riesgos internos para cumplir regulaciones. Los tres ámbitos comparten el Purview Data Map y se alinean con las soluciones de cumplimiento en Microsoft 365.

En Purview conviven componentes Azure‑native y Microsoft 365‑native con modelos distintos. Los primeros, como Data Map o Unified Catalog, se contratan por capacidad en Azure y suelen gestionarse desde equipos de datos. Los segundos, como Information Protection o Insider Risk, se licencian por usuario dentro de planes Microsoft 365 y se operan desde el portal de cumplimiento. Esta convergencia responde a la estrategia de Microsoft y favorece una gobernanza transversal entre tecnología, seguridad y legal.





# Microsoft Purview: Conceptos clave y notas de soporte  

## Conceptos clave (Nivel 2)  

### 1. Los dos mundos previos: plataformas sofisticadas sin integración nativa  

Entender la magnitud de la integración de Microsoft Purview requiere identificar qué existía antes y por qué, aun siendo plataformas avanzadas, resultaban operativamente limitadas cuando se analizaban por separado.  

**Azure Purview (antes de la consolidación)** era una plataforma empresarial orientada principalmente a datos estructurados y semiestructurados, con:  

- Escaneo automatizado de fuentes de datos.  
- Linaje automático de datos.  
- Catálogo centralizado para facilitar descubrimiento y gobierno.  

**Microsoft 365 Compliance (antes de la consolidación)** gestionaba contenido no estructurado de colaboración y productividad, como:  

- Correos en Exchange.  
- Documentos en SharePoint.  
- Conversaciones en Teams.  
- Archivos en OneDrive.  

Con capacidades como:  

- Etiquetas de sensibilidad.  
- Políticas DLP (Data Loss Prevention).  
- eDiscovery.  
- Auditoría.  

La separación entre ambos entornos era estructural. Por ejemplo, un documento etiquetado como "Altamente confidencial" en SharePoint no conservaba necesariamente esa etiqueta al pasar a procesos en Azure Data Factory o a un almacenamiento fuera de Microsoft 365, salvo que se configuraran integraciones y conectores específicos.  

---

### 2. El Purview Data Map como corazón técnico compartido  

El **Purview Data Map** permite centralizar y consultar información sobre el patrimonio de datos de la organización, incluyendo metadatos clave sobre la propiedad y características de los datos, a través de múltiples orígenes en la medida en que están soportados por conectores y configuración.  

Características relevantes:  

- El Data Map escala en unidades de capacidad (Capacity Units o CU) de 10 GB.  
- Puede conectarse a fuentes multicloud y on-premises, según la compatibilidad documentada.  
- Funciona como base para actividades de descubrimiento, clasificación y gobierno de datos.  

Referencias técnicas:  

- Precios y capacidades del Purview Data Map:  
  <https://azure.microsoft.com/en-us/pricing/details/purview/>  
- Fuentes soportadas por el Data Map:  
  <https://learn.microsoft.com/en-us/azure/purview/tutorial-scan-data-sources>  

---

### 3. La etiqueta que viaja: persistencia de etiquetas de sensibilidad  

Un caso de uso ilustrativo de la integración es el de las **etiquetas de sensibilidad persistentes**.  

En escenarios compatibles:  

- Una etiqueta aplicada en SharePoint o Excel puede conservarse y respetarse en servicios como Microsoft Fabric, Power BI y otros productos integrados.  
- La infraestructura de metadatos compartida y, en muchos casos, el cifrado persistente asociado mediante AIP/RMS, permiten que la protección viaje con el dato cuando así lo soportan los sistemas implicados.  

Sin embargo:  

- La persistencia de etiquetas entre todos los entornos (por ejemplo, Azure SQL o Azure Data Factory) depende de la compatibilidad técnica de cada servicio, de las integraciones disponibles y de la configuración aplicada.  
- No todos los formatos ni sistemas garantizan interoperabilidad automática de etiquetas y protecciones.  

Referencias técnicas:  

- Etiquetas de sensibilidad soportadas en Purview:  
  <https://learn.microsoft.com/en-us/azure/purview/sensitivity-labels>  
- Etiquetas de sensibilidad en Microsoft Fabric:  
  <https://learn.microsoft.com/en-us/fabric/security/sensitivity-labels-overview>  
- Cifrado basado en etiquetas en Microsoft 365:  
  <https://learn.microsoft.com/en-us/microsoft-365/compliance/encryption?view=o365-worldwide>  

---

### 4. Los tres grandes pilares funcionales: nueva arquitectura del control  

Tras la reorganización funcional de Microsoft Purview, las capacidades se agrupan en tres grandes pilares:  

1. **Data Governance**  
   - Visibilidad y descubrimiento de datos.  
   - Catalogación y clasificación.  
   - Entendimiento del linaje y del patrimonio de datos.  

2. **Data Security**  
   - Protección activa de la información.  
   - Etiquetas de sensibilidad, cifrado y DLP.  
   - Controles que reducen el riesgo de exposición y fugas.  

3. **Risk & Compliance**  
   - Cumplimiento regulatorio y normativo.  
   - Auditoría, eDiscovery y gestión de riesgos internos.  

Los tres pilares comparten el Purview Data Map como base de metadatos y conocimiento del entorno de datos, siempre dentro del alcance de las fuentes y conectores configurados.  

Referencia general de soluciones de cumplimiento:  

- Soluciones de cumplimiento de Microsoft 365:  
  <https://learn.microsoft.com/es-es/microsoft-365/compliance/compliance-solutions-overview>  

---

### 5. Azure-native y M365-native: distinción crítica de diseño y licenciamiento  

Dentro de Microsoft Purview conviven dos dominios con modelos de consumo y operación distintos:  

**Componentes Azure-native**  

- Ejemplos: Data Map, Unified Catalog, Data Estate Insights, Data Policy, Data Sharing.  
- Se adquieren y facturan por capacidad en Azure (modelo pay-as-you-go).  
- Suelen ser operados por equipos de datos, ingeniería y arquitectura a través del portal de Azure.  

**Componentes M365-native**  

- Ejemplos: Information Protection, DLP, Insider Risk Management, eDiscovery, Audit, Communication Compliance.  
- Se licencian por usuario y están incluidos en determinados planes de Microsoft 365 E3/E5.  
- Su administración se realiza principalmente desde el portal purview.microsoft.com, en el ámbito de seguridad y cumplimiento.  

Aunque ambos dominios se visualizan y gestionan a través del portal de Purview, algunas operaciones avanzadas siguen requiriendo el portal de Azure.  

Referencias de licenciamiento:  

- Licenciamiento de Purview (gobernanza):  
  <https://learn.microsoft.com/en-us/azure/purview/governance-pricing>  
- Licenciamiento de Information Protection y cumplimiento en Microsoft 365:  
  <https://learn.microsoft.com/en-us/microsoft-365/compliance/pricing?view=o365-worldwide>  

---

## Notas de soporte e insights de profundidad (Nivel 3)  

### Nota 1. Significado del nombre "Purview"  

El término **"purview"** comunica la idea de alcance, autoridad y capacidad de visibilidad extendida. Esta elección de nombre está alineada con la ambición de Microsoft de ofrecer una plataforma que unifique la gobernanza y el cumplimiento sobre datos estructurados y no estructurados en múltiples entornos.  

---

### Nota 2. Convergencia con Microsoft Fabric  

Tras la unificación bajo la marca Microsoft Purview, uno de los avances relevantes es la integración con **Microsoft Fabric** (lanzado en 2023).  

Esta integración permite:  

- Clasificar y proteger datos analíticos directamente en Fabric.  
- Extender las capacidades de gobernanza y protección de Purview al ecosistema analítico moderno de Microsoft.  

Referencia:  

- Visión general de Microsoft Fabric y su integración con Purview:  
  <https://learn.microsoft.com/en-us/fabric/data-activator/microsoft-fabric-overview>  

---

### Nota 3. Unificación como respuesta competitiva  

Desde una perspectiva consultiva, la estrategia de Microsoft Purview busca integrar la gobernanza de datos estructurados y no estructurados en una sola plataforma, lo que la diferencia de aproximaciones en otros proveedores cloud, como:  

- AWS (por ejemplo, combinaciones de Glue, Macie y otros servicios).  
- Google Cloud (por ejemplo, Data Catalog y DLP).  

No existen benchmarks públicos oficializados por Microsoft que comparen de forma directa y estandarizada el grado de integración horizontal frente a estas alternativas. La evaluación comparativa suele hacerse caso por caso, en función de los requisitos de cada organización.  

---

### Nota 4. Impacto organizacional interno  

La consolidación de capacidades en Microsoft Purview tiene implicaciones potenciales en la estructura de los equipos internos. En muchas organizaciones puede motivar:  

- Una mayor coordinación entre equipos de Data Governance, Seguridad de la Información y Legal/Compliance.  
- La creación de modelos de gobierno cruzado donde se comparten herramientas, métricas y repositorios de metadatos.  

Estas recomendaciones se enmarcan en buenas prácticas consultivas y no constituyen un estándar obligatorio ni una directriz oficial de Microsoft.
