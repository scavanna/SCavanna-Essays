---
layout: page
title: "MS_Purview_M365E5-SecOps_e01v01_Index-MS_CSU_Security_MCSA"
permalink: /ms-purview/datasec-e01/ms-purview-m365e5-secops-e01v01-index-ms-csu-security-mcsa/
---

# MS_Purview_M365E5-SecOps_e01v01_Index-MS_CSU_Security_MCSA

# Microsoft Purview en M365 E5 / E5 Compliance

## Serie de 10 artefactos

### Índice de artefactos

| #      | Título sugerido                                              | Descripción del bloque                                       |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **01** | **El problema que Purview vino a resolver**                  | Los 4 fallos estructurales del mundo pre-Purview: silos, visibilidad limitada, fragmentación y riesgo normativo. La génesis como imperativo estratégico. |
| **02** | **La gran unificación de abril 2022**                        | La fusión de Azure Purview y M365 Compliance en una sola marca: qué se unió, por qué importa y el Purview Data Map como corazón técnico compartido. |
| **03** | **Arquitectura de los tres pilares funcionales**             | Data Governance, Data Security, Risk and Compliance: estructura, componentes por pilar y la distinción crítica entre capacidades Azure-native y M365-native. |
| **04** | **Information Protection: del dato al cifrado persistente**  | El motor de clasificación y etiquetado: SITs, clasificadores entrenables, EDM, autoetiquetado y la protección que viaja con el archivo. |
| **05** | **DLP avanzado: el brazo ejecutor de las políticas**         | Las siete superficies de control DLP (Exchange, SharePoint, OneDrive, Teams, Endpoint, Power Platform, Cloud Apps), Adaptive Protection y Shadow AI como caso límite. |
| **06** | **Insider Risk y Communication Compliance: la amenaza desde adentro** | Análisis de comportamiento (UBA), correlación de señales, escenarios de riesgo y la capa de supervisión de comunicaciones con diseño centrado en privacidad. |
| **07** | **eDiscovery Premium y Audit Premium: la memoria forense**   | El flujo legal end to end y los logs de alto valor. MailItemsAccessed como evento forense crítico. Diferencias entre E3 y E5 en contexto de litigio. |
| **08** | **E3 vs E5: la regla de oro del licenciamiento**             | Comparativa estructurada de capacidades: control manual frente a inteligencia automatizada. Add-ons modulares para organizaciones que no migran a E5 completo. |
| **09** | **Purview como pilar de datos en Zero Trust**                | Los tres principios Zero Trust implementados por Purview: verificación explícita, menor privilegio y assume breach. Flujo de decisión integrado con Entra ID e Intune. |
| **10** | **El futuro: IA generativa, agentes autónomos y DSPM**       | DSPM para IA, gobernanza de Copilot, herencia de políticas en agentes, integración de Security Copilot con Purview y hoja de ruta 2025-2026. |

### Secuencia temática de la serie

1. Identificación del problema estructural previo a Purview: silos de información, baja visibilidad, fragmentación de controles y exposición normativa.
2. Presentación de Purview como plataforma unificada que reúne gobierno de datos, seguridad y cumplimiento sobre un mapa de datos común.
3. Desglose de la arquitectura en tres pilares funcionales y su diferencia entre componentes nativos de Azure y de M365.
4. Profundización en la protección del dato: clasificación, etiquetado y cifrado persistente mediante Information Protection.
5. Materialización de las políticas en controles DLP avanzados sobre múltiples superficies y escenarios como Shadow AI.
6. Tratamiento de la amenaza interna mediante Insider Risk y Communication Compliance, apoyados en señales de comportamiento y privacidad por diseño.
7. Capacidades forenses y legales con eDiscovery Premium y Audit Premium, con énfasis en eventos de alto valor y diferencias de licencia.
8. Análisis comparativo entre E3 y E5, incluyendo el rol de los add-ons en estrategias de adopción gradual.
9. Integración de Purview como componente de datos dentro de una arquitectura Zero Trust coordinada con Entra ID e Intune.
10. Evolución hacia escenarios de IA generativa, agentes autónomos y DSPM aplicados a Copilot y a la protección de datos en 2025-2026.

### Conexiones entre capacidades

- Adaptive Protection en DLP avanzado se apoya en señales generadas por Insider Risk Management para ajustar dinámicamente la aplicación de políticas según el nivel de riesgo del usuario y su comportamiento reciente.

- Insider Risk y Communication Compliance comparten y correlacionan señales de actividad de usuario, permitiendo pasar de simples alertas basadas en reglas a investigaciones centradas en contexto y patrones de comportamiento.

- El flujo de decisión Zero Trust en torno a los datos utiliza de forma conjunta:
  - Information Protection para clasificar y etiquetar la información sensible y aplicar cifrado persistente.
  - DLP avanzado para controlar la exfiltración y uso indebido del contenido en correo, colaboración, endpoints y aplicaciones en la nube.
  - Insider Risk y Communication Compliance para incorporar señales de riesgo interno y supervisión de comunicaciones en la evaluación continua de confianza.

- La información forense de Audit Premium y los casos de eDiscovery Premium se nutren de las mismas etiquetas y políticas de Purview, lo que permite mantener coherencia entre protección operativa, investigación posterior y respuesta a requerimientos legales.

---
