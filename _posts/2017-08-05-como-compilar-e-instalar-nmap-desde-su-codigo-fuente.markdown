---
layout: post
title: Como compilar e instalar Nmap desde su código fuente
date: '2017-08-05 14:14:57'
tags:
- nmap
- howto
---

Sabemos que es fácil instalar Nmap desde `apt install nmap` o `yum install nmap` o cualquiera sea tu gestor de paquetes lo mas seguro es que tenga Nmap.

Pero compilar e instalar Nmap desde el código fuente tiene sus ventajas, por ejemplo **tiene una mayor cantidad de NSE scripts** que puedes usar ya que son añadidos constantemente, o también tienes **la versión mas actualizada**, además que la compilamos directamente de su Git el cual se le da mantenimiento muy seguido y **se introducen mejoras al programa**.

Es muy fácil realmente, primeros instalamos los paquetes: `git wget build-essential checkinstall libpcre3-dev libssl-dev`

```
root@nmap-compilation:~# apt install git wget build-essential checkinstall libpcre3-dev libssl-dev
```

Una vez instalados los paquetes de desarrollo para compilar Nmap, clonamos su código fuente de Git:

```
root@nmap-compilation:~# git clone https://github.com/nmap/nmap.git
Cloning into 'nmap'...
remote: Counting objects: 67674, done.
remote: Compressing objects: 100% (93/93), done.
--- SNIP ---
```

Ahora entramos al directorio de Nmap y configuramos con `./configure`:

```
root@nmap-compilation:~# cd nmap/
root@nmap-compilation:~/nmap# ./configure
checking whether NLS is requested... yes
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables...
checking whether we are cross compiling... no
checking for suffix of object files... no
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89...
--- SNIP ---
```

Y procedemos a compilar e instalar Nmap:

```
root@nmap-compilation:~/nmap# make && make install
g++ -MM -I./liblinear -I./liblua -I./libdnet-stripped/include -I./libssh2/include  -I./libpcap -I./nbase -I./nsock/include -DHAVE_CONFIG_H -DNMAP_NAME=\"Nmap\" -DNMAP_URL=\"https://nmap.org\" -DNMAP_PLATFORM=\"x86_64-unknown-linux-gnu\" -DNMAPDATADIR=\"/usr/local/share/nmap\" -D_FORTIFY_SOURCE=2 charpool.cc FingerPrintResults.cc FPEngine.cc FPModel.cc idle_scan.cc MACLookup.cc main.cc nmap.cc nmap_dns.cc nmap_error.cc nmap_ftp.cc NmapOps.cc NmapOutputTable.cc nmap_tty.cc osscan2.cc osscan.cc output.cc payload.cc portlist.cc portreasons.cc protocols.cc scan_engine.cc scan_engine_connect.cc scan_engine_raw.cc scan_lists.cc service_scan.cc services.cc
--- SNIP ---
```

Y una ves compilado e instalado procedemos a confirmar la versión de Nmap:

```
root@nmap-compilation:~/nmap# nmap -V
Nmap version 7.60SVN ( https://nmap.org )
Platform: x86_64-unknown-linux-gnu
Compiled with: nmap-liblua-5.3.3 openssl-1.0.2g nmap-libssh2-1.8.0
libz-1.2.11 libpcre-8.39 nmap-libpcap-1.7.3 nmap-libdnet-1.12 ipv6
Compiled without:
Available nsock engines: epoll poll select
```

Listo, puedes notar que hay mas scripts, también hay mas control de lo que instalas y puedes modificar el código fuente si gustas.

# <script type="text/javascript" src="https://asciinema.org/a/Xwgir0OwOzYJyutiADpFHQR1V.js" id="asciicast-Xwgir0OwOzYJyutiADpFHQR1V" async></script>