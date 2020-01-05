---
layout: page
title: LF7 Server
---
(LPIC-2: Linux Engineer Exam 202)
# DNS-Server

Als Betriebssystem verwenden wir CentOS 7.

## Grundkonfiguartionen eines DNS-Servers

Die am häufigsten angewendete Konfiguration eines DNS-Systems ist das Zwischenspeichern (cachen) von Namensauflösungen. Der DNS-Server in eurem Router zu Hause macht z. B. genau dies.

Welcher DNS-Server wie verwendet wird, wird in Unix-Systemen über die Datei /etc/resolv.conf gesteuert. Diese Schauen wir uns mal an:

```bash
cat /etc/resolv.conf
# This file is managed by man:systemd-resolved(8). Do not edit.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "systemd-resolve --status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs must not access this file directly, but only through the
# symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a different way,
# replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 127.0.0.53
options edns0
```

In der vorletzten Zeile steht der verwendete DNS-Server (hier 127.0.0.53), es können hier auch mehrere DNS-Server stehen. In der Zeile danach wird eine Erweiterung aktiviert: Extension Mechanisms for DNS (EDNS0) die unter <https://tools.ietf.org/html/rfc2671> erklärt wird.

Besonders häufig wird die folgende Zeile in der Datei resolv.conf verwendet:

```bash
search your-server.de
```

Sie bedeutet, dass bei Eingabe eines Computernamens der ein lokaler Name ist, also nicht den Regeln des FQDN (<https://de.wikipedia.org/wiki/Fully-Qualified_Host_Name>) entspricht durch den Eintrag "your-server.de" entsprechend ergänzt wird. Aus lokaler-name wird also lokaler-name.your-server.de. Der search-Eintrag entspricht meistens dem lokalen Netz, dies muss aber nicht sein. Es darf nur eine Domain beim search-Eintrag angegeben werden. 

Die Datei /etc/resolv.conf wird aber zur Änderung des DNS-Servers nicht selbst verändert, sondern nur über ein Programm oder Meta-Konfigurationsdateien. Diese Programme unterscheiden sich stark von System zu System und dann auch noch welche Tools eingesetzt werden wie z. B. ein Networkmanager.

## Aufgabe
Warum wird der DNS-Server immer über IP-Adresse angesprochen und nie über den Namen?

## Testen der Namensauslösung

```bash
root@test:~# nslookup ferdinand-braun-schule.de
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   ferdinand-braun-schule.de
Address: 5.175.14.20
```

Wir sehen den DNS-Server der verwendet wurde (127.0.0.53) mit dem verwendeten Port (53) und das Ergebnis der Abfrage(5.175.14.20). Diesen Vorgang nennt man forward-lookup (Vorwärtsauflösung). Entsprechend gibt es auch eine Rückwärtsauflösung (reverse-lookup) die versucht einer IP-Adresse einen Namen zuzuordnen. Dies geht aber häufig schief:

```bash
root@test:~# nslookup 5.175.14.20
20.14.175.5.in-addr.arpa        name = wp498.webpack.hosteurope.de.

Authoritative answers can be found from:
```

Wir sehen, dass diese Website bei Host Europe gehostet wird und ein Webpacket ist. Dort werden jetzt auf einem Server viele Websites mit jeweils meist einen FQDN vorgehalten. Diese Auflösung wäre nicht eindeutig und damit wurde die Reverse-Auflösung abgeschaltet. Für Mail-Server ist die Reverse-Auflösung sehr wichtig da dort der Name für eine positive Zustellung von E-Mails eindeutig sein muss.

Wir nehmen noch ein Beispiel einer funktionierenden Rückwärtsauflösung:

```bash
joerg@fachinformatiker-forum:~$ nslookup fbs-fd.de
Server:         127.0.0.1
Address:        127.0.0.1#53

Non-authoritative answer:
Name:   fbs-fd.de
Address: 5.175.14.20
Name:   fbs-fd.de
Address: 2a01:488:42:1000:50ed:8214:ff8e:232a
```

Und sehen entsprechend die Ausgabe der IPv4 und IPv6-Adresse. Das ist der Server der schulischen E-Mail-Adressen. Bei nicht funktionierender Rückwärtsauflösung würden die von diesem Server versendeten E-Mails von anderen Servern nicht angenommen werden.

## Server installieren

Als Erstes installieren wir den am häufigsten verwendeten DNS-Server Bind (<https://de.wikipedia.org/wiki/BIND>):

```bash
yum install bind bind-utils
```

Hauptkonfigurationsdatei bearbeiten:

```bash
vim /etc/named.conf
```

Wir schauen uns die ersten Zeilen an:

```bash
options {
        listen-on port 53 { 127.0.0.1; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        recursing-file  "/var/named/data/named.recursing";
        secroots-file   "/var/named/data/named.secroots";
        allow-query     { localhost; };
```

Unser DNS-server hört auf Port 53 auf dem lokalen Rechner. Bei "directory" wir angegeben, in welchem Verzeichnis die Konfigurationsdateien liegen.
Dann wird das Verzeichnis für die Dump-Dateien angegeben. Diese entstehen beim Einsatz von Tools wie z. B. die Konfiguration des DNS-Servers zu überprüfen.

"allow-query" gibt die Computer an, die Anfragen stellen dürfen. Wir erweitern die Zeile:

```bash
allow-query {localhost; 172.31.44.88; !172.31.44.111; 172.31.44.0/24;}
```
Wir erlaube hier den Zugriff für die IP-Adresse 172.31.44.88 und localhost, darüber hinaus für das Subnetz 172.31.44.0. Wir verbieten den Zugriff für 172.31.44.111.

Weiter unten finden wir die Einstellung für DNSSEC, die aktiviert ist:

```bash
        dnssec-enable yes;
        dnssec-validation yes;
```
Darunter befinden sich die entsprechende Schlüssel.
Zum automatischen starten des Dienstes geben wir in die Konsole ein:

```bash
systemctl enable named
```

Zum starten des Systems:

```bash
systemctl start named
```

Beim Start wir ein Schlüssel erzeugt, der in der Datei /etc/rndc.key liegt.

Der Schlüssel wird z.B. gebraucht, damit rndc mit dem Server kommunizieren kann. RNDC wird verwendet um den DNS-Server zu steuern und Fehler zu finden.

Mit dem Befehl

```bash
rndc reload
```

werden alle Konfiguratinsdateien des DNS-Servers neu eingelesen.

Den Status des DNS-Servers kann man sehen mit Hilfe folgenden Befehls:

```bash
systemctl status named
```

Wir haben jetzt einen DNS-Server eingerichtet, der sicher ist und cached. Wir testen ihn mit dem Befehl:

```bash
dig @localhost ferdinand-braun-schule.de
```

## Cache löschen

Wir können mit 

```bash
rndc flush
```

den Cache löschen.

## Konfiguration testen

Mit dem Befehl

```bash
named-checkconf
```

kann die Konfiguration des DNS-Servers überprüft werden. Wenn der Befehl wie oben geschrieben ausgeführt wird, dann wird nur die Datei /etc/named.conf ohne(!) inkludierte Dateien überprüft. Einzelne Dateien werden mit

```bash
named-checkconf /pfad/zur/Datei
```

überprüft.

## Zonen einrichten

In der Datei /etc/named.conf fügen wir vor der Zeile mit dem rndc-key folgendes ein um eine Namensauflösung einzurichten:

```bash
zone "meinedomain.de" {
	type master;
	file "fwd.meinedomain.de.db";
};
```

Nach der Vorwärtsauflösung richten wir die Rückwärtsauflösung ein. Der Eintrag kommt direkt nach dem obigen Eintrag:

```bash
zone "44.31.172.in-addr.arpa" {
	type master;
	file "44.31.172.db";
};
```

Der Name besteht in der umgekehrt geschriebenen IP-Adresse, die Datei beschreibt also die IP-Adressen 172.31.44.*.

Wir legen jetzt die Datei fwd.meinedomain.de.db und 44.31.172.db an:

```bash
touch /var/named/fwd.meinedomain.de.db
touch /var/named/44.31.172.db
```

Wir öffnen die Datei fwd.meinedomain.de.db mit vim und geben folgendes ein:

```bash
$TTL 86400
@  IN  SOA     named.meinedomain.de. root.meinedomain.de. (
			10030	;Serial
			3600	;Refresh
			1800	;Retry
			604800	;Expire
			86400	;Minimum TTL
)
; Name Server
@	IN	NS	named.meinedomain.de.
; A Record
named	IN	A	172.31.44.88
mail	IN	A	172.31.44.89
mail2	IN	A	172.31.44.90
; C Record
dns	IN	CNAME	named.meinedomain.de.
; MX-Eintrag
@	IN	MX	10	mail ;Produktion
@	IN	MX	20	mail2	; Backup
```

Jetzt öffnen wir die Datei 44.31.172.db und geben ein:

```bash
$TTL 86400
@	IN	SOA	named.meinedomain.de. root.meinedomain.de. (
				10030 ;Serial
				3600	;Refresh
				1800	;Retry
				604800	;Expire
				86400	;Minimum TTL
)
; Name Server
@	IN	NS	named.meinedomain.de.
; Pointer
88	IN	PTR	named.meinedomain.de.
89	IN	PTR	mail.meinedomain.de.
90	IN	PTR	mail2.meinedomain.de.

```

## Überprüfen der Konfiguration

Als erstes überprüfen wir die Datei named.conf:

```bash
named-checkconf
```

Eine Zonen-Datei testen wir mit:

```bash
named-checkzone meinedomain.de fwd.meinedomain.de.db
```

Die Reverseauflösung mit:

```bash
named-checkzone 44.31.172.in-addr.arpa 44.31.172.db
```

Wir starten den DNS-Server neu:

```bash
systemctl restart named
```

## Aufgabe

1.) Teste ob der eingerichtete DNS-Server die Namen richtig auflöst.

2.) Kann unser DNS-Server auch ferdinand-braun-schule.de auflösen?

Es gibt übrigens 2 Arten von Konfigurationsfiles, einmal das Textbasierte das wir hier verwenden und auf der anderen Seite das binary-Format (masterfile-format). Bei DBS-Servern mit hoher Auslastung ist das Binary-Format von Vorteil da es die Geschwindigkeit erhöhen kann. Dies aber nur nötig wenn viele tausend Anfragen pro Stunde beantwortet werden müssen, daher überspringe ich hier die Umwandlung.

![Aufgabe](./images/pages/DNS.png)

# Chroot Jail

Wir schauen uns die Rechte von passwd an:

```bash
ls -al /etc/passwd
```

Was fällt auf? Wer darf die Datei lesen? Welche Informationen bekommt man über das System?

Dieser Umstand beinhaltet Sicherheitsprobleme da bei einem Fehler im DNS eventuell ein Zugriff auf die Datei /etc/passwd und andere Dateien möglich sein könnte.

Um dies auszuschließen wurde Chroot Jail entwickelt. Bei diesem Verfahren wird der Server gekapselt und hat nur Zugriff auf das Verzeichnis in dem er läuft.

Wir verwenden den von uns erstellten DNS-Server und halten ihn an:

```bash
systemctl stop named
```

Als erster Schritt muss dem System mitgeteilt werden, dass unser DNS in einem Chroot Jail läuft:

```bash
vim /etc/sysconfig/named
```

und tragen am Ende der Datei ein:

```bash
-t /chroot/named
```

Als nächstes legen wir ein Verzeichnis an:

```bash
mkdir /chroot
```

In diesem Verzeichnis wird der Prozess dann "eingeschlossen". Damit er funktioniert, müssen wir ein paar Unterverzeichnisse anlegen:

```bash
mkdir -p /chroot/named
mkdir -p /chroot/named/dev
mkdir -p /chroot/named/etc
mkdir -p /chroot/named/var/named
mkdir -p /chroot/named/var/run
```

Als nächstes müssen wir die Verzeichnisse mit Inhaltn füllen:

```bash
cp /etc/named.conf /chroot/named/etc
cp /etc/localtime /chroot/named/etc
cp -rf /var/named/* /chroot/named/var/named
```

Jetzt müssen wir die Berechtigungen setzen:

```bash
chown named:named -R /chroot/named
```

Wir überprüfen die Einstellungen:

```bash
cd /chroot
ll
cd var/named
ll
```

Wir müssen jetzt noch den Zufallszahlen-Generator und das "Null-Device" zur Verfügung stellen:

```bash
mknod /chroot/named/dev/random c 1 8
mknod /chroot/named/dev/null c 1 3
chmod 666 /chroot/named/dev/*
```

Wir überprüfen die Einstellungen:

```bash
ls -al /chroot/named/dev
```

Wir starten jetzt den Server:

```bash
systemctl start named
```

Und überprüfen den Start:

```bash
ps aux|grep named
```

Wir schauen uns jetzt das var-Verzeichnis an:

```bash
ll /chroot/named/var/named
```

Und können uns dor auch die Dateien anschauen:

```bash
cat /chroot/named/var/named/fwd.meinedomain.de.db
```

Wir testen den DNS-Server:

```bash
dig @localhost mail.meinedomain.de
```

# DNS Security Tools

In dieser Stunde geht es darum, dass der DNS-Server per Schlüssel überprüfen kann ob die DNS-Daten von einem Server kommen dem er vertraut. Das Feature ist standarmäßig installiert und muss nur fertig eingestellt werden. Siehe auch https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions. Der erste Schritt ist die Erstellung von Schlüsseln.


## Entropie

Für die Erstellung von Zufallszahlen ist eine hohe Entropie des Systems nötig (siehe auch https://de.wikipedia.org/wiki/Zufallszahlengenerator), daher erhöhen wir diese durch den Befehl rngd (siehe auch https://linux.die.net/man/8/rngd).

Das Tool rngd, dass Hardwaredaten des Computers verwendet um die Entropie zu erhöhen, ist noch nicht installiert. Wir durchsuchen zuerst das Repository nach möglichen Programmen:

```bash
yum provides "*/rngd"
```

Und istallieren den besten Kandidaten:

```bash
yum install rng-tools
```

Dann führen wir das Programm aus:

```bash
rngd -r /dev/random -o /dev/random -b
```

Wir erzeugen ein Schlüsselpaar für unsere Zone:

```bash
dnssec-keygen -a RSASHA256 -b 1024 -n ZONE -f KSK meinedomain.de
```

-a gibt den Algorithmus an

-b gibt die Schlüssellänge an. Wir nehemen aus Zeitgründen nur 1024, für ein Produktivsystem bitte mindestens 2048 verwenden.

Es werden zwei Dateien angelegt: .key und .private.

Wir brauchen noch ein zweites Schlüsselpaar:

```bash
dnssec-keygen -a RSASHA256 -b 1024 -n ZONE meinedomain.de
```

Wir kopieren zum signieren der Zonendatei diese in unsere Home-Verzeichnis:

```bash
cp /var/named/fwd.meinedomain.de.db .
```

Nun signieren wir die Zone:

```bash
dnssec-signzone -o meinedomain.de -S fwd.meinedomain.de.db
```

Es werden zwei neue Dateien angelegt: dsset-* und *.signed. Die signierte Datei *.signed ist die deutlich größere Datei.

Wir schauen uns die Datei an:

```bash
cat fwd.meinedomain.de.db.signed 
```

und sehen die Signaturen.

Wir kopieren die Datei dann zurück und tragen sie in die Datei named.conf ein (einfach den unsignierten Namen ersetzen).

# Web-Server

Wir installieren einen Webserver, hier den Apache:

```bash
yum install httpd
```

Die Konfiguaration findest, wie bei allen Servern im Verzeichnis /etc statt und hier speziell im Verzeichnis /etc/httpd.

Eine der wichtigsten Konfigurationsdateien ist:

```bash
vim /etc/httpd/conf/httpd.conf
```

Hier insbesondere der Eintrag ServerRoot, der das Verzeichnis mit der Konfigurationsdateien angibt. 

## Port und IP-Adresse

Der Eintrag Listen gibt an auf welchen Port bzw. IP der Server reagiert. Standardmäßig hört er auf alle IP-Adressen und Port 80.

DocumentRoot gibt das Veröffentlichungsverzeichnis an, indem die Dateien stehen, die nach aussen exportiert werden.

## LogLevel

In der Datei httpd.conf wird ebenfalls das LogLevel eingestellt. Bitte beachte, dass ein hoher LogLevel wie Debug die Festplatte und insbesondere die SSD sehr belasten.

Die Logfiles werden unter /var/log/httpd gespeichert.

Es gibt das access_log indem alle Zugriffe auf die angebotene Website protokoliert werden. Es gibt auserdem das error_log indem Fehler des Webservers protokolliert werden. 

## Automatischer start

Mit dem Befehl

```bash
systemctl enable httpd
```

wird der Webserver automatisch bei reboot etc. gestartet.

## Starten und Anhalten des Webservers

Starten des Webservers:

```bash
systemctl start httpd
```

Anhalten des Servers:

```bash
systemctl stop httpd
```

## Installation einiger Tools

Wir möchten jetzt noch einen Telnet-Client und 2 Browser für die Konsole installieren:

```bash
yum install telnet lynx w3m
```

Telnet wird z.B. verwendet um den Server schnell zu testen:

```bash
telnet localhost 80
```

Danach geben wir "helo" ein. Ebenso verwnden wir die anderen beiden Tools:

```bash
lynx http://localhost
w3m http://localhost
```

## PHP-Modul installieren





