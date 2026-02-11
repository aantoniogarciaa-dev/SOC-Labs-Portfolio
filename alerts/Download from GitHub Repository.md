# Análisis de Alerta: Download from GitHub Repository

## 1. Captura de la Alerta

<img width="1491" height="347" alt="image" src="https://github.com/user-attachments/assets/8fcf819b-a353-4e53-b506-fd763ecf1e7a" />


*Captura del SIEM mostrando descarga detectada desde repositorio GitHub.*

---

## 2. Resumen de la Alerta

- **Nombre de la alerta:** Download from GitHub Repository  
- **Severidad:** Low  
- **Descripción de la regla:** Detecta descargas realizadas desde GitHub, ya que la plataforma puede alojar tanto proyectos legítimos como scripts maliciosos.  
- **Estado inicial:** Awaiting action  

---

## 3. Triage Inicial

Datos observados:

- **Usuario:** G.Chandler  
- **Host:** LPT-IT-063  
- **Segmento de red:** VPN / DEVELOPERS  
- **URL accedida:** https://github.com/facebook/react  

La regla se activa por acceso/descarga desde GitHub.

---

## 4. Proceso de Investigación

1. **Revisión del dominio.**  
   GitHub es una plataforma legítima y ampliamente utilizada en entornos tecnológicos.

2. **Análisis del repositorio accedido.**  
   El repositorio corresponde a `facebook/react`, proyecto oficial de React, framework ampliamente utilizado en desarrollo web.

3. **Evaluación del contexto del usuario.**  
   El usuario pertenece al segmento `DEVELOPERS`, lo que hace coherente el acceso a repositorios de desarrollo.

4. **Revisión de severidad.**  
   La alerta presenta nivel Low, indicando que se trata de una regla preventiva más que de un indicador directo de compromiso.

5. **Búsqueda de indicadores adicionales.**  
   No se observan descargas desde repositorios sospechosos ni ejecución de binarios asociados.

---

## 5. Evaluación de Riesgo

Aunque GitHub puede alojar código malicioso, el acceso a un repositorio oficial de React por parte de un usuario del departamento de desarrollo es coherente con la actividad esperada.

No se identifican indicadores que sugieran actividad maliciosa.

---

## 6. Conclusión

La actividad es consistente con tareas legítimas de desarrollo.

**Clasificación final:** Falso Positivo  
**Acción tomada:** Cierre del ticket sin necesidad de escalado.

---

## 7. Lecciones Aprendidas

- No todas las descargas desde plataformas públicas implican riesgo.
- El contexto del usuario y su función es clave en el análisis.
- Las reglas genéricas deben evaluarse cuidadosamente para evitar falsos positivos.
