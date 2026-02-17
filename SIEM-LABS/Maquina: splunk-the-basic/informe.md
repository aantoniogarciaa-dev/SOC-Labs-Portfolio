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

<img width="1356" height="859" alt="image" src="https://github.com/user-attachments/assets/9b2f3339-5a66-4e33-918a-45989cbe0f48" />


# 🔎 Pregunta 1  
## ¿Cuántos eventos están presentes en el log?
2862

<img width="798" height="125" alt="image" src="https://github.com/user-attachments/assets/d267b736-5fad-4e66-9e7d-31e209bd9099" />


# 🔎 Pregunta 2  
## ¿Cuántos eventos de log fueron capturados por el usuario Maleena?

Para obtener el número de eventos asociados al usuario "Maleena", se puede realizar el análisis de dos formas distintas dentro de Splunk.

---

## ✅ Método 1 – Uso de la interfaz gráfica (filtrado manual)

<img width="1245" height="787" alt="image" src="https://github.com/user-attachments/assets/7f8648d8-b7c8-4a46-8707-cc4c37897db7" />

serian

   
