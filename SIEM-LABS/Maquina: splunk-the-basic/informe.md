# 🛡️ Splunk Basic – Análisis de Logs VPN en SIEM

---

## 🎯 Objetivo del laboratorio

El objetivo de esta práctica es cargar un conjunto de logs VPN en Splunk, crear un índice específico llamado `VPN_Logs` y realizar búsquedas para analizar la información contenida en los eventos.

La actividad simula tareas reales de un Analista SOC Nivel 1, centradas en la validación de la ingesta de datos y el análisis básico de logs de red.

---

## 🛠️ Configuración del entorno

- Plataforma: Splunk Enterprise 8.2.6  
- Archivo cargado: `VPNLogs.json`  
- Tipo de logs: VPN en formato JSON  
- Índice creado: `VPN_Logs`  
- Sourcetype: `_json`  
- Rol simulado: Analista SOC L1  

---

## 📥 Creación del índice `VPN_Logs`

Para organizar correctamente los eventos, se creó un índice específico en Splunk siguiendo estos pasos:

1. Acceso a **Settings → Indexes**
2. Selección de **New Index**
3. Creación del índice con el nombre:
<img width="1392" height="914" alt="image" src="https://github.com/user-attachments/assets/61dd4889-4609-45e1-9164-e409c8c7b48c" />

4. Guardado de la configuración.

Posteriormente, durante la carga del archivo `VPNLogs.json`, se seleccionó el índice `VPN_Logs` y el sourcetype `_json` para asegurar una correcta interpretación del formato estructurado.

En un entorno real de SOC, la correcta separación por índices mejora el rendimiento de búsqueda, la organización de datos y la capacidad de análisis.

<img width="1254" height="706" alt="image" src="https://github.com/user-attachments/assets/20d849ae-14b0-42c8-acb5-ede754ca26de" />



# 🔎 Pregunta 1  
## ¿Cuántos eventos están presentes en el log?
2862

<img width="918" height="63" alt="image" src="https://github.com/user-attachments/assets/51f5fc07-6f5e-4ec6-83d9-f9be984694b6" />



# 🔎 Pregunta 2  
## ¿Cuántos eventos de log fueron capturados por el usuario Maleena?

Para obtener el número de eventos asociados al usuario "Maleena", se puede realizar el análisis de dos formas distintas dentro de Splunk.

---

## ✅ Método 1 – Uso de la interfaz gráfica (filtrado manual)

<img width="1215" height="713" alt="image" src="https://github.com/user-attachments/assets/29dd29d1-2b3d-4814-9804-8e7fb751798d" />


serian 60

## ✅ Método 2 - Uso de consulta

<img width="1249" height="129" alt="image" src="https://github.com/user-attachments/assets/bf4845f6-d0f1-4365-9cff-1a7d4ef552f3" />


# 🔎 Pregunta 3  
## ¿Cuál es el nombre de usuario asociado a la IP 107.14.182.38?

Para identificar el usuario asociado a la dirección IP **107.14.182.38**, se realizó un filtrado específico dentro del índice correspondiente, manteniendo el contexto del host utilizado en el laboratorio.

El usuario es smith

<img width="1209" height="633" alt="image" src="https://github.com/user-attachments/assets/d605d8cf-a756-4d43-af59-9828cb498cff" />

## ✅ Consulta ejecutada

``spl
index=vpn_logs host="host-names" 107.14.182.38

# 🔎 Pregunta 4  
## ¿Cuál es el número de eventos que se originaron en todos los países excepto Francia?

Para responder a esta pregunta, se filtraron los eventos excluyendo aquellos cuyo campo `Source_Country` tiene el valor **France**.

---

## ✅ Método 1 – Consulta SPL directa

Se ejecutó la siguiente consulta:

``spl
index=vpn_logs host="host-names" Source_Country!="France"

<img width="1280" height="599" alt="image" src="https://github.com/user-attachments/assets/f614b94b-ceb2-4309-8108-7488a2e703c6" />



respuesta correcta 2814





## ✅ Método 2 - Uso de consulta


# 🔎 Pregunta 5  
## ¿Cuántos eventos VPN estuvieron asociados a la IP 107.3.206.58?

Para determinar el número de eventos asociados a la dirección IP **107.3.206.58**, se realizó un filtrado específico dentro del índice correspondiente, manteniendo el contexto del host utilizado en el laboratorio.

---

## ✅ Método 1 – Consulta SPL directa

Se ejecutó la siguiente consulta:

``spl
index=vpn_logs host="host-names" Source_ip="107.3.206.58"

<img width="1233" height="615" alt="image" src="https://github.com/user-attachments/assets/53c67c7c-b467-4ee3-a5d9-79f4d70e570b" />



respuesta correcta: 14

FINALIZADO.

<img width="491" height="874" alt="image" src="https://github.com/user-attachments/assets/abb46a54-a6f3-43da-8c9b-57cc340e0ec5" />



