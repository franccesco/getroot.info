---
layout: post
title: Off-line Password Cracking con Hashcat
---

Hashcat es uno de los password crackers mas poderosos y avanzados de la industria, tiene un sinnúmero de opciones, utiliza tu GPU para hacer el trabajo múchisimo mas rápido y es el mejor en lo que hace. Para enseñarte rapidamente lo que hace, no indagaremos en sus tantas opciones, sino solamente en las mas basicas y el resto puedes leerlo en el [Hashcat Wiki](https://hashcat.net/wiki/)

# Instalación
La instalación es bastante sencilla:
* Clonamos el repositorio: `git clone https://github.com/hashcat/hashcat.git`
* Obtenemos una copia de OpenCL Headers: `git submodule update --init`
* Compilamos: `make`
* E instalamos: `sudo make install`

# <script src="https://asciinema.org/a/156893.js" id="asciicast-156893" async></script>

Now that hashcat is installed we can 