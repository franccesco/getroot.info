---
layout: post
title: 'Docker de Kali Linux: Metasploit en 5 minutos.'
date: '2017-06-14 19:51:34'
tags:
- metasploit
- kali
- docker
---

Una demostración rapida de como desplegar un servidor con Docker y Kali Linux para comprometer una red rapidamente con Metasploit.

Requisitos:

* Docker
* Cerebro

Comandos:
```
docker pull kalilinux/kali-linux-docker
docker run -t -i kalilinux/kali-linux-docker /bin/bash
```

Dentro de la maquina corremos:
```
apt update
apt install metasploit-framework
service postgresql start
msfdb init
msfconsole
```

Demo:
<center><script type="text/javascript" src="https://asciinema.org/a/aa733eowe298i8cgld1xvomzf.js" id="asciicast-aa733eowe298i8cgld1xvomzf" async></script></center>
---------------------