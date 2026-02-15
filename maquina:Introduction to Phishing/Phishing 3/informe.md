# 🛡️ Informe de Investigación SOC: Bloqueo de URL en Lista Negra (Caso #8816)

**Analista:** Antonio García García  
**Fecha del Incidente:** 15/02/2026  
**ID del Caso:** #8816  
**Severidad:** Alta  
**Estado:** Resuelto (Verdadero Positivo - Bloqueado)  
**Certificación/Ruta:** TryHackMe - SOC Analyst / Blue Team

---

## 📝 Resumen Ejecutivo
Investigación de una alerta de alta prioridad disparada cuando un endpoint interno intentó acceder a una URL externa listada en la **blacklist** de inteligencia de amenazas de la organización. En este incidente, el Firewall interceptó y bloqueó la conexión saliente con éxito, evitando un posible compromiso del sistema.

---

## 🔍 Investigación y Análisis

### 1. Detalle de la Alerta
El sistema de monitoreo detectó el intento de navegación hacia un dominio malicioso, activando la regla de seguridad perimetral de forma automática.

* **Timestamp:** 15/02/2026 19:10:35.447.
* **Tipo de Alerta:** Access to Blacklisted External URL Blocked by Firewall.
* **URL de Destino:** `http://bit.ly/3sHkX3da12340`.
* **IP de Destino:** `67.199.248.11` (Puerto 80/TCP).

> ![Alerta de Firewall 8816] <img width="1445" height="801" alt="image" src="https://github.com/user-attachments/assets/81d817e1-4039-4fc6-bebb-d03eac642c8b" />


 

---

### 2. Auditoría del Endpoint y SIEM (Splunk)
El análisis de logs en **Splunk** confirma que, a diferencia de otros hosts en el mismo segmento, las políticas aplicadas al host `10.20.2.17` fueron efectivas:

* **Host de Origen:** `10.20.2.17`.
* **Aplicación:** `web-browsing`.
* **Acción del Firewall:** **`blocked`** (Acceso Denegado).
* **Regla de Firewall Aplicada:** `Blocked Websites`.

Mientras que otros dispositivos (como la IP `10.20.2.10`) operaban bajo la regla permisiva `Allow-Internet`, este host fue filtrado correctamente al intentar conectar con la infraestructura del atacante.

> ![Logs de Bloqueo en Splunk] <img width="1705" height="192" alt="image" src="https://github.com/user-attachments/assets/1b66d59d-ebff-4187-bca0-881e58a4c86d" />

> *Figura 2: Correlación de logs en el SIEM donde se verifica la acción "blocked" para el intento de conexión maliciosa.*

---

### 3. Indicadores de Compromiso (IoCs)
| Tipo | Valor |
| :--- | :--- |
| **URL Bloqueada** | `http://bit.ly/3sHkX3da12340` |
| **IP de Destino** | `67.199.248.11` |
| **Host Interno** | `10.20.2.17` |

---

## 🛡️ Remediación y Escalado
**Clasificación:** **Verdadero Positivo (True Positive)**.  
**Escalado:** **No** (La amenaza fue contenida por el control automático).

### Acciones de Respuesta:
1. **Confirmación de Bloqueo:** Se verificó en los logs que no hubo intercambio de datos (0 bytes transferidos) con la IP externa.
2. **Investigación de Origen:** El intento de acceso sugiere que el usuario interactuó con un enlace malicioso previo; se recomienda auditar el buzón de correo de `10.20.2.17`.
3. **Escaneo de Endpoint:** Análisis forense del host para descartar la presencia de malware que esté intentando realizar llamadas a servidores de Comando y Control (C2).
4. **Refuerzo de Reglas:** Revisar la arquitectura de red para asegurar que la regla `Blocked Websites` se aplique a todos los segmentos, evitando las excepciones observadas en el host `10.20.2.10`.

---

## 🛠️ Herramientas y Metodología
* **Splunk (SIEM):** Correlación de logs de firewall y eventos de red.
* **Firewall Management:** Evaluación de políticas de filtrado URL y listas negras.
* **Metodología:** Análisis de eficacia de controles defensivos bajo el marco NIST.

---
