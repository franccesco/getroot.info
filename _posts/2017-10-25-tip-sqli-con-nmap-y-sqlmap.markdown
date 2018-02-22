---
layout: post
title: 'Tip: SQLi con Nmap y SQLMap'
date: '2017-10-25 14:23:23'
tags:
- vuln
- scanning
- tip
- nmap
- howto
- tools
- sqlmap
- sqli
---

Un tip muy facil que nos viene bien hacer es encontrar vulnerabilidades SQL rapidamente con Nmap con el script NSE `http-sql-injection` para luego pasarlos a SLQMap. Este script nos encontrara los posibles agujeros que podria tener el objetivo para realizar ataques SQLi.

# <script type="text/javascript" src="https://asciinema.org/a/vvoBLnRI0d7ibCuOnrkzFFFBW.js" id="asciicast-vvoBLnRI0d7ibCuOnrkzFFFBW" async></script>

--------------------------------------------------

Tambien podemos hacer esto solamente usando SQLMap, pero no funciona bien para listas de objetivos. Con las opciones `--crawl` le decimos a la herramienta que pruebe las peticiones GET y POST de la pagina web y con `--batch` saltamos las preguntas que nos hace SQLMap a los que estan por defecto:

# <script type="text/javascript" src="https://asciinema.org/a/yQR5uuTxcmC2Y9Vli2h9Kdo96.js" id="asciicast-yQR5uuTxcmC2Y9Vli2h9Kdo96" async></script>