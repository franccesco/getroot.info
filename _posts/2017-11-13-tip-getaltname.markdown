---
layout: post
title: Encontrar subdominios en certificados SSL
date: '2017-11-13 20:17:24'
tags:
- dns
- vm
- info-gathering
- osint
- tip
- howto
- tools
- discovery
---

A veces cuando hacemos un brute force no podemos encontrar todos los subdominios o servidores virtuales ya que pueden ser muchisimos y tenemos un set de palabras limitados en un diccionario.

Es por esto que debemos de buscar tener la mayor cantidad de información posible, una de las técnicas donde podemos encontrar subdominios, direcciones IP y correos incluso es en el _Subject Alternative Name_ de los certificados SSL.

Esto no significa que _siempre_ encontraremos todos los subdominios ahí, _no_, mas bien encontraremos poca información pero que pudiera sernos útil en caso de que solo nos toparamos con servidores muy bien parcheados.

## Obteniendo el SubjectAltName
Este es un buen caso, hace poco exploraba un rango de direcciones en busca de subdominios de un servidor para encontrar un vector de ataque, encontrando un servidor (`dtic.exampleweb.com`) me di cuenta que era una dirección IP que servía varios servidores virtuales, poniendo en práctica lo aprendido, vemos el certificado SSL:

![Certificado SSL emitido por LetsEncrypt](/content/images/2017/11/cap1.PNG)

Podemos ver que fue emitido a `dtic.exampleweb.com` por Let's Encrypt y tiene fecha de expiración al próximo año, viendo los detalles del certificado podemos ver que algo no calza:

![Common Name (CN) is different than the website](/content/images/2017/11/cap2.PNG)

Segun las medidas de seguridad de los exploradores web, si el `Common Name (CN)` del certificado no es igual al de la pagina web que visitamos, deberia tirar un error de que el sitio no es seguro, en este caso no tira ese error, sino que la página se muestra como totalmente legítima:

![El certificado se muestra como válido al sitio web](/content/images/2017/11/Capture-1.PNG)

Si el `Common Name` del certificado y la dirección del sitio web no calzan, es porque el certificado tiene una extensión llamada _Subject Alternative Name_ el cual dicta que además del `Common Name`, hay otros nombres de dominio los cuales tambien **son validos**. Donde encontramos esos otros subdominios en el certificado?

![Nombres Alternativos del Certificado](/content/images/2017/11/cap3.png)

En la pestaña de *Details* podemos encontrar en la casilla *Certificate Fields* en el cual encontramos la extension llamada *Certificate Subject Alt Name* donde encontramos los nombres alternativos (pero pertenecientes al dominio principal) a los que el certificado es válido, por esta razon el certificado se presenta legítimo aun cuando el **CN (Common Name)** es diferente.

Por supuesto esto no es una vulnerabilidad, pero si nos sirve para ampliar nuestro margen de ataque, y conducir pruebas mas eficaces. Por supuesto es solo una de las muchas maneras de las cuales podemos conseguir información.

## [getaltname.rb](https://github.com/franccesco/getaltname)

Para automatizar este trabajo hice una pequeña herramienta en Ruby el cual te permite obtener estos subdominios, si tienes suerte.

### Uso:
```
Usage:  getaltname.rb [OPTIONS]

Specific Options:
    -h, --host host                  Host to extract alternative names.
Common Options:
    -p, --port port                  Port to connect to (default 443)
        --help                       Show this message
```

### Ejemplo:
```
λ linuxbox getaltname → λ git release/0.1.0* → ./getaltname.rb -h dtic.examplewebsite.com
100 Subject Alternative Names found:

adquisiciones.examplewebsite.com
bib.examplewebsite.com
ccpe.examplewebsite.com
cedocfec.examplewebsite.com
compufec.examplewebsite.com
control-co.examplewebsite.com
dbe.examplewebsite.com
dde-e.examplewebsite.com
dde.examplewebsite.com
di.examplewebsite.com
diex.examplewebsite.com
docentes.examplewebsite.com
dtic.examplewebsite.com
extension.examplewebsite.com
fcys.examplewebsite.com
--- SNIP ---
```

En este ejemplo incluso encontre 50 nombres virtuales donde pude encontrar mucha mayor información y vulnerabilidades que haciendo un brute force, claro esta que la suerte también influye aquí.

[https://github.com/franccesco/getaltname](https://github.com/franccesco/getaltname)

_Aunque en el ejemplo dice 100, en realidad todos los nombres se duplicaron anteponiendo un `www`. Así que chequea también ante duplicados._