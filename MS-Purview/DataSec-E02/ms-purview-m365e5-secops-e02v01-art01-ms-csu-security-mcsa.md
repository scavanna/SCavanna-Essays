---
layout: page
title: "MS_Purview_M365E5-SecOps_e02v01_Art01-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e02/ms-purview-m365e5-secops-e02v01-art01-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e02v01_Art01-MS_CSU_Security_MCSA

Este texto explica el origen y la arquitectura conceptual de Microsoft Purview como plataforma unificada de gobierno, seguridad y cumplimiento del dato. Describe la fusión de Azure Purview y Microsoft 365 Compliance, y cómo se alinean los equipos de datos y de seguridad. Presenta los tres grandes pilares funcionales y sus modelos de licenciamiento. Detalla el nivel avanzado de capacidades llamado Purview Suite. Finalmente expone el principio técnico clave de Purview: la protección persiste y acompaña al dato en todo su ciclo de vida.

En primer lugar, el documento describe el problema previo a 2022. Existían dos plataformas potentes pero aisladas. Azure Purview gobernaba datos estructurados y semiestructurados en nubes y entornos locales. Microsoft 365 Compliance protegía correos, documentos y chats en herramientas de productividad. Una etiqueta aplicada en colaboración no se respetaba en sistemas analíticos. La gobernanza del dato era parcial. Además, esta fragmentación chocaba con la convergencia natural entre equipos de datos y de seguridad.

En segundo lugar, se analiza la unificación de abril de 2022 bajo la marca Microsoft Purview. Se introduce un portal común de administración. También un modelo de metadatos compartido, que permite que las mismas etiquetas de sensibilidad se entiendan en todos los entornos. Aparece una identidad de plataforma única para tratar con auditores y reguladores. El texto recuerda que persisten diferencias entre componentes nativos de Azure y de Microsoft 365. Además, destaca el portal unificado y su modelo de permisos basado en roles.

A continuación, el texto ordena Purview en tres pilares funcionales que comparten el mapa de datos. El pilar de gobierno cubre inventario, linaje, descubrimiento y políticas de acceso, con servicios nativos de Azure y cobro por consumo. El pilar de seguridad de datos abarca clasificación, etiquetado y protección en documentos y correos, con licencias por usuario. El pilar de riesgo y cumplimiento incluye auditoría, eDiscovery y gestión de registros. Esta división también marca responsabilidades de equipos y presupuestos.

En cuarto lugar, el documento explica el nivel de licenciamiento máximo llamado Purview Suite. Es el nuevo nombre del antiguo paquete avanzado de cumplimiento de Microsoft 365. No cambia precios ni capacidades. Reúne funciones avanzadas como prevención de pérdida de datos en dispositivos, gestión de riesgos internos y descubrimiento electrónico mejorado. Añade automatización y modelos de aprendizaje automático. La diferencia frente a Microsoft 365 E3 no es solo cantidad. Cambia el modo operativo, desde controles manuales hasta un control escalable e inteligente.

Finalmente, el texto presenta el principio de diseño central de Purview: la protección viaja con el dato. Una etiqueta de alta confidencialidad cifra el contenido y mantiene restricciones de acceso en cualquier ubicación. Esto aplica a descargas locales, envíos por correo y repositorios externos. El servicio solo libera la clave de descifrado si la identidad, el dispositivo y el contexto cumplen la política. Así, Purview se distingue de enfoques basados en perímetros. La seguridad no desaparece cuando el dato abandona el contenedor original.

# Génesis y arquitectura conceptual de Microsoft Purview

## Conceptos clave

| Concepto                                              | Explicación / Data                                           |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| **El problema estructural previo**                    | Antes de abril de 2022 coexistían dos plataformas sin integración técnica real: Azure Purview, lanzado en 2021, gestionaba el inventario y la gobernanza de activos de datos estructurados y semiestructurados en Azure, AWS, GCP y fuentes on-premises. Microsoft 365 Compliance, por su parte, gestionaba la protección de datos no estructurados en el ecosistema de productividad de Exchange, SharePoint, OneDrive y Teams. Una etiqueta de sensibilidad aplicada a un documento en SharePoint no era reconocida ni respetada cuando ese mismo documento o sus datos derivados llegaban a Azure SQL, a un pipeline de Azure Data Factory o a una herramienta analítica como Power BI conectada a Synapse. La ausencia de puente entre ambas plataformas hacía que la gobernanza del dato fuera, en el mejor caso, parcial: se protegía el dato en su contexto de colaboración o en su contexto de infraestructura, pero rara vez en ambos de forma coordinada. |
| **La unificación de abril de 2022**                   | Microsoft consolidó Azure Purview y Microsoft 365 Compliance bajo la marca Microsoft Purview, con tres cambios técnicos y operativos fundamentales: (1) un portal de administración unificado, purview.microsoft.com, que centraliza la gestión de todos los componentes; (2) un modelo de metadatos compartido que permite que una sensitivity label asignada en Microsoft 365 sea reconocida en el Data Map de Azure y viceversa; (3) una única identidad de plataforma para el gobierno del dato que simplifica la comunicación con reguladores, auditores y juntas directivas. La unificación no eliminó las diferencias arquitectónicas entre componentes Azure-native y M365-native, que mantienen modelos de licenciamiento y administración distintos, pero creó una capa de coherencia semántica que antes no existía. |
| **Los tres pilares funcionales**                      | La familia Purview se organiza en tres grandes áreas operativas que comparten el Purview Data Map como infraestructura común de metadatos: **Data Governance** (inventario del patrimonio de datos, linaje, descubrimiento, calidad y política de acceso centralizada, con componentes predominantemente Azure-native y modelo pay-as-you-go); **Data Security** (clasificación, etiquetado, protección cifrada, DLP, Insider Risk Management, Communication Compliance, con componentes M365-native licenciados por usuario); **Risk & Compliance** (evaluaciones normativas, eDiscovery, Auditoría, Information Barriers y Records Management, también M365-native). La distinción entre pilares no es solo funcional, también define qué equipo administra cada componente, bajo qué modelo de licenciamiento y con qué consola. En organizaciones donde el equipo de Data y Analytics y el equipo de Seguridad y Compliance operan de forma separada, esta distinción es crítica para diseñar el modelo operativo. |
| **Purview Suite como nivel de licenciamiento máximo** | A partir del 1 de octubre de 2025, el paquete de licencias avanzado previamente denominado Microsoft 365 E5 Compliance pasa a llamarse Microsoft Purview Suite. El cambio es de nomenclatura y posicionamiento, sin modificación de capacidades, SKUs ni precios. Purview Suite representa el nivel máximo de capacidades de los pilares de Data Security y Risk & Compliance: incluye automatización con machine learning, Endpoint DLP, Insider Risk Management, Communication Compliance, eDiscovery Premium, Audit Premium con un año de retención, Information Barriers, Customer Key, Customer Lockbox y Privileged Access Management. La diferencia con Microsoft 365 E3 no es solo el número de herramientas, sino el modo de operación: E3 provee control principalmente manual, mientras que Purview Suite aporta inteligencia y automatización para escalar ese control en organizaciones con miles de usuarios y millones de documentos. |
| **Persistencia del control como principio de diseño** | El principio arquitectónico central de Purview es que la protección viaja con el dato, no con el contenedor. Un documento cifrado mediante una sensitivity label de "Altamente confidencial" mantiene sus restricciones de acceso si se descarga a un dispositivo local, si se adjunta a un correo y se envía a un destinatario externo, si se sube a un repositorio de terceros o si se sincroniza en el OneDrive de un usuario comprometido. El cifrado AIP o RMS asociado a la etiqueta implementa este principio: la clave de descifrado solo se entrega si la identidad del solicitante, su dispositivo y su contexto de acceso cumplen las condiciones definidas en la política. Esta persistencia diferencia a Purview de soluciones de seguridad perimetral o basadas en contenedores, en las que la protección desaparece cuando el dato abandona el perímetro controlado. |

## Notas de soporte

### Convergencia entre equipos de datos y seguridad

La unificación bajo Purview en 2022 refleja una tendencia de la industria: los equipos de seguridad y los equipos de datos convergen en torno al mismo problema, gobernar quién accede a qué información, con qué propósito y bajo qué condiciones.  
Los equipos de datos priorizan inventario, linaje y descubrimiento.  
Los equipos de seguridad priorizan clasificación, control y auditoría.  

Purview busca resolver ambas necesidades con una plataforma unificada, evitando que las organizaciones mantengan dos stacks paralelos sin integración.

### Diferencias operativas entre componentes Azure-native y M365-native

La distinción entre componentes Azure-native y M365-native tiene implicaciones directas en el modelo administrativo:

- Los componentes Azure-native, como Data Map, Unified Catalog, Data Policy y Data Sharing, se configuran y administran desde una cuenta de Microsoft Purview en el portal de Azure, bajo un modelo pay-as-you-go medido en unidades de capacidad.
- Los componentes M365-native, como Information Protection, DLP, Insider Risk y eDiscovery, se administran desde el portal de Purview en purview.microsoft.com, con licenciamiento por usuario.

En organizaciones donde los equipos de Cloud e Infraestructura y los equipos de Security y Compliance son distintos, esta separación de planos de administración define responsabilidades, permisos y presupuestos de forma diferenciada.

### Portal unificado y modelo RBAC

El portal unificado purview.microsoft.com consolidó en 2023 funcionalidades que antes estaban distribuidas entre compliance.microsoft.com y el portal de Azure Purview.  

La migración al portal unificado no fue solo estética: introdujo un modelo de control de acceso basado en roles unificado que permite asignar permisos granulares sobre componentes específicos de Purview sin conceder acceso administrativo global.  

Esto resulta relevante para organizaciones que necesitan que los equipos de Legal, Recursos Humanos, Data Governance y Seguridad trabajen en la misma plataforma con alcances de acceso diferenciados.

### Cambio de nombre a Purview Suite

El cambio de nombre de Microsoft 365 E5 Compliance a Purview Suite fue anunciado por Microsoft en septiembre de 2025 y entró en vigor el 1 de octubre de 2025.  

Para organizaciones con contratos Enterprise Agreement o Microsoft Customer Agreement vigentes, el cambio de nombre no requiere acciones contractuales adicionales.
