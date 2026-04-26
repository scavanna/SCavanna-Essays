---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art08-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art08-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art08-MS_CSU_Security_MCSA

Este documento técnico explica las diferencias de cumplimiento entre Microsoft 365 E3 y E5. Se centra en capacidades de Microsoft Purview, protección de información, prevención de pérdida de datos y eDiscovery. Describe cómo los add-ons permiten una migración gradual desde E3. Además, propone un marco financiero de retorno de inversión basado en riesgo y contexto regulatorio. También detalla funciones avanzadas como Customer Key y Customer Lockbox, clave para soberanía de datos. Finalmente, ofrece criterios prácticos de decisión para combinar E3, add-ons y E5 de forma óptima. Incluye notas sobre licenciamiento, momentos de renovación y comparación con soluciones de terceros también.  

El texto desglosa una matriz detallada que compara E3 y E5 por capacidad concreta. En protección de información, E5 añade etiquetado automático, clasificadores entrenables y marcas de agua dinámicas. En prevención de pérdida de datos amplía la cobertura a Teams, endpoints, navegador y aplicaciones en la nube. En auditoría y eDiscovery incorpora más retención, eventos críticos y análisis avanzados. Además, solo E5 incluye Insider Risk, Communication Compliance, gestión avanzada de registros y protección adaptable integrada.  

En cuanto a los add-ons, el documento muestra cómo extender E3 sin saltar inmediatamente a E5. Presenta la suite de cumplimiento Purview para E3 y módulos específicos de protección e información y gobierno. Describe complementos separados para eDiscovery y auditoría avanzada, riesgos internos y cumplimiento de comunicaciones. Añade extensiones de retención de auditoría hasta diez años y capacidades forenses. También comenta opciones recortadas para Business Premium, que requieren análisis función por función antes de decidir.  

El documento propone un argumento financiero de tipo consultivo para valorar E3, E5 y add-ons. No usa calculadoras oficiales, sino estimaciones basadas en buenas prácticas del sector. Invita a cada organización a cuantificar sus propios costos de incidentes, honorarios legales y sanciones regulatorias. Conecta las capacidades de cumplimiento con reducción de multas, ahorro en horas de investigación y menor probabilidad de brechas. Así se construye un marco de retorno de inversión adaptado al contexto real.  

Otro eje clave son Customer Key y Customer Lockbox, pensados para entornos altamente regulados. Customer Key permite que el cliente genere y guarde sus claves maestras en Azure Key Vault. Si revoca el acceso, los datos quedan inaccesibles incluso para Microsoft. Customer Lockbox exige aprobación explícita antes de que soporte consulte datos del cliente y deja trazabilidad auditable. Estas funciones refuerzan la soberanía de datos y justifican E5 en organizaciones con requisitos estrictos de control.  

Finalmente, se presenta una regla práctica para decidir el licenciamiento. Usa preguntas sobre escala, complejidad regulatoria, exposición a riesgos internos y carga de litigios. Con ello propone tres patrones: seguir en E3 con add-ons puntuales, pasar a E5 completo o segmentar licencias por colectivos. Las notas finales advierten sobre la trampa de add-ons acumulativos y el licenciamiento por usuario. También resaltan las renovaciones contractuales y la integración de E5 frente a soluciones aisladas externas claramente.





# Nivel 2: Conceptos clave

## Concepto 1. Matriz de comparativa detallada: capacidad por capacidad, sin ambigüedad

La comparativa entre Microsoft 365 E3 y E5 debe realizarse por área funcional, siguiendo la oferta oficial de Microsoft 365 Enterprise ([comparativa de productos](https://learn.microsoft.com/en-us/microsoft-365/enterprise/compare-microsoft-365-enterprise-plans)).

- **Information Protection**
  - **E3**: Etiquetado manual por el usuario en aplicaciones Office, web y móvil, con cifrado básico.
  - **E5**: Auto-labeling con clasificadores entrenables de machine learning. El sistema detecta y etiqueta documentos sensibles de forma automática, permite etiquetado por defecto en librerías de SharePoint y marcas de agua dinámicas que incluyen la identidad del usuario ([Auto-labeling E5](https://learn.microsoft.com/en-us/microsoft-365/compliance/data-classification-content-explorer#auto-label-policies)).

- **Data Loss Prevention (DLP)**
  - **E3**: DLP para Exchange, SharePoint y OneDrive.
  - **E5**: DLP para Teams, Endpoint DLP para Windows y macOS, Browser DLP en Microsoft Edge gestionado, DLP para aplicaciones cloud mediante Microsoft Defender for Cloud Apps y Adaptive Protection ([DLP por SKU](https://learn.microsoft.com/en-us/microsoft-365/compliance/dlp-license-requirements?view=o365-worldwide), [Adaptive Protection](https://learn.microsoft.com/en-us/microsoft-365/compliance/dlp-adaptive-protection?view=o365-worldwide)).

- **Auditoría**
  - **E3**: Audit Standard con 90 días de retención de logs.
  - **E5**: Audit Premium con un año de retención por defecto, eventos críticos como `MailItemsAccessed` y API para extracción masiva. Existe un add-on opcional para extender la retención hasta 10 años ([Audit Standard vs Premium](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-solutions#compare-audit-standard-and-audit-premium), [retención de auditoría](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-retention)).

- **eDiscovery**
  - **E3**: eDiscovery Standard.
  - **E5**: eDiscovery Premium, con gestión avanzada de custodios, análisis asistido por IA, legal holds, conjuntos de revisión (review sets) y exportación con cadena de custodia documentada ([eDiscovery Premium E5](https://learn.microsoft.com/en-us/microsoft-365/compliance/ediscovery-licensing-permissions)).

- **Insider Risk Management**
  - **E3**: No incluido.
  - **E5**: Insider Risk Management completo, con correlación multicanal, escenarios de riesgo predefinidos, integración con procesos de RRHH y soporte a Adaptive Protection ([Insider Risk Management](https://learn.microsoft.com/en-us/microsoft-365/compliance/insider-risk-management?view=o365-worldwide)).

- **Communication Compliance**
  - **E3**: No incluido.
  - **E5**: Communication Compliance para Teams, Exchange, Viva Engage y otros canales, con clasificadores de procesamiento de lenguaje natural y detección de acoso y riesgos regulatorios ([Communication Compliance](https://learn.microsoft.com/en-us/microsoft-365/compliance/communication-compliance-licensing)).

- **Records Management**
  - **E3**: Capacidades de retención básica y eliminación automática.
  - **E5**: Advanced Records Management, Regulatory Records, declaración automática mediante clasificadores entrenables y Proof of Disposal con evidencia auditada ([Records Management E5](https://learn.microsoft.com/en-us/microsoft-365/compliance/records-management-solution-licensing)).

- **Clasificadores entrenables**
  - **E3**: Limitado a tipos de información confidencial (SITs) predefinidos.
  - **E5**: Clasificadores entrenables de machine learning reutilizables en auto-labeling, Communication Compliance, Records Management e Insider Risk ([trainable classifiers](https://learn.microsoft.com/en-us/microsoft-365/compliance/classifier-overview)).

- **Adaptive Protection**
  - **E3**: No disponible.
  - **E5**: Integración entre Insider Risk y DLP para aplicar políticas DLP dinámicas basadas en el nivel de riesgo del usuario ([Adaptive Protection](https://learn.microsoft.com/en-us/microsoft-365/compliance/dlp-adaptive-protection?view=o365-worldwide)).


## Concepto 2. Add-ons modulares: migración progresiva desde E3

La decisión entre E3 y E5 no es estrictamente binaria. Microsoft ofrece add-ons de cumplimiento que permiten a organizaciones con E3 incorporar capacidades específicas de E5 de forma modular ([add-ons de cumplimiento](https://learn.microsoft.com/en-us/microsoft-365/compliance/microsoft-365-compliance-add-on-licenses)).

**Add-ons relevantes:**

- **Microsoft Purview Suite para E3**  
  Añade el conjunto de capacidades de cumplimiento de E5, excluyendo las funciones de protección contra amenazas y seguridad de identidad.

- **E5 Information Protection & Governance**  
  Incorpora auto-clasificación y etiquetado por defecto, marcas de agua dinámicas, así como Advanced Records Management.

- **E5 eDiscovery & Audit**  
  Incluye Audit Premium con un año de retención y eventos avanzados como `MailItemsAccessed`, además de eDiscovery Premium.

- **E5 Insider Risk Management**  
  Añade Insider Risk Management y Communication Compliance para gestionar riesgos internos y comunicaciones reguladas.

- **10-year Audit Log Retention**  
  Extiende la retención de logs por encima de lo que ofrece Audit Premium, hasta 10 años ([retención de auditoría](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-retention)).

- **Forensic Evidence**  
  Add-on independiente que puede estar disponible incluso sobre E5, orientado a capacidades forenses avanzadas ([Forensic Evidence](https://learn.microsoft.com/en-us/microsoft-365/compliance/forensic-evidence?view=o365-worldwide)).

- **Add-ons para Microsoft 365 Business Premium**  
  Algunas funciones avanzadas de Purview están disponibles también para Business Premium, pero no son equivalentes en alcance a lo que proporciona E5 Enterprise ([add-ons para Business Premium](https://learn.microsoft.com/en-us/microsoft-365/compliance/microsoft-365-compliance-add-on-licenses)).

**Nota**: Las opciones para Business Premium deben evaluarse función por función, ya que su alcance difiere respecto a E5.

La lógica de decisión con add-ons debe considerar perfil regulatorio, exposición al riesgo, frecuencia esperada de uso de cada capacidad y el costo total de la combinación de add-ons frente al costo de E5 completo.


## Concepto 3. Argumento financiero: marco consultivo de ROI

El análisis financiero y de retorno de inversión asociado a E3, E5 y los add-ons es de carácter consultivo y debe contrastarse con datos internos de cada organización.

- Las estimaciones de ROI no se basan en calculadoras oficiales de Microsoft, sino en buenas prácticas habituales del sector.
- Aspectos como honorarios de consultores y abogados, multas regulatorias o costos de respuesta a incidentes deben cuantificarse con información propia de la organización.
- Las referencias a multas bajo GDPR o LGPD se apoyan en la legislación aplicable y no en documentación de Microsoft ([GDPR fines](https://gdpr-info.eu/art-83-gdpr/), [LGPD fines](https://www.gov.br/anpd/pt-br/assuntos/legislacao/lgpd)).


## Concepto 4. Customer Key y Customer Lockbox: control y soberanía de datos

Las capacidades de Customer Key y Customer Lockbox son argumentos clave a favor de E5 en contextos de alta sensibilidad y regulación de datos.

- **Customer Key**  
  El cliente genera y almacena las claves maestras en Azure Key Vault. Si el cliente revoca el acceso a esas claves, los datos protegidos se vuelven inaccesibles incluso para Microsoft ([Customer Key](https://learn.microsoft.com/en-us/microsoft-365/compliance/customer-key-overview)).

- **Customer Lockbox**  
  El acceso de soporte técnico de Microsoft a los datos solo se produce tras una aprobación explícita del cliente. Cada solicitud queda registrada de forma auditable ([Customer Lockbox](https://learn.microsoft.com/en-us/microsoft-365/compliance/customer-lockbox-requests)).

Estas capacidades resultan especialmente relevantes en sectores con requisitos estrictos de soberanía de datos, regulaciones sectoriales o expectativas elevadas de control por parte del cliente sobre el acceso a su información.


## Concepto 5. Regla de oro aplicada: cómo tomar la decisión de licenciamiento

La aplicación práctica de la diferencia E3 vs E5 se articula en torno a un conjunto de preguntas diagnósticas y rutas de decisión que, aunque no constituyen un framework oficial de Microsoft, están alineadas con la forma en que se estructura el licenciamiento por riesgo y perfil organizacional ([licencias por plan](https://learn.microsoft.com/en-us/microsoft-365/enterprise/compare-microsoft-365-enterprise-plans)).

Elementos clave del enfoque:

- Utilizar preguntas sobre escala, complejidad regulatoria, exposición a insider risk y carga de litigios o investigaciones para determinar el nivel de capacidades necesario.
- Considerar tres patrones típicos:
  - Permanecer en E3 con add-ons específicos cuando solo se necesitan capacidades puntuales.
  - Migrar a E5 completo cuando el volumen de datos, amenazas y obligaciones regulatorias hace insuficiente la gestión manual.
  - Segmentar licencias por usuario o colectivo cuando distintas áreas del negocio tienen perfiles de riesgo muy diferentes.

Este modelo sirve como guía práctica para decidir entre E3, E3 más add-ons o E5 completo, en función de la combinación de riesgo, escala operativa y presupuesto.


# Nivel 3: Notas de soporte e insights de profundidad

## Nota 1: La trampa del add-on acumulativo

La suma de múltiples add-ons sobre una base E3 puede superar el costo de migrar directamente a E5, especialmente cuando se agregan capacidades de eDiscovery, auditoría avanzada, Insider Risk y protección de información. Es fundamental calcular el costo total de propiedad comparando ambos escenarios.

## Nota 2: Licenciamiento por usuario frente a licenciamiento por tenant

Algunas capacidades de cumplimiento se habilitan y se aplican por usuario y no a nivel global de tenant. Por ejemplo, características como Endpoint DLP y Audit Premium requieren licencia E5 para cada usuario cuya actividad y dispositivos se desean proteger o auditar ([licenciamiento Endpoint DLP](https://learn.microsoft.com/en-us/microsoft-365/compliance/endpoint-dlp-getting-started?view=o365-worldwide#licensing)).

## Nota 3: Renovación contractual y timing de migración

Las recomendaciones sobre aprovechar ventanas de renovación de contratos para ajustar volúmenes de E3 y E5, negociar precios o introducir add-ons son de carácter consultivo. Sin embargo, el momento de renovación suele ser el más eficiente para realizar cambios significativos en la mezcla de licencias.

## Nota 4: Contexto competitivo e integración nativa

En comparativas con soluciones puntuales de terceros, la integración nativa de E5 en el ecosistema Microsoft 365 simplifica la operación, reduce fricciones entre herramientas y puede reducir costos indirectos de administración, soporte y respuesta a incidentes. Esta integración debe considerarse como parte del análisis de coste-beneficio al evaluar alternativas a E5.
