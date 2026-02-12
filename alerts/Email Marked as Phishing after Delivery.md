🚨 SIEM Alert Report

<img width="1836" height="474" alt="image" src="https://github.com/user-attachments/assets/5d694c13-0fcf-4afd-a5a5-fdae30e734c2" />


1️⃣ Alert Overview

Alert Name: Email Marked as Phishing after Delivery

Severity: Medium

Date & Time: March 27th, 2025 – 19:25

Source IP: Not available in provided logs

Destination IP: Not available in provided logs

Hostname: Not available in provided logs

User: Eddie Huffman (IT Manager) – e.huffman@tryhackme.thm

Event ID: Not available in provided logs
SIEM / Tool: Not available in provided logs

2️⃣ Executive Summary

Se detectó una alert clasificada como phishing post-delivery, indicando que el correo fue entregado inicialmente y posteriormente identificado como malicioso tras un análisis automatizado. El mensaje simulaba provenir de “Microsoft Support” y utilizaba un asunto con carácter urgente relacionado con un incremento del 600% en precios de Microsoft Teams.

Los controles de autenticación SPF y DKIM fallaron, sugiriendo posible sender spoofing. Además, el correo incluía un archivo adjunto comprimido en formato .rar, técnica comúnmente utilizada para evadir controles de seguridad y facilitar la entrega de malware.

El contenido del mensaje presenta claras tácticas de social engineering orientadas a generar urgencia e inducir la descarga del archivo adjunto. El riesgo potencial incluye malware execution, compromiso del endpoint y posible acceso inicial a la red corporativa.

3️⃣ Technical Analysis
🔎 Trigger Event

La alert fue generada cuando el sistema de seguridad marcó el correo como phishing tras su entrega al buzón del usuario. La detección posterior sugiere análisis heurístico o reputacional.

📧 Email Analysis

Subject: “Important Update: Microsoft Teams Pricing Increase”

Body Keywords: “600% price increase”, “urgent notice”, “download the report”

Sender: Microsoft Support <support@microsoft.com>

Authentication Checks:

SPF: Fail

DKIM: Fail

El fallo simultáneo de SPF y DKIM indica alta probabilidad de spoofed sender domain.

📦 Attachment Analysis

File Name: REPORT.rar

Tipo de archivo: Compressed archive (.rar)

El uso de archivos comprimidos .rar es una técnica común para:

Evadir email filtering engines

Ocultar payloads maliciosos

Entregar ejecutables o scripts encubiertos

No se proporcionan hashes ni análisis de contenido interno del archivo.
Binary architecture: Not available in provided logs.

🎯 Indicators of Compromise (IOCs) Identified

Email spoofing indicators (SPF/DKIM failure)

Social engineering tactics (urgency + financial impact narrative)

Suspicious compressed attachment

🧠 Observed Behavior Pattern

La actividad observada corresponde a una fase potencial de:

Initial Access mediante phishing email

Posible Execution si el usuario abre el archivo comprimido

Riesgo de descarga o ejecución de payload malicioso

🗺 MITRE ATT&CK Mapping

Tactic: Initial Access

Technique: T1566 – Phishing

Posible extensión si el adjunto contiene script o ejecutable:

T1204 – User Execution

🧩 Potential Threat Scenario

Un threat actor envía un correo suplantando a Microsoft con temática financiera crítica para inducir descarga y apertura del archivo adjunto. Una vez ejecutado el payload, podría establecer:

Malware persistence

Credential harvesting

Command & Control (C2)

Lateral movement

No hay evidencia confirmada de ejecución en los logs proporcionados.

4️⃣ Indicators of Compromise (IOCs)

IP addresses: Not available in provided logs
Domains: support@microsoft.com
 (spoofed sender)
File hashes: Not available in provided logs
URLs: None
File names: REPORT.rar
Registry keys: Not available in provided logs

5️⃣ Triage Process

Revisión del encabezado completo del email (header analysis).

Verificación de resultados SPF, DKIM y DMARC.

Análisis de reputación del dominio remitente.

Extracción segura del archivo .rar en entorno sandbox.

Cálculo de hash (MD5/SHA256) del archivo adjunto.

Análisis dinámico y estático del contenido interno.

Revisión de logs EDR del endpoint del usuario para identificar:

File execution

Suspicious processes

Outbound network connections

Verificación de actividad anómala asociada al usuario.

Confirmación de que no hubo interacción o ejecución maliciosa adicional.

6️⃣ Risk Assessment

Classification: True Positive – Malicious

Justificación técnica:

Fallo de SPF y DKIM (indicador fuerte de spoofing)

Uso de técnica clásica de social engineering

Archivo comprimido como posible vector de malware

Mensaje diseñado para generar urgencia

No hay evidencia de compromiso activo, pero el vector es claramente malicioso.

Confidence Level: High

7️⃣ Recommended Actions

✅ Confirmar si el usuario abrió el archivo adjunto.

✅ Realizar EDR full scan en el endpoint afectado.

✅ Aislar el host si se detecta ejecución sospechosa.

✅ Bloquear hashes del archivo en sistemas de seguridad.

✅ Implementar bloqueo del patrón de campaña en el email gateway.

✅ Reforzar políticas DMARC.

✅ Campaña de concienciación sobre phishing para usuarios.

8️⃣ Lessons Learned

Este incidente demuestra la importancia de:

Implementar políticas estrictas de autenticación de correo (SPF/DKIM/DMARC).

Fortalecer análisis sandbox automático de adjuntos comprimidos.

Mantener capacitación continua en detección de phishing.

Reducir ventana entre delivery y post-delivery detection.

La detección fue efectiva, pero ocurrió después de la entrega, lo que representa una oportunidad de mejora en el pipeline de email security.

9️⃣ Incident Classification

Kill Chain Phase: Initial Access

MITRE ATT&CK Technique:
T1566 – Phishing
