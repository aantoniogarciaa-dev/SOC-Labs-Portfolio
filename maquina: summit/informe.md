# 🛡️ TryHackMe: Detección de Amenazas y Gestión de IoCs

## 📝 Resumen del Ejercicio
* **Plataforma:** TryHackMe
* **Rol:** Analista SOC / Blue Team (Defensa)
* **Objetivo:** Analizar muestras de malware simuladas por un pentester, extraer Indicadores de Compromiso (IoCs) y crear reglas de detección en la solución EDR (PicoSecure) para bloquear los ataques.

---

## 📧 1. El Escenario Inicial: Recepción del Malware
El ejercicio comienza con la recepción de un correo por parte de "Sphinx", el encargado de realizar las pruebas de penetración (Red Team) contra nuestra infraestructura (PicoSecure). 

Sphinx nos informa de que va a ejecutar diferentes muestras de malware de forma iterativa para comprobar la eficacia de nuestras herramientas de detección. Nos proporciona la primera muestra: `sample1.exe`, y nos reta a analizarla en el *Malware Sandbox* para crear una regla que lo bloquee.

> **Captura 1: Correo inicial de Sphinx**
<img width="1225" height="611" alt="image" src="https://github.com/user-attachments/assets/1c499788-1df9-4bd8-bbd2-6a7cc85b5ca9" />

---

## 🔬 2. Análisis de Malware (Sandbox)
Para poder bloquear la amenaza, primero necesitamos identificarla. Procedemos a ejecutar `sample1.exe` en la herramienta **Malware Sandbox** para obtener un reporte detallado de su comportamiento y firmas.

El análisis revela información crucial:
* **Firma detectada:** `Trojan.Metasploit.A` (Nos indica que el atacante está utilizando un payload generado probablemente con MSFvenom/Metasploit).
* **Comportamiento malicioso:** El proceso `sample1.exe` (PID: 2492) intenta conectarse a un puerto inusual y realiza comprobaciones en el sistema (LSA protection, lectura de la GUID de la máquina).
* **Indicadores de Compromiso (IoCs):** Obtenemos las firmas criptográficas del archivo. El hash **MD5** es `cbda8ae000aa9cbe7c8b982bae006c2a`.

> **Captura 2: Reporte del Malware Sandbox**
> <img width="1207" height="815" alt="image" src="https://github.com/user-attachments/assets/cce77f78-dc6c-4a51-8fd4-e078d25be525" />

---

## 🚫 3. Implementación de la Detección (Bloqueo por Hash)
Con el hash MD5 en nuestro poder, nos dirigimos a la sección de **Gestión de IoCs (Manage Hashes)** de nuestro EDR. 

El hash es un identificador único del archivo. Al añadir este MD5 a la **Hash Blocklist**, le estamos diciendo a nuestra herramienta de seguridad que bloquee automáticamente cualquier archivo en la red que coincida con esa firma exacta.

* **Algoritmo:** MD5
* **Valor:** `cbda8ae000aa9cbe7c8b982bae006c2a`

Al introducirlo, el sistema nos confirma el éxito de la operación: hemos prevenido la ejecución de `sample1.exe`.

> **Captura 3: Añadiendo el hash a la Blocklist**
> <img width="1894" height="811" alt="image" src="https://github.com/user-attachments/assets/2f81b82a-1f32-4fad-8b7f-4528a38ce2c5" />

---

## 🚩 4. Resultados y Lecciones Aprendidas (Obtención de la Flag)
Tras implementar la regla, recibimos un nuevo correo de Sphinx confirmando que hemos bloqueado su ataque con éxito. El sistema nos recompensa con nuestra primera flag:

`THM{f3cbf08151a11a6a331db9c6cf5f4fe4}`

**Lección de Blue Team (La Pirámide del Dolor):**
Sphinx nos da una lección vital en la ingeniería de detección. Aunque los hashes son indicadores de altísima confianza (es casi imposible que haya un falso positivo), también son el mecanismo de defensa más débil. Como indica Sphinx, para el atacante es tan sencillo como recompilar el malware o cambiar un solo bit del código original para que el hash cambie por completo, haciendo inútil nuestra regla de bloqueo actual. 

Esto da paso a la siguiente fase del reto: enfrentarnos a `sample2.exe`, donde tendremos que buscar métodos de detección más robustos que un simple hash.

> **Captura 4: Correo de confirmación y Flag**
> <img width="1256" height="735" alt="image" src="https://github.com/user-attachments/assets/aa9ff239-9978-4c5d-b261-dcdf4fecebcd" />





## 🌐 5. Escalando la Defensa: Bloqueo por Dirección IP (sample2.exe)

Tras comprobar lo fácil que es evadir un bloqueo basado en Hash, el atacante (Sphinx) nos envía `sample2.exe`. Sabiendo que el hash será diferente, debemos analizar de nuevo la muestra en el **Malware Sandbox** para identificar nuevos Indicadores de Compromiso (IoCs).

<img width="1851" height="917" alt="image" src="https://github.com/user-attachments/assets/4812337f-660e-4eb0-b085-31ad12d69671" />

Durante el análisis dinámico de `sample2.exe`, observamos su comportamiento de red. El malware intenta establecer una conexión externa hacia un servidor de Comando y Control (C2) operado por el atacante. 

Identificamos la dirección IP de destino: **`154.35.10.113`**.

Con esta información, procedemos a crear una regla de bloqueo a nivel de red, denegando cualquier comunicación hacia esa infraestructura maliciosa.

**Creación de Regla en el Firewall (Firewall Rule Manager):**
Nos dirigimos al gestor del firewall de PicoSecure y configuramos una regla de denegación de salida (Egress):
* **Type:** Egress (Tráfico de salida)
* **Source IP:** ANY (Cualquier equipo de nuestra red)
* **Destination IP:** `154.35.10.113` (El servidor C2 del atacante)
* **Action:** Deny (Bloquear)

Al guardar la regla, el sistema confirma que hemos evitado que `sample2.exe` se conecte con el servidor del atacante.

> **Captura 5: Configuración de la regla de Firewall**
> <img width="1886" height="743" alt="image" src="https://github.com/user-attachments/assets/d8a4a57f-e78b-4d0f-9a5b-e086392c94d6" />


---

## 🚩 6. Subiendo en la Pirámide del Dolor (Nueva Flag)

Una vez implementado el bloqueo por IP, recibimos otro correo de Sphinx. Nos felicita por haber identificado la conexión de red de su malware, confirmando que hemos bloqueado el ataque y otorgándonos la siguiente flag:

`THM{2ff48a3421a938b388418be273f4806d}`

**Lección de Blue Team (Direcciones IP y la nube):**
Al igual que con los hashes, Sphinx vuelve a darnos una valiosa lección. Bloquear por dirección IP es un paso adelante en la defensa (y un poco más molesto para el atacante que cambiar un hash), pero sigue sin ser definitivo. 

Como él mismo explica, con la facilidad que ofrecen los proveedores de servicios en la nube (Cloud Service Providers), para un adversario motivado es trivial dar de baja un servidor y levantar otro con una dirección IP pública completamente nueva. De hecho, nos avisa de que ya tiene "servidores de respaldo" listos para usar.

Esto nos prepara para el siguiente nivel del reto con `sample3.exe`, donde tendremos que buscar artefactos o comportamientos del malware aún más difíciles de modificar por parte del atacante.

> **Captura 6: Correo de confirmación y bypass de IP**
> <img width="1885" height="616" alt="image" src="https://github.com/user-attachments/assets/b55a002e-63b0-43a3-ab21-806317dfd5c2" />


## 🕵️ 7. Infraestructura Dinámica: Análisis de sample3.exe

Como Sphinx nos advirtió, el bloqueo por IP fue fácilmente evadido migrando a un nuevo servidor en la nube con una IP distinta. En su nuevo correo nos entrega `sample3.exe`, retándonos a encontrar un método de detección más persistente.

Volvemos al **Malware Sandbox** para realizar el análisis dinámico de esta nueva muestra. Al revisar la sección de Actividad de Red (Network Activity), descubrimos un cambio importante en la estrategia del atacante:

1. **Descarga de payloads adicionales:** El malware (PID: 1021) ahora realiza peticiones HTTP GET para descargar un archivo llamado `backdoor.exe`.
2. **Identificación del Dominio (IoC):** En lugar de conectarse a una IP desnuda, el malware se está comunicando con un nombre de dominio específico: `http://emudyn.bresonicz.info`.

Este dominio es nuestro nuevo Indicador de Compromiso.

> **Captura 7: Análisis de red y descubrimiento del dominio**
> <img width="1877" height="883" alt="image" src="https://github.com/user-attachments/assets/bb293448-4284-4e6e-867a-69c880694c53" />

---

## 🛑 8. Interrupción de la cadena de ataque (Bloqueo DNS)

Sabiendo que el atacante utiliza `emudyn.bresonicz.info` para alojar sus payloads y comunicarse con la máquina víctima, el siguiente paso lógico es bloquear la resolución de este dominio a nivel de red. 

Nos dirigimos al **Gestor de Reglas DNS (DNS Rule Manager)** de PicoSecure para implementar el bloqueo. Al denegar el acceso a este dominio, impedimos que el malware pueda descargar `backdoor.exe`, rompiendo así la cadena de ataque (Kill Chain) independientemente de la dirección IP que el dominio tenga asociada en ese momento.

**Configuración de la Regla DNS:**
* **Rule Name:** emudyn.bresonicz.info
* **Category:** Malware
* **Domain Name:** emudyn.bresonicz.info
* **Action:** Deny (Bloquear)

> **Captura 8: Implementación de la regla de bloqueo DNS**
<img width="1895" height="756" alt="image" src="https://github.com/user-attachments/assets/c50ccd02-5372-4968-acc5-974a189e216a" />


## 🚩 9. El coste para el atacante (Nueva Flag) y el salto a los Artefactos

Al implementar la regla DNS, logramos frenar de nuevo el ataque. Sphinx nos envía un correo confirmando nuestro éxito y nos entrega la siguiente flag:

`THM{4eca9e2f61a19ecd5df34c788e7dce16}`

**Lección de Blue Team (Rozando la cima de la Pirámide del Dolor):**
En su correo, Sphinx hace una confesión clave: tener que comprar y registrar nuevos dominios y modificar registros DNS le resulta molesto ("annoying"). Esto ilustra perfectamente el objetivo del Blue Team: **hacer que el ataque sea lo más costoso y tedioso posible para el adversario**. 

Hemos pasado de bloquear cosas triviales (Hashes, IPs) a bloquear Infraestructura (Dominios), lo que requiere que el atacante invierta tiempo y dinero real para recuperarse.

**El nuevo reto (sample4.exe):**
Sphinx nos advierte que para su próxima iteración (`sample4.exe`), bloquear hashes, IPs o dominios ya no servirá de nada. Nos reta a subir un peldaño más en la Pirámide del Dolor y empezar a buscar **Artefactos de Host** (cambios que el malware realiza en el sistema operativo de la víctima, como modificaciones en el registro, creación de tareas programadas o archivos soltados en disco).

> **Captura 9: Correo de Sphinx admitiendo la dificultad y entregando sample4.exe**
<img width="1903" height="829" alt="image" src="https://github.com/user-attachments/assets/d51b9797-4437-4fd6-9a6d-4bc81298c1f6" />


## ⚙️ 10. Análisis de Artefactos de Host (sample4.exe)

Siguiendo las advertencias de Sphinx, sabíamos que bloquear infraestructura (Dominios o IPs) ya no sería suficiente. El atacante nos envía `sample4.exe`. Al ejecutarlo en el **Malware Sandbox**, dejamos de centrarnos en la actividad de red y pasamos a analizar los cambios que el malware realiza en el sistema operativo (Artefactos).

En la sección de **Registry Activity (Actividad del Registro)**, detectamos un comportamiento altamente sospechoso y clásico del malware: un intento de evasión de defensas.

El proceso `sample4.exe` (PID 3806) realiza una operación de escritura (`write`) en el registro de Windows con el objetivo de desactivar la protección en tiempo real de Windows Defender:
* **Llave (Key):** `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection`
* **Nombre:** `DisableRealtimeMonitoring`
* **Valor:** `1` (Activando la desactivación)

> **Captura 10: Análisis de artefactos y modificaciones del registro**
> <img width="1753" height="834" alt="image" src="https://github.com/user-attachments/assets/04f08be3-cd81-4933-acd7-098007186af5" />

---

## 🛡️ 11. Detección Basada en Comportamiento (Reglas Sigma y MITRE ATT&CK)

Para detectar y bloquear este artefacto, no podemos usar listas negras de red. Necesitamos crear una regla que alerte a nuestro sistema (EDR/SIEM) cada vez que un proceso intente modificar esa clave de registro específica.

Utilizamos el **Sigma Rule Builder** para generar una regla basada en los eventos de **Sysmon** (System Monitor). 

Configuramos la regla apuntando exactamente a la ruta del registro y su valor malicioso, y lo más importante: **Mapeamos esta alerta con el framework MITRE ATT&CK**. Asignamos la táctica de *Defense Evasion (TA0005)*, lo que proporciona contexto vital para el equipo del SOC a la hora de responder a la alerta.

Al validar la regla, generamos el código Sigma listo para ser desplegado en nuestra infraestructura.

> **Captura 11: Creación de Regla Sigma para la modificación del registro**
> <img width="1903" height="905" alt="image" src="https://github.com/user-attachments/assets/eb915206-b268-44ba-87e3-18a574f5d667" />

---

## 🏆 12. Impacto Real en el Adversario (TTPs y Nueva Flag)

La implementación de la regla Sigma es un éxito rotundo. Recibimos un nuevo correo de Sphinx donde su frustración es evidente ("SUPER ANNOYING!"). 

Nos confiesa que hemos "estropeado" su muestra de malware y que ha tenido que gastar muchísimo tiempo y dinero en hacer que su equipo de desarrollo rediseñe las herramientas y metodologías de ataque. Hemos alcanzado la cima táctica de la defensa. Como recompensa, nos entrega una nueva flag:

`THM{c956f455fc076aea829799c0876ee399}`

**Lección de Blue Team (La Cima de la Pirámide - TTPs):**
Hemos obligado al atacante a abandonar sus herramientas actuales. Ahora, con `sample5.exe`, Sphinx nos advierte que toda la lógica del malware está en su servidor y solo deja comportamientos anómalos en nuestra red. 

Para detectar esta última fase, tendremos que enfocarnos en el nivel más alto y doloroso para el atacante en la Pirámide del Dolor: los **TTPs (Tácticas, Técnicas y Procedimientos)**. Para ello, nos proporciona un archivo de registros (`outgoing_connections.log`) donde deberemos buscar patrones de comportamiento inusuales, independientemente de la IP, dominio o hash que utilice.

> **Captura 12: Correo de frustración de Sphinx, flag y transición a los TTPs**
> <img width="1887" height="857" alt="image" src="https://github.com/user-attachments/assets/ca3b02f7-db7c-4d14-bad7-2fb53b1764d1" />


## 🕵️ 13. Caza de Amenazas (Threat Hunting): Identificando el "Beaconing"

Con la llegada de `sample5.exe`, el atacante ha ocultado toda la lógica maliciosa en su servidor, por lo que ya no deja "artefactos" obvios en el registro o sistema de archivos. Para detectarlo, Sphinx nos proporcionó un registro de tráfico de red (`outgoing_connections.log`).

Al analizar los logs, en lugar de buscar direcciones IP o dominios estáticos, buscamos **patrones de comportamiento**. Al revisar las marcas de tiempo y el peso de los paquetes, observamos una anomalía clarísima:
* Hay conexiones recurrentes hacia la IP `51.102.10.19`.
* El tamaño de la conexión es exactamente siempre de **97 bytes**.
* Ocurren en un intervalo de tiempo matemáticamente perfecto: cada 30 minutos (por ejemplo, 09:00:00, 09:30:00, 10:00:00).

Este patrón se conoce en ciberseguridad como **Beaconing** (balizamiento). Es el "latido" periódico que emite el malware para avisar a su servidor C2 (Command and Control) de que sigue activo y a la espera de instrucciones.

> **Captura 13: Análisis del log revelando el patrón de Beaconing**
> <img width="1740" height="836" alt="image" src="https://github.com/user-attachments/assets/a9d488f7-034d-4d51-a437-25857cc60db8" />

---

## 📡 14. Detección de Herramientas por Comportamiento de Red

Ya no nos importa a qué IP se conecte el atacante; si su infraestructura genera este patrón, la vamos a cazar. Volvemos al **Sigma Rule Builder** para crear una regla basada en conexiones de red (Network Connections) puramente comportamental.

Configuramos la regla de Sysmon para que se active sin importar la IP o el puerto de destino (`Any`), centrándonos exclusivamente en el rastro que deja la herramienta del atacante:
* **Size (Tamaño):** 97 bytes.
* **Frequency (Frecuencia):** 1800 segundos (30 minutos).

Como en el paso anterior, mapeamos esta alerta con el framework MITRE ATT&CK, esta vez bajo la táctica de **Command and Control (TA0011)**.

> **Captura 14: Regla Sigma para detectar el Beaconing**
> <img width="1030" height="865" alt="image" src="https://github.com/user-attachments/assets/0170048c-3da9-4f24-9b71-b16433c3958a" />
> <img width="1083" height="659" alt="image" src="https://github.com/user-attachments/assets/696488c0-3749-4e80-8f20-7a1af50cd7f8" />

---

## 👑 15. La Cúspide de la Pirámide: TTPs y el Abandono del Atacante

¡Jaque mate a las herramientas de Sphinx! Nuestra regla comportamental funciona a la perfección. Al recibir nuestro bloqueo, Sphinx nos envía un correo donde confiesa su derrota técnica. 

Obligado a crear herramientas desde cero (lo que supone una inmensa inversión de tiempo y presupuesto) o a volver a formarse en nuevas metodologías, el atacante admite que **la recompensa ya no compensa el coste de la intrusión**. En un escenario real, aquí es donde el adversario se rinde y busca una víctima con peores defensas. Recibimos nuestra flag:

`THM{46b21c4410e47dc5729ceadef0fc722e}`

**El desafío final (sample6.exe):**
Para su último intento, Sphinx nos lleva a la mismísima punta de la Pirámide del Dolor: los **TTPs (Tácticas, Técnicas y Procedimientos)**. Esta vez no se trata de detectar sus herramientas, sino su *forma de actuar* humana ("algo extremadamente difícil de cambiar subconscientemente"). Para ello nos entrega el archivo `commands.log`, donde tendremos que perfilar al atacante analizando qué comandos suele ejecutar manualmente cuando vulnera un sistema.

> **Captura 15: Correo de rendición de Sphinx y entrega del log de comandos**
> <img width="1884" height="647" alt="image" src="https://github.com/user-attachments/assets/8667b855-867c-4fb5-bb38-d76c58f67642" />


## 🧠 16. La Cima de la Pirámide: Detectando TTPs (sample6.exe)

Para el desafío final, Sphinx nos advirtió de que tendríamos que detectar su forma de operar a nivel humano: sus Tácticas, Técnicas y Procedimientos (TTPs). Los atacantes, por muy avanzados que sean, tienen "manías" o hábitos a la hora de moverse por un sistema comprometido.

Aunque podíamos perfilar los comandos que suele escribir manualmente, decidimos analizar `sample6.exe` en el **Malware Sandbox** para buscar el reflejo de esos procedimientos humanos automatizados en la herramienta.

Al observar la actividad de archivos (Files Activity), detectamos un procedimiento clásico de exfiltración de datos. El atacante utiliza comandos del sistema (`cmd.exe`) para volcar información y guardarla temporalmente en un archivo antes de enviarla a su servidor. 

Este comportamiento repetitivo genera el siguiente artefacto/procedimiento:
* **Ruta:** `%temp%` (Carpeta de archivos temporales del usuario).
* **Archivo:** `exfiltr8.log`

> **Captura 16: Análisis de sample6.exe revelando la técnica de almacenamiento para exfiltración**
> <img width="1240" height="871" alt="image" src="https://github.com/user-attachments/assets/b6c043a6-9a5d-4e38-9fd2-7205aa6dd037" />

---

## 🎯 17. Regla de Detección de Procedimientos (MITRE ATT&CK: Discovery)

Sabemos que este atacante en particular siempre usa ese nombre de archivo (`exfiltr8.log`) en ese directorio específico cuando intenta recopilar datos. En lugar de bloquear la red, vamos a detectar su **Técnica**.

Usando el **Sigma Rule Builder**, creamos una regla centrada en la "Creación y Modificación de Archivos" (File Creation and Modification). 
* **File Path:** `%temp%`
* **File Name:** `exfiltr8.log`

Mapeamos esta regla con la táctica **Discovery (TA0007)** del framework MITRE ATT&CK, ya que el atacante está en la fase de descubrir y empaquetar información del sistema.

> **Captura 17: Creación de regla Sigma para detectar el procedimiento de exfiltración**
> <img width="1011" height="855" alt="image" src="https://github.com/user-attachments/assets/b7bd4fd9-0da7-445c-aef6-8eea493618e3" />
> <img width="1054" height="780" alt="image" src="https://github.com/user-attachments/assets/4c3d4f56-79c2-4d74-bdfe-dafa11f18ef1" />

---

## 🏁 18. Conclusión y Victoria Final

La detección a nivel de TTPs es el golpe definitivo. Al bloquear la forma misma en la que el atacante trabaja y recopila datos, le quitamos todo su poder. 

Recibimos un último correo de Sphinx titulado *"I'm Giving Up" (Me rindo)*. Confiesa que lo hemos llevado a lo más alto de la Pirámide del Dolor y que no es un lugar divertido para un atacante. Para poder seguir atacándonos, tendría que volver a formarse por completo y cambiar sus rutinas subconscientes. El coste es inasumible. Sphinx se retira y nos entrega la flag final:

`THM{c8951b2ad24bbcbac60c16cf2c83d92c}`

> **Captura 18: El atacante se rinde y obtenemos la flag final**
> <img width="1862" height="551" alt="image" src="https://github.com/user-attachments/assets/1f6f7934-f164-4aca-9883-0349945eaaee" />

---

## 🎓 Resumen de Habilidades Demostradas (Para Portfolio)
A través de la resolución de esta máquina, se han puesto en práctica las siguientes competencias clave de Blue Team:
1. **Análisis de Malware (Sandbox):** Extracción de IoCs (Hashes, IPs, Dominios).
2. **Gestión de EDR/Firewall/DNS:** Implementación de reglas de bloqueo a distintos niveles de red y host.
3. **Ingeniería de Detección (Sigma):** Creación de reglas de Sysmon basadas en el comportamiento.
4. **Threat Hunting:** Identificación de patrones anómalos de red (Beaconing).
5. **Framework MITRE ATT&CK:** Mapeo de alertas y tácticas (Defense Evasion, C2, Discovery).
6. **Aplicación Práctica de la "Pyramid of Pain":** Comprensión profunda de cómo aplicar defensas que maximicen el coste para el adversario.















