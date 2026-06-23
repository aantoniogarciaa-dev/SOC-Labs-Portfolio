# 🛡️ Documentación de Sala: Invite Only

**📅 Fecha:** 23/06/2026  
**👤 Analista:** Antonio Garcia

## 📝 Descripción de la Sala

Eres un analista SOC en el equipo SOC del proveedor de servicios administrados **TrySecureMe**. Hoy, estás apoyando a un analista L3 en la investigación de IPs, hashes, URLs o dominios marcados como parte de las actividades de respuesta a incidentes (IR). Uno de los analistas L1 marcó dos hallazgos sospechosos a primera hora de la mañana y los escaló. Tu tarea es analizar estos hallazgos más a fondo y destilar la información en inteligencia de amenazas utilizable.

*   **IP Sospechosa:** `101[.]99[.]76[.]120`
*   **Hash SHA256 Sospechoso:** `5d0509f68a9b7c415a726be75a078180e3f02e59866f193b0a99eee8e39c874f`

Recientemente compramos una nueva aplicación de búsqueda de inteligencia de amenazas llamada TryDetectThis2.0. Puedes usar esta aplicación para recopilar información sobre los indicadores anteriores.

*   **Herramientas:** TryDetectME 2.0 (ACCESIBLE SOLO DENTRO DEL LABORATORIO)

---

### 🔍 Q1: What is the name of the file identified with the flagged SHA256 hash?

Vamos a usar el hash SHA256 para encontrar el nombre del archivo en la Plataforma de Inteligencia de Amenazas.

**Respuesta:** `syshelpers.exe` 
<img width="886" height="271" alt="image" src="https://github.com/user-attachments/assets/eb03e909-3176-45c8-ba06-79a40c4a8d4f" />


---

### 📂 Q2: What is the file type associated with the flagged SHA256 hash?

El valor del hash SHA256 sospechoso corresponde a un archivo ejecutable de Windows (Win32 EXE).

**Respuesta:** Win32 EXE 
<img width="886" height="299" alt="image" src="https://github.com/user-attachments/assets/3cfb82f3-dec6-454d-bfa0-967eeac4e6e5" />


---

### ⛓️ Q3: What are the execution parents of the flagged hash? List the names chronologically, using a comma as a separator. Note down the hashes for later use.

Vamos a encontrar los padres de ejecución del hash sospechoso usando la pestaña de relaciones (relation tab) en la plataforma de Inteligencia. Allí se muestran 2 procesos de ejecución (`361GJX7J`, `installer.exe`). 

**Respuesta:** `361GJX7J,installer.exe` 
<img width="886" height="303" alt="image" src="https://github.com/user-attachments/assets/d0b61b0e-4c43-4392-9f7e-bb0fde3faf22" />


TOMAR NOTA DEL HASH PARA AMBOS ARCHIVOS:
*   **361GJX7J:** `047c5eec0445746862710d20e50a5dd04510b7e625fa5c1f5d48ce078001c0de`
*   **installer.exe:** `fa102d4e3cfbe85f5189da70a52c1d266925f3efd122091cdc8fe0fc39033942`

---

### 📥 Q4: What is the name of the file being dropped? Note down the hash value for later use.

Al usar la inteligencia de amenazas, se muestra que se suelta (drop) un archivo tras la ejecución del archivo malicioso.

**Respuesta:** `AClient.exe` 
<img width="886" height="336" alt="image" src="https://github.com/user-attachments/assets/90d31bee-fce9-4e22-9cc7-ebac664f6570" />


**NOTA DEL VALOR HASH de AClient.exe:** `dd02c105809e4ca41a5489e585ba025eddb89a91703b73a566c9903e6406a08c`

---

### ⚠️ Q5: Research the second hash in question 3 and list the four malicious dropped files in the order they appear (from up to down), separated by commas.

Ya anotamos los hashes de los dos archivos padres de ejecución (`361GJX7J`, `installer.exe`), pero la pregunta pide específicamente usar el valor del segundo hash (`installer.exe`). Al usar este hash para analizar los archivos soltados correspondientes, vemos que hay 20 archivos soltados, pero encontramos que 4 archivos están marcados como maliciosos y los demás están etiquetados como limpios.

**Respuesta:** `searchhost.exe,syshelpers.exe,nat.vbs,runsys.vbs` 
<img width="886" height="328" alt="image" src="https://github.com/user-attachments/assets/05b2a2cc-85aa-4930-8d87-a3c02ed82735" />


---

### 🕸️ Q6: Analyse the files related to the flagged IP. What is the malware family that links these files?

Usando la IP sospechosa `101[.]99[.]76[.]120` para analizar la familia de malware asociada a ella, da un poco de dolor de cabeza encontrarla, así que busqué en la pestaña de la comunidad (community tab) para encontrar alguna etiqueta dada por algún investigador. Finalmente encontré el contexto de un investigador que menciona MALPEDIA: AsyncRAT. Así que confirmo que es la familia de malware AsyncRAT.

**Respuesta:** AsyncRAT 
<img width="886" height="236" alt="image" src="https://github.com/user-attachments/assets/e13b374d-7926-4bce-b024-7d6e17d5abbb" />


---

### 📄 Q7: What is the title of the original report where these flagged indicators are mentioned? Use Google to find the report.

No confiamos ciegamente en este contexto para estar seguros. Usando el título del contexto encontré el informe original en Google. El título es: "From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery". Luego encontré el enlace al informe en la página oficial.

**Respuesta:** From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery 
<img width="886" height="517" alt="image" src="https://github.com/user-attachments/assets/76c699ef-3bb3-4741-b7d3-54af0fd08987" />


---

### 🍪 Q8: Which tool did the attackers use to steal cookies from the Google Chrome browser?

Siguiendo el informe mencionado: La operación continúa evolucionando, y los actores de amenazas ahora pueden eludir el cifrado App Bound Encryption (ABE) de Chrome mediante el uso de herramientas adaptadas como ChromeKatz para robar cookies de las nuevas versiones del navegador Chromium.

**Respuesta:** ChromeKatz 
<img width="886" height="438" alt="image" src="https://github.com/user-attachments/assets/db67a7c7-80fd-445f-9a94-d10057635d62" />


---

### 🎣 Q9: Which phishing technique did the attackers use? Use the report to answer the question.

El informe mencionado indica que los atacantes utilizaron la técnica de phishing ClickFix.

**Respuesta:** ClickFix 
<img width="886" height="322" alt="image" src="https://github.com/user-attachments/assets/8b2b3dc3-dcf6-4d92-a2db-780b71234507" />


---

### 💬 Q10: What is the name of the platform that was used to redirect a user to malicious servers?

El informe mencionaba: "De Enlaces de Confianza a Servidores Maliciosos de Discord". Usando el método descrito anteriormente, los atacantes secuestran enlaces de invitación de Discord compartidos originalmente por comunidades legítimas. Los usuarios que siguen estos enlaces de confianza, encontrados en publicaciones de redes sociales o sitios web oficiales de la comunidad, terminan sin saberlo en servidores maliciosos cuidadosamente diseñados para parecer auténticos. 

Al unirse, los nuevos miembros suelen ver que la mayoría de los canales están bloqueados y solo un canal, normalmente llamado "verify", es accesible. En este canal, un bot llamado "Safeguard" pide a los usuarios que completen un paso de verificación para obtener acceso completo al servidor.

**Respuesta:** DISCORD 

<img width="819" height="688" alt="image" src="https://github.com/user-attachments/assets/a7d0fd78-1dc7-45d5-be18-4dc3225f9531" />

RESPUESTAS CORRECTAS:

<img width="945" height="829" alt="image" src="https://github.com/user-attachments/assets/042259e8-4239-450e-ba47-fa5eab5f6cd2" />



