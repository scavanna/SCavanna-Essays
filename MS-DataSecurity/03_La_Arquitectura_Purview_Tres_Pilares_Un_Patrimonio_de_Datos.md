# La Arquitectura de los 3 Pilares Funcionales de Microsoft Purview

### Pilar I: Data Governance — El Sistema de Descubrimiento

El pilar de Data Governance es infraestructural y opera bajo la línea de visibilidad del usuario final. Sus componentes principales:

- **Data Map:** Repositorio dinámico de metadatos que escanea activos en entornos multicloud y on-premises, manteniendo el inventario de datos actualizado https://learn.microsoft.com/en-us/purview/data-map
- **Unified Catalog:** Interfaz de búsqueda y descubrimiento basada en Data Map para localizar activos por lenguaje natural, clasificación o glosario de negocio https://learn.microsoft.com/en-us/fabric/governance/onelake-catalog-govern
- **Data Estate Insights:** Panel ejecutivo que sintetiza la información del Data Map en métricas y tendencias informativas https://learn.microsoft.com/en-us/purview/legacy/concept-insights
- **Data Policy:** Administración centralizada de accesos para fuentes Azure, regulando permisos desde una interfaz unificada https://learn.microsoft.com/en-us/purview/legacy/concept-policies-devops
- **Data Sharing:** Permite compartir activos entre organizaciones sin duplicación, asegurando control propietario [Azure Storage in-place data sharing with Microsoft Purview (preview) | Microsoft Learn](https://learn.microsoft.com/en-us/purview/legacy/concept-data-share)

La operación usual es pay-as-you-go, gestionada por equipos de Data Engineering/Data Architecture y dirigida a Chief Data Officers y Data Stewards. Responde a: “¿Qué datos tenemos, dónde están, cuál es su calidad y quién debe tener acceso?”

------

### Pilar II: Data Security — El Sistema de Protección Activa

El pilar de Data Security es perceptible en controles visibles para usuarios y ejecuta políticas de protección:

- **Information Protection:** Clasifica y etiqueta datos, vinculando controles como cifrado o restricciones de acceso. Funciona en aplicaciones M365 y aplicaciones Office; permite etiquetas automáticas basadas en clasificadores avanzados https://learn.microsoft.com/en-us/purview/sensitivity-labels
- **Data Loss Prevention (DLP):** Controla y bloquea movimientos de datos sensibles en puntos como correo, Teams, endpoints, o aplicaciones cloud de terceros https://learn.microsoft.com/en-us/purview/dlp-learn-about-dlp
- **Insider Risk Management:** Analiza el comportamiento para identificar riesgos internos https://learn.microsoft.com/en-us/purview/insider-risk-management
- **Communication Compliance:** Supervisa comunicaciones para detectar incumplimientos regulatorios o conductuales https://learn.microsoft.com/en-us/purview/communication-compliance
- **Adaptive Protection:** Integra análisis de riesgos y DLP para ajustar restricciones dinámicamente ante señales inminentes https://learn.microsoft.com/en-us/purview/dlp-adaptive-protection-learn

Se licencia por usuario (incluido en M365 E3/E5) y se administra desde `purview.microsoft.com`. La audiencia principal son CISOs y equipos de Seguridad. Pregunta central: “¿Cómo protegemos los datos sensibles para que solo sean accesibles correctamente?”

------

### Pilar III: Risk & Compliance — El Sistema de Demostración

El pilar de Risk & Compliance se ocupa de la evidencia:

- **Compliance Manager:** Gestiona la postura de cumplimiento, incluye evaluaciones para más de 300 marcos regulatorios y genera un Compliance Score https://learn.microsoft.com/en-us/purview/compliance-manager
- **eDiscovery Premium:** Facilita investigaciones y litigios mediante flujos completos de preservación, recopilación y revisión de evidencia [[Learn about eDiscovery | Microsoft Learn](https://learn.microsoft.com/en-us/purview/edisc)](https://www.google.com/search?q=https://learn.microsoft.com/en-us/purview/ediscovery-solutions)
- **Audit Premium:** Ofrece registro forense de actividades con retención prolongada, cubriendo eventos críticos https://learn.microsoft.com/en-us/purview/audit-premium-setup
- **Records Management Advanced:** Gestiona todo el ciclo de vida documental con registros inmutables y pruebas de disposición https://learn.microsoft.com/en-us/purview/records-management
- **Information Barriers:** Mantiene separación regulatoria entre grupos conforme a requisitos https://learn.microsoft.com/en-us/purview/information-barriers

También licenciado por usuario en M365 E3/E5. Dirigido a Compliance Officers, Legal y Auditores. Responde a: “¿Podemos demostrar el cumplimiento y tener evidencia verificable?”

------

### Distinción Operativa Crítica — Azure-Native vs. M365-Native

- **Azure-Native:** Gestión y costeo desde Azure Portal, operado por Data Engineering, enfocado en datos estructurados; incluye Data Map, Unified Catalog, Data Insights, Data Policy y Data Sharing https://learn.microsoft.com/en-us/purview/data-governance-billing
- **M365-Native:** Licencia por usuario, operado por Security & Compliance, enfocado a datos no estructurados colaborativos; cubre Information Protection, DLP, Insider Risk, eDiscovery, Audit y Compliance Manager [https://www.microsoft.com/licensing/guidance/Microsoft-Purview](https://www.google.com/search?q=https://learn.microsoft.com/en-us/purview/compliance-licensing-availability)
- El portal unificado `purview.microsoft.com` presenta ambos modelos; configuraciones avanzadas de gobernanza requieren acceso al portal de Azure.

------

### Flujo de Valor Entre Pilares

La arquitectura recomienda la integración porque los pilares producen señales que los demás consumen:

- **Governance → Security:** El descubrimiento de activos alimenta la aplicación automática de etiquetas en Security.
- **Security → Compliance:** Las etiquetas alimentan controles DLP y pueden convertirse en evidencia o eventos para auditoría y eDiscovery.
- **Compliance → Governance:** Requerimientos detectados originan acciones de mejora que ajustan políticas o escaneos en Governance.

La sinergia de señales transforma Purview de herramienta a sistema adaptativo.

------

## NIVEL 3 — NOTAS DE SOPORTE / INSIGHTS DE PROFUNDIDAD

- **Implementación común fallida:** Implementar primero DLP/Information Protection sin una capa de Data Governance produce cobertura parcial y riesgos operativos no detectados.
- **Impacto del portal unificado:** La fusión de portales fuerza el trabajo colaborativo entre equipos antes aislados; este efecto organizacional es consultivo.
- **Madurez de la arquitectura:** Se pueden identificar niveles: fundamentos (pilares básicos operando), automatización (pilares intercambiando señales), y optimización (sistema autosustentado y con controles dinámicos). Esta taxonomía es consultiva.
- **Racional financiero:** El costo total de operación y mantenimiento se reduce implementando el sistema integrado versus productos aislados; esto corresponde a una opinión consultiva basada en ahorro por integración.

------

## REPRESENTACIÓN VISUAL DEL MODELO

```
┌─────────────────────────────────────────────────────────────────────┐
│  CAPA BASAL — PURVIEW DATA MAP (infraestructura de metadatos)       │
├──────────────────┬──────────────────────┬───────────────────────────┤
│  PILAR I         │  PILAR II            │  PILAR III                │
│  🗺️ Governance  │  🛡️ Security        │  ⚖️ Compliance           │
│  Azure-Native    │  M365-Native         │  M365-Native              │
│  Pay-as-you-go   │  Por usuario (E3/E5) │  Por usuario (E3/E5)      │
│  • Data Map      │  • Info Protection   │  • Compliance Manager     │
│  • Catalog       │  • DLP (Advanced)    │  • eDiscovery Premium     │
│  • Insights      │  • Insider Risk Mgmt │  • Audit Premium          │
│  • Policy        │  • Comm. Compliance  │  • Records Management     │
│  • Sharing       │  • Adaptive Protect. │  • Information Barriers   │
│  Data Eng/COO    │  Security            │  Compliance/Legal         │
│  “¿Qué datos?”   │  “¿Cómo proteger?”   │  “¿Demostrable?”          │
├──────────────────┴──────────────────────┴───────────────────────────┤
│  FLUJO CIRCULAR DE VALOR:                                            │
│  Governance ↔ Security ↔ Compliance ↔ Governance                    │
└─────────────────────────────────────────────────────────────────────┘
```

------

**Referencias**

- https://learn.microsoft.com/en-us/purview/

- https://learn.microsoft.com/en-us/purview/purview

  