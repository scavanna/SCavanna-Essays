---
layout: page
title: "MS_Purview_M365E5-SecOps_e03v01_Art03-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e03/ms-purview-m365e5-secops-e03v01-art03-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e03v01_Art03-MS_CSU_Security_MCSA



Este documento describe un mapa de decisión para comparar Microsoft 365 E3 y Microsoft Purview Suite. Explica cómo cambia el modelo operativo de seguridad y cumplimiento cuando se pasa de controles manuales a controles automatizados con inteligencia integrada. Detalla el cambio de nombre de las licencias E5 Compliance hacia la marca Purview. Presenta el rol de los add-ons sobre una base E3, una matriz de madurez por superficie de riesgo y el argumento financiero de consolidación. Está dirigido a CISOs, CFOs y equipos de arquitectura de seguridad.

La primera idea explica la regla de oro del licenciamiento. Microsoft 365 E3 ofrece herramientas donde la clasificación, las decisiones y la ejecución son principalmente manuales. Purview Suite, en cambio, introduce automatización a gran escala. El sistema clasifica, decide y ejecuta con apoyo de modelos de machine learning, tipos de datos estructurados y entidades reconocidas. Siempre existe supervisión humana, pero la operación deja de depender del usuario final. Así se habilita gobernanza consistente y controles sostenibles en entornos grandes y cambiantes.

La segunda idea aclara el cambio de nombre de octubre de 2025. Antes existían varios componentes bajo la marca E5 Compliance, como Information Protection, Governance, eDiscovery y Audit. Desde entonces, todo se agrupa bajo el nombre Microsoft Purview Suite. Las capacidades, el licenciamiento por usuario, los portales y los precios de referencia se mantienen. Lo que cambia es el nombre comercial y la alineación con la marca Purview. En cualquier conversación actual se recomienda hablar solo de Purview Suite para evitar confusiones.

La tercera idea se centra en los add-ons para organizaciones con base E3. Explica que existen complementos específicos para información y gobierno, eDiscovery y auditoría, riesgo interno, comunicación, retención de auditoría a diez años y evidencia forense. Estos módulos permiten una adopción gradual según los riesgos prioritarios. El documento propone una regla práctica. Cuando una organización necesita tres o más add-ons, el costo resultante suele justificar pasar directamente a Purview Suite completo, con mejor integración y operación más simple.

La cuarta idea describe la matriz de madurez por superficie de riesgo. Compara la cobertura de E3 frente a Purview Suite en correos, documentos en reposo, dispositivos, Teams, aplicaciones de terceros y comportamiento de usuarios internos. E3 ofrece controles básicos de DLP, retención y auditoría. Purview agrega clasificación automática, DLP en endpoint, análisis de riesgo interno, supervisión de comunicaciones y capacidades forenses avanzadas. También incluye administración privilegiada con acceso justo a tiempo y control de claves de cifrado mediante Customer Key, además de protección frente a uso de inteligencia artificial no autorizada.

La quinta idea desarrolla el argumento financiero y de consolidación. Purview Suite puede reemplazar múltiples soluciones de terceros en áreas como riesgo interno, DLP de endpoint, comunicación supervisada, eDiscovery y auditoría avanzada. El texto sugiere conversaciones con CFOs basadas en el gasto actual en estas herramientas, más la complejidad de integrarlas, en lugar de discutir solo el precio del bundle. Además, resalta el valor de Customer Key y Customer Lockbox para soberanía de datos en sectores regulados. El análisis de TCO siempre debe hacerse con datos reales del cliente.









# El mapa de decisión: licenciamiento E3 vs Purview Suite

## Nivel 2: Conceptos clave

### 1. La regla de oro del licenciamiento

La diferencia entre Microsoft 365 E3 y Microsoft Purview Suite no es solo de cantidad de herramientas, sino de modelo operativo:

- **M365 E3 = herramientas de control manual**  
  - El humano clasifica  
  - El humano decide  
  - El humano ejecuta

- **Purview Suite = inteligencia y automatización a gran escala**  
  - El sistema clasifica  
  - El sistema decide  
  - El sistema ejecuta  
  - Siempre con supervisión humana, pero sin dependencia constante del operador

El cambio real no es de features; es de modelo operativo:

| Dimensión                | M365 E3                          | Purview Suite                                           |
| ------------------------ | -------------------------------- | ------------------------------------------------------- |
| **Clasificación**        | Manual por el usuario            | Automática por ML + EDM + Named Entities                |
| **DLP**                  | Exchange + SharePoint + OneDrive | + Teams + Endpoint + Browser + Network + Cloud Apps     |
| **Retención**            | Políticas básicas                | + Ámbitos Adaptativos + Retención por Eventos           |
| **Registros**            | Gestión básica                   | + Regulatory Records + Proof of Disposal                |
| **Auditoría**            | 90 días                          | 1 año + eventos especiales + API de alto ancho de banda |
| **eDiscovery**           | Standard                         | Premium + threading + análisis con IA                   |
| **Riesgo interno**       | No incluido                      | Insider Risk Management completo                        |
| **Cifrado avanzado**     | Cifrado básico de mensajes       | + Customer Key + Customer Lockbox                       |
| **Control privilegiado** | No incluido                      | PAM (Just-In-Time)                                      |
| **Compliance scoring**   | No incluido                      | Compliance Manager con evaluaciones                     |
| **Adaptive Protection**  | No incluido                      | IRM integrado con DLP                                   |
| **Shadow AI Protection** | No incluido                      | Detección de IA no autorizada                           |

Este cambio de modelo operativo explica por qué Purview Suite permite operar controles de seguridad y cumplimiento a escala, mientras que con E3 la organización depende mucho más de acciones manuales.

---

### 2. Anatomía del cambio de nombre (octubre 2025)

En octubre de 2025 se produjo un cambio de nombre que genera dudas frecuentes entre CISOs y CFOs. La equivalencia es:

- **Antes (hasta septiembre 2025)**  
  - Microsoft 365 E5 Compliance  
  - E5 Information Protection & Governance  
  - E5 eDiscovery & Audit  

- **Ahora (desde octubre 2025)**  
  - Todo lo anterior se consolida en **Microsoft Purview Suite**

#### Lo que no cambió

- Las capacidades técnicas son idénticas  
- El modelo de licenciamiento por usuario es idéntico  
- Los portales de administración son los mismos  
- Los precios de referencia son equivalentes

#### Lo que sí cambió

- El nombre comercial del bundle  
- La alineación con la marca Purview como paraguas unificado  
- La señal estratégica de consolidación de portafolio por parte de Microsoft  

En cualquier conversación posterior a octubre de 2025 se debe usar el nombre **Microsoft Purview Suite** para evitar confusiones con el nombre histórico M365 E5 Compliance.

---

### 3. Los add-ons: cirugía de precisión para usuarios E3

No todas las organizaciones necesitan o pueden adquirir Purview Suite completo. Sobre una base M365 E3 existen add-ons que incorporan capacidades específicas:

| Add-on                                     | Capacidades incluidas                                        | Caso de uso típico                                           |
| ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Microsoft Purview Suite (add-on E3)**    | Todas las capacidades E5 Compliance / Purview Suite          | Organizaciones que requieren el portafolio completo sin migrar a E5 |
| **E5 Information Protection & Governance** | Auto-labeling con ML, Ámbitos Adaptativos, records avanzados, marcas de agua dinámicas | Organizaciones con alto volumen de datos no estructurados y necesidad de clasificación automática |
| **E5 eDiscovery & Audit**                  | eDiscovery Premium, Audit Premium (1 año), MailItemsAccessed | Organizaciones con litigios frecuentes o requisitos de retención regulada |
| **E5 Insider Risk Management**             | IRM completo + Adaptive Protection                           | Organizaciones con alta exposición a riesgo interno (offboarding, fusiones, sectores regulados) |
| **E5 Communication Compliance**            | Supervisión de comunicaciones + modelos de detección con ML  | Servicios financieros, salud, gobierno u otros con obligaciones de monitoreo |
| **Audit (Premium) 10-year**                | Retención de 10 años en logs de auditoría                    | Sectores con requisitos regulatorios de retención extendida de registros de actividad |
| **Forensic Evidence**                      | Capturas de pantalla en investigaciones de IRM               | Investigaciones formales de riesgo interno donde se requiere evidencia visual |

**Regla de decisión de licenciamiento:**  
Si la organización necesita **tres o más add-ons**, el costo total tiende a justificar la adquisición de **Purview Suite completo**, aprovechando la consolidación funcional y la integración nativa.

---

### 4. Matriz de madurez por superficie de riesgo

La decisión de licenciamiento también define el nivel de cobertura de riesgo sobre distintas superficies:

| Superficie de riesgo                           | Cobertura E3                     | Cobertura Purview Suite                                 |
| ---------------------------------------------- | -------------------------------- | ------------------------------------------------------- |
| **Correo electrónico (Exchange)**              | DLP básico + cifrado básico      | DLP avanzado + Customer Key + Audit Premium             |
| **Documentos en reposo (SharePoint/OneDrive)** | Etiquetado manual + DLP básico   | Auto-labeling + etiquetado por defecto + marcas de agua |
| **Dispositivos de usuario final (Endpoint)**   | Sin cobertura DLP                | Endpoint DLP (Windows y macOS)                          |
| **Colaboración en tiempo real (Teams)**        | Sin DLP en Teams                 | DLP en Teams + etiquetado de reuniones                  |
| **Aplicaciones SaaS de terceros**              | Sin control inline               | MDCA integrado + Browser DLP                            |
| **Comportamiento de usuarios internos**        | Sin análisis de riesgo           | Insider Risk Management                                 |
| **Comunicaciones internas**                    | Sin supervisión                  | Communication Compliance                                |
| **Evidencia legal y forense**                  | eDiscovery básico (90 días)      | eDiscovery Premium (1 año o más)                        |
| **Administración privilegiada**                | Sin JIT/JEA                      | PAM completo                                            |
| **Control de claves de cifrado**               | Claves gestionadas por Microsoft | Customer Key (claves gestionadas por el cliente)        |
| **IA no autorizada (Shadow AI)**               | Sin detección                    | Shadow AI Protection                                    |

Esta matriz permite visualizar qué partes del mapa de riesgo permanecen expuestas con E3 y cuáles se cubren al incorporar Purview Suite.

---

### 5. El argumento financiero: TCO y consolidación

El impacto más relevante para un CFO suele ser financiero. Purview Suite consolida capacidades que muchas organizaciones cubren hoy con múltiples herramientas de terceros:

| Capacidad Purview Suite      | Alternativa de mercado típica              |
| ---------------------------- | ------------------------------------------ |
| **Insider Risk Management**  | Varonis, Securonix, Exabeam (IRM)          |
| **DLP Endpoint**             | Forcepoint, Digital Guardian, Symantec DLP |
| **Communication Compliance** | Smarsh, Global Relay, NICE Actimize        |
| **eDiscovery Premium**       | Relativity, Nuix, Exterro                  |
| **Audit Premium**            | Splunk, Sumo Logic (para logs de M365)     |
| **Customer Key / Lockbox**   | Thales, Entrust (HSM y KMS externos)       |

En una organización que ya tiene M365 E3 y contrata soluciones de terceros para IRM, DLP Endpoint y eDiscovery, el gasto combinado suele ser superior al costo incremental de Purview Suite, sin contar la complejidad de integración y operación de un entorno multivendor.

**Nota de cautela:**  
El análisis de TCO debe realizarse con datos específicos del cliente. El ahorro real depende del tamaño de la organización, los contratos vigentes y el grado de uso efectivo de las capacidades nativas de Microsoft 365.

---

## Nivel 3: Notas de soporte e insights

- **Dato crítico para audiencias ejecutivas:**  
  El add-on de **10 años para Audit** suele ser el primer punto de conversación con CISOs de banca y salud en Latinoamérica, donde marcos como BCRA y regulaciones de datos personales exigen retención extendida de registros de actividad.

- **Enfoque de conversación financiera con CFOs:**  
  Resulta más efectivo mostrar cuánto paga hoy la organización por herramientas de IRM, DLP, eDiscovery, auditoría y cifrado que Purview Suite puede reemplazar, en lugar de enfocarse únicamente en el precio de Purview Suite. Este cambio de enfoque facilita discutir consolidación y optimización de costos.

- **Soberanía y regulaciones locales:**  
  **Customer Key** y **Customer Lockbox** son argumentos clave para responder objeciones de soberanía de datos en sectores regulados en países como Argentina (BCRA), Brasil (LGPD), Colombia (Ley 1581) y México (LFPDPPP). Es importante destacarlos explícitamente cuando la organización tiene requisitos estrictos de control sobre claves y acceso a datos.

- **Uso consistente del nombre del producto:**  
  En presentaciones y documentación posteriores a octubre de 2025 debe utilizarse únicamente la denominación **Microsoft Purview Suite**. Referirse al bundle como **M365 E5 Compliance** puede percibirse como desactualizado y generar dudas en la interlocución con clientes.
