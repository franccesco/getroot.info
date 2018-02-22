---
layout: post
title: 'Empire F#ckery  — Pt. 02: Office Staging Methods'
featured: true
date: '2017-12-18 01:20:42'
tags:
- powershell
- framework
- tools
- empire
---

Esta es una serie de articulos los cuales nos brindaran una serie de tecnicas que podemos usar para inyectar payloads de Empire. [Ya vimos como funciona Empire](https://getroot.info/2017/08/05/empire-powershell/) de manera general.

Tengo pensado en partir estos articulos en pequeños segmentos por falta de tiempo y creo que asi hare actualizaciones mas frequentes para los lectores.

## Office Modules
Al ver los metodos de *staging* que nos ofrece para **Windows** podemos ver:
```
(Empire) > usestager windows/
bunny             ducky             launcher_bat      launcher_sct      macro             teensy            
dll               hta               launcher_lnk      launcher_vbs      macroless_msword  
```

Ok encontramos 3 vectores las cuales podemos integrar en un documento de Office: *launcher_bat (OLE), macro y macroless_msword*.

### Macrosoft Word Injection
El primero que vamos a probar sera la inyeccion de macros en Microsoft Word, primero veremos la preparacion del codigo, y como incluimos este codigo en Word:

#### Generamos el codigo en Empire
Para hacer esto seguimos estos pasos generales que tambien aplicaremos en los demas, solo un poco distinto depende del escenario:
1. Establecer un _Listener_
2. Usar el _Stager_
3. Establecer un _OutFile_ y generarlo.
# <script src="https://asciinema.org/a/nkc7pEaAU28vDrBXOPHtzk4kW.js" id="asciicast-nkc7pEaAU28vDrBXOPHtzk4kW" async></script>

Una vez que hacemos tenemos el codigo macro podremos añadirlo a un documento en Word, veamos como hacer esto:
<iframe width="560" height="315" src="https://www.youtube.com/embed/XKO2essugMU?rel=0" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

**En el video no me sale la opcion de activar el macro porque ya lo habia activado antes**, pero hay que convencer al usuario de permitir ejecutar codigo macro en la suite de office.

### Objeto OLE (Object Linking and Embedding)
Este es bastante bueno ya que es tenemos mas mecanismos para engañar a la victima, siempre debemos hacer que la victima diga _"sí, quiero apretar este boton"_, y este vector de ataque es muy facil para llevar esto a cabo.

Lo que haremos sera insertar un **objeto OLE** el cual lanzara un BAT adjuntado al archivo:
<iframe width="560" height="315" src="https://www.youtube.com/embed/Xs7zhW0r5sI?rel=0" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

Claro que debemos disfrazar mejor el documento, pero esas ideas seran depende del reconocimiento e inteligencia que tengamos del objetivo, ==por esta razon es importante saber como trabaja la organizacion y conocer la jerga de trabajo== (leyendo correos, support tickets, leaks de datos, etc.)

### DDE (Dynamic Data Exchange) Macroless
Este es uno de los mejores en mi opinion, pero detectable, **si el sistema esta fuertemente protegido y el antivirus actualizado entonces esta opcion no servira de mucho**, pero es altamente efectivo si es pasado por alto.

Abusaremos de la opcion DDE en Word el cual nos permite vincular un objeto, incluso un commando de powershell con un payload, [aqui esta el research](https://sensepost.com/blog/2016/powershell-c-sharp-and-dde-the-power-within/).

<iframe width="560" height="315" src="https://www.youtube.com/embed/UtJs3Pcb31M?rel=0" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>

Como podran ver este ataque *es muy facil de realizar y conveniente si el sistema no esta protegido con un AV actualizado*, lo cual muchas organizaciones pasan por alto, es increible la cantidad de inseguridad que hay.

## Solo di 'NO'
La muy mayoria de los ataques en la Suite Office pueden mitigarse en dos simples pasos:
1. **No abras archivos los cuales no provienen de fuentes _legitimas_ y _confirmadas_**
2. **Si Office te grita cualquier error o peticion, cliquea el boton _'No'_.** No sabes cuanto puedes ahorrarte solo con ese simple tip.

Eso es todo en esta entrada, ya vimos como abusar de Office, tal vez a la proxima buscaremos abusar de Outlook, excel, o tal vez el HTA y otros metodos de entrada, tambien la integracion con Metasploit.

*Hasta la proxima.*