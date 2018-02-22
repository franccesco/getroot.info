---
layout: post
title: Habilitar Servicios Permanentemente en Kali Linux
date: '2017-06-15 17:15:33'
tags:
- kali
---

Uno de los problemas que veo constantemente es que los servicios en Kali Linux, tales como apache2, postgresql, etc., no son habilitados por defecto al iniciar la maquina, y tampoco persisten en reinicios.

Esto es asi por defecto, segun las politicas de seguridad de Kali:

> Kali Linux is a penetration testing toolkit, and may potentially be used in “hostile” environments. Accordingly, Kali Linux deals with network services in a very different way than typical Linux distributions. Specifically, **Kali does not enable any externally-listening services by default** with the goal of minimizing exposure when in a default state.

-- snip --

> Kali Linux, as a standard policy, **will disallow network services from persisting across reboots by default**.

Por defecto, siendo Kali un arma de penetración, desactiva los servicios permanentes por defecto, incluso al instalar nuevas aplicaciones que requieren servicios.

Para habilitar un servicio que persista en reinicios podemos usar la herramienta `systemctl`:

La sintaxis es:
`systemctl <enable|disable> <servicio>`

De esta manera, si queremos habilitar SSH entre reinicios entonces seria:
`systemctl enable ssh`

Aqui hay un pequeño ejemplo:
<center><script type="text/javascript" src="https://asciinema.org/a/edx3ywczwiqqllx7wgsc58a2g.js" id="asciicast-edx3ywczwiqqllx7wgsc58a2g" async></script></center>