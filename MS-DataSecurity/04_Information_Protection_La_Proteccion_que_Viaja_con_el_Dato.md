# Information Protection: Del Dato al Cifrado Persistente

## Los Tres Vectores de Clasificación — Del Instinto Humano a la Inteligencia Artificial

Information Protection ofrece tres mecanismos de clasificación complementarios y progresivos en sofisticación, según el nivel de licencia:

**Clasificación Manual (E3 y E5)**

- El usuario elige la etiqueta del documento o correo que crea/modifica.
- Requiere criterio humano y formación. Es el punto de partida; automatizaciones dependen de una base validada manualmente.
- Escala limitada: en grandes organizaciones, la cobertura es incompleta por fatiga y olvido humano: https://learn.microsoft.com/en-us/purview/sensitivity-labels

**Clasificación Automática por Reglas/SITs (E3 y E5)**

- El sistema detecta automáticamente patrones de más de 300 Sensitive Information Types (SITs): tarjetas de crédito, identificaciones nacionales, datos médicos, bancarios, etc.
- Validación contextual y por proximidad: refina la confianza, evitando falsos positivos.
- Cobertura para América Latina con SITs propios por país:  [Learn about sensitive information types | Microsoft Learn](https://learn.microsoft.com/en-us/purview/sit-sensitive-information-type-learn-about)

**Clasificación Automática por Machine Learning/Clasificadores Entrenables (solo E5)**

- Reconoce documentos por patrón semántico y estructural, trascendiendo el contenido literal.
- Ejemplos: contratos legales, nóminas, informes financieros o correspondencia estratégica.
- Soporta clasificadores preentrenados y el entrenamiento con muestras de la organización: https://learn.microsoft.com/en-us/purview/classifier-learn-about

------

## Exact Data Match y Named Entities — Precisión Quirúrgica

Para escenarios críticos con alto costo de falsos positivos:

**Exact Data Match (EDM)**

- Detecta datos específicos comparando contra un dataset de referencia cifrado (lista de cuentas de clientes, empleados, etc.) convertido en hashes criptográficos.
- Solo los valores exactos activan las políticas, eliminando falsos positivos a partir de patrones genéricos: 
- [Get started with exact data match based sensitive information types | Microsoft Learn](https://learn.microsoft.com/en-us/purview/sit-get-started-exact-data-match-based-sits-overview)

**Named Entities**

- Reconoce entidades nombradas (personas, organizaciones, lugares, términos médicos), incluso cuando ningún dato individual fuese suficiente para identificar.
- Fundamental para cumplimiento de regulaciones de privacidad basadas en identificabilidad : https://learn.microsoft.com/en-us/purview/sit-learn-about-exact-data-match-based-sits

------

## El Alcance del Etiquetado — Geometría de Cobertura

El etiquetado va más allá del archivo individual y se despliega en varias dimensiones:

**Contenido individual**

- Archivos Office, PDFs, correos y adjuntos. La etiqueta viaja con el archivo en todos sus movimientos.

**Contenedores (E5)**

- Teams, Microsoft 365 Groups, sitios de SharePoint. La etiqueta aplicada al contenedor define políticas globales y hereda a todo el contenido creado allí: https://learn.microsoft.com/en-us/purview/sensitivity-labels-teams-groups-sites

**Etiquetado por defecto en SharePoint (E5)**

- Permite asignar una etiqueta predeterminada para toda una librería. Todo lo que se suba/herede, elimina la dependencia del usuario: https://learn.microsoft.com/en-us/purview/sensitivity-labels-sharepoint-onedrive-files

**Etiquetado de reuniones online**

- Aplica a reuniones Teams: controla acceso, grabación y clasificación de transcripciones. Requiere Teams Premium para experiencia completa: [Use Teams meeting templates, sensitivity labels, and admin policies together for sensitive meetings - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/meeting-templates-sensitivity-labels-policies)

**Marcas de agua dinámicas (E5)**

- Insertan el nombre del usuario, fecha, etiqueta al abrir el documento. Efecto disuasorio, rastreabilidad frente a exfiltraciones vía fotografía de pantalla: https://support.microsoft.com/en-us/office/apply-sensitivity-labels-to-your-files-2f96e7cd-d5a4-403b-8bd7-4cc636bae0f9

------

## Auto-Labeling y el Modo Simulación — Implementar sin Disrumpir

El etiquetado automático requiere calibración fina para evitar perjudicar flujos productivos:

**Simulation Mode**

- Analiza y proyecta qué etiquetas serían aplicadas antes de ejecutar cambios reales. Proporciona reportes de cobertura y falsos positivos, permitiendo validación previa al despliegue: https://learn.microsoft.com/en-us/purview/apply-sensitivity-label-automatically

**Threshold de Confianza**

- Determina el nivel de certeza requerido para aplicar una etiqueta automáticamente.
- Threshold bajo: más cobertura, más falsos positivos. Threshold alto: menos falsos, menor cobertura. La calibración es clave.

------

## El Cifrado Persistente AIP/RMS — Última Línea de Defensa

**Funcionamiento técnico**

- Aplica cifrado con RMS y AES-256 cuando la política de etiqueta lo indica.
- Controla las claves en Azure Key Vault (Customer Key), permitiendo que el cliente mantenga el control total.
- Políticas definen quién accede, qué puede hacer, hasta cuándo y bajo qué condiciones.
- El cifrado persiste al mover el archivo entre ubicaciones o dispositivos si el entorno es compatible; el archivo es ilegible sin clave válida: https://learn.microsoft.com/en-us/purview/customer-key-overview y https://learn.microsoft.com/en-us/azure/information-protection/

**Customer Key**

- Ofrece máxima soberanía: si el cliente retira el acceso, ni Microsoft puede descifrar el contenido.

------

## Notas de Soporte / Insights de Profundidad

- **Taxonomía de Etiquetas:** Se recomiendan 4-6 etiquetas claras, no sistemas excesivamente complejos. La simplicidad es requisito funcional: https://learn.microsoft.com/en-us/purview/get-started-with-sensitivity-labels
- **Unified Labeling Client:** Requiere Office actualizado y, para extender cobertura a formatos no-Office (como PDFs de terceros), se debe instalar el Purview Information Protection client: https://learn.microsoft.com/en-us/azure/information-protection/
- **Aplicaciones móviles y web:** Etiquetado disponible en Office para móviles y versiones web, relevante en modelos híbridos: https://learn.microsoft.com/en-us/purview/sensitivity-labels
- **Diferencia E3 vs. E5 en auditoría:** E3 depende de disciplina humana y políticas básicas; E5 automatiza y amplía cobertura, incluso retroactivamente, mejorando gobernanza y rastreo: https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-purview-service-description