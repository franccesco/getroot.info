---
layout: post
title: Bypass Hikvision Authentication
---

Hay muchas camaras disponibles in internet, muchas incluso que no sus usuarios ni siquiera se dan cuenta que son accesibles, ahora podrias pensar que *es facil mitigar un ataque... esta bien, pongamosle una contraseña a la camara y asi podre vigilar mi casa desde internet*. Querido amigo, no podrias estar mas equivocado.

## Multiples vulnerabilidades en camaras Hikvision

Esto no es reciente, ya tiene mucho tiempo, el problema es que estos dispositivos todavia existen, sus firmwares no estan actualizados, y a las personas tampoco les interesa.

Hay tres *advisories* que quisiera demostrar: **CVE-2013-4975, CVE-2013-4976, CVE-2013-4977.**

### CVE-2013-4975 (Privilege Escalation through ConfigurationData Request)

> Tambien conocido como ser *Admin* sin realmente serlo.

**Escenario:** Tenemos un dispositivo Hikvision con un usuario y contraseña, pero somos usuarios regulares, ¿Como nos apropiamos de la cuenta *Admin*?

![Inside Hikvision](/content/images/2017/10/Selection_001.png)


