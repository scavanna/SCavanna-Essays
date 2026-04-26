---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art01-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art01-ms-csu-security-mcsa/
---

# Introducción: Tipo de Contenido y Objetivo

El documento describe el escenario de gestión de datos y cumplimiento normativo previo a la llegada de Microsoft Purview. Aborda las tecnologías principales ligadas a Microsoft 365 y Azure, así como las políticas de seguridad y gobernanza antes de su integración. El fin del texto es evidenciar los problemas críticos de este modelo fragmentado: silos de políticas, visibilidad limitada, administración dispersa del entorno y riesgo regulatorio elevado. Este diagnóstico prepara el terreno para comprender por qué y cómo la unificación de soluciones—lanzada en abril de 2022—transformó fundamentalmente la protección y manejo de datos empresariales.

En la era pre-Purview, las organizaciones enfrentaban un entorno dividido donde las reglas y políticas de seguridad vivían en universos separados. Por un lado, Microsoft 365 gestionaba la protección de contenido en correos, sitios y mensajería, usando portales y normas propias. Por otro, Azure controlaba las bases de datos estructuradas bajo una lógica y administración totalmente distinta. Así, la clasificación de un documento como “Confidencial” en SharePoint no se transfería automáticamente a su nueva ubicación en Azure SQL, generando brechas de protección. Aunque las tecnologías de Microsoft permitían la extensión de etiquetas, el proceso era manual y propenso a errores, lo que llevaba a datos sobreprotegidos en un sistema y desprotegidos en otro, dependiendo de quién aplicara las políticas.

La visibilidad resultaba un desafío mayor. En sistemas fragmentados, seguir el recorrido completo de un archivo era prácticamente imposible. Los datos podían migrar entre usuarios, dispositivos y servicios sin un registro unificado, dificultando enormemente tanto la detección de incidentes como las respuestas a solicitudes legales sobre datos personales. La operación diaria también se veía afectada, dado que la incapacidad para localizar datos sensibles impedía gestionar su retención y eliminación correctamente, incrementando el riesgo y los costos.

Por otro lado, la administración de la seguridad era un proceso disperso. Los equipos debían coordinarse navegando múltiples consolas e interfaces, como Exchange, SharePoint y Azure, además de ocasionalmente terceros o simples hojas de cálculo. Esto creaba un “archipiélago de consolas” donde manejar incidentes o retenciones especiales requería esfuerzo manual, mucha comunicación informal y aceptaba como norma la pérdida de información clave en los traspasos.

Finalmente, este conjunto de factores desembocaba en un riesgo severo de incumplimiento ante auditorías y regulaciones como el GDPR, la Ley 25.326 de Argentina o la LGPD de Brasil. Las organizaciones raramente sabían con certeza dónde estaban sus datos personales, quién tenía acceso o si eliminaban información de ex-empleados a tiempo. Ante nuevas exigencias, debían multiplicar controles, capacitaciones y validaciones, aumentando costes y complejidad sin obtener garantías plenas. Este “control fragmentado” ofrecía sólo una falsa certeza de cumplimiento. El problema se agravaba porque la mayoría de los datos eran no estructurados—correos, documentos, chats—escapando a herramientas inicialmente creadas para tablas y bases. Por todo esto, la gestión de datos dejó de ser una cuestión solo técnica y se volvió un pilar central para la resiliencia y sostenibilidad del negocio.

Fuentes: 
[https://learn.microsoft.com/en-us/purview/purview-compliance-solutions-overview]
[https://learn.microsoft.com/en-us/purview/data-map-overview]
[https://learn.microsoft.com/en-us/purview/retention-policies]
[https://learn.microsoft.com/en-us/purview/compliance-manager-overview]
[https://learn.microsoft.com/en-us/security/zero-trust/data/unstructured-data-governance]
[https://learn.microsoft.com/en-us/purview/compliance-manager-templates-gdpr]



# Silos, Visibilidad Limitada, Fragmentación y Riesgo: El Escenario Pre-Purview

## Conceptos Clave

### Silos de Políticas: "El Idioma Roto"

En el modelo pre-Purview, las organizaciones operaban con al menos dos universos de políticas desconectados:

- Las reglas de seguridad de Microsoft 365 (Exchange, SharePoint, Teams) residían en el portal de cumplimiento de M365 y estaban diseñadas para proteger la productividad y colaboración  
  [Microsoft 365 compliance solutions overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/microsoft-365-compliance-solutions-overview).
- Las políticas de gobernanza de Azure, orientadas a datos estructurados, existían en Azure Portal, con lógicas y administradores separados  
  [Azure SQL Database documentation](https://learn.microsoft.com/en-us/azure/azure-sql/).

Un documento clasificado como "Confidencial" en SharePoint podía migrar a una base de datos de Azure SQL sin arrastrar la señal de sensibilidad. Aunque Microsoft Information Protection permite extender etiquetas más allá de servicios M365, el proceso no era automático ni uniforme  
[Sensitivity labels integration](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels).

La consecuencia era que un mismo dato podía estar sobreprotegido en un entorno y menos protegido en otro, según qué equipo lo gestionara y qué herramienta aplicara las políticas.

### Visibilidad Limitada: "El Dato Sin Pasaporte"

En arquitecturas fragmentadas, rastrear el ciclo completo de un dato era imposible. Un archivo podía originarse en el buzón de un ejecutivo, ser transferido a Teams, descargado a dispositivos personales y cargado en servicios externos, sin registro unificado de su trayecto  
[Purview Data Map overview](https://learn.microsoft.com/en-us/azure/purview/data-map-overview).

- **Seguridad:** Sin linaje de datos, era inviable identificar el alcance de una brecha, requiriendo investigaciones manuales prolongadas.
- **Cumplimiento:** Responder a una DSAR (Data Subject Access Request) implicaba búsquedas manuales en múltiples sistemas, con alta probabilidad de respuestas incompletas.
- **Operación:** La falta de visibilidad sobre la ubicación de datos sensibles impedía aplicar retención y eliminación coherente, acumulando riesgos y costos  
  [Retention policies overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/retention-policies).

### Fragmentación de Gobernanza: "El Archipiélago de Consolas"

Los equipos de seguridad debían navegar entre múltiples consolas de administración con nociones y flujos incompatibles  
[Microsoft 365 compliance solutions overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/microsoft-365-compliance-solutions-overview):

- Exchange Admin Center para reglas de correo.
- SharePoint Admin para permisos y sitios.
- Azure Portal para datos estructurados.
- Herramientas de terceros para registros, eDiscovery o monitoreo.
- Hojas de cálculo manuales para consolidación.

El resultado era un archipiélago de islas administrativas sin puentes, donde coordinar incidentes que cruzaban sistemas requería comunicación manual y alta probabilidad de pérdida de contexto en las transferencias. Por ejemplo, retener documentos de un empleado bajo investigación forzaba acciones manuales en varios sistemas, sin garantía de coordinación ni de captura completa  
[Retention policies overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/retention-policies).

### Riesgo de Incumplimiento Normativo: "La Auditoría como Ruleta Rusa"

Las organizaciones rara vez tenían visibilidad consolidada de su cumplimiento  
[Compliance Manager documentation](https://learn.microsoft.com/en-us/microsoft-365/compliance/compliance-manager-overview).

Preguntas frecuentes imposibles de responder con precisión:

- ¿Qué documentos contienen datos personales sin clasificar?
- ¿Se eliminan datos de ex-empleados en los plazos legales?
- ¿Puede algún administrador de Microsoft acceder a datos sensibles sin ser detectado?
- ¿Qué datos sensibles han salido de la organización recientemente?

El resultado era un riesgo regulatorio activo: las multas bajo GDPR pueden alcanzar el 4 % de la facturación global  
[GDPR penalties - Microsoft guide](https://learn.microsoft.com/en-us/compliance/regulations/gdpr). Además, existen riesgos de daño reputacional y exposición jurídica.

En auditorías externas, las evidencias debían reconstruirse manualmente, exponiendo a la organización a sanciones inesperadas.

## Notas de Soporte

### Costo Oculto de la Fragmentación

Cada regulación nueva obligaba a implementar controles en sistemas independientes, creando más configuración, más validaciones y más capacitación. Esto elevaba el costo operativo y la complejidad de forma acumulativa  
[Global regulatory compliance challenges](https://learn.microsoft.com/en-us/microsoft-365/compliance/compliance-manager-overview).

### El Dato No Estructurado como Epicentro

La mayoría del dato corporativo es no estructurado: correos, documentos, mensajes de chat, grabaciones. Las herramientas de gobernanza tradicionales estaban diseñadas para datos estructurados (bases de datos, tablas). El crecimiento de Microsoft 365, especialmente Teams, amplió la brecha entre producción y gobernanza de este tipo de dato  
[Unstructured data and governance](https://learn.microsoft.com/en-us/security/zero-trust/strategy-unstructured-data).

### La Trampa de la Falsa Certeza

El control fragmentado daba una confianza engañosa. Las políticas de retención parecían cubrir todos los canales hasta que una auditoría mostraba huecos, como mensajes de Teams no gestionados. Las reglas DLP no bloqueaban la exfiltración si existían endpoints fuera de gestión centralizada  
[Retention policies overview](https://learn.microsoft.com/en-us/microsoft-365/compliance/retention-policies).

### Relevancia en LATAM

La simultaneidad de marcos regulatorios en Latinoamérica eleva la complejidad: Ley 25.326 (Argentina), LGPD (Brasil), Ley 19.628 (Chile) y otros, además de requerimientos internacionales como GDPR. Microsoft reconoce que la gestión multinorma en ambientes fragmentados representa un reto significativo y costoso  
[Global regulatory compliance challenges](https://learn.microsoft.com/en-us/microsoft-365/compliance/compliance-manager-overview).
