---
layout: post
title: 'Scrub: Borrar archivos de manera segura en Linux'
date: '2017-07-20 16:45:51'
tags:
- deletion
- shredding
- scrub
---

En una entrada pasada miramos como encriptar archivos en linux de manera segura con OpenSSL,[^n] para lo cual es útil si queremos proteger información. Pero **una vez que esa información ya no es útil, debe ser borrada de manera segura para evitar la recuperación de ese archivo.**

Para esto tenemos muchas herramientas que nos sirven de utilidad para el propósito, una de ellas es `scrub` en linux. Estos programas funcionan de diferentes maneras y tienen diferentes estándares de funcionamiento.

## Instalación
Su instalación es sencilla, y el administrador de paquetes debería indexarlo sin problemas, por ejemplo para Debian y derivados:

`apt install scrub`

También puedes contar con el código fuente en GitHub si así lo prefieres.[^n]

## Métodos en `scrub`:
```
Available patterns are:
  nnsa          3-pass   NNSA NAP-14.1-C
  dod           3-pass   DoD 5220.22-M
  bsi           9-pass   BSI
  usarmy        3-pass   US Army AR380-19
  random        1-pass   One Random Pass
  random2       2-pass   Two Random Passes
  schneier      7-pass   Bruce Schneier Algorithm
  pfitzner7     7-pass   Roy Pfitzner 7-random-pass method
  pfitzner33   33-pass   Roy Pfitzner 33-random-pass method
  gutmann      35-pass   Gutmann
  fastold       4-pass   pre v1.7 scrub (skip random)
  old           5-pass   pre v1.7 scrub
  dirent        6-pass   dirent
  fillzero      1-pass   Quick Fill with 0x00
  fillff        1-pass   Quick Fill with 0xff
  verify        1-pass   Quick Fill with 0x00 and verify
  custom        1-pass   custom="str" 16 chr max, C esc like \r, \xFF, \377, \\
```

Si quieres saber mas sobre los métodos utilizados puedes encontrarlos en el *README.md* de Scrub.[^n]

`Scrub` nos facilita el trabajo ya que tiene determinado patrones de borrado como los que vimos anteriormente. La sintaxis del programa es simple:

`root@thisbutascratch:~# scrub archivo|disco`

De esa manera simple usara el método NNSA NAP-14.1-C (3-passes) para borrar el archivo de manera rápida y segura. Si quieres usar otros patrones como Gutmann entonces tienes que usar la opcion `-p` y para eliminar el archivo del disco usamos la opción `-r` (aunque es opcional, por ejemplo, no eliminaríamos /dev/sda):

`root@thisbutascratch:~# scrub -p gutmann archivo|disco`

Claro que el método Gutmann, aunque es muy seguro, tarda bastante en borrar archivos grandes. Por ejemplo un archivo de 1G en un SSD tarda en borrarse al menos 2 minutos:

`root@thisbutascratch:~# scrub -p gutmann -r file.data`

```
scrub: using Gutmann patterns
scrub: scrubbing file.data 1073741824 bytes (~1024MB)
scrub: random  |................................................|
scrub: random  |................................................|
scrub: random  |................................................|
scrub: random  |................................................|
scrub: 0x55    |................................................|
scrub: 0xaa    |................................................|
scrub: 0x924924|................................................|
--- SNIP ---
scrub: unlinking file.data

real    2m30.126s
user    1m56.528s
sys     0m3.492s
```
Podemos ver que el tiempo real el cual nos tardamos en borrar ese archivo de 1G es de *2 minutos con 30 segundos*.

Mientras que el **método DoD** tarda 18 segundos:
```
scrub: using DoD 5220.22-M patterns
scrub: scrubbing file.data 1073741824 bytes (~1024MB)
scrub: random  |................................................|
scrub: 0x00    |................................................|
scrub: 0xff    |................................................|
scrub: verify  |................................................|
scrub: unlinking file.data

real    0m18.438s
user    0m14.796s
sys     0m0.484s
```

y el **método NNSA** tarda 31 segundos para el mismo archivo:
```
scrub: using NNSA NAP-14.1-C patterns
scrub: scrubbing file.data 1073741824 bytes (~1024MB)
scrub: random  |................................................|
scrub: random  |................................................|
scrub: 0x00    |................................................|
scrub: verify  |................................................|
scrub: unlinking file.data

real    0m31.166s
user    0m29.264s
sys     0m0.400s
```
La seguridad vs. velocidad es un tema que se define por contexto, donde en ciertas ocasiones debemos sacrificar velocidad por seguridad, y en otras donde podemos sacrificar seguridad por velocidad.

Espero te haya gustado el material, si tienes preguntas o dudas, puedes comentar abajo de esta entrada.

----------------
[^n]: https://www.getroot.info/2017/07/17/encriptar-archivos-con-openssl/
[^n]: https://github.com/chaos/scrub
[^n]: https://github.com/chaos/scrub/blob/master/README.md