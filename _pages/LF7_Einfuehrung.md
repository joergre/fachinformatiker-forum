---
layout: page
title: LF7 Einführung
image: '/images/posts/Tux.png'
---
Von <a rel="nofollow" class="external text" href="http://www.isc.tamu.edu/~lewing/">Larry Ewing</a>, <a rel="nofollow" class="external text" href="http://www.home.unix-ag.org/simon/">Simon Budig</a>, <a rel="nofollow" class="external text" href="https://github.com/garrett/Tux">Garrett LeSage</a> - <a rel="nofollow" class="external free" href="http://www.home.unix-ag.org/simon/penguin/">http://www.home.unix-ag.org/simon/penguin/</a>, <a rel="nofollow" class="external text" href="https://github.com/garrett/Tux">garrett/Tux</a> on GitHub, <a href="http://creativecommons.org/publicdomain/zero/1.0/deed.en" title="Creative Commons Zero, Public Domain Dedication">CC0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=753970">Link</a>

# Bind installieren

```bash
apt update
apt dist-upgrade
apt-get install bind9 bind9utils vim-nox
```

Wir löschen den kompletten Inhalt der Datei  /etc/bind/named.conf.options und fügen folgendes ein:

```bind
forwarders {
# Your ISP DNS IP(s) Here
10.0.0.2; # EC2 DNS for network
8.8.8.8; # Google DNS
};
```

Der Eintrag sorgt dafür, dass alle Anfragen zur Namensauflösung die der lokale DNS-Server nicht auflösen kann, an die DNS-Server von Amazon und Google weitergeleitet werden.

Die nächste Datei ist  /etc/bind/named.conf.local. Auch hier löschen wir den gesamten Inhalt und fügen den Inhalt der unten steht neu ein. Die IP-Adresse für die der Server in dem Fall zuständig ist, lautet 192.168.1.0-192.168.1.255. Beachte bitte die Schreibweise, beim Dateinamen werden die Tupel umgekehrt sortiert:

```bash
# Forwarding zone
zone "test.lab"{
        type master;
        file "etc/bind/zones/db.test.lab";
};

# Reserve lookup and server info
zone "1.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/zones/db.192";
};
```

Wir haben jetzt zwei Dateien definiert für die Namensauflösung. Einmal für die Vorwärtsauflösung (IP-Adresse -> Name) und die Reverseauflösung (Name -> IP-Adresse). Die eine Datei ist für die Domain test.lab zuständig und die zweite Datei für das IP-Netz 192.168.1.0.

# Zone anlegen

Wir müssen jetzt das Unterverzeichnis anlegen, dass wir in der Datei named.conf.local angegeben haben:

```bash
mkdir /etc/bind/zones
cd /etc/bind/zones
```

und es mach auch Sinn in das Verzeichnis mit cd zu wechseln, damit wir dort in Ruhe die Konfigurationsdateien anlegen können. Wir können eine Vorlage in das Verzeichnis kopieren oder die Datei komplett neu anlegen. Wir verwenden eine Vorlage:

```bash
cp ../db.local db.test.lab
```

Wie ändern die Datei so ab, dass sie wir folgt aussieht:

```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns.test.lab. root.(
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
test.lab.       IN      NS      ns.test.lab.
test.lab.       IN      A       192.168.1.50
test2.lab.      IN      A       192.168.1.51
```

# Reversezone anlegen

Auch für die Reversezone kopieren wir uns eine Corlage in das Verzeichnis /etc/bind/zones:
```bash
cp ../db.127 db.192
```

