---
layout: post
title: Como instalar Metasploit en Unix/Linux
date: '2017-09-03 15:54:17'
tags:
- metasploit
- howto
---

Si ya tienes Kali Linux en tu maquina entonces no tienes nada que hacer porque Metasploit ya viene instalado por defecto y esta a un `msfdb init` a tu alcance.

Sin embargo si tu distribucion preferida no es Kali Linux y quisieras trabajar desde tu distribucion por preferencia (como yo), entonces encontraras que instalar Metasploit ya no es como antes.

Antes tenias que registrar tu version de comunidad de Metasploit y obtener una llave en tu correo que te permitia darle actualizaciones y obtener nuevos exploits, auxiliaries y payloads, entre otras cosas.

![Nop, ningun lado aqui.](/content/images/2017/09/Screenshot-from-2017-09-03-09-38-18.png)

Sin embargo, el formulario para pedir la llave no esta aqui.

![](/content/images/2017/09/Screenshot-from-2017-09-03-09-39-06.png)

Tampoco aqui, y pense que ese formulario era para la llave de metasploit, pero es para descargar Metasploitable. *Derp*.

![](/content/images/2017/09/Screenshot-from-2017-09-03-09-39-21.png)

Y la segunda imagen me redirige al Github, donde al parecer tengo que clonar el repositorio? Tambien hice eso y esa no es la solucion.

Ellos ofrecen instaladores \*.sh y tener una instalacion en /opt/ pero no lo hagas. Opta por la instalacion con tu administrador de paquetes (en mi caso APT) el cual es mucho mejor y mas rapido tanto como para instalar como para actualizar.

# Instalacion:
Solamente copia y pega estas lineas en la terminal que [vienen desde el repositorio oficial](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers#linux-and-os-x-quick-installation) de Metasploit:

```
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
chmod 755 msfinstall && \
./msfinstall
```

Ahora ejecutamos `msfconsole` sin necesidad de activar servicios ni ejecutar `msfupdate` y ya tendremos nuestra instalacion asi de facil:

![Wohoo!](/content/images/2017/09/final_step-1.png)

*Y decir que me llevo dias averiguar esta...*