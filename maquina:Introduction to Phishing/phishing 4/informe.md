# Informe de Incidente: Phishing y Compromiso de Cuenta (Microsoft ATO)

**Analista:** Antonio García García
**Fecha del Incidente:** 15/02/2026
**ID del Caso:** #8817
**Severidad:** media
**Estado:** Resuelto (True Positive - Compromiso Confirmado)
**Certificación/Ruta:** TryHackMe - SOC Analyst / Blue Team

---

## 📝 Resumen Ejecutivo
Investigación técnica de un ataque de **Phishing** mediante suplantación de identidad de Microsoft utilizando la técnica de **Typosquatting**. Tras el análisis en el SIEM, se ha confirmado que un endpoint interno interactuó con el enlace malicioso y que la conexión fue permitida por el Firewall, lo que confirma un compromiso activo del equipo afectado.

---

## 🔍 Investigación y Análisis

### 1. Vector de Ataque Inicial (Email)
El incidente comenzó con un correo entrante que simulaba una alerta de seguridad legítima de Microsoft sobre una supuesta actividad inusual en Nigeria para presionar al usuario.

* **Timestamp del Email:** 15/02/2026 19:11:39.447.
* **Remitente:** `no-reply@m1crosoftsupport.co` (Dominio fraudulento que sustituye la "i" por un "1").
* **Asunto:** *Unusual Sign-In Activity on Your Microsoft Account*.
* **Enlace Malicioso:** `https://m1crosoftsupport.co/login`.

> ![Alerta de Phishing 8817] <img width="1531" height="519" alt="image" src="https://github.com/user-attachments/assets/232f45fd-5659-47fe-9b4b-3e368e8006d2" />

> *Figura 1: Detalle de la alerta de email donde se identifica el dominio de Typosquatting.*

---

### 2. Evidencia de Conexión Exitosa (Network Log)
La investigación reveló que el host del usuario estableció una conexión web con la infraestructura del atacante poco después de recibir el correo:

* **Timestamp de Conexión:** 15/02/2026 19:12:48.447.
* **IP de Origen:** 10.20.2.25.
* **IP de Destino:** 45.148.10.131 (Puerto 443/TCP).
* **Acción del Firewall:** **allowed** (Conexión permitida).
* **Regla Aplicada:** Allow-Internet.

> ![Log de red en Splunk] <img width="1736" height="297" alt="image" src="https://github.com/user-attachments/assets/403b6949-e97e-46b0-b3ac-b3a9042f851e" />

> *Figura 2: Log de red en Splunk confirmando que el tráfico hacia la URL de phishing no fue interceptado por el firewall.*

---

## 🛡️ Remediación y Escalado
**Clasificación:** Verdadero Positivo (True Positive).
**Escalado:** Sí.

### Acciones Realizadas:
* **Aislamiento Preventivo:** Solicitud de aislamiento del host 10.20.2.25 para mitigar riesgos de exfiltración de credenciales.
* **Bloqueo de IoCs:** Inclusión del dominio `m1crosoftsupport.co` en la lista negra (blacklist) de navegación corporativa.
* **Gestión de Identidades:** Notificación inmediata al usuario para el cambio de contraseña de su cuenta de Microsoft y auditoría de accesos recientes.
* **Erradicación:** Eliminación del correo malicioso de todos los buzones de entrada detectados mediante el SIEM.

---

## 🛠️ Herramientas y Metodología
* **Splunk (SIEM):** Correlación de logs JSON de correo y tráfico de red.
* **Análisis de Dominios:** Identificación de técnicas de suplantación de marca y Typosquatting.
* **Metodología:** Seguimiento del ciclo de vida de respuesta a incidentes basado en marcos de trabajo SOC.
