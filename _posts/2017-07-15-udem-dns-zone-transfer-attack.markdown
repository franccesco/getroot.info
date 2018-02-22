---
layout: post
title: 'Universidad UdeM: Vulnerable a ataque AXFR'
featured: true
date: '2017-07-15 05:27:25'
tags:
- dns
- axfr
- vuln
---

Sinceramente me quede boquiabierto al ver esto, ya que es una de las vulnerabilidades mas tontas que se puede cometer hoy en día. Aunque al mismo tiempo no me sorprendió tanto al ver que el tema de la seguridad informática en Nicaragua es bastante vago.

Solo he visto *UNA VEZ* en mi vida un ataque AXFR exitoso ya que es un ataque bastante viejo y fácilmente mitigado incluso por sistemas automatizados.

Sin más que decir, aquí la prueba, UdeM con todos sus subdominios... *sin esfuerzo alguno*:

<center><script type="text/javascript" src="https://asciinema.org/a/iEKioyRVta3qywqJYdzLcN5qQ.js" id="asciicast-iEKioyRVta3qywqJYdzLcN5qQ" async></script></center>
--------------

La prueba fue hecha con [fierce](https://www.getroot.info/2017/06/15/dns-brute-force-buscando-servicios-por-sub-dominios/).

#### Prueba Manual

**Linux:**

Primero averiguamos los nombres de DNS que el dominio usa:
`root@kali:~# dig NS nombre_dominio`

Luego procedemos a hacer una transferencia de zona del DNS:
`root@kali:~# dig AXFR nombre_dominio @dns_del_dominio`

**Demostración:**

<center><script type="text/javascript" src="https://asciinema.org/a/tR7e7gucSsd90iq7C3BIqsIHv.js" id="asciicast-tR7e7gucSsd90iq7C3BIqsIHv" async></script></center>
--------------

**Windows:**

Primero entramos a `NSLookup`:

1. `nslookup`
2. `server dns_del_dominio`
3. `ls -d nombre_dominio`

**Demostración:**
<iframe width="1280" height="720" src="https://www.youtube.com/embed/KZQRswXjV90?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

Es una lástima que no se invierta en seguridad e infraestructura en Nicaragua. Realmente no sé la razón para que cosas tan fácilmente arreglables pasen de esta manera, ¿Descuido o pereza? Quién sabe.

Como siempre, *Happy Hacking.*