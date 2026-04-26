---
layout: page
title: "MS_Purview_M365E5-SecOps_e03v01_Art04-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e03/ms-purview-m365e5-secops-e03v01-art04-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e03v01_Art04-MS_CSU_Security_MCSA



Este documento describe cómo Microsoft Purview actúa como pilar de control dentro de una arquitectura Zero Trust moderna orientada a datos. Explica conceptos clave para CISOs y arquitectos de seguridad. Se apoya en tecnologías como Entra ID, Intune, Microsoft Sentinel y la familia Defender, integradas con Purview. Su finalidad es mostrar cómo clasificar, proteger, supervisar y gobernar la información crítica. También aborda soberanía del dato, controles administrativos privilegiados y capacidades forenses. Finalmente conecta estos controles con marcos regulatorios y el marco NIST de ciberseguridad. El texto ofrece ejemplos concretos, flujos técnicos y argumentos de negocio para conversaciones con reguladores.

El eje presenta los tres principios de Zero Trust y el rol específico de Purview. Verificar significa usar cada punto de datos disponible. Aquí las etiquetas de sensibilidad, las barreras de información y el registro de auditoría enriquecen el contexto de autorización. El principio de mínimo privilegio se materializa con acceso privilegiado justo a tiempo, claves controladas por el cliente y aprobaciones explícitas. Finalmente, asumir brecha implica segmentar datos, aplicar prevención y garantizar cifrado efectivo.

Bajo el modelo de capas, Purview es protagonista en el plano de datos y participa en infraestructura y visibilidad. Entra ID protege la identidad. Intune protege el dispositivo. Purview protege el contenido y regula lo que puede hacerse con él. Para un CISO, esto significa que, incluso si un atacante supera controles de identidad y dispositivo, encontrará datos clasificados, cifrados y vigilados por políticas y análisis de riesgo. Además, cualquier movimiento inusual generará alertas adicionales.

El tercer concepto desarrolla Adaptive Protection como integración avanzada entre Insider Risk Management y prevención de pérdida de datos. Un motor calcula un puntaje de riesgo por usuario según comportamientos inusuales, como descargas masivas. Ese puntaje ajusta automáticamente la severidad de las políticas. Un usuario estándar mantiene controles normales. Uno de riesgo elevado ve bloqueos de dispositivos extraíbles, impresiones y cargas web. Cuando el riesgo desciende, las restricciones se relajan sin intervención humana directa posterior.

El cuarto bloque explica cómo Purview contribuye a la soberanía del dato mediante capas. Primero, con Customer Key, el cliente administra sus claves de cifrado en Azure y puede revocarlas. Segundo, con Customer Lockbox, todo acceso de soporte de Microsoft requiere aprobación explícita y queda auditado. Tercero, con administración privilegiada, ningún operador interno tiene acceso permanente y todo permiso es temporal, acotado y justificado. Esta tríada responde exigencias de banca y salud reguladas en Latinoamérica.

El quinto eje describe la integración entre Purview y Microsoft Sentinel para lograr visibilidad forense transversal. Los registros de auditoría, las alertas de políticas de datos y los puntajes de riesgo alimentan el motor de correlación. Sentinel une estas señales con eventos de identidad, red y endpoint. Así se detectan patrones complejos y se responde con automatización. Este enfoque cierra el ciclo proteger, detectar y responder, alineado con el marco NIST de ciberseguridad hoy vigente.



# Purview como Pilar de Control en una Arquitectura Zero Trust

## Nivel 2: Conceptos clave

### Concepto 1: Los tres principios Zero Trust y el rol de Purview

**Explicación/Data**

El modelo Zero Trust de Microsoft se articula sobre tres principios fundacionales. Purview tiene contribuciones técnicas concretas y verificables en cada uno.

**PRINCIPIO 1: VERIFY EXPLICITLY**  
*"Siempre autenticar y autorizar basándose en todos los puntos de datos disponibles"*

| Contribución Purview                           | Mecanismo técnico                                            |
| ---------------------------------------------- | ------------------------------------------------------------ |
| **Clasificación del dato como punto de datos** | Las etiquetas de sensibilidad enriquecen el contexto de autorización. Un dato clasificado como "Confidencial" puede requerir MFA adicional o dispositivo gestionado para ser accedido |
| **Information Barriers**                       | Verificación de que el acceso solicitado no viola separaciones regulatorias (ejemplo: research vs. trading en servicios financieros) |
| **Audit Premium**                              | Registro forense de cada acceso. El "verify" se vuelve auditable y retroactivo |
| **Compliance Manager**                         | Verifica el estado de cumplimiento del entorno como condición de confianza |

**PRINCIPIO 2: USE LEAST PRIVILEGE**  
*"Limitar el acceso de usuarios con acceso Just-In-Time y Just-Enough-Access"*

| Contribución Purview                   | Mecanismo técnico                                            |
| -------------------------------------- | ------------------------------------------------------------ |
| **Privileged Access Management (PAM)** | JIT/JEA para administradores de datos sensibles. Ningún acceso permanente a datos críticos |
| **Customer Key**                       | El cliente controla las claves de cifrado. Microsoft no puede acceder sin autorización explícita |
| **Customer Lockbox**                   | Aprobación explícita requerida para cualquier acceso de ingeniería Microsoft |
| **Information Barriers**               | Restricción técnica de acceso entre segmentos organizacionales |
| **Adaptive Protection**                | Eleva automáticamente las restricciones para usuarios con perfil de riesgo elevado |

**PRINCIPIO 3: ASSUME BREACH**  
*"Minimizar el radio de explosión, segmentar acceso, verificar cifrado end to end"*

| Contribución Purview            | Mecanismo técnico                                            |
| ------------------------------- | ------------------------------------------------------------ |
| **DLP (todas las superficies)** | Contiene la exfiltración post brecha. Si un atacante obtiene acceso, DLP limita lo que puede extraer |
| **Endpoint DLP**                | Bloquea rutas de exfiltración físicas (USB, impresión, uploads) incluso en dispositivos comprometidos |
| **Insider Risk Management**     | Detecta comportamiento anómalo interno. El tipo de brecha más difícil de detectar es la interna |
| **eDiscovery Premium**          | Capacidad forense post incidente. Reconstrucción del vector de ataque y del alcance de la brecha |
| **Encryption (Customer Key)**   | Datos cifrados con claves del cliente. Incluso si el almacenamiento es comprometido, los datos son ilegibles |
| **Communication Compliance**    | Detecta canales de exfiltración mediante comunicación (ejemplo: envío de datos sensibles por Teams a externos) |

---

### Concepto 2: El modelo de capas, dónde vive Purview en Zero Trust

**Explicación/Data**

El modelo Zero Trust de Microsoft organiza la arquitectura en seis planos de protección. Purview opera principalmente en tres de ellos, con presencia transversal:

```text
┌─────────────────────────────────────────────────────────┐
│              MODELO ZERO TRUST - 6 PLANOS              │
├─────────────────────────────────────────────────────────┤
│  IDENTIDADES     → Entra ID / MFA / SSPR               │
│  DISPOSITIVOS    → Intune / Defender for Endpoint      │
│  REDES           → Azure Firewall / Defender for Network│
│  APLICACIONES    → Defender for Cloud Apps / MCAS      │
│ ▶ DATOS          → PURVIEW (núcleo de este plano)      │
│ ▶ INFRAESTRUCTURA→ Defender for Cloud / Purview Data Map│
├─────────────────────────────────────────────────────────┤
│  VISIBILIDAD Y   → Microsoft Sentinel / Purview Audit  │
│  AUTOMATIZACIÓN     (transversal a todos los planos)   │
└─────────────────────────────────────────────────────────┘
```

**El plano de DATOS es el único donde Purview es el actor principal:**

- Entra ID protege *quién* accede  
- Intune protege *desde dónde* se accede  
- **Purview protege *qué* se accede y *qué* puede hacerse con ello***

Para un CISO, la distinción crítica es:

Un atacante que compromete credenciales válidas (bypasa Entra ID) y usa un dispositivo gestionado (bypasa Intune) aún enfrenta Purview como última línea de defensa: el dato está cifrado, clasificado, y cualquier movimiento anómalo activa Insider Risk Management y DLP.

---

### Concepto 3: Adaptive Protection, la integración Zero Trust más sofisticada

**Explicación/Data**

Adaptive Protection ejemplifica la convergencia entre Zero Trust y Purview porque conecta dos dominios que históricamente operaban en silos:

```text
    INSIDER RISK MANAGEMENT          DATA LOSS PREVENTION
    (Detecta el riesgo)          →   (Ejecuta el control)
            ↓                               ↓
      Score de riesgo              Política adaptada al score
      por usuario                  en tiempo real, sin intervención
            ↓                               ↓
      BAJO RIESGO                   Políticas DLP estándar
      RIESGO ELEVADO                Restricciones adicionales automáticas
      RIESGO CRÍTICO                Bloqueo total de canales de salida
```

**Flujo técnico concreto**

1. Insider Risk Management detecta que un usuario descargó 500 archivos en las últimas 2 horas (señal de exfiltración).  
2. Insider Risk Management eleva el score de riesgo del usuario a "Elevado".  
3. Adaptive Protection notifica automáticamente a DLP.  
4. DLP aplica una política más restrictiva sobre ese usuario específico (bloqueo de USB, uploads, impresión).  
5. El usuario recibe una notificación de política, sin que ningún administrador haya intervenido.  
6. Cuando el score de riesgo baja, las restricciones se relajan automáticamente.

**Por qué esto es Zero Trust en acción**

Adaptive Protection implementa *Use Least Privilege* de forma dinámica y contextual. El privilegio de mover datos no es fijo, se ajusta en tiempo real al nivel de riesgo del usuario, lo que representa una implementación madura del principio de Least Privilege.

---

### Concepto 4: Purview y la soberanía del dato, Customer Key + Customer Lockbox + PAM

**Explicación/Data**

En arquitecturas Zero Trust para sectores regulados, la pregunta clave del CISO es:

> *"¿Quién controla realmente mis datos en la nube de Microsoft?"*

Purview Suite responde con tres capas de control soberano.

#### Capa 1: Customer Key (cifrado)

- El cliente provee sus propias claves de cifrado desde Azure Key Vault.  
- Microsoft opera el servicio pero **no puede leer el contenido** sin las claves del cliente.  
- El cliente puede **revocar las claves** en cualquier momento (Data Purge, "BYOK nuclear option").  
- Aplica a: Exchange Online, SharePoint, OneDrive, Teams.

#### Capa 2: Customer Lockbox (acceso humano)

- Si ingeniería de Microsoft necesita acceder a datos del cliente para soporte, debe solicitar aprobación.  
- El cliente recibe una solicitud con identidad del ingeniero, motivo, alcance y duración.  
- El cliente aprueba o rechaza. Microsoft **no puede proceder sin aprobación**.  
- Cada acceso queda registrado en Audit Premium.

#### Capa 3: PAM (acceso administrativo interno)

- Ningún administrador interno del cliente tiene acceso permanente a datos sensibles.  
- Cada acceso requiere aprobación Just In Time con justificación documentada.  
- El alcance se limita al mínimo necesario (Just Enough Access).  
- La ventana de tiempo es acotada y auditable.

**Tríada de soberanía para audiencias reguladas**

```text
Customer Key      = Nadie lee sin mis claves
Customer Lockbox  = Nadie de Microsoft accede sin mi aprobación
PAM               = Nadie interno accede sin flujo de aprobación
```

---

### Concepto 5: Purview + Sentinel, visibilidad forense transversal

**Explicación/Data**

Zero Trust requiere no solo controles, sino visibilidad total para detectar lo que los controles no bloquearon. La integración Purview + Microsoft Sentinel cierra este ciclo.

| Señal Purview                           | Consumida por Sentinel | Capacidad resultante                                         |
| --------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| **Audit Premium logs**                  | Sentinel SIEM          | Correlación de actividad de usuarios con alertas de seguridad |
| **DLP alerts**                          | Sentinel incidents     | Investigación unificada de intentos de exfiltración          |
| **Insider Risk Management risk scores** | Sentinel watchlists    | Monitoreo proactivo de usuarios de alto riesgo               |
| **Communication Compliance flags**      | Sentinel analytics     | Detección de patrones de comunicación sospechosa             |
| **Information Barriers violations**     | Sentinel alerts        | Detección de accesos que violan separación regulatoria       |

**Loop de inteligencia**

```text
    Purview detecta → Sentinel correlaciona → Defender responde → Purview registra
         ↑                                                              ↓
         └──────────────── Loop de mejora continua ────────────────────┘
```

**Relevancia en Zero Trust**

*"Assume Breach"* no significa solo prepararse para una brecha, sino tener la capacidad de detectarla, contenerla y reconstruirla con evidencia forense. Purview Audit Premium combinado con Sentinel proporciona exactamente esa capacidad.

---

## Nivel 3: Notas de soporte e insights

🏦 **Insight regulatorio para Latinoamérica**  
La tríada Customer Key + Customer Lockbox + PAM es un argumento técnico eficaz para responder objeciones de soberanía de datos en conversaciones con CISOs de banca (BCRA Argentina, CMF Chile, CNBV México) y salud. Estos reguladores preguntan específicamente *"¿quién tiene acceso a mis datos?"*. La respuesta técnica de Purview es directa y auditable.

⚡ **Insight de posicionamiento**  
La pregunta útil para abrir una conversación Zero Trust con un CISO no es *"¿tienen Zero Trust?"* sino *"¿su arquitectura Zero Trust protege el dato o solo el acceso?"*. La mayoría de las implementaciones Zero Trust en el mercado se detienen en Identidad y Dispositivo. Purview completa el modelo en el plano de Datos.

🔗 **Conexión con NIST CSF 2.0**  
Los controles de Purview descritos en este bloque se mapean directamente a funciones del NIST CSF:  
- **PROTECT (PR.DS, Data Security)**  
- **DETECT (DE.CM, Continuous Monitoring)**  
- **RESPOND (RS.AN, Incident Analysis)**  

Para audiencias que usan NIST CSF como framework de referencia, esta correlación agrega claridad y valor.

📊 **Dato técnico de profundidad**  
Adaptive Protection requiere que Insider Risk Management y DLP estén configurados y activos. Es una capacidad de integración, no un módulo independiente. Su activación implica madurez previa en ambos dominios. Es un indicador de madurez avanzada en la implementación de Purview.
