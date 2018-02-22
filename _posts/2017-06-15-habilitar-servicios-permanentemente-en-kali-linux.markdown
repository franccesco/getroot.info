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

Créditos de la imagen a: <a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="http://unsplash.com/@timdwright?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Tim Wright"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title></title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Tim Wright</span></a>