# 🛡️ Informe de Investigación SOC: Phishing de Suplantación de Amazon (Caso #8815)

**Analista:** Antonio García García  
**Fecha de Investigación:** 15/02/2026  
**ID del Incidente:** #8815  
**Severidad:** Media-Alta  
**Estado:** Cerrado (True Positive)  
**Entorno:** TryHackMe SOC Lab - Blue Team

---

## 📝 Resumen Ejecutivo
Este informe documenta la investigación de una alerta de **Phishing** que suplantaba la identidad de Amazon. Mediante la correlación de logs en el SIEM (**Splunk**), se confirmó que un correo malicioso fue entregado y que el sistema de defensa perimetral permitió la conexión saliente hacia la infraestructura del atacante, lo que indica un compromiso potencial del endpoint.

---

## 🔍 Análisis Técnico del Incidente

### 1. Vector de Entrada (Email)
El ataque se originó mediante un correo electrónico entrante diseñado con tácticas de ingeniería social (urgencia por paquete no entregado).

* **Timestamp:** 15/02/2026 19:09:21.447.
* **Remitente:** `urgents@amazon.biz` (Dominio no oficial).
* **Destinatario:** `h.harris@thetrydaily.thm`.
* **Asunto:** *Your Amazon Package Couldn’t Be Delivered – Action Required*.
* **Enlace Detectado:** `http://bit.ly/3sHkX3da12340` (URL acortada para evadir firmas).

> ![Evidencia del Correo Malicioso] <img width="1494" height="499" alt="image" src="https://github.com/user-attachments/assets/7619f7e6-6645-42cb-adec-2a393847d4a5" />
 
> *Figura 1: Captura de la alerta 8815 mostrando el cuerpo del correo y los metadatos del atacante.*

---

### 2. Auditoría del Firewall y Tráfico de Red
Tras analizar los logs de red vinculados al host afectado, se identificó un fallo en la contención automática del Firewall.

* **Acción Realizada:** **`allowed`** (La conexión fue permitida satisfactoriamente).
* **Regla de Firewall:** `Allow-Internet` (Filtro genérico sin restricción de reputación).
* **Endpoint Afectado:** `10.20.2.10`.
* **Destino Externo:** `192.0.2.25` (Puerto 443/TCP).

> ![Logs de Tráfico Splunk] <img width="1730" height="395" alt="image" src="https://github.com/user-attachments/assets/8ea4a726-14c6-4dbb-bd37-6248f6f4093c" />
 
> *Figura 2: Detalle de los logs en Splunk donde se evidencia que el tráfico hacia la URL sospechosa no fue bloqueado.*

---

### 3. Indicadores de Compromiso (IoCs)
| Tipo | Valor |
| :--- | :--- |
| **Dominio Atacante** | `amazon.biz` |
| **URL de Phishing** | `http://bit.ly/3sHkX3da12340` |
| **IP Destino Sospechosa** | `192.0.2.25` |

---

## 🛡️ Remediación y Escalado
**Clasificación:** Verdadero Positivo (True Positive).  
**Escalado:** **Requerido** (Debido a la conexión exitosa del endpoint con el IoC).

### Acciones de Respuesta Ejecutadas:
1. **Contención:** Bloqueo inmediato del dominio `amazon.biz` y la URL `bit.ly` en el Gateway de seguridad corporativo.
2. **Erradicación:** Purga del correo malicioso en el servidor de correo para evitar la propagación a otros usuarios.
3. **Gestión de Identidades:** Notificación al usuario `h.harris` para el cambio de credenciales ante un posible robo de datos (Credential Harvesting).
4. **Mejora Continua:** Revisión de las políticas de firewall para denegar tráfico saliente a TLDs de baja reputación (como `.biz`) y restringir el uso de acortadores de URL.

---

## 🛠️ Herramientas Utilizadas
* **Splunk (SIEM):** Correlación de logs de eventos (`eventcollector`) y logs de firewall.
* **Investigación de Red:** Análisis de protocolos TCP/HTTPS y acciones de políticas de acceso.
* **Metodología:** Gestión de incidentes basada en el ciclo de vida del NIST.

---
