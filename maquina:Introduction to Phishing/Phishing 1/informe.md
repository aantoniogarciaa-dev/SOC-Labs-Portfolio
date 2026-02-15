# Informe de Incidente: Análisis de Phishing (Caso #8814)

**Analista:** Antonio García García
**Fecha del Incidente:** 15/02/2026
**ID del Caso:** #8814
**Severidad:** Baja (Reclasificado)
**Estado:** Resuelto (Falso Positivo)
**Certificación/Ruta:** TryHackMe - SOC Analyst / Blue Team

---

## 📝 Resumen Ejecutivo
Investigación técnica de una alerta de **Phishing** que involucraba un supuesto proceso de "onboarding" de la empresa. Tras realizar una correlación de eventos en el SIEM (**Splunk**) y auditar los logs de tráfico del Firewall, se determinó que el incidente es un **Falso Positivo**, ya que no se detectó interacción del usuario con el enlace malicioso ni conexiones salientes sospechosas.

---

## 🔍 Investigación y Análisis

### 1. Vector de Ataque Identificado (Email)
La alerta se disparó por un correo entrante con las siguientes características:
* **Timestamp:** 15/02/2026 19:06:08.447.
* **Remitente:** `onboarding@hrconnex.thm`.
* **Destinatario:** `j.garcia@thetrydaily.thm`.
* **Asunto:** *Action Required: Finalize Your Onboarding Profile*.
* **URL Sospechosa:** `https://hrconnex.thm/onboarding/15400654060/j.garcia`.

> ![Alerta Inicial de Phishing]<img width="1457" height="706" alt="image" src="https://github.com/user-attachments/assets/e8ef211f-24bd-43fb-86fb-8d2022127878" />  
> *Figura 1: Detalle de la alerta inicial en el panel de control del SOC donde se identifica el correo malicioso y el enlace externo.*

---

### 2. Auditoría de Red y SIEM
Para validar la alerta, se procedió a buscar cualquier rastro de conexión hacia el dominio `hrconnex.thm` en los logs de red:
* **Análisis de Firewall:** No se encontraron registros de tráfico saliente (`allowed`) desde la IP de la víctima hacia el dominio malicioso en la ventana de tiempo del incidente.
* **Comportamiento del Endpoint:** No existen evidencias de descarga de archivos, ejecución de procesos inusuales o tráfico de red anómalo posterior a la recepción del correo.

> ![Correlación en Splunk] <img width="1702" height="897" alt="image" src="https://github.com/user-attachments/assets/50e878c1-6e5d-41be-87c8-99f533145076" />



### 3. Conclusión de la Investigación
**Clasificación:** **Falso Positivo (False Positive)**.

A pesar de que el correo tiene una apariencia maliciosa y utiliza un dominio externo no corporativo, la ausencia de interacción por parte del usuario y la falta de registros en el firewall confirman que la amenaza no se materializó. El sistema de detección actuó correctamente al alertar, pero no hubo compromiso de seguridad.

---

## 🛡️ Acciones Realizadas
1. **Validación de Logs:** Verificación exhaustiva en Splunk descartando conexiones exitosas a la URL.
2. **Monitoreo Preventivo:** Se mantuvo el host bajo observación durante 24 horas sin detectar anomalías.
3. **Cierre del Caso:** Registro del evento como Falso Positivo para mejorar el ajuste de las reglas de detección del SOC.

---

## 🛠️ Herramientas y Metodología
* **Splunk (SIEM):** Correlación de logs de tráfico y eventos de seguridad.
* **Firewall Log Audit:** Inspección de políticas de acceso y registros de conexión.
* **Metodología:** Verificación de alertas y triaje de incidentes.
