# Informe de Incidente: Phishing y Compromiso de Cuenta (ATO)

**Analista:** Antonio García García  
**Fecha del Incidente:** 15/02/2026  
**ID del Caso:** #8814  
**Severidad:** Alta  
**Estado:** Resuelto (Entorno de Laboratorio)  
**Certificación/Ruta:** TryHackMe - SOC Analyst / Blue Team

---

## 📝 Resumen Ejecutivo
Este proyecto detalla la investigación técnica de un incidente de **Phishing** que resultó en un **Compromiso de Cuenta (Account Takeover)**. Utilizando **Splunk** como SIEM, identifiqué un vector de ataque inicial mediante un correo malicioso, seguido de actividad no autorizada interna en un intervalo de pocos minutos, lo que confirma el éxito del ataque de ingeniería social.

---

## 🔍 Investigación y Análisis

### 1. Vector de Ataque Inicial
El incidente comenzó con una alerta automática por un correo entrante que contenía un enlace externo sospechoso con características de suplantación de identidad para un proceso de "onboarding".

* **Timestamp:** 15/02/2026 19:06:08.447.
* **Remitente:** `onboarding@hrconnex.thm`.
* **Destinatario:** `j.garcia@thetrydaily.thm`.
* **Asunto:** *Action Required: Finalize Your Onboarding Profile*.
* **URL Maliciosa:** `https://hrconnex.thm/onboarding/15400654060/j.garcia`.

> ![Alerta Inicial de Phishing]<img width="1457" height="706" alt="image" src="https://github.com/user-attachments/assets/e8ef211f-24bd-43fb-86fb-8d2022127878" />  
> *Figura 1: Detalle de la alerta inicial en el panel de control del SOC donde se identifica el correo malicioso y el enlace externo.*

---

### 2. Cronología del Compromiso (Timeline)
La correlación de eventos en el SIEM permitió reconstruir la línea de tiempo del ataque, demostrando la rapidez con la que se generó actividad inusual tras la entrega del correo:

| Hora (15/02/2026) | Tipo de Evento | Detalles del Evento |
| :--- | :--- | :--- |
| **19:06:08** | **Correo Entrante** | Recepción del enlace de phishing desde `onboarding@hrconnex.thm`. |
| **19:12:06** | **Actividad Interna** | La cuenta comprometida envía correos sobre "New Product Launch". |
| **19:14:20** | **Actividad Interna** | Envío de correos sobre "Hiring Update" desde la cuenta de la víctima. |
| **19:14:33** | **Confirmación ATO** | **Actividad Sospechosa:** El usuario se auto-envía una encuesta de feedback interna. |

> ![Correlación en Splunk](<img width="1701" height="898" alt="image" src="https://github.com/user-attachments/assets/70823b40-e240-4330-90a6-8dd796245a14" />
)  
> *Figura 2: Análisis de logs JSON donde se observa la persistencia del atacante y el envío de correos internos no autorizados.*

---

### 3. Entidades Afectadas
* **Usuario:** `j.garcia@thetrydaily.thm` (Compromiso principal identificado).
* **Host de Origen:** `10.10.74.44` (Puerto: 8989).
* **Dominio Atacante (IOC):** `hrconnex.thm`.

---

## 🛡️ Remediación y Escalado
**Clasificación:** Verdadero Positivo (True Positive).  
**Escalado:** Requerido.

El incidente se clasificó como crítico tras hallar evidencias de **Account Takeover (ATO)**. El atacante logró que el usuario interactuara con el enlace, obteniendo acceso para realizar comunicaciones internas desde la cuenta legítima.

### Acciones Realizadas:
1. **Contención:** Bloqueo preventivo de la cuenta afectada y aislamiento del host `10.10.74.44`.
2. **Control de Acceso:** Reseteo de credenciales y revocación de sesiones activas en el sistema.
3. **Erradicación:** Bloqueo del dominio malicioso `hrconnex.thm` en el Firewall y Gateway de correo.
4. **Saneamiento:** Auditoría de los logs de Splunk para descartar movimientos laterales adicionales.

> ![Informe Final TryHackMe](<img width="1500" height="884" alt="image" src="https://github.com/user-attachments/assets/679b0f5e-4ea6-42ac-9b91-3cfb74a12a65" />
)  
> *Figura 3: Informe de incidente finalizado y clasificado correctamente para su cierre en el laboratorio.*

---

## 🛠️ Herramientas y Metodología
* **Splunk (SIEM):** Utilizado para la correlación de logs JSON y reconstrucción de la línea de tiempo.
* **Análisis de Cabeceras:** Inspección de campos `sender`, `recipient` y `direction` para identificar el flujo del ataque.
* **TryHackMe SOC Lab:** Escenario de entrenamiento para la respuesta ante incidentes en entornos corporativos.
* **Metodología:** Basada en el ciclo de vida de respuesta a incidentes del NIST.
