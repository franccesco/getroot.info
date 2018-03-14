---
title: GetAltName - Extractor de subdominios en certificados SSL
layout: post
---

Ya hemos hablado de [como extraer subdominios de certificados SSL](https://getroot.info/tip-getaltname/), lo cual es muy facil, solo tenemos que buscar la extension **SubjectAltName** en los certificados, pero nos provee de bastante información, especialmente si son certificados de uso interno que usualmente son _autofirmados ([Self-Signed](https://en.wikipedia.org/wiki/Self-signed_certificate))_.

Hace poco _re_-escribi la herramienta GetAltName para que tenga mayor funcionalidad, el motivo de el programa es automatizar la extraccion de los subdominios de estos certificados tanto **firmados por una autoridad** como **autofirmados** que son los que mas interesan ya que se usan a nivel interno.

<asciinema-player src="/assets/recordings/getaltnamev1/getalt1.cast"></asciinema-player>

Ahora, uno de los problemas al analizar estos registros es que presentan otros dominios y otros TLD's que no son los que nos interesan, por ejemplo si analizamos **google.com** nos saldra en los registros **youtube.com**, si solamente queremos los subdominios pertenecientes a **google.com** entonces podemos usar la opcion `-m` para filtrar solamente estos dominios.

<asciinema-player src="/assets/recordings/getaltnamev1/getalt2.cast"></asciinema-player>

Tambien podemos dar salida de texto a los resultados con la opcion `-o` (`--output`).

<asciinema-player src="/assets/recordings/getaltnamev1/getalt3.cast"></asciinema-player>

Ademas de salida de archivo, podemos copiar los resultados al clipboard, como una lista vertical (`-c l`) de subdominios o una lista horizontal (`-c s`) para ahorrarnos el trabajo con otras herramientas.

<asciinema-player src="/assets/recordings/getaltnamev1/getalt4.cast"></asciinema-player>

Tambien se integra con el servicio de busqueda de certificados (`-s`) [crt.sh](https://crt.sh) el cual devuelve todavia mas subdominios, aunque ojo, el servicio crt.sh es algo inestable ya que queda irresponsivo en las peticiones grandes.

<asciinema-player src="/assets/recordings/getaltnamev1/getalt5.cast"></asciinema-player>

Hasta ahora esas son las funciones mas importantes, planeo añadir mas funciones como integracion con Nmap y otros filtros. Hasta la proxima.
