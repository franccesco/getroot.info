---
layout: post
title: 'Masscan Web UI: Plataforma Web Para Masscan'
featured: true
date: '2017-07-18 14:29:50'
tags:
- masscan
- footprinting
- scanning
- info-gathering
---

Hace poco estaba jugando con la excelente herramienta de [**masscan**](https://github.com/robertdavidgraham/masscan), la cual sirve para hacer escaneos en masa de rangos de direcciones IP. De tal modo que si tienes la velocidad de internet correcta puedes escanear todo el espacio de internet en alrededor de **6 minutos**.

> MASSCAN: Mass IP port scanner

>This is the fastest Internet port scanner. It can scan the entire Internet in under 6 minutes, transmitting 10 million packets per second.

>It produces results similar to nmap, the most famous port scanner. Internally, it operates more like scanrand, unicornscan, and ZMap, using asynchronous transmission. The major difference is that it's faster than these other scanners. In addition, it's more flexible, allowing arbitrary address ranges and port ranges.

El problema es que al analizar las direcciones IP y sus puertos, se nos presenta información desordenada. Así que [Offensive Security](https://github.com/offensive-security/masscan-web-ui) se encargó de **hacer un webapp** que nos permite hacer una búsqueda de mas fácil y presentar la información de masscan de manera mas ordenada.

**Lo que use para la instalación fue:**

* Ubuntu 16.04.2 x64 (Pueden ser otras versiones u otro sistema operativo)
* Los paquetes: *apache2 php libapache2-mod-php php-mysqli php-xml mysql-server*
* Repo: *https://github.com/offensive-security/masscan-web-ui.git*

*Nota: Los paquetes pueden variar según el O.S. y la versión del mismo que tengas.*

Los primeros pasos son simples, según el repo de Offensive Security:

**Instalamos paquetes principales:**

También clonamos el repositorio y lo movemos a `/var/www/html`
```
root@kali:~# apt-get install apache2 php libapache2-mod-php php-mysqli php-xml mysql-server
root@kali:~# git clone https://github.com/offensive-security/masscan-web-ui
root@kali:~# mv masscan-web-ui/* /var/www/html/
root@kali:~# cd /var/www/html/
```

**Configuramos la base de datos (masscan):**

Creamos el usuario `masscan` con la contraseña `changem3` (la cual deberías cambiar). Le damos privilegios e importamos la base de datos `db-structure-mysql.sql`
```
root@kali:/var/www/html# mysql -u root -p
mysql> create database masscan;
mysql> CREATE USER 'masscan'@'localhost' IDENTIFIED BY 'changem3';
mysql> GRANT ALL PRIVILEGES ON masscan.* TO 'masscan'@'localhost';
mysql> exit
root@kali:/var/www/html# mysql -u root -p masscan < db-structure-mysql.sql 
root@kali:/var/www/html# rm db-structure-mysql.sql index.html README.md
```

**Configuración del WebApp:**

`nano /var/www/html/config.php` y cambiamos las opciones que nos parezcan necesarias, e.g. la contraseña `changem3` si la cambiaste anteriormente.
```
nano config.php
define('DB_DRIVER',     'mysql');
define('DB_HOST',       'localhost');
define('DB_USERNAME',   'masscan');
define('DB_PASSWORD',   'changem3');
define('DB_DATABASE',   'masscan');
```
**Demostración:**
<script type="text/javascript" src="https://asciinema.org/a/MJz81Fi2ntjMd6ApCdW52XSHA.js" id="asciicast-MJz81Fi2ntjMd6ApCdW52XSHA" async></script>

-----------
#Escaneo con Masscan

Primero instalamos masscan ya sea desde [el repositorio](https://github.com/robertdavidgraham/masscan) o desde los paquetes de nuestra máquina:

`apt install masscan`

Luego procedemos a hacer un escaneo, aunque debemos tomar en cuenta nuestra velocidad de conexión y configurar el flag `--max-rate` a como se propio.

En mi caso como mi internet no es la gran cosa, y además prefiero seguridad a velocidad, lo fijare en un máximo de 2000 para un rango pequeño de CIDR /16:

`masscan x.x.x.x/16 -p80,21,53 --banners --max-rate 2000 -oX scan.xml`

**Desglose de opciones:**

* **masscan x.x.x.x/16:** Rango de direcciones IP a escanear.
* **-p80,21,53:** Los puertos que quiero escanear. En este caso los puertos 80 (http), 21 (ftp) y 53 (dns).
* **--banners:** Recopilar el mensaje del servicio en cuestion, bastante útil para darnos una noción de lo que el servicio aloja, así como títulos de páginas web.
* **--max-rate:** La velocidad del escaneo. Esto esta ligado a nuestra conexión de internet, así que si pones un `max-rate` demasiado rápido tendrás problemas capturando los paquetes y no descubrir muchos hosts.
* **-oX:** Crear una salida de archivo en formato XML para poder importarlo en nuestro webapp.

-----------------

# Importar el escaneo a Masscan Web UI:

Esta opción es bastante fácil. De nuestro archivo XML, en este caso `scan.xml`, solamente importamos el scan a la base de datos.

Nos posicionamos en el directorio `/var/www/html` y:

`root@kali:/var/www/html# php import.php scan.xml`

Y listo! Ya integramos los hosts y sus banners encontrados en la base de datos. Puedes ver que **podemos filtrar por servicio, puertos, titulos, etc:**

![](/content/images/2017/07/cap2.PNG)

**Demostración:**
# <script type="text/javascript" src="https://asciinema.org/a/s8GblzRGLV1bbVGpE2Dz8gztL.js" id="asciicast-s8GblzRGLV1bbVGpE2Dz8gztL" async></script>

#Nota:

Si te encuentras con este problema:

![](/content/images/2017/07/Capture-2.PNG)

Es sencillo componerlo, simplemente reiniciamos apache2: `service apache2 restart`

Y eso sería todo en la entrega de hoy, si quieres aportar algo mas puedes sentirte libre para comentar abajo!

Como siempre, *Happy Hacking.*

*No me hago responsable del daño que puedas causar ya que esta entrada es con fines educativos solamente.*