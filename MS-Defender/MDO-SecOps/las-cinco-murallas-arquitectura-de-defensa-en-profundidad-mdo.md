---
layout: page
title: "Las Cinco Murallas: Arquitectura de Defensa en Profundidad MDO"
subtitle: "Capas de protección, orquestación y opciones de configuración"
description: "Análisis detallado de las 5 capas de Microsoft Defender for Office 365: EOP, Safe Attachments, Safe Links, Anti-Phishing y ZAP"
permalink: /mdo-secops-02-murallas/
author: Santiago Cavanna
date: 2026-03-18
modified: 2026-03-20
tags: [MDO, SecOps, Defensa, Email, Arquitectura]
reading_time: 18
---

## ARQUITECTURA DE LAS CINCO CAPAS

Microsoft Defender for Office 365 (MDO) implementa un modelo de defensa **progresivo y asimétrico**: cada capa utiliza metodologías distintas, obligando al atacante a evadir múltiples técnicas sucesivas.

---

## CAPA 1: Edge / Exchange Online Protection (EOP)

### Descripción
EOP es la primera barrera incluida en todas las licencias Exchange Online. Filtra el volumen más alto del tráfico malicioso con técnicas de bajo costo computacional.

### Tecnologías Activas

#### 1.1 Connection Filtering (Reputación IP)
- **Motor**: Microsoft Threat Intelligence
- **Acción**: Bloqueo de IPs reportadas maliciosas
- **Cobertura**: DNSBL, IP reputation database

#### 1.2 Email Authentication (SPF/DKIM/DMARC)
- **SPF**: Verifica dominio autorizado mediante DNS
- **DKIM**: Verifica firmas criptográficas
- **DMARC**: Indica política ante fallos SPF/DKIM
- **Decisión**: Microsoft combina estos métodos con análisis de reputación y contenido

#### 1.3 Anti-Spam Filtering
- **Motores**: Reglas + clasificador ML
- **Acciones**: Delete, Quarantine, Junk, Prepend subject, Redirect
- **SCL Score**: Rango -1 a 9 permite personalización

#### 1.4 Anti-Malware (Firmas Conocidas)
- **Detección**: Basada en firmas conocidas
- **Actualización**: Cada 1-2 horas
- **Limitación**: No detecta zero-day (para eso existe Safe Attachments)

### Indicadores de Bloqueo
✅ Typosquatting + SPF fail + sin DKIM + adjunto ejecutable conocido → **Bloqueado**

---

## CAPA 2: Safe Attachments

### Descripción
Primera capacidad exclusiva de MDO Plan 1/2. Opera detonando adjuntos en sandbox virtual para observar comportamientos maliciosos antes de entregar al usuario.

### Componentes

#### 2.1 Dynamic Delivery
El usuario recibe el email con el adjunto reemplazado por un placeholder durante el análisis.

#### 2.2 Sandbox Detonation
- Se ejecuta en VM Windows reales
- Monitoreo de APIs, cambios en sistema, conexiones de red
- Tiempo promedio: 3-5 minutos

#### 2.3 Tipos de Archivos
- **Soportados**: Office, PDF, ejecutables, scripts, comprimidos
- **No analizados**: Cifrados, >150MB (acción configurable)

#### 2.4 Acciones Post-Detonación
- **Clean**: Entrega
- **Malicious**: Bloqueo y cuarentena
- **Suspicious**: Configurable
- **Timeout/Error**: Acciones según reportes

### Indicadores de Detección
✅ HTML legítimo que ensambla malware en sandbox → **Bloqueado**

---

## CAPA 3: Safe Links

### Descripción
Permite proteger URLs mediante reescritura y verificación dinámica en el momento del clic, previniendo ataques time-bomb o sitios comprometidos post-entrega.

### Componentes

#### 3.1 URL Rewriting
Transforma URLs originales a proxys de Safe Links con firma de integridad.

#### 3.2 Time-of-Click Verification
Cuando el usuario hace clic:
- Verifica en feeds de amenazas
- Realiza detonation si la reputación es desconocida
- Detecta redirecciones, scripts, forms sospechosos

#### 3.3 Protección Extendida
- Cubre Office, Teams y algunos eventos de terceros vía API
- Mensajes Outlook: reescritura automática
- Office Desktop apps: click-time verification

#### 3.4 Do Not Rewrite Lists
Permite exceptuar URLs internas o socios de confianza (con advertencia operacional).

#### 3.5 Tracking y Reporting
- Registra eventos de clicks
- Genera alertas de seguridad
- Data en Threat Explorer

### Indicadores de Detección
✅ Email sin adjunto, pero Safe Links bloquea phishing link en momento del clic

---

## CAPA 4: Anti-Phishing + Mailbox Intelligence

### Descripción
Analiza contexto, identidad y comportamiento para detectar ingeniería social sofisticada y ataques BEC usando ML.

### Componentes

#### 4.1 Impersonation Protection
- Detecta imitaciones de usuarios clave (C-level, finanzas)
- Algoritmos de similitud
- Listas de usuarios protegidos configurables

#### 4.2 Mailbox Intelligence
- Aprende relaciones normales de cada usuario
- Identifica intentos sospechosos de contacto
- Interacciones inusuales

#### 4.3 Spoof Intelligence
- Cruza autenticación compuesta con comportamiento y contexto
- Muestra insights de remitentes
- Permite decisiones manuales

#### 4.4 Safety Tips
- Banners y advertencias ante señales leves de suplantación
- Primer contacto o caracteres sospechosos
- Educación continua del usuario

### Indicadores de Detección
✅ Remitente con display name del CEO + correo externo → **Marcado con warning**

---

## CAPA 5: Zero-Hour Auto Purge (ZAP)

### Descripción
Defensa retroactiva: elimina emails después de entregados cuando nueva inteligencia detecta amenaza.

### Arquitectura

#### 5.1 Triggers
- Activa cuando surge nueva firma de malware
- Cambio de reputación en URL
- Campaña detectada globalmente

#### 5.2 Tipos de ZAP
- **Phishing ZAP**: Mueve a Junk o Quarantine
- **Malware ZAP**: Va a Quarantine
- **Spam ZAP**: Mueve a Junk

#### 5.3 Condiciones que Previenen ZAP
- Emails movidos por usuario
- Buzones en hold legal
- POP3/IMAP
- Desactivado vía política

#### 5.4 Auditoría
- Registra eventos en Threat Explorer
- Audit Log
- Tablas de PostDeliveryEvents

### Indicadores de Detección
✅ Email entregado, pero horas después reclasificado como malware → **Eliminado retroactivamente**

---

## MATRIZ DE REDUNDANCIA ASIMÉTRICA

| Capa | Metodología | Fortaleza | Debilidad |
|------|-------------|----------|----------|
| **EOP** | Reputación + Firmas | Rápida | No cubre zero-day |
| **Safe Attachments** | Sandbox | Desconocidos | Latencia (~5 min) |
| **Safe Links** | URL dinámica | Time-bomb prevention | Requiere clic |
| **Anti-Phishing** | ML contextual | Ingeniería social/BEC | Requiere baseline |
| **ZAP** | Inteligencia global | Corrige errores previos | Post-compromiso |

---

## FLUJO DE DECISIÓN: EJEMPLO REAL

```
EMAIL INCOMING:
  - Typo en dominio
  - Adjunto macro
  - URL sospechosa
  - Display name: "CEO"

CAPA 1 (EOP):
  ✓ SPF pass (error del atacante)
  → PASA

CAPA 2 (Safe Attachments):
  ✓ Macro detectado en sandbox
  → BLOQUEADO / QUARANTINED

SI NO FUERA BLOQUEADO:

CAPA 3 (Safe Links):
  ✓ URL reescrita
  ✓ Clic en URL sospechosa
  → BLOQUEADO en time-of-click

SI NO FUERA BLOQUEADO:

CAPA 4 (Anti-Phishing):
  ✓ Display name = CEO + dominio externo
  ✓ No es contacto normal del usuario
  → QUARANTINED con advertencia

SI NO FUERA BLOQUEADO:

CAPA 5 (ZAP):
  ✓ Macro es reclasificada 12h después
  → ELIMINADO del inbox
```

---

## CONFIGURACIÓN SEGÚN POSTURA DE RIESGO

### Standard Preset
- Safe Attachments: Dynamic Delivery
- Safe Links: On (track clicks)
- Impersonation: Junk Folder
- Mailbox Intelligence: Enabled
- ZAP Phishing: Enabled

### Strict Preset
- Safe Attachments: Block
- Safe Links: On (track + block)
- Impersonation: Quarantine
- Mailbox Intelligence: Enabled + aggressive
- ZAP Malware: Enabled

### Recomendación SecOps (Regulada)
- Safe Attachments: **Block** (no Dynamic Delivery)
- Safe Links: **On + block + no permitir override**
- Impersonation: **Quarantine**
- Mailbox Intelligence: **Enabled + Priority Accounts**
- ZAP Phishing/Malware: **Enabled (forzado)**

---

## CONCLUSIÓN

Las cinco capas funcionan en **sinergia progresiva**:
1. **Volumen**: EOP reduce ruido
2. **Desconocidos**: Safe Attachments cubre zero-day
3. **Tiempo-clave**: Safe Links previene time-bomb
4. **Contexto**: Anti-Phishing entiende BEC
5. **Retroactiva**: ZAP corrige falsos negativos

**Clave**: Ninguna capa es perfecta. La defensa exitosa es **défense en profundidad + automatización + operación continua**.
