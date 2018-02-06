---
layout: page
title: LF7 Einführung
image: '/images/posts/Tux.png'
---
Von <a rel="nofollow" class="external text" href="http://www.isc.tamu.edu/~lewing/">Larry Ewing</a>, <a rel="nofollow" class="external text" href="http://www.home.unix-ag.org/simon/">Simon Budig</a>, <a rel="nofollow" class="external text" href="https://github.com/garrett/Tux">Garrett LeSage</a> - <a rel="nofollow" class="external free" href="http://www.home.unix-ag.org/simon/penguin/">http://www.home.unix-ag.org/simon/penguin/</a>, <a rel="nofollow" class="external text" href="https://github.com/garrett/Tux">garrett/Tux</a> on GitHub, <a href="http://creativecommons.org/publicdomain/zero/1.0/deed.en" title="Creative Commons Zero, Public Domain Dedication">CC0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=753970">Link</a>

Bei fragen <https://discord.gg/TsaqyAQ>

# Installation von Programmen

Bei Windows wird eine Datei im Format .exi oder .msi bespielsweise aus dem Internet geladen und ausgeführt. Bei Linux gibt es für die meisten Programme ein Repository und zwar für jede Disribution ein eigenes. Das Repository sorgt auch für die Update-Versorgung sowohl der Gerätetreiber wie der Anwendungssoftware.

Es gibt Repositories die aus Quellcode bestehen (Gentoo, Slack) oder aus bereits fertig kompilierten Paketen (CentOS, Fedora, SuSe, OpenSuSe und RedHat Enterprise Linux mit .rpm und Debian, Elementary OS. Linux Mint und Ubuntu mit .deb).

## YUM Package Managment

Wir möchten auf einem CentOS einen WebServer installieren. Dazu suchen wir im Repository nach dem Paket:

```bash
yum search http
```

Wir finden in der Ausgabe das Paket httpd und schauen und die Informationen zu dem Paket an um festzustellen, ob dies ein Webserver beinhaltet:

```bash
yum info httpd
```

Wir installieren das Paket:

```bash
yum install httpd
```
Die erfolgreiche Installation können wir überprüfen:

```bash
yum list installed httpd
```

Wir starten den Web-Server:

````bash
service httpd start
````

und nüberprüfen die Funktionalität in dem wir auf die Webseite wechseln: <http://[IP-Adresse]>.

Auch die Abhängigkeiten des Pakets wie Libaries können wir uns anzeigen lassen:

```bash
yum deplist httpd
```

Wir möchten den Webserver wieder deinstallieren:

```bash
yum remove httpd
```

Der Befehl löscht nur den Webserver aber nicht die Abhängigkeiten. Um diese zu löschen, verwenden wir diesen Befehl:

```bash
yum autoremove httpd
```

Yum stellt dabei sicher, dass keine Abhängigkeiten gelöscht werden die von anderen Paketen noch gebraucht werden.

Wir können uns eine Liste der Repositories ausgen lassen, die von unserem System verwendet werden:

```bash
yum repolist
```

Die Informationen werden als Textdateien abgelegt. Wir wechseln in das Verzeichnis:

```bash
cd /etc/yum.repos.d
```

Mit 

```bash
ls
```
können wir uns die Dateien dort anschauen. Pro Repository gibt es eine Datei.

Wir schauen uns eine Datei an:

```bash
less CentOS-Base.repo
```

Wir möchten jetzt die aktuellen Updates einspielen. Dazu löschen wir zuerst die alten Repo-Dateien:

```bash
yum clean all
```

Jetzt holen wir die aktuellen Listen:

```bash
yum update
```

Wir installieren jetzt PowerShell: <https://github.com/PowerShell/PowerShell/blob/master/docs/installation/linux.md#centos-7>


# Bind installieren

Ein ergänzender Artikel ist hier zu finden <https://wiki.ubuntuusers.de/DNS-Server_Bind/>

```bash
apt update
apt dist-upgrade
apt-get install bind9 bind9utils vim-nox host telnet dnsutils
```

Wir löschen den kompletten Inhalt der Datei  /etc/bind/named.conf.options und fügen folgendes ein:

```bind
options{
forwarders {
# Your ISP DNS IP(s) Here
8.8.8.8; # Google DNS
8.8.4.4; # Google DNS #2
};
};
```

Der Eintrag sorgt dafür, dass alle Anfragen zur Namensauflösung die der lokale DNS-Server nicht auflösen kann, an die DNS-Server von Amazon und Google weitergeleitet werden.

Die nächste Datei ist  /etc/bind/named.conf.local. Auch hier löschen wir den gesamten Inhalt und fügen den Inhalt der unten steht neu ein. Die IP-Adresse für die der Server in dem Fall zuständig ist, lautet 192.168.1.0-192.168.1.255. Beachte bitte die Schreibweise, beim Dateinamen werden die Tupel umgekehrt sortiert:

```bash
# Forwarding zone
zone "test.lab"{
        type master;
        file "/etc/bind/zones/db.test.lab";
};

# Reserve lookup and server info
zone "1.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/zones/db.192.168.1";
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

Wie ändern die Datei db.test.lab so ab, dass sie wie folgt aussieht:

```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns1.test.lab. admin.test.lab. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
        IN      NS      ns1.test.lab.
ns1     IN      A       127.0.0.1
test    IN      A       192.168.1.50
test2   IN      A       192.168.1.51
test3   IN      A       192.168.1.52
```

# Reversezone anlegen

Auch für die Reversezone kopieren wir uns eine Vorlage in das Verzeichnis /etc/bind/zones:
```bash
cp ../db.127 db.192.168.1
```

Wir editieren jetzt die Datei db.192.168.1:

```bash
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns1.test.lab. admin.test.lab. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
        IN      NS      ns1.test.lab.

50      IN      PTR     test.test.lab.
51      IN      PTR     test2.test.lab.
52      IN      PTR     test3.test.lab.
```

# resolve.conf

Die Einrichtung des Servers ist soweit abgeschlossen. Jetzt müssen wir den DNS-Server unserem System bekannt machen, damit unser Server auch etwas zu tun bekommt. Dies geschieht in der Datei  /etc/resolv.conf, die wir wie folgt abändern:

```bash
nameserver 127.0.0.1 # Unser Nameserver
```

# Testen der Koniguration

Testen der Forward-Auflösung:

```bash
named-checkzone test.lab /etc/bind/zones/db.test.lab
zone lab/IN: loaded serial 2
OK
```

Testen der Reverse-Auflösung:

```bash
named-checkzone 1.168.192.in-addr-arpa /etc/bind/zones/db.192.168.1
zone test.lab/IN: loaded serial 1
OK
```
Und zum Abschluss:

```bash
named-checkconf -zj
zone test.lab/IN: loaded serial 2
zone 1.168.192.in-addr.arpa/IN: loaded serial 1
zone localhost/IN: loaded serial 2
zone 127.in-addr.arpa/IN: loaded serial 1
zone 0.in-addr.arpa/IN: loaded serial 1
zone 255.in-addr.arpa/IN: loaded serial 1
```
Wir starten den Bind neu, damit alle Konfigurationsdateien neu eingelesen werden:

```bash
service bind9 restart
 * Stopping domain name service... bind9                                        waiting for pid 3988 to die
                                                                         [ OK ]
 * Starting domain name service... bind9                                 [ OK ]
```

Wir überprüfen, dass der Dienst auch wirklich läuft:

```bash
root@acde03affad0:/etc/bind/zones# service bind9 status
 * bind9 is running
```

## rndc: connect failed: 127.0.0.1#953: connection refused

Diese Fehlermeldung weist darauf hin, das Bind nicht gestartet wurde. Es ist extrem unwahrscheinlich, dass der Fehler im Programm rndc liegt. Vielmehr benötigt rndc einen laufenden Bind und da er nicht richtig gestartet ist kommt die Fehlermeldung.

# Tests

Forward-Auflösung testen:

```bash
root@acde03affad0:/etc/bind/zones# host test.test.lab
test.lab has address 192.168.1.50
root@acde03affad0:/etc/bind/zones# host test2.test.lab
test2.lab has address 192.168.1.51
```

Rückwärtsauflösung:

```bash
root@acde03affad0:/etc/bind/zones# host 192.168.1.50
50.1.168.192.in-addr.arpa domain name pointer test.test.labs.
root@acde03affad0:/etc/bind/zones# host 192.168.1.51
51.1.168.192.in-addr.arpa domain name pointer test2.test.labs.
```

## Test mit nslookup

```bash
root@acde03affad0:/etc/bind/zones# nslookup test.test.lab
Server:         192.168.1.50
Address:        192.168.1.50#53

Name:   test.test.lab
Address: 192.168.1.50
```

oder

```bash
root@acde03affad0:/etc/bind/zones# nslookup test2.test.lab
Server:         192.168.1.50
Address:        192.168.1.50#53

Name:   test2.lab
Address: 192.168.1.51
```

Und Reverse:

```bash
root@acde03affad0:/etc/bind/zones# nslookup 192.168.1.50
Server:         192.168.1.50
Address:        192.168.1.50#53

50.1.168.192.in-addr.arpa       name = test.test.labs.
```

oder

```bash
root@acde03affad0:/etc/bind/zones# nslookup 192.168.1.51
Server:         192.168.1.50
Address:        192.168.1.50#53

51.1.168.192.in-addr.arpa       name = test2.test.labs.
```

Wir können auch eine Anwendung verwenden um die Auflösung zu überprüfen wie telnet:

```bash
root@acde03affad0:/etc/bind/zones# telnet test2.test.lab
Trying 192.168.1.51...
telnet: Unable to connect to remote host: No route to host
```
Der Befehl bricht mit einem Fehler ab, da kein Telnet-Server auf dem System läuft bzw. der Computer nicht existiert. Aber die Namensauflösung ware erfolgreicht, da die IP-Adresse korrekt angezeigt wird.

# Apache

