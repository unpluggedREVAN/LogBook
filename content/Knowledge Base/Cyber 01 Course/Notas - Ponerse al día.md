## Montar lab Kali:
Link: https://youtu.be/Vg9zyqZzDeM?si=i-c3UfhJAKbjLSmn

## Clases grabadas - Cyber 01
Link: https://netorgft13965927-my.sharepoint.com/:f:/g/personal/hawks_hawksec-academy_com/Eqm4QRWSQOxDvYM0yUudAfIBM3mcxvf5oPiVMg7s92nTcQ?e=tKyrgF

### Clase 21 de noviembre
* https://netorgft13965927-my.sharepoint.com/:v:/g/personal/hawks_hawksec-academy_com/EUcTUPEKtpxCtZiIfjAFwKMB-tv7D1Kx8almM-U8EGPI_Q?e=gIEhly&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D

+ https://netorgft13965927-my.sharepoint.com/:v:/g/personal/hawks_hawksec-academy_com/Ebd8F0gByL9Kpqu9zE2it9EBFTEW_eICZW37OYITriE05A?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=J5jsY9
+ https://netorgft13965927-my.sharepoint.com/:v:/g/personal/hawks_hawksec-academy_com/EeJs6vi8cc5KmDLRRxXIX9YBv3uyvSgNEjWLslo3uFJqtw?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=wGbxsM

## Cheat sheets - Kali linux
Link: https://github.com/bhavesh-pardhi/KALI-CMDs/blob/main/100%20Kali%20linux%20Commands%20for%20Hackers.md

## Noticias ciber
* https://www.bleepingcomputer.com/
* https://thehackernews.com/
* https://krebsonsecurity.com/
* https://darknetdiaries.com/
* https://www.tierradehackers.com/

## Comunidad de ex alumnos de Hawks
* https://t.me/+fwPfty6B6msxYjAx

### Noticia
* https://bugs.xdavidhu.me/google/2021/01/11/stealing-your-private-videos-one-frame-at-a-time/
* https://www.bleepingcomputer.com/news/security/over-2-000-palo-alto-firewalls-hacked-using-recently-patched-bugs/
* https://cybersecuritynews.com/7-zip-vulnerability-arbitrary-code/

## Malware Traffic Analysis
* https://youtu.be/eQItiKZpuSc?t=0
* https://www.youtube.com/watch?v=ibSqqWZq9sk
* https://malware-traffic-analysis.net/2019/sharkfest/index.html

## DNS explained
* https://youtu.be/72snZctFFtA?si=b_DPAv9GSOhH47UI
* https://www.youtube.com/watch?v=nyH0nYhMW9M&t=14s
* https://www.youtube.com/watch?v=U-i_UDDYLxY
* https://www.youtube.com/watch?v=qhiyTH5B21A
* https://www.youtube.com/watch?v=jdKRx2BxSMs
* 

## TCP vs UDP
* https://youtu.be/uwoD5YsGACg?si=QEengm8tVNoCfR8d
* https://youtu.be/xfWK5GfpWpM?si=IOrEsRMjrHXwjpyl

## Implementación MOD Security AWS:
* ![[Implementacion_MOD_Security_AWS.docx.pdf]]
* ![[Pasted image 20250109132807.png]]
* ![[Pasted image 20250109132833.png]]
* ![[Pasted image 20250109133100.png]]
* Yo monté una EC2 con Amazon Linux y estos son los pasos que no he podido lograr:

- 5.2 Copiar las reglas al directorio de configuración de MOD Security.

En mi instancia, no encontré ningún archivo con el nombre modsecurity-crs dentro de usr/share. Corrí el ls -a por si estaba oculto pero nada. 

Hice un poco de research para ver dónde se ubica el directorio de configuración de MOD en CentOS para Apache y decía que era en etc/httpd/modsecurity.d. Si me meto a ese directorio, veo dos archivos:

1. activated_rules
2. modsecurity_crs_10_config.conf

Como vi el activated_rules ya dentro del config directory, asumí que no tengo que hacer el paso de copiar las reglas.

- 6.2 Habilitar el modo detección si no está activo (SecRuleEngine DetectionOnly)

Al principio pensé que esto se haría en el modsecurity_crs_10_config.conf que está en etc/httpd/modsecurity.d. Pero al abrir el archivo con Nano, me decía que cualquier cambio de la configuración del MOD Security lo tengo que hacer en el archivo modsecurity.conf-recommended. 

Volví a hacer research para ver dónde se debería encontrar este config file y supuestamente debería estar dentro de etc/modsecurity o usr/share/modsecurity. En ninguna de las dos opciones logré encontrar ese config file.

También leí que podría estar en los tmp pero no lo vi ni con ls -a

* https://github.com/owasp-modsecurity/ModSecurity/wiki/Reference-Manual-(v2.x)#user-content-Installation_Methods
## Modelo OSI
* https://www.youtube.com/watch?v=jdKRx2BxSMs

## Norcoreanos usan IA para robar crypto
* https://unaaldia.hispasec.com/2024/11/ciberdelincuentes-norcoreanos-emplean-ia-para-estafas-en-linkedin-robando-mas-de-10-millones-de-dolares-en-criptomonedas.html

## Extraño caso de servers rusos que desaparecen
* https://isc.sans.edu/diary/The+strange+case+of+disappearing+Russian+servers/31476

## Hackeo de RECOPE
* https://www.nacion.com/el-pais/recope-confirma-ataque-informatico-en-sistemas/P7L46UDTGFDM5OKUYBOVJL5CEE/story/

## Hackeo canal 6
* https://www.teletica.com/nacional/ministro-confirma-ciberataque-de-72-horas-contra-migracion_373237
* Gbm abiertos al público
* RaaS
* ![[Pasted image 20250109134315.png]]
* ![[Pasted image 20250109134449.png]]
* https://www.picussecurity.com/resource/blog/ransomhub-ransomware-cisa-alert-aa24-242a

## RansomHub
* https://go.recordedfuture.com/hubfs/reports/mtp-2024-0620.pdf
* https://ransomxifxwc5eteopdobynonjctkxxvap77yqifu2emfbecgbqdw6qd.onion - Datos cargados en TOR

## Deloitte Hacked
* https://cybersecuritynews.com/deloitte-hacked/

## DVWA
* ![[Pasted image 20250109161723.png]]
## Manipulación de trolls
* https://www.youtube.com/watch?v=LCIGB-qWZsE

