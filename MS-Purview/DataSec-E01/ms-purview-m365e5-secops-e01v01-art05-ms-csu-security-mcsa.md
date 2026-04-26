---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art06-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art05-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art06-MS_CSU_Security_MCSA

Este bloque describe un modelo avanzado de prevención de fuga de datos en Microsoft Purview para Microsoft 365, con foco en licenciamiento E5 y E5 Compliance. Explica cómo un motor central de políticas gobierna múltiples superficies técnicas, como correo, colaboración, dispositivos, navegador, red y aplicaciones en la nube. El documento orienta a equipos de seguridad y cumplimiento que necesitan diseñar, calibrar y operar políticas de DLP modernas, integradas con clasificación de información, gestión de riesgo interno y controles adaptativos para inteligencia artificial. También resume dependencias arquitectónicas, buenas prácticas de despliegue progresivo y consideraciones sectoriales para organizaciones latinoamericanas fuertemente reguladas.

El modelo operativo parte de reconocer que los datos sensibles ya no salen solo por correo. Ahora fluyen por chats, sitios colaborativos, dispositivos, navegadores, red y aplicaciones en la nube. Purview ofrece un motor de políticas central que publica reglas coherentes en todas esas superficies. Cada intento de mover información se compara con esas reglas y se decide permitir, registrar, notificar, pedir justificación o bloquear. Así se equilibra protección, trazabilidad y continuidad del negocio.

El motor de políticas se gestiona desde el portal de Purview con un modelo unificado. Cada política define condiciones basadas en tipos de información, etiquetas de sensibilidad, destinatarios y contexto de la acción. También fija acciones, desde auditar hasta cifrar o bloquear sin excepción. Las notificaciones al usuario, como los avisos de política, explican por qué una acción se limita y orientan alternativas seguras. La implantación pasa por fases de auditoría, ajuste y bloqueo gradual.

Las superficies de control cubren correo, almacenamiento en SharePoint y OneDrive, mensajería en Teams, actividades del sistema operativo, uso del navegador, puntos de salida de red y aplicaciones en la nube de terceros. En conjunto detienen fugas clásicas, como adjuntos enviados fuera, y vectores nuevos. Entre ellos destaca el uso de inteligencia artificial. Browser DLP y Defender para aplicaciones cloud detectan pegados o subidas a servicios no aprobados y pueden bloquearlos o redirigirlos hacia Copilot.

Adaptive Protection conecta la evaluación de riesgo de Insider Risk Management con la ejecución de DLP. El sistema asigna niveles de riesgo a cada usuario y, según ese nivel, endurece o relaja las acciones de protección. Un mismo intento de copiar datos puede quedar auditado en usuarios de riesgo bajo y bloqueado en perfiles críticos. Esta señal se combina con etiquetas de sensibilidad, registros unificados, eDiscovery y soluciones SIEM para dar contexto a las investigaciones.

Por último, el texto aborda cómo parametrizar las políticas según sector, región y licenciamiento. Distingue capacidades presentes en planes E3, centradas en correo y almacenamiento, de las superficies avanzadas exclusivas de E5, como Teams, Endpoint, navegador, red y aplicaciones cloud. Explica que todas comparten el mismo motor de Purview y las mismas etiquetas de sensibilidad. Sobre esa base se ajustan umbrales, excepciones y controles para banca, salud u otras industrias latinoamericanas con fuertes exigencias regulatorias.





**Nota de Verificación Técnica:**  
Este documento ha sido auditado contra documentación oficial de Microsoft.  

- ✅ Claims verificados incluyen referencias inline a Microsoft Learn/Docs  
- ⚠️ Contenido marcado como "análisis consultivo" no constituye estándar oficial Microsoft  
- 📊 Métricas, scores o niveles sin referencia explícita han sido eliminados o reformulados  

# Bloque 05 · DLP avanzado en Microsoft Purview (M365 E5 / E5 Compliance)

## 1. Modelo operativo del DLP moderno en M365

El DLP clásico en Microsoft 365 se centraba casi exclusivamente en correo electrónico, inspeccionando mensajes salientes de Exchange, detectando información sensible y aplicando acciones configuradas como bloqueo, notificación o requerimiento de justificación.  
[Referencia: Data Loss Prevention Overview – Microsoft Learn]

En una organización con M365 E5, los usuarios pueden sacar información sensible por diversos canales:

- Correo electrónico corporativo hacia dominios externos  
- Chat y canales en Microsoft Teams, incluidos invitados y federación  
- Copia a dispositivos USB y otros medios extraíbles  
- Impresión física de documentos  
- Captura de pantalla y herramientas de recorte  
- Carga de archivos a servicios personales en la web  
- Sincronización con almacenamiento personal  
- Uso de aplicaciones SaaS no autorizadas  
- Transferencia por Bluetooth u otros canales locales  
- Acceso desde aplicaciones no gestionadas en dispositivos personales

Un DLP que solo inspecciona correo electrónico cubre un subconjunto reducido de estos vectores. Microsoft Purview DLP en M365 E5 responde con una arquitectura de siete superficies de control coordinadas. Todas consumen políticas definidas en un motor central en el portal de Purview, pero cada superficie aplica acciones compatibles con su contexto técnico y sus capacidades específicas.  
[Referencia: Locations for DLP policies – Microsoft Learn]

La función de DLP en este modelo es evaluar cada intento de mover datos sensibles contra políticas centralizadas y decidir si permite, registra, notifica, requiere justificación o bloquea la acción.

## 2. Motor central de políticas DLP en Purview

### 2.1 Modelo de política unificada

El motor de políticas de DLP se gestiona desde `purview.microsoft.com` y utiliza un modelo de definición única de políticas aplicables a múltiples ubicaciones. Al crear una política, el administrador selecciona:

- Ubicaciones de aplicación: Exchange Online, SharePoint, OneDrive, Microsoft Teams, dispositivos con Endpoint DLP, navegadores soportados, Network DLP y Cloud Apps a través de Defender for Cloud Apps  
- Condiciones y acciones específicas por ubicación, respetando las diferencias funcionales de cada superficie  

La interfaz es unificada, pero el plano de ejecución es distribuido. No todas las acciones están disponibles en cada superficie y la granularidad de las condiciones varía según el tipo de carga de trabajo.  
[Referencia: Data Loss Prevention policies – Locations – Microsoft Learn]

### 2.2 Anatomía de una política DLP

Una política DLP en Purview se estructura en componentes:

- **Condiciones**  
  - Tipos de información sensible (Sensitive Information Types, SITs)  
  - Etiquetas de sensibilidad aplicadas al contenido (Microsoft Purview Information Protection)  
  - Destinatarios o destinos (por ejemplo, dominios externos específicos)  
  - Contexto de la acción (envío de correo, compartición de archivo, copia a USB, carga web, etc.)

- **Acciones**  
  - Permitir y auditar  
  - Notificar al usuario sin bloqueo  
  - Bloquear con posibilidad de override mediante justificación registrada  
  - Bloquear sin posibilidad de override  
  - Cifrar el contenido (por ejemplo, cifrado de mensajes de Microsoft o protección RMS)  
  - Notificar a administradores o equipos de seguridad a través de alertas  

- **Notificaciones, auditoría y evidencias**  
  - Registro de eventos en el Unified Audit Log de Microsoft 365  
  - Generación de alertas consultables en el portal de Purview  
  - Integración con eDiscovery (eDiscovery Premium) para que los eventos de DLP se utilicen como evidencia en investigaciones, incluyendo quién intentó mover qué contenido, hacia qué destino y qué acción tomó DLP  
  [Referencia: Unified Audit Log and DLP; eDiscovery integration – Microsoft Learn]

### 2.3 Notificaciones al usuario como control y formación

Las notificaciones al usuario (Policy Tips y mensajes de bloqueo o advertencia) son configurables dentro de las políticas DLP:

- Se pueden personalizar textos, idiomas y enlaces a documentación interna  
- Pueden explicar por qué la acción se ha bloqueado o marcado y qué alternativas están permitidas  
- Funcionan como microformación contextual, reforzando las políticas de clasificación y uso de datos sin depender solo de campañas de concienciación generales  

[Referencia: DLP policy tips – Microsoft Learn]

Esta dimensión educativa permite reducir el uso de bloqueos absolutos y disminuye incidentes repetidos por desconocimiento de la política.

### 2.4 Calibración operativa y gestión de falsos positivos

Las acciones DLP mal calibradas generan fricción operativa:

- Umbrales de detección demasiado sensibles incrementan falsos positivos  
- Bloqueos agresivos en fases iniciales interrumpen procesos legítimos y deterioran la percepción del sistema

Práctica recomendada:

1. **Fase de auditoría**  
   - Configurar políticas en modo de auditoría (permitir y registrar) sin bloquear  
   - Analizar volúmenes de eventos, tipos de datos detectados y flujos de trabajo afectados  

2. **Ajuste iterativo**  
   - Refinar condiciones (por ejemplo, tipos de datos, número de incidencias en un mismo documento, dominios de excepción)  
   - Definir acciones diferenciadas por sensibilidad del dato y perfil de usuario  

3. **Introducción gradual de bloqueos**  
   - Empezar con bloqueo condicionado a justificación y registro  
   - Escalar a bloqueos absolutos solo en información de alta sensibilidad o en combinaciones de alto riesgo  

Este enfoque reduce el costo humano y organizativo de la implantación de DLP, en especial en superficies intrusivas como Endpoint DLP.

## 3. Superficies de control DLP en Microsoft Purview

### 3.1 Exchange Online (M365 E3 y E5)

- DLP en Exchange Online analiza principalmente correos salientes en tiempo real  
- Inspecciona cuerpo del mensaje y adjuntos para detectar tipos de información sensible y etiquetas de sensibilidad  
- Puede aplicar acciones como:  
  - Bloquear el envío  
  - Requerir justificación de negocio para permitir el envío  
  - Notificar al remitente y a administradores  
  - Cifrar el mensaje automáticamente  
  - Enviar copia a buzones de supervisión o a flujos de revisión  

Caso típico: adjuntar un archivo etiquetado como "Altamente confidencial" a un correo dirigido a un dominio externo. La política puede bloquear el envío o exigir justificación, incluso si el texto del mensaje no contiene patrones sensibles.  
[Referencia: Data Loss Prevention policies in Exchange Online – Microsoft Learn]

### 3.2 SharePoint Online y OneDrive for Business (M365 E3 y E5)

- DLP opera sobre archivos en reposo y sobre acciones de compartición  
- Escanea el contenido de archivos almacenados en bibliotecas de SharePoint y OneDrive  
- Evalúa la sensibilidad del contenido y la configuración de compartición cuando:  
  - Se comparte un archivo con usuarios externos  
  - Se genera un enlace anónimo o de amplio alcance  

Acciones habituales:

- Bloquear el uso compartido externo si el contenido supera umbrales de información sensible  
- Revertir o restringir enlaces de compartición existentes  
- Notificar al propietario del sitio, al propietario del documento y al equipo de seguridad  

[Referencia: DLP for SharePoint and OneDrive – Microsoft Learn]

### 3.3 Microsoft Teams (superficie exclusiva M365 E5)

- DLP analiza mensajes en chats 1:1, chats de grupo y canales de Teams, incluyendo conversaciones con invitados y usuarios externos de otras organizaciones  
- La inspección se realiza en tiempo casi real antes de que el mensaje se entregue al destinatario  

Acciones típicas:

- Bloquear el envío del mensaje que contiene datos sensibles hacia destinatarios que infringen la política (por ejemplo, usuarios externos o ciertos grupos)  
- Informar al usuario mediante un mensaje de política indicando el motivo  
- Generar alertas para el equipo de seguridad  

Esta superficie cubre el envío directo de datos sensibles en mensajes de chat, un vector de exfiltración de rápido crecimiento en entornos colaborativos.  
[Referencia: Use DLP to protect sensitive items in Microsoft Teams – Microsoft Learn]

### 3.4 Endpoint DLP en Windows (GA) y macOS (preview) exclusivo M365 E5

Endpoint DLP extiende las políticas de Purview al sistema operativo en dispositivos cliente:

- Se distribuye a través del agente de Microsoft Defender for Endpoint en Windows 10/11 (GA) y macOS (versión preliminar)  
- Requiere que el dispositivo esté incorporado correctamente en Defender for Endpoint y en el tenant de M365  
[Referencia: Endpoint DLP prerequisites and deployment – Microsoft Learn]

Operaciones que Endpoint DLP puede interceptar según política:

- Copia de archivos a unidades USB u otros dispositivos extraíbles  
- Impresión de documentos en impresoras físicas o virtuales  
- Captura de pantalla o uso de herramientas de recorte sobre aplicaciones o documentos protegidos  
- Carga de archivos a través de navegadores web  
- Sincronización con aplicaciones de almacenamiento en la nube personales  
- Transferencias mediante Bluetooth u otros canales de proximidad  
- Acceso a archivos etiquetados desde aplicaciones no autorizadas  

Ejemplos de respuesta graduada según sensibilidad del contenido y perfil de riesgo del usuario:

- Archivo con sensibilidad media copiado a USB por usuario sin historial de riesgo: acción de auditoría  
- Archivo etiquetado como confidencial copiado a USB: notificación al usuario y requerimiento de justificación para permitir la acción  
- Archivo etiquetado como altamente confidencial copiado a USB: bloqueo sin posibilidad de override  
- Archivo de cualquier sensibilidad copiado a USB por usuario clasificado como riesgo alto por Insider Risk Management: bloqueo y generación de alerta  

[Referencias: Endpoint DLP learn-about; Endpoint DLP for macOS – Microsoft Learn]

Dado que Endpoint DLP actúa sobre actividades habituales de productividad, la calibración descrita en la sección 2.4 es especialmente relevante para evitar interrupciones masivas de trabajo legítimo.

### 3.5 Browser DLP con Microsoft Edge for Business exclusivo M365 E5

Browser DLP aplica políticas de DLP a nivel de navegador cuando este se ejecuta en modo de negocio gestionado:

- Requiere Edge for Business con gestión corporativa, incluyendo configuración de políticas y, cuando aplique, extensiones de Purview  
- Se aplica sobre aplicaciones web tanto corporativas como SaaS no gestionadas a las que se accede desde el navegador  

Capacidades típicas:

- Controlar uploads de archivos a dominios o aplicaciones web definidos como no autorizados  
- Interceptar operaciones de pegado de contenido sensible en formularios web  
- Restringir descargas de datos sensibles desde aplicaciones corporativas hacia entornos no confiables  

Browser DLP es clave cuando el vector de exfiltración es el navegador y el endpoint está gestionado, pero la aplicación de destino no.  
[Referencia: Browser DLP – Microsoft Learn]

### 3.6 Network DLP en M365 E5 con licenciamiento adicional

Network DLP introduce controles de DLP a nivel de red sin depender exclusivamente de agentes en el endpoint:

- Inspecciona tráfico de red en puntos de salida definidos hacia Internet o hacia entornos segmentados  
- Aplica políticas de DLP sobre flujos que atraviesan esos puntos, identificando y actuando sobre patrones de datos sensibles  
- Se orienta a organizaciones que requieren controles complementarios o redundantes a los del endpoint  

Requiere licenciamiento adicional sobre M365 E5.  
[Referencia: Network DLP overview – Microsoft Learn]

### 3.7 Cloud Apps con Microsoft Defender for Cloud Apps exclusivo M365 E5

La integración con Microsoft Defender for Cloud Apps (MDCA) permite aplicar DLP sobre aplicaciones SaaS de terceros:

- **App Discovery**  
  - Analiza logs de tráfico para identificar aplicaciones cloud utilizadas en la organización  
  - Clasifica aplicaciones por categorías y nivel de riesgo  

- **Inline controls y Session controls**  
  - Aplican políticas DLP en tiempo real sobre sesiones de usuario en aplicaciones específicas  
  - Pueden imponer restricciones como bloqueo de upload de archivos sensibles, permitir solo lectura o aplicar controles adaptativos según el contexto de sesión (origen, dispositivo, usuario)  

[Referencia: What is Microsoft Defender for Cloud Apps – Microsoft Learn]

Esta superficie extiende el alcance de DLP más allá de M365 hacia SaaS externos priorizados por riesgo y uso real.

## 4. Control de Shadow AI como caso límite de DLP

### 4.1 Shadow AI como vector de exfiltración

El uso de herramientas de inteligencia artificial generativa externas por parte de empleados genera un vector específico de salida de datos:

- Usuarios que copian contenido de documentos corporativos sensibles y lo pegan en prompts de servicios como ChatGPT, Claude o Gemini en navegadores web  
- En estos escenarios, la información sale del entorno gobernado por la organización y pasa a infraestructuras externas sobre las que la organización no tiene control directo  

Este caso no estaba cubierto por diseños de DLP centrados únicamente en correo y almacenamiento local y se ha convertido en uno de los casos de uso más relevantes en la configuración actual de políticas DLP.

### 4.2 Cobertura DLP de Shadow AI con Browser DLP y MDCA

La combinación de Browser DLP y MDCA ofrece dos capas de control:

- **Browser DLP en Edge for Business**  
  - Identifica operaciones de pegado de contenido en formularios web de herramientas de IA catalogadas como no autorizadas  
  - Puede bloquear el envío del prompt, notificar al usuario y registrar el intento como evento de DLP  
  [Referencia: Browser DLP – Microsoft Learn]

- **Defender for Cloud Apps (MDCA)**  
  - Detecta y clasifica el uso de aplicaciones de IA generativa mediante App Discovery  
  - Permite aplicar Session Controls que restringen uploads o inputs hacia determinadas aplicaciones o los permiten solo desde dispositivos y usuarios en condiciones específicas  
  - Facilita canalizar el uso de IA hacia aplicaciones aprobadas y gobernadas cuando existan alternativas corporativas  
  [Referencia: Information protection in Defender for Cloud Apps – Microsoft Learn]

### 4.3 Canalización hacia IA gobernada

En la configuración de DLP y Cloud Apps:

- Se definen políticas que bloquean o restringen el uso de herramientas de IA externas cuando se detecta envío de información sensible  
- Se configuran excepciones y reglas menos restrictivas para servicios de IA corporativos gobernados bajo Purview, como M365 Copilot, donde los datos permanecen en el perímetro de cumplimiento definido por la organización  

El objetivo no es prohibir el uso de IA, sino garantizar que se utilicen servicios cuya gestión de datos se alinea con las políticas y controles de cumplimiento de la organización.

## 5. Adaptive Protection y su integración con DLP e Insider Risk Management

Adaptive Protection conecta la evaluación de riesgo de usuarios de Insider Risk Management (IRM) con la ejecución de políticas DLP.  
[Referencia: Adaptive Protection in Microsoft Purview – Microsoft Learn]

### 5.1 Niveles de riesgo y señalización a DLP

- IRM evalúa comportamientos de usuarios (por ejemplo, picos de exfiltración, accesos anómalos a datos, actividades posteriores a cambios de rol) y asigna niveles de riesgo dinámicos como bajo, elevado o crítico  
- Cuando un usuario cambia de nivel de riesgo, Adaptive Protection comunica este cambio al motor de políticas DLP  
- Las políticas DLP se pueden definir con condiciones que tengan en cuenta el nivel de riesgo del usuario, de modo que la misma acción genere respuestas diferentes según ese nivel  

Ejemplos de impacto sobre DLP:

- Usuario en riesgo bajo: acciones predominantemente de auditoría o notificación  
- Usuario en riesgo elevado: incremento de acciones de bloqueo con override  
- Usuario en riesgo crítico: bloqueos absolutos para ciertas combinaciones de sensibilidad y destinos, especialmente en superficies de alto impacto como Endpoint DLP y Browser DLP  

### 5.2 Dependencias de arquitectura

- Adaptive Protection requiere que Insider Risk Management esté desplegado y configurado para generar niveles de riesgo  
- IRM y DLP comparten insumos como las etiquetas de sensibilidad aplicadas a documentos y correos, lo que permite correlacionar comportamiento de usuario con criticidad del contenido  
- Los eventos generados por Adaptive Protection y las acciones DLP asociadas se registran en los logs unificados y pueden ser consumidos por eDiscovery, SIEMs y otros componentes de la arquitectura de seguridad  

## 6. Parametrización sectorial y regional

El motor de políticas DLP permite ajustar reglas por sector y por tipo de dato:

- Uso de tipos de información sensible específicos para datos financieros, de salud, identificación personal, propiedad intelectual u otros dominios  
- Aplicación de plantillas de políticas o reglas adaptadas a regulaciones y prácticas locales  

En entornos latinoamericanos, la prioridad de casos de uso varía por sector (por ejemplo, servicios financieros, salud, manufactura exportadora). Por ejemplo:

- Las políticas de correo y compartición de archivos suelen enfocarse en datos regulados y documentación contractual con terceros  
- Endpoint DLP se configura de forma más estricta en roles con acceso privilegiado a datos críticos o a sistemas de producción  
- Las políticas de Shadow AI se ajustan según la madurez de adopción de servicios de IA aprobados y las restricciones regulatorias sobre exportación de datos  

Estas adaptaciones se implementan sobre la misma arquitectura de DLP descrita en las secciones anteriores, usando los componentes estándar de Purview.

## 7. Integraciones y dependencias arquitectónicas clave

Relaciones relevantes entre DLP y otros bloques de la arquitectura de seguridad y cumplimiento:

| Elemento DLP                     | Componente relacionado                           | Descripción técnica de la relación                           |
| -------------------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| Adaptive Protection              | Insider Risk Management (IRM)                    | IRM calcula niveles de riesgo de usuario que Adaptive Protection traslada a DLP para ajustar acciones por nivel de riesgo. |
| Eventos DLP                      | eDiscovery Premium                               | Los eventos de DLP en el Unified Audit Log se usan como evidencia en investigaciones y casos legales. |
| Endpoint DLP                     | Microsoft Defender for Endpoint                  | Endpoint DLP requiere agentes de Defender for Endpoint correctamente desplegados y dispositivos gestionados. |
| Condiciones basadas en etiquetas | Information Protection (etiquetas del Bloque 04) | Las etiquetas de sensibilidad son insumo primario para condiciones DLP basadas en clasificación de contenido. |
| DLP dentro de Data Security      | Arquitectura de seguridad de datos (Bloque 03)   | DLP se ubica en el pilar de protección de datos, complementando clasificación, etiquetado y cifrado. |
| Superficies DLP E3 frente a E5   | Modelo de licenciamiento M365                    | Exchange y SharePoint/OneDrive están en E3; Teams, Endpoint DLP, Browser DLP, Network DLP y Cloud Apps requieren E5. |
| Controles DLP para IA generativa | M365 Copilot y servicios de IA gobernados        | DLP y MDCA canalizan el uso de IA hacia servicios gobernados y restringen herramientas externas no gestionadas. |

## 8. Resumen ASCII de superficies y módulos DLP en Purview

```text
Motor de políticas central (Purview portal)
  - Definición unificada de:
      · Condiciones (SITs, etiquetas, contexto)
      · Acciones (permitir, auditar, notificar, bloquear, cifrar)
      · Notificaciones y alertas
  - Publicación de políticas a múltiples superficies

Superficies DLP disponibles con M365 E3 y E5
  - Exchange Online:
      · Inspección de correos salientes y adjuntos
      · Acciones de bloqueo, justificación, cifrado y notificación
  - SharePoint Online y OneDrive for Business:
      · Escaneo de archivos en reposo
      · Control de uso compartido externo y enlaces anónimos

Superficies DLP adicionales con M365 E5
  - Microsoft Teams:
      · Inspección de mensajes en chats y canales
      · Bloqueo y notificación en tiempo casi real
  - Endpoint DLP (Windows GA, macOS preview):
      · Control de copia a USB, impresión, captura de pantalla, carga web, apps no autorizadas
  - Browser DLP (Edge for Business):
      · Control de uploads y pegado en aplicaciones web, incluidas herramientas de IA externas
  - Network DLP:
      · Inspección de tráfico en puntos de salida de red definidos
  - Cloud Apps (Defender for Cloud Apps):
      · App Discovery y Session Controls sobre SaaS de terceros

Módulo de Adaptive Protection
  - Entrada: niveles de riesgo de usuario desde Insider Risk Management
  - Salida: endurecimiento o relajación de acciones DLP por usuario según riesgo

Módulo de control de Shadow AI
  - Browser DLP y Cloud Apps:
      · Restricción de envío de datos sensibles a herramientas de IA externas
      · Permisos diferenciados para servicios de IA corporativos gobernados
```





