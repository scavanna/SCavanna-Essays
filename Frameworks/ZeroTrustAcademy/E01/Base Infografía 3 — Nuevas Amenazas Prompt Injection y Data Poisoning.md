# Nuevas Amenazas

## Prompt Injection, Data Poisoning y Movimiento Lateral entre Agentes

La adopción de inteligencia artificial agéntica no solo amplía la superficie de ataque existente. Introduce vectores de amenaza genuinamente nuevos que los marcos de seguridad tradicionales no contemplaban. Prompt injection, data poisoning y movimiento lateral entre agentes representan una categoría de riesgo que requiere controles específicos, distintos de los que protegen a usuarios humanos o aplicaciones convencionales.
Entender estas amenazas no es un ejercicio académico. Microsoft ha documentado incidentes simulados en su AI Red Team que demuestran cómo estas técnicas pueden ser explotadas con efectividad en entornos de producción real.

## Prompt Injection: Más que un Problema de Texto

El prompt injection es frecuentemente descripto como un problema de generación de contenido incorrecto. Esa descripción subestima radicalmente el riesgo. Cuando un agente tiene acceso a herramientas críticas —APIs internas, sistemas de archivos, bases de datos, capacidades de ejecución de código— una instrucción maliciosa inyectada en su contexto puede convertirse en un vector de ataque con consecuencias operativas reales.
El mecanismo es directo: un actor malicioso introduce instrucciones en el contenido que el agente procesa —un correo electrónico, un documento, una página web, un resultado de búsqueda— y esas instrucciones anulan o modifican el comportamiento original del agente. El agente no fue hackeado en el sentido técnico tradicional: simplemente siguió instrucciones que parecían legítimas dentro de su contexto de ejecución.

### **ESCENARIOS DE PROMPT INJECTION**

1. Documento con instrucciones ocultas: un agente que analiza contratos recibe un documento que contiene, en texto invisible o al final del archivo, instrucciones para enviar el contenido a una dirección externa.
2. Correo de phishing con payload en el cuerpo: un agente de triage recibe un mail cuyo cuerpo contiene, junto con el contenido de phishing visible, instrucciones para reclasificarlo como legítimo y liberar el dominio del remitente.
3. Resultado de búsqueda envenenado: un agente que investiga en internet recibe un resultado que contiene instrucciones para persistir una instrucción en su memoria de largo plazo.
4. Instrucción en botón de resumen: Microsoft documentó más de 50 prompts de envenenamiento en botones «Resumir con IA» en sitios web, que intentan inyectar instrucciones de persistencia cuando un usuario hace clic.


"Microsoft acuñó el término «double agents» para describir agentes manipulados —mediante prompt injection o envenenamiento de modelos— que actúan en contra de los intereses de la organización que los desplegó."

## Data Poisoning: Envenenar la Fuente de Decisiones

El data poisoning opera en una capa diferente: no manipula las instrucciones del agente en tiempo de ejecución, sino los datos sobre los que el agente basa sus decisiones. Si un actor logra introducir información corrupta o deliberadamente sesgada en los datasets, bases de conocimiento o fuentes de contexto que alimentan al agente, puede influir en las decisiones que ese agente toma sin necesidad de acceder directamente a su código o configuración.
Esta amenaza introduce una dimensión que Zero Trust no necesitaba contemplar en el mundo humano. Con usuarios, bastaba con controlar a qué recursos podían acceder. Con agentes, hay que controlar también qué información están consumiendo, porque esa información puede modificar su comportamiento de forma tan efectiva como una instrucción directa.

DATA POISONING / PROMPT INJECTION
Afecta los datos de entrenamiento o contexto / Afecta el contexto de ejecución en tiempo real
Modifica comportamiento de forma persistente / Modifica comportamiento en una sesión específica
Difícil de detectar sin baseline de comportamiento / Detectable con análisis de inputs y outputs
Requiere controles en el pipeline de datos / Requiere sanitización de inputs y prompt shields
Ejemplo: sesgar un modelo de clasificación de riesgo crediticio / Ejemplo: inyectar instrucciones en un correo procesado por un agente

## Movimiento Lateral entre Agentes

En arquitecturas multiagente —donde un agente orquestador coordina agentes especializados— aparece un riesgo análogo al movimiento lateral en redes tradicionales. Si un agente es comprometido, puede ser usado para atacar a otros agentes del mismo ecosistema, especialmente si comparten contexto, memoria o credenciales.
El principio de segmentación debe aplicarse también a las comunicaciones entre agentes: no todos los agentes deberían poder comunicarse con todos los demás. Las relaciones de confianza entre agentes deben ser explícitas, definidas por propósito, y verificadas de la misma forma que se verifica el acceso de un usuario a un sistema.

### **CONTROLES PARA MOVIMIENTO LATERAL ENTRE AGENTES**

- Registro explícito de relaciones entre agentes: documentar qué agente puede invocar a qué otro y bajo qué condiciones.
- Credenciales independientes por agente: evitar que el compromiso de un agente exponga las credenciales de otros.
- Validación de identidad en cada llamada entre agentes: no asumir que una solicitud interna es confiable por el hecho de ser interna.
- Segmentación lógica del ecosistema de agentes por dominio de negocio o nivel de criticidad.

## Zero Trust Debe Extenderse al Pipeline Completo de Datos

La conclusión operativa de este análisis es clara: Zero Trust para agentes no puede limitarse a controlar el acceso a recursos. Debe extenderse al pipeline completo de datos que alimenta las decisiones del agente. Esto incluye las fuentes de contexto, los documentos que procesa, los resultados de herramientas externas que consume, y las instrucciones que recibe tanto de usuarios como de otros sistemas.
En términos prácticos, esto requiere aislamiento de herramientas, sanitización y validación de inputs, monitoreo de outputs, y políticas explícitas sobre qué fuentes de datos son confiables para cada agente. Es, como alguien señaló acertadamente, una versión modernizada de SQL injection: el vector de ataque cambia, la lógica de defensa es análoga.

# Productos Microsoft que materializan este principio:

Prompt Shields (Entra)
Protección contra prompt injection a nivel de red, aplicada a todas las apps y agentes sin necesidad de cambios en código. Habilitado en AI Gateway.


Microsoft Defender for AI
Detección de amenazas específicas a sistemas de IA incluyendo prompt injection, model poisoning y comportamiento anómalo de agentes.


Microsoft Purview SDK
Controles de seguridad de datos y compliance para apps de IA custom, agentes en Azure AI Foundry y Copilot Studio, previniendo oversharing de datos.


AI Security Posture Mgmt
Recomendaciones de postura de seguridad y alertas de protección en tiempo real integradas directamente en Azure AI Foundry.

>
> "No se trata solo de controlar a qué recursos accede el agente. Hay que controlar qué información está consumiendo, porque esa información puede modificar sus decisiones."
>

 [Microsoft Security CSU MCSA - ZeroTrust Academy (E01) Zero Trust en la Era de los Agentes de IA + Ticket de salida.md](https://github.com/scavanna/SCavanna-PoV/blob/main/ZeroTrustAcademy/E01/Microsoft%20Security%20CSU%20MCSA%20-%20ZeroTrust%20Academy%20(E01)%20Zero%20Trust%20en%20la%20Era%20de%20los%20Agentes%20de%20IA%20%2B%20Ticket%20de%20salida.md)