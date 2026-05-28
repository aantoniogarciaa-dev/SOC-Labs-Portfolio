🔎 Análisis de Exfiltración DNS con Splunk

En esta fase del laboratorio se utilizó Splunk para analizar los registros DNS y detectar posibles intentos de exfiltración de datos mediante DNS Tunneling.

📌 Búsqueda inicial de tráfico DNS

Primero se realizó una búsqueda para visualizar todos los eventos relacionados con DNS dentro del índice proporcionado:

index=data_exfil sourcetype=DNS_logs

Esta consulta mostró aproximadamente 2.000 eventos DNS registrados en el entorno del laboratorio.


<img width="1776" height="1006" alt="image" src="https://github.com/user-attachments/assets/8ec77b0c-1631-43a6-bb1f-9a6f74b51c43" />

🧠 Identificación del dominio sospechoso

Tras analizar las consultas DNS, se observaron múltiples peticiones con nombres extremadamente largos y aleatorios, algo muy característico del DNS Tunneling.

Entre las consultas destacaban dominios como:

036er95gzqeabe3ymiht.tunnelcorp.net
0apj5ix7azdeikfa5wjt.tunnelcorp.net
0fgvpdsiybcbma94rq7e.tunnelcorp.net

Estos subdominios parecen contener datos codificados enviados mediante peticiones DNS hacia un único dominio externo.


<img width="585" height="139" alt="image" src="https://github.com/user-attachments/assets/5a2bd32d-07ca-4d13-ac24-57c56ac59146" />

🚨 Dominio malicioso identificado

Después del análisis se consiguió identificar el dominio sospechoso encargado de recibir el tráfico DNS:

tunnelcorp.net

Este comportamiento es típico de un ataque de exfiltración de datos mediante DNS Tunneling, donde la información robada se divide en fragmentos y se envía oculta dentro de consultas DNS.


<img width="633" height="190" alt="image" src="https://github.com/user-attachments/assets/2a9661f7-23a0-499c-90d0-f83d23f83441" />

🔎 Filtrado del dominio sospechoso en Splunk

Una vez identificado el dominio sospechoso tunnelcorp.net, se aplicó un filtro específico en Splunk para mostrar únicamente los eventos relacionados con dicho dominio.

La búsqueda utilizada fue:

index=data_exfil sourcetype=DNS_logs
query="*tunnelcorp.net*"

<img width="1848" height="1144" alt="image" src="https://github.com/user-attachments/assets/8a4e99ee-d1a5-4f7b-bd6a-d6e5ee72b432" />


📊 Resultados obtenidos

El filtro devolvió un total de:

315 eventos

Esto confirmó que existía una gran cantidad de tráfico DNS sospechoso relacionado con el dominio malicioso.

Además, se pudo observar que múltiples hosts internos estaban realizando consultas DNS hacia este dominio externo, reforzando la evidencia de una posible exfiltración de datos mediante DNS Tunneling.

🚨 Indicadores observados

Durante el análisis se identificaron varios indicadores típicos de actividad maliciosa:

🌍 Comunicación repetitiva con un único dominio externo.
📈 Alto volumen de consultas DNS.
🔤 Subdominios largos y aparentemente aleatorios.
🕵️ Posible fragmentación de datos dentro de consultas DNS.

<img width="618" height="149" alt="image" src="https://github.com/user-attachments/assets/3a97abb2-4db4-40ec-9443-1e699467aa68" />

🖥️ Identificación del host más comprometido

Después de confirmar la existencia de tráfico DNS sospechoso hacia el dominio tunnelcorp.net, el siguiente paso consistió en identificar qué equipo interno estaba generando la mayor cantidad de peticiones maliciosas.

Para ello, se analizaron los valores del campo src_ip dentro de Splunk.

<img width="1145" height="997" alt="image" src="https://github.com/user-attachments/assets/2e82a9f2-4a26-4cf6-9ff8-5c36136f3ca3" />


📊 Resultados del análisis

El análisis mostró las siguientes direcciones IP y su cantidad de solicitudes DNS sospechosas:

Dirección IP	Número de eventos
192.168.1.103	72
192.168.1.105	63
192.168.1.104	62
192.168.1.102	60
192.168.1.101	58

La dirección IP que generó el mayor volumen de tráfico sospechoso fue:

192.168.1.103
🚨 Interpretación

El host 192.168.1.103 destacó por ser el equipo que más consultas DNS sospechosas realizó hacia el dominio malicioso.

Esto puede indicar que:

🦠 El sistema estaba comprometido.
📤 Estaba participando activamente en la exfiltración de datos.
🌐 Utilizaba DNS Tunneling como canal de comunicación encubierto.

El comportamiento observado es típico de malware que divide información en pequeños fragmentos y los transmite mediante consultas DNS aparentemente legítimas.

✅ Confirmación

Finalmente, la plataforma validó correctamente la IP identificada como el host más activo durante el ataque:

192.168.1.103

<img width="634" height="161" alt="image" src="https://github.com/user-attachments/assets/dd24675a-b7cb-4304-a11d-419f9c792783" />



🌐 Análisis de conexiones FTP sospechosas con Wireshark

En esta fase del laboratorio se analizó tráfico FTP utilizando Wireshark con el objetivo de detectar conexiones sospechosas relacionadas con posibles actividades de exfiltración de datos.

🔎 Filtrado de conexiones FTP

Para identificar accesos realizados con la cuenta guest, se aplicó el siguiente filtro en Wireshark:

ftp.request.arg == "guest"

Este filtro permitió visualizar todas las peticiones FTP donde se utilizó el usuario guest para autenticarse contra el servidor FTP.

<img width="1004" height="328" alt="image" src="https://github.com/user-attachments/assets/737fbfaf-f180-4f83-82bd-c54e59ac9b30" />


📊 Resultados obtenidos

El análisis mostró múltiples conexiones FTP provenientes de diferentes hosts internos hacia la dirección externa:

185.203.119.12

Las conexiones detectadas correspondían a peticiones:

Request: USER guest

En total, se identificaron:

5 conexiones

realizadas utilizando la cuenta guest.

🚨 Interpretación

El uso repetido de la cuenta guest puede ser un fuerte indicador de:

🔓 Uso de credenciales débiles o anónimas.
📤 Transferencias sospechosas de archivos.
🕵️ Actividad de exfiltración mediante FTP.
🌐 Comunicación hacia infraestructura externa potencialmente maliciosa.

Además, varios hosts internos participaron en las conexiones, lo que podría indicar múltiples sistemas comprometidos dentro de la red.

✅ Confirmación

La plataforma validó correctamente que el número total de conexiones observadas desde la cuenta guest fue:

5

<img width="616" height="150" alt="image" src="https://github.com/user-attachments/assets/7830a929-5587-4ecf-bd10-22c34dbc39cf" />

📤 Detección de exfiltración de archivos mediante FTP

En esta parte del laboratorio se continuó investigando la actividad FTP sospechosa utilizando Wireshark para identificar posibles archivos exfiltrados desde cuentas privilegiadas.

🔎 Filtrado de autenticaciones con la cuenta root

Para localizar conexiones realizadas con el usuario root, se aplicó el siguiente filtro en Wireshark:

ftp contains "root"

Este filtro permitió mostrar las sesiones FTP donde se utilizó la cuenta root para autenticarse contra el servidor remoto.

<img width="1001" height="628" alt="image" src="https://github.com/user-attachments/assets/7ff5ce33-7195-43ec-a70c-d292941f81e9" />


🧠 Análisis de la sesión FTP

Al inspeccionar los paquetes FTP, se observó la siguiente secuencia:

USER root
PASS toor
STOR customer_data.xlsx

La instrucción:

STOR customer_data.xlsx

indica que un archivo fue subido al servidor FTP remoto.

En FTP, el comando STOR se utiliza para almacenar o transferir archivos al servidor, lo que representa una posible acción de exfiltración de información.

🚨 Archivo exfiltrado identificado

El archivo relacionado con clientes que fue exfiltrado desde la cuenta root fue:

customer_data.xlsx

Este tipo de archivo puede contener:

👥 Información de clientes.
📧 Correos electrónicos.
💳 Datos sensibles.
📊 Información corporativa confidencial.

La presencia de este tráfico sugiere una posible fuga de datos hacia infraestructura externa.

✅ Confirmación

La plataforma confirmó correctamente que el archivo exfiltrado fue:

customer_data.xlsx

<img width="628" height="169" alt="image" src="https://github.com/user-attachments/assets/eb5dfa02-3672-41f4-85ca-e40ff35b3ac8" />


📡 Identificación del host con mayor volumen de exfiltración FTP

En esta fase del análisis se investigó qué equipo interno estaba enviando la mayor cantidad de datos hacia una dirección IP externa mediante el protocolo FTP.

🔎 Filtrado de paquetes FTP grandes

Para detectar posibles transferencias de archivos o cargas de datos sospechosas, se aplicó el siguiente filtro en Wireshark:

ftp && frame.len > 90

Este filtro permitió mostrar únicamente paquetes FTP con un tamaño superior a 90 bytes, facilitando la identificación de transferencias potencialmente relacionadas con exfiltración de información.

<img width="1008" height="636" alt="image" src="https://github.com/user-attachments/assets/ef42bbee-4ed5-4329-9f7a-a060f60fd5dd" />


🧠 Análisis del tráfico detectado

Durante la inspección de los paquetes FTP se observaron múltiples autenticaciones y transferencias realizadas por distintos hosts internos hacia la dirección externa:

185.203.119.12

Uno de los eventos más sospechosos mostraba la siguiente secuencia:

USER guest
PASS guest
STOR internal_passwords.csv

La instrucción:

STOR internal_passwords.csv

indica que se estaba subiendo un archivo sensible al servidor FTP remoto.

Además, el análisis reveló que la IP:

192.168.1.105

era la que estaba enviando los paquetes de mayor tamaño hacia el servidor externo.

🚨 Posible exfiltración de credenciales

El nombre del archivo detectado:

internal_passwords.csv

sugiere que podría contener:

🔑 Contraseñas internas.
👥 Credenciales corporativas.
🖥️ Información sensible de sistemas.
🕵️ Datos utilizados para movimientos laterales dentro de la red.

La transferencia de este tipo de archivos representa un fuerte indicador de compromiso y exfiltración de información crítica.

✅ Confirmación

Finalmente, se confirmó que la dirección IP interna que enviaba el mayor payload hacia una IP externa era:

192.168.1.105

<img width="620" height="141" alt="image" src="https://github.com/user-attachments/assets/c52487de-15d3-4b68-9b0c-9a223e553d41" />


🚩 Extracción de la flag oculta en el flujo FTP

En la última fase del laboratorio se analizó el flujo FTP utilizado para transferir el archivo CSV sospechoso hacia la dirección IP externa detectada anteriormente.

El objetivo era identificar información oculta dentro del tráfico FTP y recuperar la flag del laboratorio.

🔎 Filtrado de tráfico relacionado con archivos CSV

Para localizar las transferencias relacionadas con archivos CSV, se aplicó el siguiente filtro en Wireshark:

ftp contains "csv"

Este filtro permitió identificar rápidamente las sesiones FTP donde se estaba transfiriendo el archivo sospechoso:

internal_passwords.csv

<img width="1001" height="623" alt="image" src="https://github.com/user-attachments/assets/5958b780-ef9a-4a95-8686-2c4dee72cd03" />


🧠 Inspección del flujo FTP

Al analizar el contenido del paquete FTP, se observó la siguiente secuencia:

USER guest
PASS guest
STOR internal_passwords.csv

Debajo de la transferencia apareció un campo DATA que contenía información oculta dentro del flujo FTP:

DATA: THM{ftp_exfil_hidden_flag}

Esto confirmó que la flag había sido enviada junto con la transferencia del archivo CSV hacia el servidor remoto.

🚨 Hallazgo final

La flag encontrada dentro del flujo FTP fue:

THM{ftp_exfil_hidden_flag}

Este hallazgo demuestra cómo los atacantes pueden ocultar información sensible dentro de transferencias aparentemente legítimas utilizando protocolos comunes como FTP.

<img width="629" height="170" alt="image" src="https://github.com/user-attachments/assets/c001aa5d-9a8d-4be0-bc8c-f9f34549fa98" />

🌐 Análisis de exfiltración de datos mediante HTTP

En esta última parte del laboratorio se investigó tráfico HTTP sospechoso con el objetivo de detectar posibles transferencias de información sensible hacia una infraestructura externa.

El análisis se realizó utilizando tanto Splunk como Wireshark.

🔎 Identificación de tráfico HTTP sospechoso en Splunk

Primero, se ejecutó una búsqueda en Splunk para localizar peticiones HTTP POST con un tamaño elevado de datos enviados.

La consulta utilizada fue:

index="data_exfil" sourcetype="http_logs" method=POST bytes_sent > 600 
| table _time src_ip uri domain dst_ip bytes_sent 
| sort - bytes_sent

Esta búsqueda permitió identificar tráfico HTTP potencialmente relacionado con exfiltración de datos.

<img width="1849" height="591" alt="image" src="https://github.com/user-attachments/assets/23c9ec34-64f0-4d16-ac18-628cebdb1c09" />


📊 Resultados obtenidos

El análisis reveló una petición HTTP POST sospechosa con las siguientes características:

Campo	Valor
IP origen	192.168.1.103
URI	/v1/sync/upload
Dominio	api.cloudsync-services.com
IP destino	185.203.119.12
Bytes enviados	654

La dirección IP interna identificada como comprometida fue:

192.168.1.103
🧠 Análisis del flujo HTTP en Wireshark

Después de identificar el tráfico sospechoso en Splunk, se analizó el flujo HTTP en Wireshark utilizando el siguiente filtro:

tcp.stream eq 3586

Esto permitió reconstruir completamente la conversación HTTP entre el host comprometido y el servidor externo.

<img width="611" height="143" alt="image" src="https://github.com/user-attachments/assets/f0dd4da1-de1f-4adb-8378-03eed49b2f3a" />


🚨 Información sensible detectada

Dentro del flujo HTTP se encontraron múltiples datos sensibles transmitidos en texto plano:

👤 Credenciales internas.
🔑 Contraseñas.
🌐 Información de VPN.
🗄️ Datos de conexión a bases de datos.
🔒 Huellas SSH.
📝 Notas internas confidenciales.

Parte del contenido detectado incluía:

Username: finance_admin
Password: F!n@nc3#2025

y también:

User: db_reader
Password: R3@d0nly!2025
🚩 Recuperación de la flag

Al continuar inspeccionando el contenido del flujo HTTP, se encontró la flag oculta dentro de los datos exfiltrados:

THM{http_raw_3xf1ltr4t10n_succ3ss}

<img width="863" height="1197" alt="image" src="https://github.com/user-attachments/assets/697ae0f6-eba3-433b-9ce8-63239b7e89b1" />


✅ Confirmación

La plataforma confirmó correctamente la flag obtenida durante el análisis:

THM{http_raw_3xf1ltr4t10n_succ3ss}

<img width="619" height="155" alt="image" src="https://github.com/user-attachments/assets/72b50e6a-ae3d-4e14-92ba-8f7c88ac4ab0" />


📡 Exfiltración de datos mediante ICMP

En la fase final del laboratorio se investigó tráfico ICMP sospechoso para detectar posibles intentos de exfiltración de información utilizando paquetes Ping.

Aunque ICMP normalmente se utiliza para tareas de diagnóstico de red, los atacantes también pueden abusar de este protocolo para ocultar transferencias de datos y evitar ciertos controles de seguridad.

🔎 Filtrado de paquetes ICMP sospechosos

Para identificar paquetes ICMP con cargas de datos anormalmente grandes, se aplicó el siguiente filtro en Wireshark:

icmp.type == 8 and frame.len > 100

Este filtro mostró únicamente solicitudes ICMP Echo Request (ping) con tamaños superiores a 100 bytes, algo poco habitual en tráfico normal de red.

<img width="789" height="793" alt="image" src="https://github.com/user-attachments/assets/0ed26fb3-af38-4051-86a1-c99279e20524" />


🧠 Análisis de los paquetes ICMP

Durante la inspección de los paquetes se observó que el campo Data contenía información legible en texto plano.

Dentro del payload ICMP aparecía información sensible junto con una flag oculta:

THM{icmp_3ch0_3xf1ltr4t10n_succ3ss}

Esto confirmó que los datos estaban siendo exfiltrados mediante el payload de paquetes ICMP Echo Request.

🚨 Indicadores de exfiltración ICMP

Los principales indicadores detectados fueron:

📦 Paquetes ICMP con tamaños inusualmente grandes.
📝 Datos en texto plano dentro del payload.
🌐 Comunicación repetitiva hacia la IP:
10.0.0.254
🕵️ Uso de ICMP como canal encubierto para transmitir información.

Este tipo de técnica es común en ataques avanzados, ya que ICMP suele estar permitido en muchas redes corporativas y raramente es inspeccionado en profundidad.

🚩 Flag recuperada

La flag encontrada dentro de los datos exfiltrados mediante ICMP fue:

THM{icmp_3ch0_3xf1ltr4t10n_succ3ss}

<img width="616" height="205" alt="image" src="https://github.com/user-attachments/assets/a691db9b-10c8-458d-aacb-223ae57af5bc" />


🏁 Conclusión final del laboratorio

A lo largo de este laboratorio se analizaron diferentes técnicas utilizadas para la exfiltración de datos mediante protocolos comunes de red:

Protocolo	Técnica detectada
DNS	DNS Tunneling
FTP	Transferencia de archivos sensibles
HTTP	Exfiltración mediante peticiones POST
ICMP	Datos ocultos en paquetes Echo Request

Gracias al uso de herramientas como:

🦈 Wireshark
📊 Splunk

fue posible:

Detectar tráfico anómalo.
Identificar hosts comprometidos.
Reconstruir sesiones de red.
Analizar payloads sospechosos.
Recuperar información sensible y flags ocultas.

Este laboratorio demuestra cómo protocolos aparentemente legítimos pueden ser utilizados como canales encubiertos para la fuga de información dentro de entornos corporativos.


