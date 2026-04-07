# Los Dos Mundos Previos

Antes de la integración, **Azure Purview** era una plataforma empresarial enfocada en datos estructurados, con funciones como escaneo automatizado, linaje y catálogo centralizado. Por su parte, **Microsoft 365 Compliance** gestionaba contenido no estructurado con herramientas basadas en etiquetas de sensibilidad, DLP, eDiscovery y auditoría.

Ambas plataformas funcionaban separadamente y no permitían, por ejemplo, que una etiqueta de sensibilidad aplicada en SharePoint se mantuviera necesariamente al procesar ese archivo fuera del entorno M365, salvo configuración e integración específica.

### El Purview Data Map

El **Purview Data Map** centraliza y facilita la consulta sobre la propiedad de datos a través de múltiples fuentes (en la medida soportada por conectores y configuración). Su capacidad se escala en unidades de 10 GB y soporta integración multicloud y on-premises [https://azure.microsoft.com/en-us/pricing/details/purview/](https://azure.microsoft.com/en-us/pricing/details/purview/), [https://learn.microsoft.com/en-us/azure/purview/microsoft-purview-connector-overview](https://learn.microsoft.com/en-us/azure/purview/microsoft-purview-connector-overview).

### La Etiqueta que Viaja

En escenarios compatibles, las etiquetas de sensibilidad aplicadas en productos como SharePoint o Excel pueden mantenerse y ser reconocidas en servicios adicionales como Fabric o Power BI. Sin embargo, la persistencia depende de la compatibilidad y configuración entre sistemas. No existe interoperabilidad automática entre todos los formatos y sistemas 
[Learn about sensitivity labels | Microsoft Learn](https://learn.microsoft.com/en-us/purview/sensitivity-labels#what-sensitivity-labels-can-do)
[https://learn.microsoft.com/en-us/fabric/security/sensitivity-labels-overview](https://learn.microsoft.com/en-us/fabric/security/sensitivity-labels-overview).

Este comportamiento depende del uso de metadatos compartidos y, en muchos casos, de cifrado persistente asociado a etiquetas mediante tecnologías como AIP/RMS 
[https://learn.microsoft.com/en-us/purview/encryption](https://learn.microsoft.com/en-us/purview/encryption).

### Los Tres Grandes Pilares Funcionales

La nueva organización funcional tras la integración se compone de tres pilares:

  - **Data Governance** (visibilidad y descubrimiento)
  - **Data Security** (protección activa)
  - **Risk & Compliance** (cumplimiento y auditoría)

Estos pilares comparten el Data Map como base 
[Microsoft Purview data compliance solutions | Microsoft Learn](https://learn.microsoft.com/en-us/purview/purview-compliance)


### Azure-Native vs. M365-Native

Los servicios **Azure-native** (Data Map, Unified Catalog, Data Estate Insights, Data Policy, Data Sharing) se adquieren y pagan por capacidad en Azure, gestionados desde el portal de Azure. Los componentes **M365-native** (Information Protection, DLP, Insider Risk, eDiscovery, Audit, Communication Compliance) se licencian por usuario y se gestionan desde el portal purview.microsoft.com 
[Learn about Microsoft Purview billing models | Microsoft Learn](https://learn.microsoft.com/en-us/purview/purview-billing-models) / [Microsoft Purview service description - Service Descriptions | Microsoft Learn](https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-purview-service-description#which-users-need-a-license)

Ciertas acciones avanzadas todavía requieren el portal de Azure.

-----

## Notas de Soporte / Insights de Profundidad

### La convergencia con Microsoft Fabric

Uno de los avances tras la unificación fue la integración directa con **Microsoft Fabric** (lanzado en 2023), permitiendo clasificar y proteger datos analíticos en dicha plataforma [https://learn.microsoft.com/en-us/fabric/governance/microsoft-purview-fabric](https://learn.microsoft.com/en-us/fabric/governance/microsoft-purview-fabric).

### La unificación como respuesta a la competencia

Microsoft Purview busca integrar la gobernanza de datos estructurados y no estructurados, ofreciendo una propuesta técnicamente diferenciada respecto a plataformas como AWS Glue/Macie o GCP Data Catalog/DLP. No se han publicado benchmarks oficiales por parte de Microsoft.

### Impacto en la estructura organizacional

La implementación de Purview puede llevar a una reorganización interna en áreas como Data Governance, Information Security y Legal/Compliance, aunque no es una práctica obligatoria ni un estándar oficial.