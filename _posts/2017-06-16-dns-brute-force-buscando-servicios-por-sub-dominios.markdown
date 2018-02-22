---
layout: post
title: 'DNS brute force: Buscando servicios por subdominios'
featured: true
date: '2017-06-16 02:59:35'
tags:
- dns
- bruteforce
---

Una de las fases mas importantes en un test de penetración es la búsqueda de información acerca de tu objetivo, y usualmente los nombres de dominio guardan muchos servicios independientes que se conectan con tu objetivo para las tareas de dia a dia.

Es por esto que es importante buscar subdominios en tu objetivo, porque siempre se encuentran servicios abiertos, mal configurados o simplemente configurados a medias, donde puedes encontrar cuentas por defecto, archivos importantes, configuraciones, incluso software!

Vamos a probar varias herramientas y métodos para buscar subdominios, entre ellas fierce.pl, recon-ng, dnsrecon y dnsmap.

[Fierce.pl](https://github.com/mschwager/fierce)
==========
Fierce.pl fue escrito originalmente por RSnake y otros personajes en http://ha.ckers.org/ (ahora difunto) y es actualmente mantenido en https://github.com/mschwager/fierce.

Estas son los argumentos que podemos proveer:
```
fierce.pl (C) Copywrite 2006,2007 - By RSnake at http://ha.ckers.org/fierce/

	Usage: perl fierce.pl [-dns example.com] [OPTIONS]

Overview:
	Fierce is a semi-lightweight scanner that helps locate non-contiguous
	IP space and hostnames against specified domains.  It's really meant
	as a pre-cursor to nmap, unicornscan, nessus, nikto, etc, since all 
	of those require that you already know what IP space you are looking 
	for.  This does not perform exploitation and does not scan the whole 
	internet indiscriminately.  It is meant specifically to locate likely 
	targets both inside and outside a corporate network.  Because it uses 
	DNS primarily you will often find mis-configured networks that leak 
	internal address space. That's especially useful in targeted malware.

Options:
	-connect	Attempt to make http connections to any non RFC1918
		(public) addresses.  This will output the return headers but
		be warned, this could take a long time against a company with
		many targets, depending on network/machine lag.  I wouldn't
		recommend doing this unless it's a small company or you have a
		lot of free time on your hands (could take hours-days).  
		Inside the file specified the text "Host:\n" will be replaced
		by the host specified. Usage:

	perl fierce.pl -dns example.com -connect headers.txt

	-delay		The number of seconds to wait between lookups.
	-dns		The domain you would like scanned.
	-dnsfile  	Use DNS servers provided by a file (one per line) for
                reverse lookups (brute force).
	-dnsserver	Use a particular DNS server for reverse lookups 
		(probably should be the DNS server of the target).  Fierce
		uses your DNS server for the initial SOA query and then uses
		the target's DNS server for all additional queries by default.
	-file		A file you would like to output to be logged to.
	-fulloutput	When combined with -connect this will output everything
		the webserver sends back, not just the HTTP headers.
	-help		This screen.
	-nopattern	Don't use a search pattern when looking for nearby
		hosts.  Instead dump everything.  This is really noisy but
		is useful for finding other domains that spammers might be
		using.  It will also give you lots of false positives, 
		especially on large domains.
	-range		Scan an internal IP range (must be combined with 
		-dnsserver).  Note, that this does not support a pattern
		and will simply output anything it finds.  Usage:

	perl fierce.pl -range 111.222.333.0-255 -dnsserver ns1.example.co

	-search		Search list.  When fierce attempts to traverse up and
		down ipspace it may encounter other servers within other
		domains that may belong to the same company.  If you supply a 
		comma delimited list to fierce it will report anything found.
		This is especially useful if the corporate servers are named
		different from the public facing website.  Usage:

	perl fierce.pl -dns examplecompany.com -search corpcompany,blahcompany 

		Note that using search could also greatly expand the number of
		hosts found, as it will continue to traverse once it locates
		servers that you specified in your search list.  The more the
		better.
	-suppress	Suppress all TTY output (when combined with -file).
	-tcptimeout	Specify a different timeout (default 10 seconds).  You
		may want to increase this if the DNS server you are querying
		is slow or has a lot of network lag.
	-threads  Specify how many threads to use while scanning (default
	  is single threaded).
	-traverse	Specify a number of IPs above and below whatever IP you
		have found to look for nearby IPs.  Default is 5 above and 
		below.  Traverse will not move into other C blocks.
	-version	Output the version number.
	-wide		Scan the entire class C after finding any matching
		hostnames in that class C.  This generates a lot more traffic
		but can uncover a lot more information.
	-wordlist	Use a seperate wordlist (one word per line).  Usage:

	perl fierce.pl -dns examplecompany.com -wordlist dictionary.txt

```

La sintaxis que usaremos ahora sera muy sencilla:
`fierce -threads <numero_de_hilos> -dns <servidor_a_buscar>`

Lo que hará `fierce` sera buscar direcciones IP en los bloques de la dirección de dominio y filtrar los resultados de aquellas direcciones IP que resulten tener un dominio terminado en la dirección proveída.

Aquí tenemos un ejemplo de subdominios de `uni.edu.ni`
<center><script type="text/javascript" src="https://asciinema.org/a/4ulhtq3d527g05b6m0abyn48u.js" id="asciicast-4ulhtq3d527g05b6m0abyn48u" async></script></center>
--------------

[DNSMap](https://github.com/makefu/dnsmap)
=======
DNSMap no busca un espacio o bloque de IP's bajo el nombre de dominio que buscamos, sino que prueba a traves de un diccionario los subdominios que podemos encontrar en un determindo dominio, asi como **cpanel.**`uni.edu.ni` ó **admin.**`uni.edu.ni`

Opciones:
```
dnsmap 0.30 - DNS Network Mapper by pagvac (gnucitizen.org)

usage: dnsmap <target-domain> [options]
options:
-w <wordlist-file>
-r <regular-results-file>
-c <csv-results-file>
-d <delay-millisecs>
-i <ips-to-ignore> (useful if you're obtaining false positives)

e.g.:
dnsmap target-domain.foo
dnsmap target-domain.foo -w yourwordlist.txt -r /tmp/domainbf_results.txt
dnsmap target-fomain.foo -r /tmp/ -d 3000
dnsmap target-fomain.foo -r ./domainbf_results.txt
```

Demo:
<center><script type="text/javascript" src="https://asciinema.org/a/ahbe0rvvtp9ahgkhgr46tdptj.js" id="asciicast-ahbe0rvvtp9ahgkhgr46tdptj" async></script></center>
--------------

[Recon-NG](https://bitbucket.org/LaNMaSteR53/recon-ng)
==========
El enfoque de Recon-NG es otro, en vez de usar la fuerza bruta, o escanear espacios de direcciones IP, lo que hace es buscar dominios usando una metodologia OSINT, en esta caso una busqueda en google (o bing, puede ser de los dos) para encontrar direcciones de subdominios sin tocar servidor alguno.

Demo con `uni.edu.ni`, `uca.edu.ni` y `uam.edu.ni` la cual podemos buscar los tres al mismo tiempo:
<center><script type="text/javascript" src="https://asciinema.org/a/85otkm9j2t9jm76aqdlziwjmw.js" id="asciicast-85otkm9j2t9jm76aqdlziwjmw" async></script></center>
--------------

Conclusion
============

No es una buena opcion solo quedarse con un metodo para obtener subdominios que nos pueden dar otro vector de ataque. Es mejor probar varios metodos, asi el de buscar por bloques de IP (Fierce), busquedas por Google y Bing (Recon-NG), fuerza bruta por diccionarios (DNSMap) e incluso otros metodos OSINT como Shodan, censys.io, NetCraft.
