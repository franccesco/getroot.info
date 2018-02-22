---
layout: post
title: De Microsoft SQL Brute force a una Sesion RDP
date: '2018-01-13 01:35:54'
tags:
- metasploit
- footprinting
- powershell
- evasion
- framework
- tools
- empire
---

Hace poco estaba navegando internet, a mi manera y me encontre con varios servidores MSSQL hasta que me encontre con una contraseña debil en un servidor. Veremos como abusar esta vulnerabilidad para conseguir una sesion RDP.

## Escenario
* Encontramos un servidor vulnerable
* Queremos ganar acceso al sistema
* Elevar nuestros privilegios a una alta integridad
* Establecer una sesion RDP

## Footprinting

Abrimos Metasploit y busco el modulo mssql_ping para ver en que ver en que puertos esta MSSQL corriendo, este modulo hace una peticion al servicio UDP en el puerto 1433 para localizar en que otros puertos (TCP) esta el servicio MSSQL.

`use auxiliary/scanner/mssql/mssql_ping`
![search mssql type:auxiliary](/images/screenshots/Screenshot-from-2018-01-11-23-21-48-1.png)

Al correr el modulo e ingresar la direccion del servidor, me da los puertos a los cuales podemos atacar al servidor.

![mssql_ping result](/images/screenshots/Screenshot-from-2018-01-11-23-22-39.png)

A como podemos ver, hay dos puertos en los cuales podemos acceder y encontrar bases de datos, el puerto 1433 (MSSQLSERVER) y el puerto 49633 (CENSORED).

Atacando el puerto 1433, hacemos un ataque de fuerza bruta para conocer si hay un usuario y contraseña facil de adivinar. _Conste que los ataques de fuerza bruta tiran alarmas a lo loco a los IDS/IPS y Firewalls, asi que deberias abstenerte de hacer esto en una red bien vigilada._

`use auxiliary/scanner/mssql/mssql_login`
![mssql_login options](/images/screenshots/Screenshot-from-2018-01-11-23-27-23.png)

Configuramos bien nuestras opciones:
* Establecemos probar un password vacio, una lista de usuarios comunes, una lista de passwords (usualmente uso SecLists)
* 12 Threads (dependiendo de nuestros recursos de red)
* `VERBOSE false` porque no quiero ver cada intento fallido
* `run -j` para correr el modulo en el background y que asi nos de tiempo de hacer otras cosas mientas se ejecuta.

![Creds found](/images/screenshots/Selection_002.png)

Una vez que encontramos credenciales validas, procedemos a ejecutar un commando con el modulo `mssql_exec`

`use auxiliary/admin/mssql/mssql_exec`
![Selection_003](/images/screenshots/Selection_003.png)

Configuramos nuestras opciones acorde a nuestros hallazgos:
* **CMD**: whoami (para saber la cuenta en la cual corre MSSQL).
* **PASSWORD**: admin
* **USERNAME**: admin

¡¿`NT Authority\System`?!
![NT Authority\System](/images/screenshots/Selection_004.png)

Supongo que esta es una de esas dulces ocasiones donde el Administrador te deja todo el trabajo hecho para que no tengas que preocuparte por la parte dificil /s.

![Merl's right](https://i.imgur.com/JktnQQN.gif)

## Meterpreter
Antes de tirar de un solo un Payload, quiero saber que tipo de seguridad tiene la red, no vaya ser que un IDS bloquee mi direccion y me haga la vida imposible tratandome de conectar con un reverse shell.

Vemos los procesos que se ejecutan en el sistema:

![tasklist /SVC](/images/screenshots/Workspace-1_006.png)

Y observamos algo que nos interesa:

![MWAGENT.EXE](/images/screenshots/Selection_007.png)

Segun [File.net](https://www.file.net/process/mwagent.exe.html)
> The genuine mwagent.exe file is a software component of eScan Antivirus Suite by MicroWorld Technologies.

Ya sabemos que el sistema tiene un AV, casi todos lo tienen, no es de sorprenderse, podriamos usar Veil para hacer un payload indetectable y subirlo por el modulo `mssql_payload` pero no quiero complicarme, ademas que el modulo toca el disco, y no me gusta hacer eso.

## Empire F#ckery again.
Ya que no quiero tocar el disco, vamos a injectar un payload de Empire a traves del modulo `mssql_exec`, teniendo una sesion en Empire vamos a injectar un payload de meterpreter y mantener una session en la memoria.

Para hacerlo mas sencillo:
* Creamos dos listeners en Empire, uno para escuchas de paylodas de Empire, y otro para pasar la session a Meterpreter
* Injectamos el codigo generado en Empire a traves del modulo mssql_exec
* Una vez que el agente se conecte a Empire, pasamos la sesion de a meterpreter

```
        +-> Metasploit +-----------> Empire HTTP Listener +--+
        |                                                    |
        |                                                    |
        |                                                    |
        +-------------+ Meterpreter Injection <--------------+
```

Se que no es necesario usar Metasploit _y_ Empire, pero personalmente lo he encontrado muy util, muchas cosas que puedo hacer facilmente en Meterpreter, no las puedo llevar a cabo en Empire y visceversa, no por falta de conocimiento, sino por fallas de codigo, facilidad, diseño, etc. Creo que son un perfecto complemento, manos a la obra.

### Creacion de Listeners con Empire y Metasploit

Esta es mi sesion de escucha **General** donde tengo activa la opcion **SSL** con `CertPath` indicando el directorio `data/` en el cual se generan los certificados de Empire.

![General Listener](/images/screenshots/Workspace-1_008.png)

Y aqui tengo mi sesion de meterpreter, la cual usaremos para inyectar el codigo una vez que nuestro agente se conecte.

![Meterpreter Listener](/images/screenshots/Selection_009-1.png)

Una vez hecho esto, debemos usar el modulo `exploit/multi/handler` en Metasploit y establecer el payload `windows/meterpreter/reverse_https` para que escuche y establezca una sesion de meterpreter una vez que inyectemos nuestro codigo.

![Selection_010-1](/images/screenshots/Selection_010-1.png)

Debemos procurar que el puerto de escucha `LHOST` sea el mismo que elegimos para el listener de meterpreter en Empire.

### Inyectando el primer payload
Lo primero que haremos sera generar el payload de Empire e inyectarlo a traves de `mssql_exec`:

![Payload General](/images/screenshots/Selection_011.png)

Vamos a Metasploit y lo inyectamos con `mssql_exec`

![set CMD](/images/screenshots/Selection_012.png)

Ahora tenemos nuestro Agente en Empire

![Empire Agent](/images/screenshots/Selection_013-1.png)

Una vez que tenemos nuestro agente en Empire, interactuamos con el e inyectamos el shellcode de meterpreter

![Injecting Meterpreter](/images/screenshots/Selection_014.png)

Y como podran ver, ya Metasploit obtuvo un Staged Meterpreter

![Staged x64 Meterpreter](/images/screenshots/Selection_015.png)

## Obteniendo RDP
Para obtener una sesion RDP, primero debemos activar el RDP en el servidor con el modulo `enable_rdp` de meterpreter, por conveniencia el modulo nos permite directamente crear un usuario y contraseña para acceder y tener un escritorio virtual remoto.

`use post/windows/manage/enable_rdp`
![enable RDP](/images/screenshots/Selection_017.png)

Como pueden ver, RDP ya esta activado, esto es usual en Windows Server ya que es conveniente, ahora podemos entrar y tener una sesion RDP usando la herramienta `rdesktop`

![Workspace-1_018](/images/screenshots/Workspace-1_018.png)

## Ultimas palabras
No es necesario poner Empire y Metasploit, ademas Empire tiene el modulo `powershell/management/enable_rdp` el cual activa el RDP, pero he encontrado muchas veces que las cosas se complican dependiendo el sistema operativo e incluso el lenguaje del mismo, ademas metasploit tambien tiene el modulo `exploit/multi/script/web_delivery` pero no funciona en todas las versiones de powershell y no tan bien como Empire.

Asi que juega a como quieras, descubre tus metodos, y hasta la proxima.