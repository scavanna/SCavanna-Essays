---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art06-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art06-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art06-MS_CSU_Security_MCSA

Este documento describe las capacidades de Insider Risk Management y Communication Compliance en Microsoft Purview. Explica cómo estas soluciones ayudan a detectar, investigar y mitigar riesgos internos vinculados al uso de datos y canales de comunicación en Microsoft 365. Detalla escenarios típicos de exfiltración, sabotaje, offboarding y filtración accidental, así como la arquitectura de señales que alimenta los modelos de riesgo. Además, profundiza en controles de privacidad y gestión de evidencia para investigaciones formales. También aborda implicancias regulatorias, factor humano, liderazgo ético y adopción organizacional como pilares del programa. Se enmarcan dentro de iniciativas de seguridad y cumplimiento corporativo regulados.

Primero, el texto describe los cuatro escenarios de riesgo interno gestionados por Insider Risk Management. La exfiltración de datos se centra en descargas masivas, copias a medios externos y accesos inusuales. El sabotaje aborda eliminaciones, modificaciones maliciosas y abusos de privilegios. El riesgo de offboarding combina eventos de recursos humanos con monitoreo reforzado. La filtración accidental cubre errores cotidianos de compartición. En conjunto, estos escenarios permiten distinguir actividades normales de comportamientos que indican posible daño.

En segundo lugar, se explica la arquitectura de señales que alimenta los modelos de riesgo. Insider Risk Management integra actividad de SharePoint, OneDrive, Exchange, Teams, Entra ID, Intune, Defender y sistemas de recursos humanos. Cada origen aporta eventos específicos sobre archivos, comunicaciones, identidad y dispositivos. La solución correlaciona estas señales en el tiempo y el contexto del usuario. Así construye un perfil dinámico con niveles de riesgo que reflejan cambios de comportamiento y situación laboral.

En tercer lugar, el documento destaca el diseño de privacidad de Insider Risk Management. La herramienta aplica pseudonimización para ocultar identidades mientras no exista investigación formal. Además, separa roles para que quien analiza alertas no sea quien revela identidades. Toda acción, incluida la desanonimización, queda registrada en auditoría unificada. Así se reduce el riesgo de abuso y se demuestra que el foco está en patrones de riesgo, no en vigilancia masiva de personas en organizaciones.

En cuarto lugar, se presenta Communication Compliance como complemento enfocado en comunicaciones. Supervisa contenido de Teams, Exchange, Viva Engage y mensajería financiera mediante clasificadores de lenguaje natural. Detecta acoso, discriminación, lenguaje inapropiado, posibles abusos de mercado y violaciones de políticas de conducta. Las coincidencias se envían a colas de revisión donde personas autorizadas analizan contexto y registran decisiones. Este proceso, completamente auditado, ayuda a demostrar diligencia y cumplimiento, especialmente en entidades financieras y mercados regulados.

Por último, el texto detalla cómo Insider Risk Management transforma señales en evidencia útil para investigaciones. Explica las fases de detección, triaje, investigación y resolución, con posibilidad de exportar datos a eDiscovery. Aclara la diferencia con Data Loss Prevention, más centrada en contenido. También resalta la importancia de la adopción organizacional, la comunicación transparente y la formación continua. Todo se enmarca en una visión donde la tecnología complementa, pero nunca sustituye, la responsabilidad humana compartida.





# Insider Risk Management y Communication Compliance en Microsoft Purview

## Nivel 2: Conceptos clave

### Concepto 1. Los cuatro escenarios de riesgo interno  
*El catálogo de las amenazas con cara conocida*

Insider Risk Management se basa en escenarios de riesgo predefinidos con lógica de detección específica. Esta especialización permite distinguir actividades rutinarias de comportamientos que, por su contexto y secuencia, representan riesgo.

#### Escenario 1. Exfiltración de datos

Es el escenario de mayor prevalencia e impacto económico. El sistema identifica cuando un usuario construye una colección de datos corporativos con intención de extraerla fuera de la organización.  
Patrones típicos:

- Descargas masivas de archivos desde SharePoint o OneDrive en un periodo corto.  
- Copia posterior a almacenamiento extraíble o repositorios personales.  
- Acceso repentino a tipos de archivo que el usuario no consulta habitualmente.

La señal no es la acción aislada sino la secuencia de actividades dentro de una ventana temporal concreta.

#### Escenario 2. Sabotaje de datos

Detecta comportamientos destructivos o de modificación maliciosa sobre activos corporativos.  
Ejemplos de señales:

- Eliminación masiva de archivos o correos fuera del patrón histórico del usuario.  
- Modificación sistemática de documentos críticos.  
- Revocación injustificada de accesos por parte de administradores o usuarios con permisos elevados que normalmente no realizan esas operaciones.

Este escenario es especialmente relevante en cuentas privilegiadas y en usuarios en conflicto abierto con la organización.

#### Escenario 3. Riesgo de offboarding

Se activa ante eventos de RR. HH. como renuncia o desvinculación. Estudios de la industria indican que entre el 60 % y el 70 % de las personas que dejan una organización se llevan algún tipo de información corporativa, desde listados de clientes hasta documentación técnica propietaria.  

IRM integra señales de RR. HH. como:

- Fecha de presentación de renuncia.  
- Fecha de último día laboral.  

Con estos disparadores aplica monitoreo reforzado durante el periodo de transición. Un empleado que, tras comunicar su salida, comienza a descargar volúmenes inusuales de archivos en sus últimas semanas genera alertas de alta severidad.

#### Escenario 4. Filtración accidental

Una parte significativa de las brechas internas es consecuencia de errores no intencionales. Ejemplos frecuentes:

- Adjuntar el archivo equivocado en un correo dirigido a destinatarios externos.  
- Compartir un informe financiero o documento sensible con un equipo de Teams más amplio de lo debido.  
- Configurar de forma incorrecta los permisos de un sitio de SharePoint.

IRM detecta estos patrones y genera alertas de menor riesgo que permiten corregir el incidente antes de que se convierta en una infracción regulatoria o en un problema reputacional.

---

### Concepto 2. Arquitectura de señales  
*El sistema que escucha en todas las cargas de trabajo*

La ventaja diferencial de Insider Risk Management frente a muchas soluciones UEBA de terceros es su integración nativa con el ecosistema Microsoft 365. Recibe señales de actividad de múltiples servicios de Microsoft y de sistemas de RR. HH., lo que permite construir una visión completa del comportamiento del usuario.

**Fuentes de señales y aportes principales**

- **SharePoint y OneDrive**  
  Reportan descargas, cargas, movimientos y eliminaciones de archivos, con metadatos completos, incluyendo las etiquetas de sensibilidad de cada elemento afectado.

- **Exchange Online**  
  Informa de envíos externos, adjuntos y patrones de comunicación inusual, como volúmenes atípicos de correo hacia dominios específicos.

- **Microsoft Teams**  
  Proporciona señales sobre mensajes, compartición de archivos y comunicaciones con usuarios externos o invitados.

- **Entra ID**  
  Aporta señales de identidad: intentos de autenticación fallidos, accesos desde ubicaciones o dispositivos inusuales, actividad fuera del horario laboral típico, cambios en el perfil de acceso y accesos a recursos que el usuario no suele utilizar.

- **Intune y Defender for Endpoint**  
  Registran actividad de dispositivos como uso de USB, impresión de documentos sensibles, instalación de software no autorizado y eventos de Endpoint DLP bloqueados o justificados por el usuario.

- **Sistemas de RR. HH.**  
  A través de conectores o cargas periódicas desde plataformas como Workday o SAP SuccessFactors, se incorporan eventos críticos como renuncias, advertencias disciplinarias o cambios de rol.

IRM correlaciona temporal y contextualmente estas señales para construir un perfil dinámico de riesgo por usuario, asignando un nivel de riesgo que puede cambiar según evoluciona el comportamiento.

---

### Concepto 3. Diseño privacy first  
*Investigar sin vigilancia masiva*

Insider Risk Management incorpora controles estructurales de privacidad para equilibrar la detección de riesgos con la protección de la identidad personal. El diseño prioriza la anonimización, la segregación de funciones y la trazabilidad de cada acción dentro de la herramienta.

**Controles clave de privacidad**

- **Pseudonimización**  
  Los usuarios aparecen con identificadores anónimos en el panel de IRM. Mientras un caso no se eleve a investigación formal, las identidades reales permanecen ocultas para los analistas.

- **Segregación de roles**  
  La revelación de identidad requiere la intervención de un rol diferente al del analista que revisa las alertas. Esto introduce un control de doble validación y reduce el riesgo de abuso.

- **Trazabilidad completa**  
  Todas las acciones realizadas en IRM, incluidas las decisiones de desanonimización, quedan registradas en el registro unificado de auditoría. Esto proporciona transparencia y soporte a auditorías internas o externas.

De esta forma, IRM se posiciona como una herramienta de investigación dirigida a patrones de riesgo, no como un sistema de inspección masiva de personas.

---

### Concepto 4. Communication Compliance  
*Supervisión que protege a la organización y a las personas*

Communication Compliance aborda riesgos en los canales de comunicación corporativos, tanto internos como externos. Admite contenido de Microsoft Teams, Exchange, Viva Engage y conectores para plataformas de mensajería financiera como Bloomberg, ICE Chat y Reuters Eikon.

La solución utiliza clasificadores de lenguaje natural entrenados para identificar:

- Acoso y conductas abusivas.  
- Discriminación y lenguaje inapropiado.  
- Conflictos de interés y posibles prácticas de abuso de mercado.  
- Riesgos regulatorios específicos del sector financiero.  
- Vulneraciones de políticas corporativas de conducta y comunicación.

Las comunicaciones que coinciden con estos patrones se enrutan a colas de revisión. Revisores humanos examinan el contexto, confirman o descartan la alerta y documentan la decisión. Todo el proceso queda auditado, lo que contribuye a demostrar diligencia y cumplimiento ante reguladores y auditores.

---

### Concepto 5. Evidencia de investigación en IRM  
*De la sospecha a la evidencia*

Insider Risk Management identifica que algo anómalo está ocurriendo, pero la gestión formal de un incidente requiere transformar esa sospecha en evidencia estructurada. Para ello, IRM organiza el ciclo de investigación en cuatro fases principales:

1. **Detección**  
   Generación de alertas basadas en escenarios de riesgo y correlación de señales.

2. **Triaje**  
   Priorización de casos según severidad, volumen de señales y contexto del usuario. Se descartan falsos positivos y se identifican los incidentes que requieren investigación detallada.

3. **Investigación**  
   Análisis del caso mediante revisión de actividades, cronologías y señales vinculadas. En esta fase se evalúa la intención y el impacto potencial o real.

4. **Resolución**  
   Cierre del caso con la acción correspondiente, que puede ir desde formación adicional hasta medidas disciplinarias o acciones legales.

Durante la investigación, IRM recopila registros de actividad y señales correlacionadas que pueden exportarse a eDiscovery Premium para revisión legal y conservación probatoria. La evidencia disponible está basada en logs y actividades registradas; no existe, en la documentación oficial, un módulo específico de capturas de pantalla ni un complemento independiente denominado de forma explícita como solución de evidencia forense.

---

## Nivel 3: Notas de soporte e insights de profundidad

### Nota 1. Diferencia conceptual entre IRM y DLP

Insider Risk Management y Data Loss Prevention son soluciones complementarias, no intercambiables. DLP analiza y protege el contenido de los datos, mientras que IRM se centra en el comportamiento del usuario y en los patrones de actividad.  

Un empleado puede exfiltrar información sin disparar ninguna regla DLP, por ejemplo copiando datos no clasificados o actuando en un contexto no previsto por las políticas de contenido. IRM detecta ese riesgo por la secuencia de acciones y su contexto, más que por el tipo de archivo o etiqueta aplicada.

---

### Nota 2. Adopción organizacional de Communication Compliance

La percepción de vigilancia puede frenar la adopción de Communication Compliance si no se gestiona bien el cambio. Es clave definir y comunicar con claridad:

- Qué tipos de riesgos se van a supervisar.  
- Qué canales entran en el alcance.  
- Quién tendrá acceso a las alertas y bajo qué controles.  
- Cómo se protege la privacidad de los empleados.

Una comunicación interna transparente antes del despliegue, acompañada de políticas accesibles y formación, reduce la resistencia y posiciona la supervisión como un mecanismo de protección tanto para la organización como para las personas.

---

### Nota 3. Relevancia regulatoria para el sector financiero en LATAM

Communication Compliance ayuda a cubrir requerimientos regulatorios de supervisión de comunicaciones para entidades financieras en Latinoamérica, de forma similar a otros mercados altamente regulados.  

El soporte de conectores para plataformas de mensajería financiera como Bloomberg y Reuters, junto con la supervisión de Teams y Exchange, permite consolidar la revisión de comunicaciones en una única herramienta, facilitando evidencias para auditorías y reguladores.

---

### Nota 4. El factor humano como riesgo irreductible

El factor humano es un vector de riesgo que ningún control técnico elimina por completo. Una gestión sólida del riesgo interno combina:

- Tecnología de detección y supervisión como IRM y Communication Compliance.  
- Cultura organizacional orientada a la ética y al cumplimiento.  
- Formación continua en manejo de información sensible y conducta digital.  
- Canales seguros de reporte de incidentes y preocupaciones.

Las capas tecnológicas y las prácticas de gestión deben operar de forma complementaria. La tecnología reduce la superficie de riesgo y aporta evidencia; la cultura y el liderazgo influyen en las motivaciones y comportamientos que, en última instancia, determinan la aparición de incidentes internos.
