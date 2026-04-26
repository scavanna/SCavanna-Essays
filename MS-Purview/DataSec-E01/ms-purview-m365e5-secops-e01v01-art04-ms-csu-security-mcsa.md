---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Art04-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-art04-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Art04-MS_CSU_Security_MCSA



El texto describe el marco de Information Protection en Microsoft 365 y su relación con etiquetas de sensibilidad, clasificación automática y cifrado persistente. Explica cómo funcionan los tres vectores de clasificación, los tipos de información confidencial, la coincidencia exacta de datos y las entidades nombradas. También detalla el alcance del etiquetado en archivos, contenedores y reuniones, junto con el modo de simulación para autoetiquetado. Finalmente, presenta el cifrado basado en Azure Information Protection y Rights Management Service y ofrece notas prácticas sobre licenciamiento, clientes y diseño de taxonomías. Está orientado a responsables de seguridad, cumplimiento y arquitectura de información empresarial.

El primer bloque presenta tres vectores de clasificación que marcan la madurez. La clasificación manual depende del criterio y la disciplina del usuario, y es el punto de partida. Después, la clasificación automática por reglas usa tipos de información confidencial para detectar patrones en el contenido. Por último, los clasificadores entrenables con aprendizaje automático, disponibles en la edición E5, reconocen patrones semánticos y estructurales. Permiten etiquetar volúmenes con menor intervención humana y resultados muy consistentes.

El segundo bloque explica cómo mejorar la precisión mediante Exact Data Match y entidades nombradas. Exact Data Match compara el contenido con un conjunto cifrado de datos reales de la organización y detecta solo coincidencias exactas. Así reduce de forma drástica los falsos positivos en números de cuenta, identificadores internos o listados de clientes. Por otro lado, las entidades nombradas permiten reconocer nombres, direcciones y otras combinaciones que constituyen datos personales identificables según el contexto.

El tercer bloque amplía la mirada y describe la geometría de la cobertura. Explica el etiquetado de documentos individuales, como archivos de Office, correos y PDF, donde la etiqueta viaja con el archivo. Además, presenta el etiquetado de contenedores en Teams, sitios de SharePoint y grupos, que heredan políticas comunes. Incluye las etiquetas por defecto en bibliotecas, el etiquetado de reuniones y las marcas de agua dinámicas como elementos clave de gobierno preventivo de seguridad.

El cuarto bloque se centra en el autoetiquetado y en cómo desplegarlo sin disrupción. El modo de simulación permite ejecutar una política durante unos días, registrar qué habría etiquetado y revisar ejemplos antes de afectar a los usuarios. Con esos datos, el equipo ajusta el umbral de confianza, equilibrando cobertura y falsos positivos. Este enfoque iterativo reduce riesgos operativos y sustenta el valor de las capacidades avanzadas incluidas en la edición E5 de Microsoft 365.

El quinto bloque aborda el cifrado persistente con Azure Information Protection y Rights Management Service como última línea de defensa. Las etiquetas pueden activar cifrado AES y derechos de uso que viajan con el archivo, incluso fuera de la organización, mientras las claves sigan vigentes. Además, se describe Customer Key, que sitúa la clave maestra en Azure Key Vault. Como soporte, el texto destaca la simplicidad en la taxonomía, el cliente y la cobertura móvil.









# Information Protection en Microsoft 365: conceptos clave y notas de soporte

## Nivel 2: Conceptos clave

### 1. Los tres vectores de clasificación: del instinto humano a la inteligencia artificial

Information Protection ofrece tres mecanismos de clasificación complementarios y progresivos en sofisticación. La madurez de implementación de una organización se mide, en parte, por hasta qué vector ha llegado en su despliegue. [`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels`](https://learn.microsoft.com/es-es/microsoft-365/compliance/sensitivity-labels)

#### 1.1 Vector 1: Clasificación manual (disponible en E3 y E5)

El usuario decide conscientemente qué etiqueta aplica al documento o correo que está creando o modificando. Este enfoque requiere:

- Criterio humano
- Formación en el esquema de clasificación
- Disciplina organizacional

Es el punto de partida inevitable; ningún sistema automático puede calibrarse correctamente sin una base de datos de decisiones humanas validadas.  
Su principal limitación es la escala: en una organización de 10.000 empleados produciendo miles de documentos diarios, la clasificación puramente manual genera brechas de cobertura proporcionales a la fatiga y al olvido humano. [`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels`](https://learn.microsoft.com/es-es/microsoft-365/compliance/sensitivity-labels)

#### 1.2 Vector 2: Clasificación automática por reglas / SITs (disponible en E3 y E5)

El sistema analiza el contenido del documento y detecta automáticamente patrones que coinciden con más de 300 Sensitive Information Types (SITs) predefinidos, por ejemplo:

- Números de tarjetas de crédito (con validación por algoritmo de Luhn)
- Números de identificación gubernamental por país
- Registros médicos con terminología clínica
- Datos bancarios con BIC/IBAN
- Decenas de categorías adicionales

Si el contenido supera el umbral de confianza configurado, el sistema aplica la etiqueta correspondiente sin intervención del usuario.

La detección de SITs no es una búsqueda simple de texto: incorpora validación contextual, análisis de proximidad entre elementos (por ejemplo, un número que parece de tarjeta de crédito pero no está cerca de un nombre o fecha de vencimiento reduce su nivel de confianza) y variantes internacionales por país.

Para organizaciones en LATAM, los SITs incluyen variantes para Argentina (CUIL/CUIT, DNI), Brasil (CPF, CNPJ), México (RFC, CURP) y otros países de la región.  
[`learn.microsoft.com/en-us/microsoft-365/compliance/sensitive-information-type-entity-definitions`](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitive-information-type-entity-definitions?view=o365-worldwide#how-sensitive-information-types-work)

#### 1.3 Vector 3: Clasificación automática por machine learning / clasificadores entrenables (exclusivo E5)

Este vector distingue cualitativamente M365 E5 de E3. Los clasificadores entrenables son modelos de aprendizaje automático capaces de reconocer documentos por su patrón semántico y estructural, no solo por su contenido literal. Esto permite clasificar automáticamente, por ejemplo:

- Contratos legales, incluso si no contienen datos personales obvios
- Nóminas y documentos de RRHH, por su formato y vocabulario específico
- Informes financieros internos, por su estructura y terminología
- Correspondencia de fusiones y adquisiciones, por la naturaleza estratégica de su contenido

Microsoft proporciona clasificadores preentrenados para categorías comunes. Además, las organizaciones pueden entrenar sus propios clasificadores con muestras de documentos internos, de modo que el sistema aprenda a reconocer formularios específicos de la organización que ningún SIT predefinido podría detectar.  
[`learn.microsoft.com/en-us/microsoft-365/compliance/classifier-learn-about`](https://learn.microsoft.com/en-us/microsoft-365/compliance/classifier-learn-about)

---

### 2. Exact Data Match y Named Entities: precisión quirúrgica

Los SITs estándar y los clasificadores entrenables operan sobre patrones genéricos, es decir, detectan cualquier número que parezca un DNI argentino, no específicamente los DNIs de los empleados de una organización concreta. Para escenarios donde la precisión es crítica y los falsos positivos tienen un costo operativo elevado, Information Protection ofrece dos capacidades de detección de alta especificidad.

#### 2.1 Exact Data Match (EDM)

EDM permite detectar datos específicos de la organización mediante comparación contra un dataset real. El proceso funciona así:

1. El equipo de seguridad sube periódicamente un archivo cifrado con los datos reales a proteger (por ejemplo, la lista de números de cuenta de clientes o el directorio de empleados con sus IDs internos).
2. El sistema convierte ese dataset en un índice de hash criptográfico; los datos reales nunca se almacenan en texto plano en el servicio.
3. Cuando Information Protection escanea un documento, compara su contenido contra ese índice de hashes para determinar si contiene alguno de los valores exactos del dataset.

El resultado es una capacidad de detección que reduce drásticamente los falsos positivos para los datos específicos de la organización. Un número que parece un CUIL pero no corresponde a ningún empleado real no genera alerta. Solo los valores exactos presentes en el dataset activan las políticas.  
[`learn.microsoft.com/en-us/microsoft-365/compliance/sit-exact-data-match-learn-about`](https://learn.microsoft.com/en-us/microsoft-365/compliance/sit-exact-data-match-learn-about#how-exact-data-match-works)

#### 2.2 Named Entities

Named Entities es la capacidad de detectar entidades nombradas (personas, organizaciones, lugares, términos médicos) más allá de los patrones estructurales de los SITs. Permite:

- Detectar la presencia de nombres propios en combinación con otros datos
- Identificar combinaciones como nombre + fecha de nacimiento + dirección, que constituyen un registro de persona identificable, incluso si cada elemento por separado no activaría un SIT estándar

Esta capacidad es especialmente relevante para el cumplimiento de regulaciones de privacidad que definen datos personales por su capacidad de identificar a una persona, no solo por su formato.  
[`learn.microsoft.com/en-us/microsoft-365/compliance/sensitive-information-type-entity-definitions`](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitive-information-type-entity-definitions?view=o365-worldwide#named-entities--definition-and-detection)

---

### 3. Alcance del etiquetado: geometría de la cobertura

Un error frecuente en la planificación de implementaciones de Information Protection es asumir que el etiquetado se aplica únicamente a archivos individuales. La cobertura real es más amplia y opera en varias dimensiones simultáneas.

#### 3.1 Etiquetado de contenido individual

Se aplica a:

- Documentos de Office (Word, Excel, PowerPoint)
- PDFs
- Correos electrónicos y sus adjuntos

La etiqueta viaja con el archivo cuando se descarga, se envía o se mueve entre servicios compatibles.

#### 3.2 Etiquetado de contenedores (exclusivo E5)

Las etiquetas pueden aplicarse no solo a archivos individuales sino también a los contenedores que los albergan, como:

- Teams
- Microsoft 365 Groups
- Sitios de SharePoint

Una etiqueta aplicada a un Team define:

- Políticas de privacidad del equipo
- Controles de acceso de invitados externos
- Restricciones de uso compartido para todos los archivos que ese equipo produzca

Es una forma de gobierno por defecto: todo lo que nace en un contenedor etiquetado hereda las políticas del contenedor.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels-microsoft-teams-sites-groups`](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-microsoft-teams-sites-groups?view=o365-worldwide)

#### 3.3 Etiquetado por defecto en SharePoint (exclusivo E5)

Es posible configurar una etiqueta de sensibilidad predeterminada para una biblioteca de SharePoint específica. Cualquier documento que se suba o cree en esa biblioteca recibe automáticamente esa etiqueta sin requerir acción del usuario.

Esto elimina la dependencia de la disciplina individual en contextos donde el nivel de sensibilidad del contenedor es conocido y constante.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels-sharepoint-onedrive-files`](https://learn.microsoft.com/es-es/microsoft-365/compliance/sensitivity-labels-sharepoint-onedrive-files?view=o365-worldwide#use-default-sensitivity-labels-for-document-libraries-in-sharepoint)

#### 3.4 Etiquetado de reuniones online

La etiqueta de sensibilidad puede aplicarse a reuniones de Teams y define:

- Quién puede unirse
- Si se permite la grabación
- Cómo se clasifica la transcripción resultante

Las experiencias completas de etiquetado de reuniones requieren Teams Premium como complemento adicional.  
[`learn.microsoft.com/microsoftteams/meeting-policies-labels`](https://learn.microsoft.com/es-es/microsoftteams/meeting-policies-labels)

#### 3.5 Marcas de agua dinámicas (exclusivo E5)

Los documentos etiquetados pueden incluir marcas de agua que incorporan dinámicamente:

- Nombre del usuario que los abre
- Fecha de acceso
- Etiqueta de clasificación

Este mecanismo tiene un efecto disuasorio importante: si alguien fotografía la pantalla para exfiltrar contenido, la marca de agua identifica exactamente qué usuario tenía abierto el documento en ese momento.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels-files`](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-files?view=o365-worldwide#watermarks-headers-and-footers)

---

### 4. Auto-labeling y Simulation Mode: implementar sin disrupción

El etiquetado automático, especialmente el impulsado por machine learning, implica un riesgo operativo real. Si las políticas están mal calibradas, pueden etiquetar incorrectamente documentos de uso cotidiano, generar fricción para los usuarios y erosionar la confianza en el sistema. Microsoft aborda este problema con un mecanismo de validación previo al despliegue.

#### 4.1 Simulation Mode (modo simulación)

Antes de activar una política de auto-labeling, el administrador puede ejecutarla en modo simulación durante un período configurable (típicamente de 7 a 10 días). En este modo:

- El sistema analiza el contenido existente.
- Registra qué etiquetas habría aplicado.
- No aplica ninguna etiqueta real ni interrumpe ningún flujo de trabajo.

El administrador recibe un informe que muestra:

- Cuántos elementos serían etiquetados por la política
- La distribución de etiquetas que se aplicarían
- Qué elementos específicos se verían afectados (con muestreo para validación manual)
- Cuántos falsos positivos probables existen según el nivel de confianza configurado

Este mecanismo permite calibrar las políticas de auto-labeling con datos reales del entorno de producción antes de activarlas, reduciendo de forma significativa el riesgo de disrupciones operativas.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels-exact-data-match-auto-labeling`](https://learn.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-exact-data-match-auto-labeling?view=o365-worldwide#simulate-auto-labeling-policies-for-edm-sensitive-info-types)

#### 4.2 Umbral de confianza (confidence threshold)

Cada política de auto-labeling tiene un umbral de confianza configurable, es decir, el porcentaje mínimo de certeza con el que el clasificador debe detectar el tipo de contenido para aplicar la etiqueta:

- Un threshold bajo (por ejemplo, 70 %) maximiza la cobertura, pero aumenta los falsos positivos.
- Un threshold alto (por ejemplo, 95 %) minimiza los falsos positivos, pero puede dejar sin etiquetar contenido sensible que el clasificador detecta con menor certeza.

La calibración correcta del threshold es una decisión de riesgo que varía según el caso de uso y el apetito de riesgo de la organización.

---

### 5. Cifrado persistente AIP/RMS: la última línea de defensa

El elemento técnico final y más crítico de Information Protection es el cifrado persistente implementado mediante Azure Information Protection (AIP) sobre Rights Management Service (RMS). El cifrado activado por políticas de etiqueta permite que los permisos viajen con el archivo mientras las claves y políticas estén vigentes y se use un entorno compatible.

#### 5.1 Cómo funciona técnicamente

Cuando se aplica una etiqueta con cifrado activado:

- El servicio RMS gestiona las claves de cifrado necesarias.
- La integración con Azure Key Vault es posible a través de Customer Key, que permite a la organización custodiar sus propias claves maestras.
- No existe necesariamente una clave única por documento en todos los escenarios; la granularidad depende de la configuración y la licencia.
- El algoritmo de cifrado estándar es AES-256.

Las políticas de uso definen:

- **Quién puede descifrar:** usuarios, grupos o dominios autorizados
- **Qué pueden hacer:** abrir, editar, copiar, imprimir, reenviar, según la política de la etiqueta
- **Hasta cuándo:** con opción de fecha de expiración tras la cual el acceso deja de ser posible
- **En qué condiciones:** por ejemplo, requerir autenticación online o uso de dispositivos gestionados

La clave de cifrado no reside en el archivo ni necesariamente en el servidor donde está almacenado el documento. Se gestiona en RMS y, si se usa Customer Key, la organización controla la clave maestra en Azure Key Vault, de modo que Microsoft no puede acceder al contenido cifrado sin esa clave.  
[`learn.microsoft.com/en-us/microsoft-365/compliance/customer-key-overview`](https://learn.microsoft.com/en-us/microsoft-365/compliance/customer-key-overview#customer-key-responsibilities-and-limitations)

#### 5.2 Persistencia del cifrado

El cifrado persiste cuando el archivo se accede en aplicaciones y entornos compatibles, mientras las claves y políticas sigan vigentes. Un archivo enviado fuera de la organización permanece cifrado, aunque puede perder funcionalidad o ser inaccesible si:

- Se abre en entornos no soportados.
- Las claves han caducado o se han revocado.  
[`learn.microsoft.com/es-es/azure/information-protection/rms-client/client-version-release-history`](https://learn.microsoft.com/es-es/azure/information-protection/rms-client/client-version-release-history)

#### 5.3 Relación con Customer Key

Con Customer Key, la organización asume control total de la clave maestra del tenant. Si el cliente retira el acceso a esa clave:

- Microsoft no puede descifrar los documentos protegidos.
- La organización puede utilizar esta capacidad como control adicional de cumplimiento y soberanía del dato.

[`learn.microsoft.com/en-us/microsoft-365/compliance/customer-key-overview`](https://learn.microsoft.com/en-us/microsoft-365/compliance/customer-key-overview#customer-key-responsibilities-and-limitations)

---

## Nivel 3: Notas de soporte e insights de profundidad

### Nota 1. La taxonomía de etiquetas como decisión estratégica

El primer error en muchas implementaciones de Information Protection no es técnico, sino conceptual. Las organizaciones que diseñan taxonomías de etiquetas excesivamente complejas (por ejemplo, 10, 12 o 15 etiquetas con múltiples subetiquetas) generan un sistema que los usuarios no pueden aplicar correctamente, porque exige un nivel de criterio que no es realista para todos los colaboradores.

Las mejores prácticas de Microsoft recomiendan:

- Comenzar con una taxonomía de 4 a 6 etiquetas.
- Definir criterios de clasificación claros e inequívocos.
- Expandir la taxonomía solo cuando la adopción de la base sea sólida.

La simplicidad en el diseño de la taxonomía es un requisito funcional, no una limitación.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels#best-practices`](https://learn.microsoft.com/es-es/microsoft-365/compliance/sensitivity-labels?view=o365-worldwide#best-practices)

---

### Nota 2. Microsoft Purview Information Protection client (antes AIP Unified Labeling client)

Las etiquetas de sensibilidad se administran de forma unificada desde el portal de Purview y se aplican desde los clientes de Office, que deben estar actualizados para tener soporte nativo.

El Microsoft Purview Information Protection client (anteriormente AIP Unified Labeling client) puede instalarse en Windows para extender el etiquetado a formatos de archivo adicionales, incluidos:

- PDFs de terceros
- Archivos no Office

Esta extensión es relevante para organizaciones que trabajan con formatos de archivo especializados fuera del ecosistema Office.  
[`learn.microsoft.com/azure/information-protection/rms-client/unifiedlabelingclient-version-release-history`](https://learn.microsoft.com/es-es/azure/information-protection/rms-client/unifiedlabelingclient-version-release-history)

---

### Nota 3. Etiquetado en aplicaciones móviles y web

El etiquetado no está limitado al cliente de escritorio de Office:

- Las aplicaciones Office para iOS y Android soportan etiquetado de sensibilidad, lo que permite que usuarios móviles vean, apliquen y respeten las etiquetas de los documentos que editan.
- Office Web Apps (la versión en navegador) también soporta visualización y aplicación de etiquetas.

Esta cobertura es especialmente relevante para organizaciones con modelos de trabajo híbrido y alta penetración de dispositivos móviles corporativos.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels#supported-apps`](https://learn.microsoft.com/es-es/microsoft-365/compliance/sensitivity-labels?view=o365-worldwide#supported-apps)

---

### Nota 4. Diferencia E3 vs E5 en contexto de auditoría

Para un CISO que debe justificar la inversión en E5 frente al CFO, el argumento de Information Protection es cuantificable:

- **E3** proporciona etiquetado manual con cifrado básico, suficiente para comenzar pero dependiente de la disciplina del usuario a gran escala.
- **E5** agrega:
  - Auto-labeling con machine learning (incluidos clasificadores entrenables).
  - Aplicación retroactiva de etiquetas al contenido existente (millones de documentos sin clasificar en SharePoint).
  - Etiquetado por defecto en SharePoint.
  - Marcas de agua dinámicas y otras capacidades avanzadas.

La diferencia no es simplemente incremental. E3 protege principalmente los documentos que los usuarios recuerdan etiquetar; E5 permite proteger todos los documentos que contienen información sensible, independientemente del comportamiento del usuario.  
[`learn.microsoft.com/microsoft-365/compliance/sensitivity-labels-licensing`](https://learn.microsoft.com/es-es/microsoft-365/compliance/sensitivity-labels-licensing)
