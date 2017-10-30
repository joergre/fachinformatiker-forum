---
layout: post
title: Tag der offenen Tür 2017
featured: true
author: joerg_reuter
tags: [tips, raspberry, screenshot]
image: '/images/posts/320px-2008-01-06-125534_1680x1050_scrot.png'
---

In dem Artikel geht es um das Problem, einen Screenshot schnell und effektiv zu erstellen. Das Problem verschärft sich in abetracht der hier eingesetzten Plattform: ein Raspberry Pi. (Scrot)[http://en.wikipedia.org/wiki/Scrot] Istein Tool für die Shell, die diese Aufgabe perfekt mit minimalen Ressourcenaufwand erfüllt. Der Name Scrot wurde von dem Wort Screenhot abgeleited.

# Installation 

Die Installation geht einfach von der Hand:

```bash
sudo apt-get install scrot
```

Ein Beispiel für die Anwendung ist:

```bash	
scrot -cd 10 screenshot.png
```

cd steht für eine Verzögerung der Aufnahme von 10 Sekunden. Mit -u wird nur das aktive Fenster aufgenommen. Eine schöne Übersicht der möglichen Paramter findert Ihr (hier)[https://wiki.ubuntuusers.de/Scrot/].
