---
layout: page
title: "MS_Purview_M365E5-SecOps_e02v01_Art03-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e02/ms-purview-m365e5-secops-e02v01-art03-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e02v01_Art03-MS_CSU_Security_MCSA





Este bloque describe cómo Microsoft Purview Information Protection permite clasificar y proteger información en Microsoft 365 y Azure. Explica el motor de detección de datos sensibles, los clasificadores entrenables y las etiquetas de sensibilidad como eje común. Detalla el etiquetado automático a gran escala y las capacidades exclusivas de Purview Suite para contenedores y reuniones. Además, propone una estrategia de adopción por fases. Finalmente, aborda la integración con aplicaciones externas, soluciones DLP de terceros y otros controles de seguridad corporativa. Su finalidad es alinear la protección de datos con requisitos regulatorios, contratos con terceros y riesgos reales de negocio.

Primero se describe el motor de detección de contenido sensible. Los tipos de información sensible, llamados SIT, usan patrones y validaciones para reconocer tarjetas, documentos nacionales, credenciales o datos de salud. La coincidencia exacta de datos, o EDM, compara valores en una base cifrada con hashes salteados. Por otro lado, las entidades con nombre identifican personas, organizaciones y ubicaciones en texto libre. Juntas reducen falsos positivos y mejoran el contexto semántico. Así preparan el terreno para una clasificación más fiable.

Además se explican los clasificadores entrenables, modelos de aprendizaje automático que aprenden categorías propias de cada organización. Se entrenan con decenas o cientos de documentos positivos y negativos, y se refinan con ciclos de retroalimentación. Gracias a ellos es posible clasificar contenido no estructurado, como contratos o correos, donde no hay patrones fijos. Combinados con el autoetiquetado del servicio, permiten aplicar etiquetas de forma masiva y retroactiva sobre repositorios históricos. Así se alcanza una escala imposible solo con acciones manuales.

Las etiquetas de sensibilidad se presentan como el núcleo operativo de la protección. Cada etiqueta combina marcas visuales, cifrado y configuración de contenedores como equipos o sitios. El cifrado usa la plataforma de derechos de Azure y protege con claves de alta seguridad. Las marcas de agua dinámicas incorporan nombre de usuario y fecha al abrir el documento. En Purview Suite, el etiquetado se extiende a equipos, grupos, bibliotecas y reuniones de Teams.

El documento propone una implantación por fases para crear cultura de clasificación. Primero se define una taxonomía sencilla de etiquetas alineada con normas y contratos. Después se habilita el etiquetado manual con avisos de política, que educan al usuario y permiten ajustar el modelo. Más tarde se activan políticas automáticas en modo simulación y luego en producción. En paralelo se revisa la compatibilidad con archivos PDF, aplicaciones externas y destinatarios sin identidad en Azure.

Finalmente se explica cómo las etiquetas se integran en un ecosistema más amplio de seguridad. Las soluciones DLP de otros fabricantes pueden leer las etiquetas como metadatos y usarlas en sus propias reglas. Al mismo tiempo, las etiquetas sirven de base para las políticas DLP nativas, las barreras de información entre equipos y el cifrado reforzado con claves del cliente. También alimentan el mapa de datos en Azure, alineando riesgo entre Microsoft 365 y la nube.





# Bloque 3/11: Information Protection, clasificación y etiquetado

## Nivel 2: Conceptos clave

| Concepto                                                     | Explicación / Data                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Motor de detección: SIT, EDM y Named Entities**            | El motor de detección de contenido sensible combina tres mecanismos. Los **Sensitive Information Types (SIT)** predefinidos son más de 300 patrones basados en expresiones regulares y funciones de validación que cubren, entre otros, números de tarjeta de crédito, identificadores nacionales de múltiples países, credenciales de autenticación, datos de salud, información financiera regulada y secretos de servicios cloud. Cada SIT tiene niveles de confianza configurables (alta, media, baja) y puede combinarse en condiciones compuestas para reducir falsos positivos. El **Exact Data Match (EDM)** se centra en datos reales de la organización: en lugar de detectar cualquier número con formato de DNI, detecta solo los DNIs presentes en una base de datos corporativa, usando coincidencia sobre valores hash salteados para que Microsoft no reciba los datos en claro. Esto permite políticas de alto valor, por ejemplo sobre padrones de millones de clientes, con falsos positivos mínimos. Las **Named Entities** son clasificadores que identifican nombres de personas, organizaciones, ubicaciones y términos médicos en texto libre, incluso cuando no siguen un patrón estructurado, aportando una capa semántica que complementa a SIT y EDM. |
| **Clasificadores entrenables (Trainable Classifiers)**       | Los clasificadores entrenables son modelos de machine learning que aprenden categorías de contenido específicas de la organización a partir de ejemplos. Microsoft proporciona modelos preconstruidos para casos habituales como currículums, estados financieros, código fuente, contenido de recursos humanos, acoso laboral y discriminación. Además, cada organización puede definir clasificadores personalizados para categorías propias, por ejemplo contratos de distribución, informes de due diligence, comunicaciones de trading o historiales clínicos en formato no estructurado. El entrenamiento inicial requiere en torno a 50 a 500 documentos positivos (que pertenecen a la categoría) y un conjunto de negativos, seguido de ciclos de retroalimentación para refinar precisión y cobertura. Estos clasificadores están disponibles solo en Purview Suite, lo que resulta clave porque sin ellos la clasificación automática del contenido propietario no estructurado, que suele concentrar el mayor valor informacional, resulta prácticamente inalcanzable. |
| **Sensitivity labels: estructura y capacidades**             | Una sensitivity label es un objeto de configuración que agrupa tres tipos de controles. Primero, **marcas visuales**: encabezados, pies de página y marcas de agua, estáticas o dinámicas, que pueden incorporar datos del usuario y marcas temporales. Segundo, **cifrado**: si está activado, define quién puede descifrar el archivo, qué permisos tiene (solo lectura, edición, reenvío, impresión) y durante cuánto tiempo, incluyendo expiración o revocación de acceso. Tercero, **configuración de contenedores**: privacidad de Teams y Sites, reglas de acceso externo y restricciones para dispositivos no gestionados. Las marcas de agua dinámicas, disponibles en Purview Suite, insertan automáticamente el nombre del usuario y la fecha de acceso en el momento de apertura del documento, creando trazabilidad visual independientemente del flujo previo del archivo. El cifrado se basa en Azure Information Protection y Azure Rights Management: el contenido se cifra con AES 256 y la clave de descifrado solo se concede si la identidad, el dispositivo y el contexto de acceso cumplen la política definida. |
| **Auto labeling: la diferencia de escala**                   | El etiquetado automático es lo que permite pasar de una clasificación manual limitada a un esquema operativo para decenas de miles o millones de documentos. Existen dos modalidades. El **auto labeling del lado del cliente** aplica etiquetas en las aplicaciones de Office cuando el usuario crea o modifica un documento, según el contenido detectado. El **auto labeling del lado del servicio** se configura en el portal de Purview y escanea contenido en reposo en SharePoint y OneDrive, y en tránsito en Exchange, aplicando etiquetas sin intervención del usuario. Combinado con clasificadores entrenables, el motor del lado del servicio puede etiquetar retroactivamente grandes repositorios heredados donde la mayoría de los documentos carece de clasificación previa, un porcentaje que suele situarse entre el 60 y el 80 por ciento del inventario. Sin este enfoque automatizado, etiquetar repositorios de millones de documentos resulta inviable en términos operativos. |
| **Alcance extendido del etiquetado (Purview Suite exclusivo)** | Purview Suite amplía el alcance de las sensitivity labels más allá del archivo individual. El **etiquetado de contenedores** aplica una etiqueta a un Team, un Microsoft 365 Group o un sitio de SharePoint, de forma que la configuración de privacidad, el acceso externo y las restricciones a dispositivos no gestionados se derivan automáticamente de esa etiqueta. El **etiquetado por defecto de bibliotecas de SharePoint** garantiza que todos los documentos cargados en determinadas bibliotecas reciban una etiqueta preconfigurada, cerrando la brecha de contenido nuevo sin clasificar. El **etiquetado de reuniones de Teams**, integrado con Teams Premium, permite clasificar reuniones con una sensitivity label que controla qué grabaciones se permiten, qué restricciones se aplican a participantes externos y si se habilita o no la transcripción automática. Las **marcas de agua dinámicas** en documentos etiquetados aportan trazabilidad de acceso por usuario en el momento de apertura. |

---

## Nivel 3: Notas de soporte

### Fases recomendadas de implementación

En organizaciones sin una cultura previa de clasificación, la adopción de Information Protection requiere un enfoque por etapas:

1. **Definir la taxonomía de etiquetas**  
   Normalmente se recomienda entre 4 y 6 niveles, desde Público hasta Altamente confidencial o Restringido, con subetiquetas para ámbitos específicos como Legal o Recursos Humanos. Es clave mapear esta taxonomía a los requisitos normativos y contractuales de la organización.

2. **Desplegar etiquetado manual con ayudas de política**  
   Se habilitan las etiquetas para que el usuario seleccione manualmente y se configuran consejos de política (policy tips) que orientan al usuario cuando selecciona una etiqueta inadecuada o cuando se detecta contenido sensible sin etiquetar. Esta fase se centra en educación del usuario y ajuste fino de la taxonomía.

3. **Activar auto labeling en modo simulación**  
   Antes del enforcement, las políticas de auto labeling se ejecutan en modo simulación. Esto permite observar qué documentos se etiquetarían, revisar estadísticas de impacto e identificar patrones de falsos positivos o falsos negativos sin modificar todavía el contenido.

4. **Pasar a auto labeling en enforcement progresivo**  
   Una vez calibradas las políticas, se activa el enforcement de forma gradual, por tipo de carga de trabajo: primero sobre Exchange (correo en tránsito), después sobre SharePoint y OneDrive (contenido en reposo) y finalmente sobre dispositivos endpoint. Esta progresión limita el riesgo de interrupciones operativas y facilita la corrección rápida de configuraciones inadecuadas.

La fase de simulación es especialmente crítica para evitar que una política excesivamente amplia etiquete como confidenciales grandes volúmenes de contenido público, lo que generaría ruido operativo, ralentización de procesos y pérdida de confianza en el esquema de clasificación.

### Integración con aplicaciones no Microsoft

La aplicación del cifrado basado en sensitivity labels a archivos y flujos de trabajo fuera del ecosistema Microsoft exige revisar compatibilidades:

- **Archivos PDF**  
  El cifrado aplicado mediante AIP está soportado en Adobe Acrobat y Adobe Reader cuando se instala el complemento de Azure Information Protection. Sin este complemento, el usuario puede encontrar archivos ilegibles aunque tenga derechos de acceso teóricos.

- **Destinatarios externos sin Azure AD**  
  Cuando el destinatario no pertenece a un tenant de Azure AD, el acceso a contenido cifrado se gestiona mediante el portal de mensajes cifrados de Microsoft. El usuario recibe un correo con un enlace seguro y se autentica mediante un código de verificación enviado a su propio correo o mediante un proveedor de identidad compatible. Es necesario incorporar estos flujos en los procedimientos de colaboración con terceros para evitar fricción.

### Coexistencia con soluciones DLP de terceros

Muchas organizaciones operan en escenarios de transición donde Purview Information Protection convive con sistemas DLP de fabricantes como Symantec, Forcepoint o Digital Guardian:

- Las **sensitivity labels** se exponen como metadatos accesibles por soluciones de terceros a través del cliente AIP, sus SDK y las API de Microsoft Graph.
- Esto permite que las plataformas DLP externas utilicen el nivel de sensibilidad como un atributo más en sus políticas de inspección y respuesta, por ejemplo para bloquear la exfiltración de documentos etiquetados como Confidencial o para aplicar reglas diferenciadas según la etiqueta.
- Un diseño consistente debe definir qué motor actúa como autoridad principal para la clasificación y cómo se sincronizan las decisiones de política entre Purview y las demás soluciones de seguridad de contenido.

### Conexiones con otros controles

Las sensitivity labels no solo protegen el contenido de forma aislada, sino que actúan como base para otros bloques de control:

- Son un **prerrequisito técnico para las políticas DLP** desarrolladas en el Bloque 4 ya que muchas reglas se apoyan en el nivel de sensibilidad del contenido.
- El **etiquetado de contenedores** se relaciona directamente con **Information Barriers** descrito en el Bloque 6, ya que la combinación de ambas capacidades permite segmentar equipos y repositorios en función de políticas de separación organizativa o de asesoramiento.
- El **cifrado administrado con Customer Key**, tratado en el Bloque 8, se añade como capa sobre el cifrado gestionado por las sensitivity labels, aportando control adicional sobre las claves raíz.
- El **Data Map** del Bloque 2 aprovecha la misma taxonomía de etiquetas para clasificar activos en la infraestructura de Azure, alineando la visión de riesgo entre datos en Microsoft 365 y cargas de trabajo en la nube.
