---
layout: page
title: "MS_Purview_M365E5-SecOps_e03v01_Art02-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e03/ms-purview-m365e5-secops-e03v01-art02-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e03v01_Art02-MS_CSU_Security_MCSA

Este documento describe la arquitectura de seguridad y cumplimiento de Microsoft Purview Suite. Presenta sus dominios de protección, desde la clasificación y gobierno del dato hasta la prevención de fuga, el riesgo interno y la memoria forense. Explica capacidades de eDiscovery, auditoría avanzada, cumplimiento normativo, cifrado y control del cliente sobre sus claves y accesos. El objetivo es mostrar cómo estas piezas se conectan para reducir riesgo, cumplir regulaciones y reforzar la soberanía sobre la información corporativa. Está dirigido a equipos de seguridad, cumplimiento, TI y negocio que necesitan una visión integrada, práctica y accionable de Purview.

El primer dominio es Information Protection y Data Governance, fundamento del resto. Define qué es cada dato mediante etiquetas aplicadas por el usuario, reglas automáticas y modelos de aprendizaje automático. Usa también coincidencia exacta con bases reales y detección de entidades nombradas para identificar información sensible. Sobre esa clasificación se construye el gobierno del ciclo de vida. Incluye políticas de retención, eliminación, eventos disparadores, ámbitos adaptativos, registros regulatorios y evidencia formal de destrucción para auditorías.

El segundo dominio es Data Loss Prevention, que transforma la clasificación en acción concreta. Vigila correos, archivos en SharePoint y OneDrive, conversaciones en Teams y actividad en endpoints de Windows y macOS. Extiende la protección al navegador, al tráfico de red y a aplicaciones en la nube de terceros. Adaptive Protection ajusta la severidad de las políticas según el riesgo del usuario. Además, Shadow AI Protection bloquea envíos a herramientas de inteligencia artificial no autorizadas.

El tercer dominio combina Insider Risk Management y Communication Compliance para tratar el riesgo interno. Insider Risk detecta exfiltración, sabotaje, actividad inusual antes del offboarding y errores accidentales, usando señales de servicios en la nube, recursos humanos y dispositivos. El complemento Forensic Evidence registra actividad en pantalla cuando se requiere prueba granular. Ese nivel de riesgo alimenta Adaptive Protection y ajusta las políticas. Communication Compliance analiza mensajes para descubrir abuso, conflictos éticos o incumplimientos regulatorios.

El cuarto dominio agrupa eDiscovery Premium y Audit. eDiscovery permite preservar contenido bajo custodia legal, gestionar custodios y trabajar en conjuntos de revisión con threading e inteligencia artificial para grandes volúmenes. Además, ofrece exportación auditada de la evidencia. Microsoft 365 E3 incluye eDiscovery Standard, mientras Purview agrega estas funciones avanzadas. Audit extiende la retención de registros, incorpora eventos detallados y ofrece un complemento de diez años pensado para sectores fuertemente regulados. Especialmente banca y gobierno.

El quinto dominio reúne Compliance, Privacy, Encryption y Customer Control, enfocado en soberanía. Compliance Manager ofrece evaluaciones frente a marcos normativos conocidos, genera un puntaje de cumplimiento y prioriza acciones, integrado con el resto de Purview. Information Barriers separa grupos que no deben interactuar. Customer Key, Customer Lockbox y Privileged Access Management forman una tríada de control sobre claves, accesos y soporte. Todo esto se entrega bajo Microsoft Purview Suite, con algunos complementos opcionales adicionales.





# Dominios de protección de Microsoft Purview Suite

## 1. Information Protection y Data Governance: la base de todo

Dominio fundacional. Sin clasificación de datos, el resto de controles de Purview no puede operar. Actúa en dos capas complementarias.

### 1.1 Capa 1: Clasificación de datos (qué es el dato)

| Mecanismo                           | Descripción                                                  | Nivel      |
| ----------------------------------- | ------------------------------------------------------------ | ---------- |
| **Manual**                          | El usuario aplica la etiqueta de forma consciente            | Base       |
| **Automático por política**         | Reglas basadas en patrones y condiciones                     | Intermedio |
| **Clasificadores entrenables (ML)** | Modelos de aprendizaje automático entrenados con contenido propio | Avanzado   |
| **Exact Data Match (EDM)**          | Detección por coincidencia exacta contra una base de datos real | Avanzado   |
| **Named Entities**                  | Detección de entidades nombradas como personas, lugares u organismos | Avanzado   |
| **+300 SIT predefinidos**           | Tipos de información sensible listos para activar            | Inmediato  |

### 1.2 Capa 2: Gobierno del ciclo de vida (cuánto tiempo vive el dato)

| Capacidad                                    | Descripción                                                  |
| -------------------------------------------- | ------------------------------------------------------------ |
| **Políticas de retención y eliminación**     | Aplicadas en Exchange, SharePoint, OneDrive, Teams y Viva Engage |
| **Retención basada en eventos**              | Se activa ante un evento externo, por ejemplo la baja de un empleado |
| **Ámbitos adaptativos**                      | Políticas que se ajustan dinámicamente a atributos de usuarios o sitios |
| **Regulatory Records**                       | Inmutabilidad estricta para registros sujetos a regulación   |
| **Proof of Disposal**                        | Evidencia auditada de eliminación, crítico para auditorías formales |
| **Declaración automática de registros (ML)** | Declaración de registros mediante modelos de aprendizaje automático |


## 2. Data Loss Prevention (DLP): el brazo ejecutor

DLP convierte la clasificación en acción. Monitorea e impide el intercambio inapropiado de datos sensibles en siete superficies de aplicación.

### 2.1 Superficies de aplicación de DLP

| Superficie                           | Acción de control                                            | Disponibilidad                |
| ------------------------------------ | ------------------------------------------------------------ | ----------------------------- |
| **Exchange**                         | Análisis de correos entrantes y salientes                    | E3 + Purview Suite            |
| **SharePoint / OneDrive**            | Control de archivos en reposo y compartidos externamente     | E3 + Purview Suite            |
| **Teams**                            | Control de chats y mensajes de canal en tiempo real          | Purview Suite                 |
| **Endpoint DLP** (Win 10/11 y macOS) | Bloqueo de copia a USB, impresión y cargas no autorizadas    | Purview Suite                 |
| **Browser DLP**                      | Protección sobre aplicaciones no gestionadas desde Microsoft Edge | Purview Suite                 |
| **Network DLP**                      | Protección en línea a nivel de red                           | Purview Suite / pay-as-you-go |
| **Cloud Apps (MDCA)**                | Control de aplicaciones SaaS de terceros mediante Defender for Cloud Apps | Purview Suite                 |

### 2.2 Capacidades técnicas diferenciales de Purview Suite

* **Adaptive Protection**  
  DLP ajusta automáticamente la severidad de sus políticas según el nivel de riesgo del usuario calculado por Insider Risk Management. Usuarios con mayor riesgo reciben controles más estrictos sin intervención manual.

* **Shadow AI Protection**  
  Detecta y bloquea el envío de datos sensibles hacia herramientas de IA no autorizadas como servicios públicos de IA generativa.

* **Exact Data Match (EDM)**  
  Permite definir políticas de DLP apoyadas en registros reales de la organización, no solo en patrones genéricos.


## 3. Insider Risk y Communication Compliance: inteligencia interna

El riesgo más difícil de detectar suele provenir de dentro de la organización. Este dominio analiza comportamiento humano además de patrones de datos.

### 3.1 Insider Risk Management

Cobertura de escenarios típicos de riesgo interno:

| Escenario cubierto        | Señal detectada                                              |
| ------------------------- | ------------------------------------------------------------ |
| **Exfiltración de datos** | Descarga masiva correlacionada con envío externo             |
| **Sabotaje**              | Eliminación anómala de activos críticos                      |
| **Riesgo de offboarding** | Actividad inusual en los días previos a la baja del empleado |
| **Error accidental**      | Compartir información sensible de forma involuntaria         |

Insider Risk Management analiza señales de múltiples fuentes, entre ellas:

* Actividad en servicios de Microsoft 365  
* Señales de RR. HH., por ejemplo notificación de baja  
* Comportamiento en endpoints administrados  

**Add-on Forensic Evidence**  
Complemento opcional que captura actividad en pantalla para investigaciones formales que requieren evidencia más granular.

**Integración con Adaptive Protection**  
El nivel de riesgo calculado por Insider Risk Management alimenta directamente las políticas de DLP. Esto permite controles dinámicos basados en comportamiento y es una de las integraciones más avanzadas del portfolio.

### 3.2 Communication Compliance

Supervisa comunicaciones para detectar incumplimientos, abuso o riesgos regulatorios.

| Canal cubierto                   | Tipo de violación detectada                  |
| -------------------------------- | -------------------------------------------- |
| **Teams, Exchange, Viva Engage** | Acoso, lenguaje tóxico, conflicto de interés |
| **Fuentes externas integradas**  | Incumplimiento regulatorio en comunicaciones |

Incluye:

* Pseudonimización de usuarios durante la investigación  
* Segregación de funciones entre investigadores y administradores operativos  


## 4. eDiscovery y Audit: memoria forense

En incidentes legales, regulatorios o de seguridad, la organización necesita localizar evidencia y demostrar que no ha sido alterada.

### 4.1 eDiscovery Premium

Capacidades principales:

| Capacidad                | Descripción                                                  |
| ------------------------ | ------------------------------------------------------------ |
| **Legal Hold avanzado**  | Preservación de contenido con custodia documentada           |
| **Gestión de custodios** | Administración de personas bajo investigación                |
| **Review Sets**          | Entornos de revisión con threading e inteligencia artificial aplicada |
| **Exportación auditada** | Trazabilidad completa del proceso legal y de las exportaciones de evidencia |

Microsoft 365 E3 incluye eDiscovery Standard. Purview Suite agrega búsqueda avanzada, threading y análisis asistido por IA para grandes volúmenes de contenido.

### 4.2 Audit Premium

| Nivel                             | Retención | Eventos especiales incluidos                                 |
| --------------------------------- | --------- | ------------------------------------------------------------ |
| **Audit Standard** (E3)           | 90 días   | Actividad básica de usuarios y administradores               |
| **Audit Premium** (Purview Suite) | 1 año     | Eventos como MailItemsAccessed, SearchQueryInitiatedExchange y API de alto ancho de banda |
| **Add-on 10 años**                | 10 años   | Diseñado para sectores regulados como financiero, salud y gobierno |


## 5. Compliance, Privacy, Encryption y Customer Control: capa de soberanía

Dominio que consolida capacidades orientadas a responder una pregunta ejecutiva clave: quién controla realmente los datos alojados en la nube de Microsoft.

### 5.1 Compliance Manager

* Evaluaciones para marcos como **NIST CSF, ISO 27001, GDPR, HIPAA, SOC2, PCI-DSS** y marcos regionales o locales.  
* **Compliance Score**, una puntuación cuantitativa del estado de cumplimiento con acciones recomendadas y priorizadas.  
* Integración nativa con el resto del portfolio de Microsoft Purview para seguimiento centralizado.

### 5.2 Information Barriers

* Separación técnica y regulatoria entre grupos de usuarios en Teams, SharePoint y Exchange.  
* Caso de uso típico: bancos de inversión que requieren separación estricta entre equipos de research y de trading.

### 5.3 Customer Key

* Las claves de cifrado residen en **Azure Key Vault** propiedad del cliente.  
* El cliente puede revocar las claves en cualquier momento, lo que impide que Microsoft acceda al contenido sin esa clave.  
* Aplica a Exchange, SharePoint, OneDrive y Teams.

### 5.4 Customer Lockbox

* Todo acceso de ingenieros de Microsoft al contenido del cliente requiere aprobación explícita del cliente.  
* El proceso completo es auditable y puede revisarse para fines regulatorios o de compliance interno.

### 5.5 Privileged Access Management (PAM)

* Acceso Just-In-Time (JIT) y Just-Enough-Access (JEA) para administradores.  
* Se eliminan los accesos permanentes a datos sensibles. Ningún administrador mantiene derechos elevados de forma continua, solo por periodos acotados y aprobados.  


## 6. Notas de soporte e insights

* **Cobertura de Endpoint DLP**  
  Endpoint DLP cubre tanto Windows 10 y 11 como macOS, algo relevante en organizaciones con entornos mixtos que suelen asumir cobertura únicamente en Windows.

* **Valor estratégico de Adaptive Protection**  
  La integración entre Insider Risk Management y DLP mediante Adaptive Protection es una de las capacidades más sofisticadas y menos conocidas del portfolio. Permite que el sistema de riesgo interno ajuste automáticamente el nivel de restricción de las políticas de DLP según el comportamiento individual de cada usuario, sin intervención manual del equipo de seguridad.

* **Soberanía para sectores regulados**  
  La combinación de Customer Key, Customer Lockbox y Privileged Access Management forma una tríada de control alineada con requisitos de soberanía de datos de marcos regulatorios como BCRA, LGPD o GDPR. Esta tríada es especialmente relevante para banca, salud y gobierno.

* **Naming y licenciamiento**  
  Todos los dominios descritos se incluyen en **Microsoft Purview Suite**, nombre vigente desde octubre de 2025 (anteriormente M365 E5 Compliance). El complemento de retención de auditoría por 10 años y Forensic Evidence para Insider Risk Management requieren licenciamiento adicional.
