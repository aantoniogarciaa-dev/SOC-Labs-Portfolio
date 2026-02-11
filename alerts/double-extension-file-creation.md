# Análisis de Alerta: Creación de Archivo con Doble Extensión

## 1. Captura de la Alerta

<img width="1447" height="436" alt="image" src="https://github.com/user-attachments/assets/dc96e601-97fd-4b45-ba3b-67e1ee48c062" />


*Captura del SIEM mostrando la detección de creación de archivo con doble extensión.*

---

## 2. Resumen de la Alerta

- **Nombre de la alerta:** Double-Extension File Creation  
- **Severidad:** High  
- **Descripción de la regla:** Detecta la creación de archivos con doble extensión (ej: *.pdf.exe, *.mp4.exe), técnica común en ataques de phishing para ocultar ejecutables maliciosos.  
- **Estado inicial:** Awaiting action  

---

## 3. Triage Inicial

Datos observados en la alerta:

- **Host:** LPT-HR-009  
- **Proceso:** chrome.exe  
- **Usuario:** S.Conway  
- **Archivo creado:** cats2025.mp4.exe  
- **Ruta:** C:\Users\S.Conway\Downloads\  
- **Origen de descarga:** https://freecatvideoshd.monster/cats2025.mp4.exe  
- **Hash MD5:** 14d8486f3f63875ef93cfd240c5dc10b  

La extensión `.mp4.exe` indica un intento claro de suplantar un archivo multimedia legítimo mediante doble extensión.

---

## 4. Proceso de Investigación

Se realizaron los siguientes pasos:

1. **Análisis del nombre del archivo.**  
   Se identifica doble extensión, técnica común para engañar al usuario y ejecutar malware.

2. **Revisión del dominio de origen.**  
   El dominio `freecatvideoshd.monster` no corresponde a un servicio corporativo ni a una fuente confiable.

3. **Evaluación del proceso padre.**  
   El archivo fue descargado desde `chrome.exe`, lo que sugiere interacción directa del usuario (posible phishing o descarga engañosa).

4. **Análisis del contexto del usuario.**  
   Equipo asignado a departamento HR (posible vector atractivo para phishing).

5. **Evaluación del hash.**  
   El archivo ejecutable presenta hash identificable para posible verificación en motores de reputación (no incluido en este laboratorio).

---

## 5. Indicadores de Compromiso (IOCs)

- Archivo con doble extensión: `cats2025.mp4.exe`
- Dominio sospechoso: `freecatvideoshd.monster`
- Descarga desde navegador
- Ubicación en carpeta Downloads

---

## 6. Evaluación de Riesgo

La técnica de doble extensión es una técnica clásica de ingeniería social utilizada para:

- Engañar al usuario ocultando la verdadera extensión.
- Ejecutar malware bajo apariencia de archivo multimedia.
- Facilitar infección inicial mediante phishing.

El patrón observado es consistente con un intento de ejecución maliciosa.

---

## 7. Conclusión

La alerta presenta múltiples indicadores consistentes con actividad maliciosa.

**Clasificación final:** True Positive  
**Recomendación:**  
- Aislamiento del host.  
- Eliminación del archivo.  
- Escalado a Nivel 2 para análisis más profundo.  
- Verificación de ejecución y posibles procesos secundarios.  

---

## 8. Lecciones Aprendidas

- Las dobles extensiones siguen siendo una técnica efectiva de ingeniería social.
- El análisis contextual (proceso, dominio y ruta) es clave en la clasificación.
- Las descargas desde dominios no confiables deben considerarse de alto riesgo.
