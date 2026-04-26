---
layout: page
title: "MS_Purview_M365E5-SecOps_e02v01_Art10-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e02/ms-purview-m365e5-secops-e02v01-art10-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e02v01_Art10-MS_CSU_Security_MCSA

Este documento describe el papel de Microsoft Purview dentro de una arquitectura Zero Trust moderna. Explica cómo Purview se posiciona como plano de control del dato frente a los pilares de identidad, dispositivos, aplicaciones, red e infraestructura. Recorre las funciones de etiquetado de sensibilidad, protección de la información y DLP. También cubre Insider Risk Management y Audit Premium, y su integración con Entra ID, Intune, Defender y Sentinel. Su finalidad es mostrar una ruta de despliegue y cómo estos controles sostienen los principios de verificar explícitamente, mínimo privilegio y asumir compromiso. También introduce riesgos ligados a agentes de IA generativa.

En el modelo Zero Trust de Microsoft, los pilares de identidad, dispositivos, aplicaciones, red e infraestructura se coordinan. Sin embargo, el plano de datos se protege con Purview. El dato se trata como bastión. Aunque una identidad, un dispositivo o la red se vean comprometidos, un archivo cifrado y etiquetado sigue inaccesible sin credenciales válidas y permisos correctos. Así, Purview desacopla la seguridad del dato de la seguridad de los planos circundantes en la organización.

Las sensitivity labels de Purview se convierten en una señal directa para las políticas de Conditional Access en Entra ID. Un documento marcado como altamente confidencial puede exigir multifactor, dispositivo gestionado o ubicación controlada aunque el acceso al entorno sea más laxo. A la inversa, el cumplimiento del dispositivo y el riesgo de la identidad limitan qué etiquetas y cifrados puede aplicar cada usuario. Este bucle bidireccional unifica los planos de identidad y de datos.

Purview DLP lleva el principio de verificar explícitamente al plano del dato. No solo controla el acceso inicial, sino cada acción sensible sobre la información. Copiar, imprimir, enviar por correo o subir a la nube se evalúa en tiempo real contra las políticas definidas. La decisión combina sensibilidad del contenido, identidad, riesgo del usuario mediante Adaptive Protection, estado del dispositivo y destino. Así, DLP deja de ser frontera estática y se vuelve inspección continua realmente.

Insider Risk Management introduce la confianza dinámica en el modelo. Asigna a cada usuario una puntuación de riesgo basada en comportamientos sobre datos sensibles. Esa puntuación alimenta Adaptive Protection y hace que las mismas políticas DLP se endurezcan automáticamente cuando el riesgo aumenta. Puede incluso influir en Conditional Access, forzando reautenticaciones o multifactor adicionales. De este modo, el nivel de acceso efectivo a la información se ajusta continuamente según el comportamiento reciente del propio usuario.

Audit Premium materializa el principio de asumir compromiso en el plano del dato. Mantiene un año de registros detallados sobre accesos y acciones, que luego se correlacionan en Microsoft Sentinel con señales de identidad, endpoint, red y aplicación. Para extraer valor, conviene desplegar antes Entra ID, Intune, DLP e Insider Risk, de modo que ya generen telemetría rica. Sobre esta base, los nuevos agentes de IA, como Copilot, pueden gobernarse como identidades adicionales muy controladas.





# Purview en una Arquitectura Zero Trust

## Nivel 2 - Conceptos clave

| Concepto                                                     | Explicación                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Los cinco planos de Zero Trust y el posicionamiento de Purview** | El modelo Zero Trust de Microsoft, documentado en el Zero Trust Guidance Center, define seis pilares: Identidad (Entra ID), Endpoints (MDE + Intune), Aplicaciones (Defender for Cloud Apps + MDCA), Red (Azure Firewall + Defender for Cloud + ZTNA), Infraestructura (Defender for Cloud) y Datos (Microsoft Purview). La posición del dato como último plano de control refleja el principio de defensa en profundidad: incluso si la identidad se compromete, el dispositivo se compromete o la red es interceptada, un archivo cifrado con una sensitivity label de alta confidencialidad y protegido con Customer Key sigue siendo técnicamente inaccesible sin las credenciales correctas y los permisos definidos en la política. Purview es el plano de control que garantiza que la seguridad del dato sea independiente de la seguridad de los planos que lo rodean. |
| **Sensitivity Labels como señal de Conditional Access**      | La integración entre Purview Information Protection y Conditional Access de Entra ID crea un bucle de control bidireccional entre los planos de identidad y dato. En la dirección Purview → Entra ID, las sensitivity labels aplicadas a documentos se consumen como condición de contexto en políticas de Conditional Access, de modo que el acceso a documentos con labels de alta confidencialidad puede requerir MFA, dispositivo gestionado o ubicación de red específica, aunque el acceso general al tenant no exija esos requisitos. En la dirección Entra ID → Purview, el estado de cumplimiento del dispositivo evaluado por Intune y el nivel de riesgo de la identidad calculado por Entra ID Protection pueden condicionar qué acciones de etiquetado y cifrado están disponibles para el usuario en ese contexto. Esta bidireccionalidad convierte a la combinación Purview + Entra ID en el núcleo integrado del plano de datos y el plano de identidad en una arquitectura Zero Trust coherente. |
| **DLP como enforcement del principio "verify explicitly" en el dato** | El principio "verify explicitly" de Zero Trust establece que cada solicitud de acceso o acción debe autenticarse y autorizarse utilizando todo el contexto disponible. En el plano del dato, Purview DLP implementa este principio: cada acción que un usuario intenta realizar sobre un dato sensible, no solo el acceso, sino también copiar, imprimir, enviar o subir, se intercepta, se analiza contra la política y se permite o bloquea según la combinación de sensibilidad del dato, identidad del usuario, nivel de riesgo del usuario (via Adaptive Protection), estado del dispositivo y destino de la acción. DLP deja de ser un control perimetral centrado en la frontera del tenant y pasa a ser un control de acción continua, con verificación en cada punto de manipulación del dato, que es la diferencia fundamental entre seguridad perimetral y modelo Zero Trust. |
| **IRM como señal de confianza dinámica en el modelo Zero Trust** | Zero Trust asume que la confianza es un valor dinámico para cada identidad, no un estado binario fijado en el onboarding. Insider Risk Management es el mecanismo de Purview que materializa este principio en el plano del dato: la puntuación de riesgo de IRM es una medida continua y actualizable del nivel de confianza que se puede depositar en las acciones de un usuario sobre datos sensibles. A través de Adaptive Protection, esta puntuación de riesgo se convierte en una variable de entrada para las políticas DLP, reduciendo la confianza efectiva y endureciendo las políticas cuando el riesgo del usuario aumenta. Esta señal también puede alimentar políticas de Conditional Access en Entra ID, por ejemplo exigiendo reautenticación o MFA adicional a usuarios en estado de riesgo elevado. IRM se convierte así en el motor de ajuste dinámico de confianza del modelo Zero Trust en el plano del usuario. |
| **Audit Premium como plano de verificación del modelo Zero Trust** | El principio "assume breach" exige que la arquitectura permita detectar compromisos, limitar el radio de explosión y reconstruir lo ocurrido. Audit Premium implementa este principio en el plano del dato de M365: la retención de un año de logs de actividad con eventos de alta fidelidad (como MailItemsAccessed, SearchQueryInitiated, FileDownloaded con metadatos completos) proporciona la base forense para reconstruir la secuencia de acciones ejecutadas por un atacante tras comprometer una identidad. La integración de Audit Premium con Microsoft Sentinel como SIEM central permite correlacionar en tiempo casi real las señales de identidad (Entra ID Protection), endpoint (MDE), red (Defender for Cloud), aplicación (MDCA) y dato (Audit). Esto habilita detecciones sobre patrones de comportamiento post-compromiso en el plano del dato tan pronto como hay señales suficientes para generar una alerta. |


## Nivel 3 - Notas de soporte

### Secuencia recomendada de despliegue de Purview en Zero Trust

La implementación de Purview dentro de una arquitectura Zero Trust requiere respetar las dependencias técnicas entre planos. Una secuencia práctica recomendada es:

1. **Identidad primero**  
   Completar el despliegue básico de Entra ID con MFA y Conditional Access antes de activar las integraciones de sensitivity labels con Conditional Access. Si las políticas de Zero Trust en identidad no están maduras, estas integraciones pueden introducir fricciones excesivas para el usuario.

2. **Gestión de dispositivos antes de Endpoint DLP**  
   Desplegar Intune con compliance policies para dispositivos antes de activar Endpoint DLP. El agente de Endpoint DLP requiere dispositivos gestionados para operar de forma fiable y para poder aplicar decisiones basadas en el estado de cumplimiento del endpoint.

3. **Information Protection y DLP antes de Adaptive Protection**  
   Activar primero Information Protection y las políticas DLP básicas. Solo después habilitar Adaptive Protection, ya que IRM necesita que DLP esté activo para poder ajustar las políticas de manera dinámica según el riesgo detectado.

4. **Audit Premium y Sentinel cuando ya existan señales**  
   Integrar Audit Premium con Microsoft Sentinel una vez que los demás componentes estén generando telemetría suficiente. De este modo, el SIEM puede correlacionar los logs de Audit con señales de identidad, endpoint, red y aplicaciones, y producir detecciones significativas en el plano del dato.

### Referencia arquitectónica oficial

La referencia arquitectónica oficial de Microsoft para Zero Trust es el Zero Trust Guidance Center, que incluye:

- El Zero Trust Adoption Framework con fases de despliegue recomendadas.
- Diagramas de integración entre pilares de identidad, endpoint, aplicación, red, infraestructura y datos.
- Guías de configuración específicas por producto.

Dentro de este framework se posiciona explícitamente a Microsoft Purview como el pilar de datos y se documenta cómo contribuye a los principios de "verify explicitly", "use least privileged access" y "assume breach".

### Extensión hacia el plano de riesgo de IA generativa

Este bloque sintetiza los controles técnicos tratados en los planos de identidad, dispositivo, red, aplicación y dato, reencuadrándolos en el modelo Zero Trust con Purview como plano de control del dato.  

En una evolución posterior, el plano de riesgo se amplía con la introducción de la IA generativa: por ejemplo, Copilot actúa como un nuevo agente que hereda los permisos del usuario y los controles de Purview, pero añade vectores de exposición que el modelo Zero Trust original no contemplaba. La arquitectura debe considerar a estos agentes de IA como identidades operativas adicionales, sujetas a las mismas dependencias e integraciones entre Purview, Entra ID, DLP, IRM y Audit Premium.
