---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art03-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art03-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art03-MS_CSU_Security_MCSA



Este documento describe la arquitectura de gobierno, seguridad y cumplimiento de datos basada en Microsoft Purview y Microsoft 365. Explica tres pilares complementarios: Data Governance como sistema de descubrimiento, Data Security como sistema de protección activa y Risk and Compliance como sistema de demostración. Relaciona estas capacidades con Azure, nubes de terceros y entornos locales. Detalla audiencias, modelos de consumo y responsabilidades operativas. Además muestra cómo el portal unificado de Purview orquesta señales entre pilares, habilita automatización progresiva y refuerza el valor financiero de una plataforma integrada. También ofrece notas sobre errores comunes, niveles de madurez y retorno de inversión.

El primer pilar, Data Governance, construye el mapa completo del patrimonio de datos. Reúne metadatos de Azure, nubes externas y entornos locales mediante conectores y escaneos continuos. Sobre ese mapa se apoya un catálogo unificado para búsqueda, glosario y análisis ejecutivos. Además incorpora políticas centrales de acceso y capacidades de intercambio controlado con terceros. Lo operan principalmente equipos de datos y arquitectura, mediante servicios nativos de Azure y un modelo de pago por capacidad escalable.

El segundo pilar, Data Security, aplica protección activa sobre la colaboración. Usa clasificación y etiquetado de sensibilidad para asociar cifrado, restricciones de acceso y marcas visuales en correos, documentos y chats. Las políticas de prevención de pérdida de datos vigilan movimientos hacia correo externo, dispositivos y aplicaciones en la nube. Además, las soluciones de riesgo interno y cumplimiento de comunicaciones detectan comportamientos anómalos, y la protección adaptativa ajusta la severidad según el riesgo real observado.

El tercer pilar, Risk and Compliance, no protege directamente los datos. Su misión es demostrar ante auditores, reguladores y tribunales que los controles existen, funcionan y dejan rastro verificable. Para ello centraliza evaluaciones de cumplimiento, flujos completos de investigación legal, registros de auditoría ampliados y gestión avanzada del ciclo de vida de registros. También incorpora barreras de información entre grupos que no deben comunicarse, todo licenciado por usuario e integrado en el portal de Purview.

La solución diferencia componentes nativos de Azure y de Microsoft 365, y define quién los compra, opera e integra. Los servicios de gobernanza suelen vivir en Azure y tratar datos estructurados o semiestructurados. Las capacidades de seguridad y cumplimiento se enfocan en contenido colaborativo como correo y archivos. El portal unificado de Purview conecta ambos mundos y permite que descubrimiento, protección y evidencia compartan señales, creando un ciclo de mejora continua entre los tres pilares.

Las notas de soporte destacan errores habituales, como desplegar controles de seguridad sin una capa previa de descubrimiento y catálogo de datos. Subrayan el papel cultural del portal unificado, que obliga a seguridad, cumplimiento e ingeniería a compartir métricas y lenguaje. Además, proponen un modelo de madurez en tres niveles, desde fundamentos hasta optimización con protección adaptativa. Finalmente, explican que integrar todo en Purview reduce costos de licencias dispersas, proyectos de integración y formación duplicada.

# Nivel 2: Conceptos clave

## Pilar I: Data Governance, el sistema de descubrimiento

El pilar de Data Governance es la capa más infraestructural y menos visible. Opera por debajo de la línea de visibilidad del usuario final y construye el mapa del territorio de datos que los otros pilares necesitan para operar con precisión.

### Componentes principales

- **Data Map**  
  Repositorio elástico de metadatos que escanea activos en Azure, AWS, GCP y fuentes on premises mediante conectores especializados. No es un snapshot estático: ejecuta escaneos programados y actualiza el inventario cuando se crean, modifican o eliminan activos. Escala por unidades de capacidad, con almacenamiento de metadatos asociado.

- **Unified Catalog**  
  Interfaz de búsqueda y descubrimiento construida sobre el Data Map. Permite localizar activos de datos usando lenguaje natural, filtros por clasificación o sensibilidad, y términos de glosario de negocio.

- **Data Estate Insights**  
  Panel analítico que agrega la información del Data Map en métricas ejecutivas, como porcentaje de activos clasificados y distribución por nivel de sensibilidad.

- **Data Policy**  
  Administración centralizada de acceso para fuentes de datos en Azure SQL y Azure Storage. Permite definir quién puede acceder a qué datos desde una interfaz unificada.

- **Data Sharing**  
  Capacidad de compartir activos de datos con organizaciones externas sin duplicar el almacenamiento. El receptor accede a una proyección del dato original bajo control del propietario.

### Modelo de consumo

- Pay as you go, gestionado desde Azure Portal.  
- Operado habitualmente por equipos de Data Engineering o Data Architecture.

### Audiencia primaria

- Chief Data Officers  
- Data Engineers  
- Data Architects  
- Data Stewards  

### Pregunta que responde

> ¿Qué datos tenemos, dónde están, cuál es su calidad y quién debería tener acceso a ellos?

---

## Pilar II: Data Security, el sistema de protección activa

El pilar de Data Security es el más operativo y el más perceptible para los usuarios finales. Sus controles se manifiestan como etiquetas visibles en documentos, bloqueos de acciones de copia, alertas de comportamiento inusual y restricciones dinámicas de acceso. Es el brazo ejecutor de las políticas definidas a nivel de gobernanza.

### Componentes principales

- **Information Protection**  
  Motor de clasificación y etiquetado que asigna etiquetas de sensibilidad a los datos y vincula a cada etiqueta controles de protección como cifrado, restricciones de acceso y marcas de agua. Opera en Exchange, SharePoint, OneDrive, Teams y aplicaciones Office. Permite etiquetado automático basado en tipos de información sensibles predefinidos y clasificadores entrenables con machine learning en capacidades E5.

- **Data Loss Prevention (DLP)**  
  Sistema de control de flujos de información que monitorea e intercepta movimientos de datos sensibles en puntos de salida como correo electrónico, chats de Teams, dispositivos endpoint (USB, impresoras, navegadores) y aplicaciones cloud de terceros. Actúa sobre el perímetro de datos, no sobre el perímetro de red.

- **Insider Risk Management**  
  Sistema de análisis de comportamiento de usuarios que correlaciona señales de actividad en múltiples cargas de trabajo para identificar patrones que sugieren riesgo interno.

- **Communication Compliance**  
  Sistema de supervisión de comunicaciones internas para detectar riesgos regulatorios y de conducta, como lenguaje inapropiado, conflictos de interés o violaciones de políticas en Teams, Exchange, Viva Engage y fuentes externas compatibles.

- **Adaptive Protection**  
  Integración entre Insider Risk Management y DLP que permite elevar de forma dinámica el nivel de restricción para un usuario cuando su comportamiento sugiere riesgo inminente.

### Modelo de consumo

- Licenciado por usuario.  
- Incluido con capacidades básicas en M365 E3 y capacidades completas en M365 E5.  
- Administrado desde el portal purview.microsoft.com.

### Audiencia primaria

- CISOs  
- Security Engineers  
- Analistas de DLP  
- Analistas de Insider Risk  

### Pregunta que responde

> ¿Cómo aseguramos que los datos sensibles sean accedidos, compartidos y gestionados solo por quien debe hacerlo y de la forma adecuada?

---

## Pilar III: Risk & Compliance, el sistema de demostración

El pilar de Risk & Compliance es el más institucional. Su función no es proteger activamente los datos, sino demostrar que los controles existen, funcionan y generan evidencia verificable ante reguladores, auditores, tribunales y órganos de gobierno.

### Componentes principales

- **Compliance Manager**  
  Centro de comando para la gestión de la postura de cumplimiento. Proporciona evaluaciones preconstruidas para numerosos marcos regulatorios (por ejemplo NIST CSF, ISO 27001, GDPR, HIPAA, CMMC, PCI DSS, SOC 2 y marcos locales). Genera un Compliance Score ponderado por riesgo que permite monitorear la postura agregada y priorizar acciones de mejora.

- **eDiscovery Premium**  
  Flujo de trabajo legal de extremo a extremo para investigaciones internas y litigios: identificación de custodios, preservación legal, recolección de evidencia, revisión de datos, análisis de hilos de conversación y exportación con cadena de custodia documentada.

- **Audit Premium**  
  Registro de eventos con retención ampliada, incluyendo eventos críticos como accesos detallados a elementos de correo y otras cargas de trabajo, para análisis forense y cumplimiento.

- **Records Management Advanced**  
  Sistema de gestión del ciclo de vida de registros regulatorios con controles de disposición, flujos de aprobación multinivel, registro inmutable para sectores regulados y evidencia auditada de eliminación.

- **Information Barriers**  
  Implementación de muros de separación regulatoria entre grupos de usuarios, aplicada en Teams, SharePoint y Exchange.

### Modelo de consumo

- Licenciado por usuario.  
- Incluido con capacidades básicas en M365 E3 y capacidades completas en M365 E5.  
- Administrado desde el portal purview.microsoft.com.

### Audiencia primaria

- Chief Compliance Officers  
- Asesoría Legal  
- Auditores internos  
- Records Managers  

### Pregunta que responde

> ¿Podemos demostrar ante cualquier auditor, regulador o tribunal que cumplimos nuestras obligaciones y que contamos con evidencia para probarlo?

---

## Distinción operativa: Azure native y M365 native en la práctica

Comprender qué componentes viven en cada entorno no es solo un detalle de licenciamiento. Define quién los opera, cómo se pagan y qué integraciones son nativas.

### Componentes Azure native

- Adquiridos y gestionados desde Azure Portal.  
- Modelo de costo pay as you go por capacidad de escaneo y almacenamiento de metadatos.  
- Operados por equipos de Data Engineering o Cloud Architecture.  
- Dominio primario: datos estructurados y semiestructurados, como SQL, Storage, Data Lake y pipelines.  
- Interfaz administrativa: Azure Portal y purview.microsoft.com para visibilidad unificada.  
- Componentes principales: Data Map, Unified Catalog, Data Estate Insights, Data Policy, Data Sharing.

### Componentes M365 native

- Incluidos como parte de la licencia M365 E3 o E5 por usuario.  
- Modelo de costo por suscripción mensual por usuario, con capacidades que varían según el nivel de licencia.  
- Operados por equipos de Security y Compliance.  
- Dominio primario: datos no estructurados de colaboración en Exchange, SharePoint, Teams y OneDrive.  
- Interfaz administrativa principal: purview.microsoft.com.  
- Componentes principales: Information Protection, DLP, Insider Risk, eDiscovery, Audit, Compliance Manager y otros servicios de cumplimiento.

### Zona de intersección

El portal purview.microsoft.com actúa como interfaz unificada para ambos mundos. Sin embargo, algunas configuraciones profundas de gobernanza, como el escaneo de SQL Server, la configuración de conectores multicloud o el linaje de pipelines de Azure Data Factory, requieren acceso directo al portal de Azure.

---

## Flujo de valor entre pilares

El valor de implementar los tres pilares de forma integrada se basa en que están diseñados para producir señales que los demás consumen, creando un ciclo de mejora continua.

### De Data Governance a Data Security

El Data Map descubre un activo de datos no estructurado en SharePoint que contiene información financiera sin clasificar. Esta señal se utiliza en Information Protection para aplicar automáticamente una etiqueta de sensibilidad adecuada basada en clasificadores de contenido. Sin la capacidad de descubrimiento de Governance, los controles de seguridad cubren solo una parte del patrimonio de datos.

### De Data Security a Risk & Compliance

Las etiquetas aplicadas por Information Protection permiten a DLP definir qué movimientos de datos deben ser monitorizados o bloqueados. Insider Risk Management puede detectar que un usuario intenta copiar múltiples archivos etiquetados como confidenciales a ubicaciones personales. Esa señal se convierte en evidencia para eDiscovery si la situación escala a una investigación, y en un evento de auditoría que el área de compliance puede presentar como prueba de control activo.

### De Risk & Compliance a Data Governance

Compliance Manager puede revelar una brecha, por ejemplo la incapacidad de demostrar que todos los datos personales de empleados se eliminan dentro del plazo legal. Esta brecha se traduce en una acción de mejora que exige actualizar las políticas de gestión del ciclo de vida de datos en el pilar de Governance. El requisito de cumplimiento se convierte en impulso para mejorar la gobernanza.

Este intercambio bidireccional de señales convierte a Purview en un sistema de gobernanza de datos que se refina con la operación y la interacción entre sus tres pilares.

---

# Nivel 3: Notas de soporte e insights de profundidad

## Nota 1: El error de implementación más común

Muchas organizaciones comienzan implementando componentes de Data Security, en particular DLP e Information Protection, sin haber construido primero la capa de Data Governance. Como consecuencia, aplican políticas de etiquetado y DLP sobre una fracción conocida del patrimonio de datos, normalmente la más visible, mientras el resto permanece sin descubrir ni catalogar y queda fuera de cobertura.

---

## Nota 2: Importancia del portal unificado en la cultura organizacional

Antes de la unificación en un portal común, la multiplicidad de portales era también un problema cultural. Los equipos de seguridad, cumplimiento e ingeniería de datos rara vez compartían herramientas, métricas o vocabulario. El portal unificado purview.microsoft.com actúa como un artefacto organizacional que facilita que disciplinas que históricamente trabajaban separadas compartan contexto, datos y prioridades.

---

## Nota 3: Relación con un modelo de madurez

La arquitectura de tres pilares se puede mapear sobre un esquema consultivo de madurez:

- **Nivel 1, fundamentos**: activación de los componentes básicos de cada pilar, visibles pero con poca integración entre ellos.  
- **Nivel 2, automatización**: los pilares empiezan a intercambiar señales, por ejemplo etiquetas de sensibilidad que alimentan DLP o eventos de auditoría que se usan en evaluaciones de cumplimiento.  
- **Nivel 3, optimización avanzada**: el ciclo virtuoso opera con alto grado de autonomía y capacidades como Adaptive Protection permiten ajustar controles de forma dinámica según el riesgo.

---

## Nota 4: Argumento financiero de la integración

Desde la perspectiva del CFO o del consejo, el valor de M365 E5 no se limita al precio de cada componente individual. Una organización que adquiere DLP, eDiscovery, UEBA e Information Governance de distintos proveedores soporta costos adicionales de integración, formación y mantenimiento. La arquitectura de tres pilares de Purview reduce esos costos de integración cuando se implementa como un sistema coherente, con un modelo operacional unificado y señales compartidas entre los componentes.
