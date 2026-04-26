---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art09-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art09-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art09-MS_CSU_Security_MCSA

Este documento explica cómo Microsoft Purview se convierte en el pilar de datos dentro de una arquitectura Zero Trust. Describe la relación entre los seis pilares clásicos, identidad, dispositivos, red, aplicaciones, infraestructura y datos. Muestra cómo Purview aporta la señal de sensibilidad del contenido para completar las decisiones de acceso. Recorre los tres principios de Zero Trust: verificación explícita, menor privilegio y asumir la brecha. Además, detalla integraciones con Microsoft Entra ID, SharePoint, OneDrive y otras soluciones de seguridad para reducir riesgos sin frenar la productividad. Presenta escenarios concretos de acceso, supervisión continua y respuesta rápida ante incidentes graves reales.

El primer eje presenta los seis pilares de Zero Trust y resalta el de datos. Los otros pilares validan usuario, dispositivo, red, aplicación e infraestructura, pero desconocen el contenido específico. Purview añade la clasificación y la etiqueta de sensibilidad del archivo o mensaje. Así responde a la pregunta crítica: quién puede abrir cada recurso concreto. Un ejemplo muestra SharePoint, donde todo parece correcto, excepto el documento altamente confidencial. Esa pieza sólo se muestra a directivos.

El segundo eje desarrolla el principio de verificación explícita aplicado a los datos. Microsoft Entra ID usa Acceso Condicional basado en identidad y estado del dispositivo. Al integrarse con Purview, SharePoint y OneDrive incorporan la sensibilidad del archivo en la decisión. El mismo usuario y el mismo equipo pueden recibir respuestas diferentes. Un documento público se abre plenamente, incluso desde un dispositivo no gestionado. Uno confidencial se limita al navegador. El más sensible se bloquea.

El tercer eje vincula el principio de menor privilegio con tres capacidades concretas. Las Barreras de información segmentan la organización y bloquean la colaboración entre grupos incompatibles, aunque alguien otorgue permisos de más. Customer Lockbox reduce el privilegio del propio proveedor. Cada acceso del soporte a datos del cliente exige solicitud, aprobación y límite temporal. La administración de acceso con privilegios aplica acceso justo a tiempo para tareas administrativas sensibles. Así se reducen abusos internos.

El cuarto eje asume que alguna capa fallará y diseña controles para contener la brecha. Las etiquetas de sensibilidad pueden aplicar cifrado persistente basado en servicios de protección de información. Aunque el archivo salga de Microsoft 365, sólo lo abren identidades autorizadas. Las políticas de ciclo de vida eliminan datos antiguos y reducen el volumen expuesto. Adaptive Protection endurece dinámicamente las políticas de prevención de fuga de datos cuando detecta comportamientos sospechosos. Facilitan investigación forense.

El quinto eje describe el flujo de decisión integrado que aplica Zero Trust a cada acceso. Primero se verifica identidad y contexto. Después, el estado del dispositivo y su cumplimiento. Luego, Purview aporta la sensibilidad del recurso y el perfil de riesgo del usuario. El resultado puede ser acceso completo, restringido, con desafío adicional o bloqueado. También aborda la madurez progresiva, el oversharing en SharePoint, la alineación con NIST y la productividad del entorno digital.





# Purview como pilar de datos en Zero Trust

## Conceptos clave

### 1. Los seis pilares y el rol de Purview: el componente que aporta la señal faltante

Zero Trust en Microsoft se articula en seis pilares: Identidad, Dispositivos, Red, Aplicaciones, Infraestructura y Datos. Los cinco primeros pueden tomar decisiones sólidas sobre el actor que solicita acceso, pero no sobre el activo específico al que intenta acceder.

Una organización puede tener:

- Identidad protegida con Microsoft Entra ID (MFA, detección de riesgo de inicio de sesión).  
- Dispositivos gestionados por Intune y protegidos por Defender for Endpoint.  
- Tráfico monitorizado por Azure Networking y Defender for Cloud Apps.  
- Aplicaciones controladas por Defender for Cloud Apps.  
- Infraestructura asegurada con Defender for Cloud.

Con este modelo, el sistema responde bien a la pregunta:  
“¿Debe este usuario, con este dispositivo, desde este contexto de red, acceder a esta aplicación?”.

Lo que no puede responder sin el pilar de Datos es:  
“¿Debe este usuario acceder a este archivo concreto dentro de esa aplicación?”.

Ejemplo típico: un empleado se autentica correctamente con MFA, usa un dispositivo gestionado, desde una ubicación conocida, para acceder a un sitio de SharePoint. Todo es correcto para los cinco primeros pilares. Sin embargo, el archivo concreto que intenta abrir puede estar etiquetado como “Altamente confidencial” y restringido a un pequeño grupo directivo. Esa información solo está en el plano de Datos, gestionado por Microsoft Purview, a través de clasificación y etiquetas de sensibilidad.  

Sin la señal de sensibilidad del dato, el sistema tiene una visión parcial: protege bien el acceso a la aplicación, pero no la exposición de cada recurso dentro de ella.

Mapa de pilares y componentes de Microsoft asociados:

- Identidad ↔ Microsoft Entra ID: verifica quién es el usuario y su nivel de riesgo.  
- Dispositivos ↔ Microsoft Intune + Defender for Endpoint: estado de gestión y salud del dispositivo.  
- Red ↔ Azure Networking + Defender for Cloud Apps: contexto y anomalías de red.  
- Aplicaciones ↔ Defender for Cloud Apps: perfil de riesgo y uso de aplicaciones.  
- Infraestructura ↔ Defender for Cloud: estado de seguridad de recursos de infraestructura.  
- Datos ↔ Microsoft Purview: clasificación, sensibilidad y protección persistente del contenido.  

Referencia: Purview Information Protection overview.  


### 2. Principio 1: Verificación explícita - la sensibilidad como factor de decisión

El principio “Verificar explícitamente” indica que toda decisión de acceso debe considerar todas las señales relevantes disponibles en el momento. En la capa de datos, Purview aporta la señal de sensibilidad para que el motor de Acceso Condicional de Entra ID tome decisiones más finas.

#### Mecanismo técnico

Microsoft Entra ID define y aplica políticas de Acceso Condicional. Tradicionalmente estas políticas usan señales como:

- Estado de autenticación (MFA, ubicación, riesgo de inicio de sesión).  
- Estado del dispositivo (marcado como conforme o no conforme por Intune).

Cuando se integra Purview, ciertas aplicaciones como SharePoint y OneDrive pueden incorporar la etiqueta de sensibilidad del archivo como condición adicional en las políticas de acceso. Esto requiere configuración específica y no está disponible de forma universal en todas las aplicaciones.  

Referencia: Conditional Access integration with sensitivity labels.  

#### Ejemplos de comportamiento granular

Mismo usuario, mismo dispositivo, misma ubicación, pero tres archivos con sensibilidad distinta:

1. Archivo etiquetado como “Público”  
   - Dispositivo no gestionado.  
   - Política: acceso permitido, incluso con descarga, porque el impacto de una fuga es bajo.

2. Archivo etiquetado como “Confidencial”  
   - Mismo usuario y dispositivo.  
   - Política: acceso solo en modo lectura en el navegador, sin descarga ni sincronización; se limita la exfiltración directa.

3. Archivo etiquetado como “Altamente confidencial”  
   - Mismo usuario y dispositivo.  
   - Política: acceso bloqueado desde dispositivos no gestionados; el usuario debe cambiar a un dispositivo corporativo y conforme.

La lógica es consistente con Zero Trust: la decisión no se toma solo en función de usuario y dispositivo, sino de la combinación con la sensibilidad del recurso concreto.


### 3. Principio 2: Menor privilegio - tres implementaciones técnicas

El principio de “menor privilegio” establece que ningún actor debe tener más acceso del estrictamente necesario para la tarea que realiza. En el pilar de Datos, Purview aporta tres capacidades clave que reducen el exceso de privilegio desde ángulos distintos.

#### 3.1. Information Barriers: menor privilegio estructural

Information Barriers definen segmentos organizativos que no pueden interactuar ni compartir información entre sí, incluso si usan las mismas herramientas de colaboración.

Casos de uso típicos:

- Separar equipos de front office y back office en organizaciones reguladas.  
- Evitar que ciertos departamentos (por ejemplo, trading y research) compartan información de forma directa.

Efectos técnicos:

- En Microsoft Teams, impiden chats, llamadas y reuniones entre usuarios de segmentos bloqueados entre sí.  
- En SharePoint y OneDrive, restringen el acceso de segmentos no autorizados a sitios y contenidos.  
- En Exchange Online, bloquean el envío de correos entre segmentos definidos como incompatibles.

De esta forma, incluso si alguien concede permisos de más en un sitio o grupo, las barreras estructurales impiden la comunicación no permitida.  

Referencia: Information barriers in Microsoft 365.  

#### 3.2. Customer Lockbox: menor privilegio hacia el proveedor

Customer Lockbox aborda el exceso de privilegio en la relación con el propio proveedor de servicio. Ocasionalmente, el soporte de Microsoft puede necesitar acceder a datos de un cliente para resolver una incidencia. Con Customer Lockbox:

- Ese acceso solo se produce tras una solicitud explícita de Microsoft.  
- El administrador del cliente debe aprobar o rechazar la solicitud.  
- El acceso concedido es temporal, limitado a la tarea y auditable.

Así, incluso en el plano del proveedor, se aplica Just Enough Access y solo bajo control del cliente.  

Referencia: Customer Lockbox.  

#### 3.3. Privileged Access Management (PAM): menor privilegio administrativo

PAM traslada los principios Just in Time (JIT) y Just Enough Access (JEA) a las tareas de administración de alto riesgo.

Características clave:

- Los administradores no mantienen privilegios elevados de forma permanente.  
- Para realizar una tarea sensible deben solicitar acceso, especificar el propósito y la duración.  
- La solicitud puede requerir aprobación de otro rol autorizado.  
- El acceso se concede solo durante la ventana aprobada y luego expira automáticamente.  

Esto reduce el impacto de credenciales administrativas comprometidas y limita la superficie de abuso interno.  

Referencia: Privileged Access Management in Microsoft 365.  


### 4. Principio 3: Asumir la brecha - diseñar para el compromiso

“Asumir la brecha” implica diseñar controles bajo la premisa de que, tarde o temprano, alguna capa preventiva fallará. En el pilar de Datos, Purview implementa este principio en cuatro mecanismos complementarios.

#### 4.1. Cifrado persistente como contención post-brecha

Las etiquetas de sensibilidad pueden aplicar cifrado basado en Azure Information Protection y Rights Management Services. El archivo se cifra de forma persistente y las claves de descifrado están ligadas a la identidad y a las políticas.

Consecuencias:

- Si el archivo se descarga a un dispositivo no controlado o se extrae fuera de M365, permanece cifrado.  
- Solo los usuarios y dispositivos autorizados pueden abrirlo, incluso fuera del entorno corporativo.  

Esto convierte la pérdida de control del archivo en un escenario de impacto limitado en lugar de una fuga completa.  

Referencia: What is Azure Information Protection.  

#### 4.2. Minimización de datos mediante Data Lifecycle Management

Las políticas de retención y eliminación automática en Microsoft Purview Data Lifecycle Management reducen la cantidad de datos accesibles en caso de brecha.

Ejemplos:

- Contenido de colaboración con retención corta para canales de proyectos acotados en el tiempo.  
- Eliminación automática de datos de bajo valor pasado el periodo legal o de negocio.  

Cuantos menos datos antiguos y sin uso haya en el entorno, menor es el volumen potencial comprometido.  

Referencia: Microsoft Purview Data Lifecycle Management.  

#### 4.3. Adaptive Protection como contención proactiva

Adaptive Protection combina señales de Insider Risk Management con políticas de DLP para ajustar dinámicamente el nivel de protección en función del comportamiento del usuario.

Ejemplo:

- Si un usuario empieza a descargar volúmenes inusuales de datos confidenciales, o a compartirlos con dominios externos, su perfil de riesgo aumenta.  
- Automáticamente se aplican controles más estrictos: bloqueos de descarga, restricciones de copia, requisitos adicionales de justificación, etc.  

El objetivo es intervenir antes de que ocurra la exfiltración efectiva, no solo reaccionar después.  

Referencia: Adaptive Protection.  

#### 4.4. Audit Premium y eDiscovery como capacidad de recuperación

Cuando una brecha ocurre, la capacidad de reconstruir los hechos es crítica. Audit Premium y eDiscovery soportan esta fase:

- Audit Premium registra eventos de alto valor, como el acceso a elementos de correo (por ejemplo, `MailItemsAccessed`), acceso a archivos y acciones administrativas.  
- eDiscovery Standard y Premium permiten identificar, preservar, analizar y exportar contenido relevante para investigaciones o litigios.  

Estos mecanismos no previenen el incidente, pero son esenciales para responder, contener, cumplir obligaciones regulatorias y mejorar controles posteriores.  

Referencias: Audit solutions in Microsoft 365, eDiscovery solutions in Microsoft 365.  


### 5. El flujo de decisión integrado: algoritmo de Zero Trust para datos

Los tres principios de Zero Trust se concretan en un flujo de decisión integrado que se ejecuta cada vez que un usuario solicita acceso a un recurso. Aunque el procesamiento real ocurre en paralelo y en milisegundos, puede describirse en cuatro verificaciones lógicas.

#### 5.1. Verificación de identidad: ¿quién eres?

- Componente: Microsoft Entra ID.  
- Elementos evaluados: autenticación, estado de MFA, riesgo de inicio de sesión, pertenencia a grupos, ubicación.  
- Resultado: permitir, requerir controles adicionales (por ejemplo MFA), o bloquear el acceso a partir de la identidad.  

Referencia: Conditional Access overview.  

#### 5.2. Verificación del dispositivo: ¿en qué condición está tu endpoint?

- Componentes: Intune y Defender for Endpoint.  
- Elementos evaluados: cumplimiento de políticas, cifrado, estado antivirus, versión del sistema operativo, jailbreak o root, entre otros.  
- Resultado: el dispositivo se marca como conforme o no conforme, lo que se usa como condición en Acceso Condicional.  

Referencia: Intune compliance policies.  

#### 5.3. Verificación de sensibilidad: ¿qué tipo de recurso estás intentando abrir?

- Componente: Microsoft Purview Information Protection.  
- Elementos evaluados: etiqueta de sensibilidad del archivo o elemento, políticas asociadas (cifrado, restricciones de acceso, justificación, etc.).  
- Resultado: el motor de decisión puede diferenciar entre recursos públicos, confidenciales o altamente confidenciales y aplicar controles proporcionados.  

Referencia: Sensitivity labels for SharePoint and OneDrive.  

#### 5.4. Verificación del perfil de riesgo del usuario

- Componentes: Insider Risk Management y Adaptive Protection.  
- Elementos evaluados: patrones de comportamiento recientes, señales de riesgo interno, actividad anómala de acceso o descarga de datos.  
- Resultado: endurecer controles para usuarios de alto riesgo o mantener una experiencia más fluida para usuarios de bajo riesgo.  

Referencia: Adaptive Protection.  

#### 5.5. Estados posibles de acceso

La combinación de estas verificaciones produce varios resultados posibles:

- Acceso completo: todas las señales son favorables para el contexto y la sensibilidad del recurso.  
- Acceso restringido: el acceso se concede con limitaciones (solo lectura, sin descarga, sin impresión, sin compartir externo).  
- Acceso con step-up: se requiere una comprobación adicional, por ejemplo MFA o justificación de negocio.  
- Acceso bloqueado: cuando la combinación de identidad, dispositivo, sensibilidad y riesgo no es aceptable.  

Este flujo refleja la implementación práctica del modelo Zero Trust maturity model en el pilar de Datos.  


## Notas de soporte

### Nota 1. Zero Trust como viaje de madurez

Zero Trust no se implementa de una sola vez. El modelo de madurez de Microsoft describe niveles que van desde enfoques tradicionales hasta estados optimizados, con incrementos progresivos en automatización, integración de señales y granularidad de controles.  

En la práctica, muchas organizaciones comienzan reforzando identidad y dispositivos, luego incorporan protección de datos con Purview y finalmente añaden capas avanzadas como Adaptive Protection y análisis de riesgo interno.  

Referencia: Zero Trust maturity model.  


### Nota 2. El reto del oversharing en SharePoint

Un obstáculo común para aplicar Zero Trust a datos es el “oversharing” histórico en SharePoint y OneDrive:

- Sitios con permisos heredados excesivamente abiertos.  
- Contenido confidencial colocado en ubicaciones de colaboración general.  
- Grupos con demasiados miembros para el nivel de sensibilidad del contenido.

Microsoft Purview Data Estate Insights ayuda a identificar:

- Dónde residen los datos sensibles.  
- Qué ubicaciones tienen mayor exposición.  
- Dónde hay configuraciones de permisos desalineadas con la sensibilidad.  

Estos datos permiten priorizar correcciones de acceso y reubicar contenido antes de endurecer políticas Zero Trust.  

Referencia: Data Estate Insights.  


### Nota 3. Alineación con NIST SP 800-207

La publicación NIST SP 800-207 define los principios y tenets de Zero Trust Architecture. La aproximación de Microsoft:

- Mapea los seis pilares (Identidad, Dispositivos, Red, Aplicaciones, Infraestructura, Datos) a los tenets de NIST.  
- Proporciona componentes específicos para implementar esos tenets de forma práctica.  

En este contexto, el pilar de Datos con Purview es la concreción de varios tenets relacionados con la protección de recursos, la política centrada en el activo y la monitorización continua.  

Referencia: NIST SP 800-207 Zero Trust Architecture.  


### Nota 4. Zero Trust como habilitador de productividad

Bien diseñado, Zero Trust no se limita a bloquear. Su objetivo es ajustar la fricción al nivel de riesgo:

- Operaciones de bajo riesgo con datos poco sensibles se mantienen casi sin fricción.  
- Operaciones de alto riesgo o con datos muy sensibles exigen más controles, verificación adicional o dispositivos más seguros.  

En entornos donde Purview está correctamente integrado con Identidad y Dispositivos, el resultado es una experiencia en la que el usuario percibe controles coherentes con el tipo de tarea y de dato, en lugar de restricciones uniformes que penalizan la productividad.  

Referencia: Zero Trust guidance.
