---
layout: page
title: "El Nuevo Perímetro"
permalink: /frameworks/zerotrustacademy/base-infografia-1-el-nuevo-perimetro-identidades-no-humanas/
---



# El Nuevo Perímetro

## Identidades No Humanas en la Era de la IA Agéntica

Durante casi una década, la industria de la ciberseguridad construyó su arquitectura sobre una premisa central: el perímetro de seguridad ya no era la red corporativa, sino la identidad. El mantra de Zero Trust —nunca confiar, siempre verificar— respondía con precisión a ese desplazamiento. Cuando el trabajo dejó de ocurrir en un edificio físico y comenzó a producirse desde cualquier dispositivo y cualquier lugar del mundo, los controles de acceso físico perdieron relevancia y la identidad humana se convirtió en el nuevo borde de defensa.
Hoy ese perímetro vuelve a moverse, y lo hace de forma más radical que en cualquier transformación anterior.

### El Cambio Estructural: De la Identidad Humana a la No Humana

En la era de la inteligencia artificial agéntica, los sistemas ya no se limitan a generar texto, clasificar datos o producir imágenes. Planifican, ejecutan, iteran y toman decisiones de forma autónoma en función de objetivos definidos. Un agente puede recibir la instrucción de optimizar costos en la nube y, a partir de allí, pausar recursos, modificar configuraciones, reorganizar arquitecturas y negociar con APIs externas —sin supervisión directa en cada paso.
Esto introduce una categoría de identidades que el modelo de Zero Trust tradicional no contemplaba: las identidades no humanas. Agentes autónomos, bots, microservicios, pipelines automatizados y workloads de IA actúan en nombre de personas y organizaciones con poder operativo real sobre sistemas críticos. No son usuarios pasivos de infraestructura: son actores con capacidad de decisión y ejecución.

> "El perímetro ya no es la red ni la identidad humana. Es el conjunto de todas las entidades que toman decisiones en nombre de la organización."

### Zero Trust: Tres Principios que No Cambian

Antes de extender Zero Trust a los agentes, conviene precisar qué es: no un producto ni una solución puntual, sino un modelo de arquitectura y una forma de pensar la seguridad. Sus tres principios fundacionales son invariables:
PRINCIPIOS FUNDACIONALES DE ZERO TRUST
Autenticación y autorización explícitas: toda identidad debe ser verificada de forma activa y continua, sin asumir confianza previa.
Mínimo privilegio: el acceso se otorga con los permisos estrictamente necesarios para la tarea en cuestión, nada más.
Asumir compromiso: operar siempre bajo la premisa de que el entorno ya está comprometido y diseñar los sistemas en consecuencia.


Estos principios no cambian con la llegada de los agentes. Lo que cambia es el universo de entidades sobre las que deben aplicarse, y la complejidad de hacerlo de forma continua y a escala de máquina.

### La IA Agéntica No es un LLM Conversacional

Es importante distinguir entre un modelo de lenguaje que genera texto y un sistema de IA agéntica. El primero responde a un prompt y produce un output. El segundo recibe un objetivo, planifica una secuencia de acciones, accede a herramientas —APIs internas, repositorios, sistemas de ticketing, CRM, bases de datos— y ejecuta esas acciones de forma iterativa, adaptándose en tiempo real a los resultados intermedios.
El problema de seguridad no reside en que los agentes sean maliciosos. Reside en que tienen poder operativo real y en que pueden hacer exactamente lo que se les permitió hacer —pero en el contexto equivocado, con datos envenenados, o manipulados por un actor externo.

### El Doble Desafío para los Equipos de Seguridad

Las organizaciones que adoptan IA agéntica enfrentan un desafío que no admite atajos presupuestarios: gestionar simultáneamente los riesgos de la identidad humana y los de las identidades no humanas, generalmente con los mismos recursos y el mismo equipo. La superficie de ataque no crece de forma lineal; crece de forma exponencial con cada agente que se despliega.

- IDENTIDAD HUMANA
- IDENTIDAD NO HUMANA (AGENTE)
- Usuario autenticado con MFA
- Token o API key con ciclo de vida propio
- Acceso basado en rol organizacional
- Acceso basado en propósito funcional
- Comportamiento predecible con desvíos detectables
- Patrones de ejecución a velocidad de máquina
- Credenciales rotadas periódicamente
- Secretos en bóveda, rotación automática
- Sesión con duración razonable
- Ejecución continua, sin sesión definida

### La Respuesta de Microsoft: Zero Trust for AI (ZT4AI)

Esta problemática no es solo teórica. Microsoft formalizó en marzo de 2026 su enfoque Zero Trust for AI (ZT4AI), que extiende los principios probados de Zero Trust al ciclo de vida completo de la IA: desde la ingesta de datos y el entrenamiento de modelos hasta el despliegue y el comportamiento de los agentes en producción. El lanzamiento incluye arquitectura de referencia, taller guiado, herramienta de evaluación y patrones prácticos de implementación.

 [Microsoft Security CSU MCSA - ZeroTrust Academy (E01) Zero Trust en la Era de los Agentes de IA + Ticket de salida.md](https://github.com/scavanna/SCavanna-PoV/blob/main/ZeroTrustAcademy/E01/Microsoft%20Security%20CSU%20MCSA%20-%20ZeroTrust%20Academy%20(E01)%20Zero%20Trust%20en%20la%20Era%20de%20los%20Agentes%20de%20IA%20%2B%20Ticket%20de%20salida.md)
