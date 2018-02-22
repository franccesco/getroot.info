---
layout: post
title: Encriptar archivos con OpenSSL
date: '2017-07-17 14:38:10'
tags:
- encriptacion
---

OpenSSL es una herramienta extremadamente flexible que nos permite encriptar o cifrar (conste que hay una diferencia) con mucha facilidad.

Vamos a encriptar un archivo en el estandar AES 256 CBC, el cual es el estandar utilizado por el gobierno de Estados Unidos para encriptar documentos de alta sensibilidad.

<script type="text/javascript" src="https://asciinema.org/a/LnkAyQ6UmMMD1Lc19TfqXECUX.js" id="asciicast-LnkAyQ6UmMMD1Lc19TfqXECUX" async></script>
--------------

El comando es el siguiente:

`openssl aes-256-cbc -a -salt -in <archivo entrada> -out <archivo encriptado>`

Miremos las opciones:

* **openssl:** ejecutable en cuestión.
* **aes-256-cbc:** estandar de encriptado. [Aquí pueden encontrar un Gist con una lista grande de encriptado.](https://gist.github.com/reggi/4459803)
* **-a:** Le decimos al programa que queremos la salida del archivo en Base64 *luego* de encriptarlo. Esto nos provee de portabilidad como enviarlo por correo o verlo como archivo de texto.
* **-salt:** Aquí proveemos de Salt al método de encriptado para mitigar ataques precomputados como [Rainbow Attacks](https://en.wikipedia.org/wiki/Rainbow_table).
* **-in:** Archivo de entrada. El que queremos encriptar.
* **-out:** Archivo de salida, ya encriptado.

Para desencriptar nuestro archivo el argumento es similar, solo que no escribiremos la opcion `-salt`:

`openssl aes-256-cbc -a -d -in <archivo encriptado> -out <archivo salida>`

Ejemplo:

<script type="text/javascript" src="https://asciinema.org/a/slMYrWpCPFR3OoBXxE0G1fxr1.js" id="asciicast-slMYrWpCPFR3OoBXxE0G1fxr1" async></script>
--------------

Espero te haya gustado la entrada. Tienes un método o sugerencia? Comenta abajo!

*Happy Hacking.*