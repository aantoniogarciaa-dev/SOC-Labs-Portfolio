# Análisis de Alerta: Posible Exfiltración de Datos

## 1. Captura de la Alerta

<img width="1465" height="381" alt="image" src="https://github.com/user-attachments/assets/f6fa625c-4a79-4ab6-97fb-c0a933eac5eb" />


*Captura del SIEM donde se observa la generación de la alerta por volumen elevado de tráfico saliente.*

---

## 2. Resumen de la Alerta

- **Nombre de la alerta:** Potential Data Exfiltration  
- **Severidad:** Critical  
- **Regla de detección:** Generada al detectar más de 5GB de tráfico saliente desde un único host hacia un mismo destino en un periodo de 24 horas.  
- **Entorno:** Laboratorio SIEM (TryHackMe)  

La alerta se clasifica inicialmente como crítica debido al posible riesgo de fuga de información sensible fuera de la organización.

---

## 3. Triage Inicial

Tras la revisión inicial de la alerta se identifican los siguientes datos relevantes:

- **IP origen:** 192.168.45.66  
- **Segmento de red:** UK04 / MEETINGROOM  
- **Dominio destino:** *.zoom.us  
- **Datos enviados:** 5.8 GB  
- **Datos recibidos:** 5.2 GB  

El alto volumen de datos salientes activa la regla de exfiltración, por lo que se procede a un análisis más profundo para determinar la naturaleza del tráfico.

---

## 4. Proceso de Investigación

Durante la investigación se realizaron las siguientes acciones:

1. **Verificación de reputación del dominio destino.**  
   Se comprueba que el dominio pertenece a Zoom, servicio legítimo de videoconferencia ampliamente utilizado en entornos corporativos.

2. **Análisis del patrón de tráfico.**  
   Se observa que el tráfico es bidireccional (volumen similar de subida y bajada), lo cual es característico de servicios de videoconferencia en tiempo real.

3. **Revisión del contexto del activo.**  
   El host pertenece al segmento de red “MEETINGROOM”, lo cual es coherente con un uso intensivo de aplicaciones de videollamada.

4. **Búsqueda de indicadores adicionales de compromiso.**  
   No se identifican conexiones a dominios sospechosos, direcciones IP de mala reputación ni uso de protocolos inusuales.

5. **Evaluación del comportamiento general.**  
   No se detectan procesos anómalos ni actividad lateral asociada al host analizado.

---

## 5. Hallazgos

- El dominio destino corresponde a un servicio legítimo (Zoom).
- El patrón de tráfico es consistente con actividad de videoconferencia.
- El volumen de datos es elevado pero coherente con una reunión prolongada.
- No existen indicadores técnicos que sugieran actividad maliciosa.
- No se observan comportamientos anómalos adicionales en el host.

---

## 6. Evaluación de Riesgo

Aunque la alerta presenta una severidad crítica basada en volumen de datos, el análisis contextual reduce significativamente el nivel de riesgo real.

El evento corresponde a una actividad empresarial legítima y no a un intento de exfiltración de información.

---

## 7. Conclusión

El comportamiento observado es consistente con uso legítimo de la plataforma Zoom en una sala de reuniones corporativa.

**Clasificación final:** Falso Positivo  
**Acción tomada:** Cierre del ticket sin necesidad de escalado a Nivel 2.  

---

## 8. Lecciones Aprendidas

- El volumen elevado de datos no implica necesariamente exfiltración.
- El contexto del activo es un factor determinante en el análisis.
- El análisis del patrón de tráfico (bidireccional vs unidireccional) es clave para diferenciar actividad legítima de comportamiento malicioso.
- La correlación entre alerta técnica y función del activo reduce falsos positivos.
