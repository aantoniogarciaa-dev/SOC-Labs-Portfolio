# Informe de Incidente: Phishing de Amazon y Bloqueo Perimetral (Caso #8815)

**Analista:** Antonio García García
**Fecha del Incidente:** 15/02/2026
**ID del Caso:** #8815
**Severidad:** medio
**Estado:** Resuelto (True Positive - Bloqueado)
**Certificación/Ruta:** TryHackMe - SOC Analyst / Blue Team

---

## 📝 Resumen Ejecutivo
Investigación técnica de una campaña de **Phishing** que suplantaba a Amazon mediante una notificación de paquete no entregado. El análisis en el SIEM confirmó la recepción del correo malicioso, pero a diferencia de otros eventos, los logs del firewall demuestran que el intento de acceso a la URL maliciosa fue interceptado y bloqueado con éxito.

---

## 🔍 Investigación y Análisis

### 1. Vector de Ataque Inicial (Email)
Se identificó un correo entrante con indicadores críticos de fraude, utilizando un dominio de baja reputación (`.biz`) y un enlace acortado para ofuscar el destino real.

* **Timestamp:** 15/02/2026 19:09:21.447.
* **Remitente:** `urgents@amazon.biz`.
* **Destinatario:** `h.harris@thetrydaily.thm`.
* **Asunto:** *Your Amazon Package Couldn’t Be Delivered – Action Required*.
* **URL Detectada:** `http://bit.ly/3sHkX3da12340`.

> ![Detalle de la Alerta 8815] <img width="1512" height="405" alt="image" src="https://github.com/user-attachments/assets/3a79baae-ca58-408e-9735-5ef541276a2d" />

> *Figura 1: Alerta generada por el sistema tras detectar el enlace externo sospechoso en el correo inbound.*

---

### 2. Auditoría del Firewall y Bloqueo (Logs)
Tras una revisión exhaustiva de los logs de tráfico web en **Splunk**, se ha verificado que la infraestructura de seguridad actuó de forma preventiva ante el clic del usuario:

* **Acción:** **blocked** (Conexión denegada por el firewall).
* **IP de Origen:** `10.20.2.17`.
* **IP de Destino:** `67.199.248.11`.
* **Regla de Firewall:** `Blocked Websites`.
* **URL Bloqueada:** `http://bit.ly/3sHkX3da12340`.

> ![Log de Bloqueo en Splunk] <img width="1626" height="611" alt="image" src="https://github.com/user-attachments/assets/415ad994-4539-4be9-94b7-4f8aca2e9240" />

> *Figura 2: Registro en Splunk confirmando la interceptación del tráfico hacia el dominio malicioso.*

---

### 3. Entidades Afectadas e IOCs
* **Usuario:** `h.harris@thetrydaily.thm`.
* **Endpoint:** `10.20.2.17`.
* **Indicadores de Compromiso (IOCs):**
    * **Dominio Atacante:** `amazon.biz`.
    * **URL Maliciosa:** `http://bit.ly/3sHkX3da12340`.
    * **IP Maliciosa:** `67.199.248.11`.

---

## 🛡️ Remediación y Escalado
**Clasificación:** **Verdadero Positivo (True Positive)**.
**Escalado:** **No** (Control preventivo efectivo).

Aunque el ataque fue contenido, se han tomado las siguientes medidas para reforzar la postura de seguridad:

### Acciones Realizadas:
* **Validación de Bloqueo:** Se confirmó que no hubo transferencia de datos (0 bytes) hacia el destino malicioso.
* **Limpieza de Buzón:** Eliminación del correo malicioso de la bandeja de entrada de `h.harris` para evitar futuros intentos de acceso.
* **Actualización de Listas Negras:** Inclusión permanente del dominio `amazon.biz` en el filtro de correo corporativo para bloquearlo en la etapa de recepción.

---

## 🛠️ Herramientas y Metodología
* **Splunk (SIEM):** Correlación de logs de correo y registros de red del firewall.
* **Firewall Analysis:** Verificación de acciones de bloqueo (`action: blocked`) y cumplimiento de reglas de filtrado URL.
* **Metodología:** Análisis forense de red basado en el ciclo de vida de respuesta a incidentes del NIST.
