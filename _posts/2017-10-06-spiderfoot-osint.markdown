---
layout: post
title: 'Spiderfoot: Automatización Y Recolección De Información De Objetivos'
date: '2017-10-06 19:08:01'
tags:
- footprinting
- info-gathering
- framework
- tools
- discovery
- spiderfoot
---

![Screenshot-2017-10-6-SpiderFoot---Open-Source-Intelligence-Automation-](/images/screenshots/Screenshot-2017-10-6-SpiderFoot---Open-Source-Intelligence-Automation-.png)

> SpiderFoot is an open source intelligence automation tool. Its goal is to automate the process of gathering intelligence about a given target, which may be an IP address, domain name, hostname or network subnet.

Spiderfoot es una herramienta de obtención de inteligencia en medios abiertos, en palabras en simple español: nos permite recavar información vital de nuestros objetivos.

![Screenshot-2017-10-6-SpiderFoot-v2-11](/images/screenshots/Screenshot-2017-10-6-SpiderFoot-v2-11.png)

Desde direcciones IP, DNS's, Status Codes, Links Externos, hasta la tecnologia que usamos en el backend de nuestros servidores.

Toda esta información nos es útil para planear un vector de ataque a nuestro objetivo, vectores de ingeniería social e incluso HUMINT (Human Intelligence) para definir las entidades humanas que trabajan en el objetivo.

![Screenshot-2017-10-6-SpiderFoot-v2-11-1-](/images/screenshots/Screenshot-2017-10-6-SpiderFoot-v2-11-1-.png)

Hay tres categorias en la cual definimos las políticas o reglas a las cuales SpiderFoot se ajustará para escanear los objetivos: **By Use Case - By Required Data - By Module**

## By Use Case

![Screenshot-2017-10-6-SpiderFoot-v2-11-2-](/images/screenshots/Screenshot-2017-10-6-SpiderFoot-v2-11-2-.png)

En este caso miramos 4 casos de uso:
* **All**: Todos los modulos de SpiderFoot estaran habilitados, este caso de uso es el *mas* recursivo, pero tardara mucho en completarse. Similar al L3 en el PTES, Full Scope.
* **Footprint**: Este caso de uso te da una comprensión de el perimetro del blanco, identidades, web crawling y uso de motores de búsqueda. Similar al L2 en PTES, la recomendación general.
* **Investigate**: Obtiene un footprinting basico del blanco, chequeando si la información del blanco (como su dirección IP) esta en alguna lista negra por malware o en alguna lista que de información general del blanco. Conjunto de L1/L2 en PTES.
* **Passive**: Cuando no queremos que el blanco sospeche que esta siendo investigado. Obtiene información a través de terceros sin comprometerse con los servidores principales. Similar al L1 en PTES.

## By Required Data

![Screenshot-2017-10-6-SpiderFoot-v2-11-3-](/images/screenshots/Screenshot-2017-10-6-SpiderFoot-v2-11-3-.png)

Analiza los objetivos dependiento de la información que requieras sobre ellos, por ejemplo Defacements, Domain Names, Device Type, Co-Hosted Sites, etc.

## By Module

![Screenshot-2017-10-6-SpiderFoot-v2-11-4-](/images/screenshots/Screenshot-2017-10-6-SpiderFoot-v2-11-4-.png)

En esta categoria eliges los modulos que quieres que corran en SpiderFoot, si no te gusta algun modulo o no lo necesitas por ahora o lo consideras muy intrusivo entonces aqui puedes definir cuales quieres y cuales no.

## Settings

![Screenshot-2017-10-6-SpiderFoot-v2-11-5-](/images/screenshots/Screenshot-2017-10-6-SpiderFoot-v2-11-5-.png)

Es muy importante que antes de correr SpiderFoot revises la configuración para que todo vaya en orden. Por ejemplo el HTTP User-Agent header el cual se enviara a todos los dispositivos que tengan HTTP(S) activado y llaves API.

## Instalación

### Requisitos de instalación con `pip` y `apt`:

```
~$ apt install install python-m2crypto libssl-dev libxml2-dev libxml2
~$ pip install lxml netaddr M2Crypto cherrypy mako requests bs4
```

### Instalación de SpiderFoot con PTF

Es muchísimo mas fácil [instalar tus programas con PTF](https://getroot.info/2017/09/10/the-pentesters-framework-ptf-instalar-y-mantener-herramientas-de-pentesting/)
Aquí te dejo un cast de como instalarlo con PTF:

# <script type="text/javascript" src="https://asciinema.org/a/0dQCZBuIX4FSnxqDU2WPx3075.js" id="asciicast-0dQCZBuIX4FSnxqDU2WPx3075" async></script>

Y para correrlo solo vamos al directorio de instalación y lo ejecutamos:
# <script type="text/javascript" src="https://asciinema.org/a/8KF6kidFSNXVRm24djktyfCug.js" id="asciicast-8KF6kidFSNXVRm24djktyfCug" async></script>

*En este cast ejecute `./sf.py` con la direccion ip 0.0.0.0 (global) y el puerto 80, esto no debería ser asi ya que expones el servidor en cualquier dirección IP que tengas asignado, como buena practica ejecuta `./sf.py` sin opciones o define cambia el puerto en una dirección IP asegurada o localhost (i.e 127.0.0.1)*

Mas información: http://www.spiderfoot.net/documentation/