---
layout: post
title: Eigenen Domän-Namen für Github-Projekt-Seite
featured: true
author: joerg_reuter
tags: [github, gh_pages]
image: '/images/posts/220px-Dns-raum.svg.png'
---
Quelle Bild: <https://de.wikipedia.org/wiki/Domain_(Internet)#/media/File:Dns-raum.svg>

Es ist möglich, mit Hilfe von Github kostenlos eine Internetseite zu hosten (siehe <https://pages.github.com/Y>). Ebrenso kann ein eigener Domän-Namen verwendet werden. Bei der Verwendung eines eigenen Namens kann allerdings kein SSL verwendet werden.

Zur Einrichtung:

1.) Richte bitte bei Deinem DNS-Anbierter einen A-Record ein, der Deine Domän auf 192.30.252.153 und/oder 192.30.252.154 umleitet. IPv6 wird nicht unterstützt!
2.) In den Zweig 'gh-pages' kommt in das oberste Verzeichnis eine Datei mit dem Namen 'CNAME' (bitte ebenfalls groß schreiben). In diese Datei kommt nur Deine Domäne (ohne http oder https und ohne Verzeichnis).

 Dann warte bitte ein paar Minuten bis der Befehl die entsprechende Eintragung ausgibt:

```bash
pi@pi-desktop:~/fachinformatiker-forum $ dig fachinformatiker-forum.de +nostats +nocomments +nocmd

; <<>> DiG 9.10.3-P4-Raspbian <<>> fachinformatiker-forum.de +nostats +nocomments +nocmd
;; global options: +cmd
;fachinformatiker-forum.de.	IN	A
fachinformatiker-forum.de. 1947	IN	A	192.30.252.153

```

Hier siehst Du eine Besipiel-Datei: <https://github.com/joergre/fachinformatiker-forum/> und die dazugehörige Webseite: <http://fachinformatiker-forum.de>