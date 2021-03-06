---
layout: page
title: LF7 Einführung
image: '/images/posts/Tux.png'
---
Von <a rel="nofollow" class="external text" href="http://www.isc.tamu.edu/~lewing/">Larry Ewing</a>, <a rel="nofollow" class="external text" href="http://www.home.unix-ag.org/simon/">Simon Budig</a>, <a rel="nofollow" class="external text" href="https://github.com/garrett/Tux">Garrett LeSage</a> - <a rel="nofollow" class="external free" href="http://www.home.unix-ag.org/simon/penguin/">http://www.home.unix-ag.org/simon/penguin/</a>, <a rel="nofollow" class="external text" href="https://github.com/garrett/Tux">garrett/Tux</a> on GitHub, <a href="http://creativecommons.org/publicdomain/zero/1.0/deed.en" title="Creative Commons Zero, Public Domain Dedication">CC0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=753970">Link</a>

Bei Fragen, bitte die Diskussionsmöglichkeit unten verwenden.

# Installation von Programmen

Bei Windows wird eine Datei im Format .exe oder .msi bespielsweise aus dem Internet geladen und ausgeführt. Bei Linux gibt es für die meisten Programme ein Repository und zwar für jede Disribution ein eigenes. Das Repository sorgt auch für die Update-Versorgung sowohl der Gerätetreiber wie der Anwendungssoftware.

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

und überprüfen die Funktionalität in dem wir auf die Webseite wechseln: <http://[IP-Adresse]>.

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

# RPM Pakete

Yum arbeitet mit Repositories, der Befehl rpm wird hingegen verwendet um einzelne Programme zu installieren, deren Paket lokal vorliegt (z.B. über download). 

Zum installieren eines Pakets wird der Parameter -i verwendet (rpm -i <rpmDateiName.rpm>). Weitere typische Parameter sind -v für die Ausgabe des Programs und -h für einen Statusbalken. Es macht jetzt wenig Sinn, den Apache über rpm zu installieren da dieser ja über die Repositories zu installieren ist. Eine typische Verwendung ist die Installation von Sun Java, dass wir hier herunterladen: <https://www.java.com/de/download/linux_manual.jsp> Leider erlauben die Lizenzbestimmungen nicht das direkte herunterladen, weshalb die Datei erst manuell auf den Desktop geladen werden muss und dann mit Filezilla oder SCP auf den Server kopiert wird. Kürzer geht es mit folgendem Befehl:

```bash
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm"
```

Und installieren wir es mit dem Befehl:

```bash
sudo rpm -ivh jdk-8u131-linux-x64.rpm
```

Mit dem Befehl

```bash
java -version
```

können wir die Installation überprüfen und mit folgendem Befehl den Ort der Installation:

```bash
which java
```

Mit dem Befehl sehen wir die Abhängigkeiten des Programms

```bash
rpm -qpR jdk-8u131-linux-x64.rpm
```

Es ist ein Update erschienen, dass unbedingt eingespielt werden muss:

```bash
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.rpm"
```

Und mit dem folgenden Befehl spielen wir das Update ein:

```bash
 rpm -Uvh jdk-8u161-linux-x64.rpm
```

Wir überprüfen nun die installierte Version:

```bash
[user@joergreuter4 ~]$ sudo rpm -qi jdk1.8
Name        : jdk1.8
Epoch       : 2000
Version     : 1.8.0_161
Release     : fcs
Architecture: x86_64
Install Date: Fri 09 Feb 2018 01:22:12 AM UTC
Group       : Development/Tools
Size        : 289899092
License     : http://java.com/license
Signature   : (none)
Source RPM  : jdk1.8-1.8.0_161-fcs.src.rpm
Build Date  : Wed 20 Dec 2017 12:29:52 AM UTC
Build Host  : sc11137389.us.oracle.com
Relocations : /usr/java
Packager    : Java Software <jre-comments@java.sun.com>
Vendor      : Oracle Corporation
URL         : URL_REF
Summary     : Java Platform Standard Edition Development Kit
Description :
The Java Platform Standard Edition Development Kit (JDK) includes both
the runtime environment (Java virtual machine, the Java platform classes
and supporting files) and development tools (compilers, debuggers,
tool libraries and other tools).

The JDK is a development environment for building applications, applets
and components that can be deployed with the Java Platform Standard
Edition Runtime Environment.
```

Und bekommen das erfolgreiche Update durch die Versionsnummer 161 bestätigt.

Mit dem Parameter -e deinstallieren wir das Paket

```bash
rpm -e jdk1.8
```

Und überprüfen die Deinstallation mit dem Befehl:

```bash
[user@joergreuter4 ~]$ sudo rpm -qi jdk1.8
package jdk1.8 is not installed
```

Es gibt auch die Möglichkeit, mit dem Parameter --test eine Ausführung zu simulieren (der Befehl wird also nicht richtig ausgeführt aber Fehlermeldungen angezeigt:

```bash
rpm -e --test jdk1.8
```

## APT

Die Repositories stehen hier in der Datei /etc/apt/sources.list und dem Verzeichnis /etc/apt/sources.list.d. Früher war die gesamte Liste in der Datei sources.list und momentan findet eine Aufteilung statt auf verschiedene Dateien im Verzeichnis /etc/apt/sources.list.d

## Aufgabe

Schaue die bitte die Datei sources.list und das Verzeichnis /etc/apt/sources.list.d an.

## apt update

Die Repositories (die Metadaten der einzelnen Programme wie Versionsnummern etc.) müssen mit dem System abgeglichen werden:

````bash
apt update
````

## apt search

Wir möchten den Apache Webserver installieren und schauen dafür nach den passenden Paketnamen:

````bash
apt search apache
````

oder

````bash
apt-cache search apache
````

Zu beachten ist hierbei, dass nicht online gesucht wird sondern nur im sogenannten Cache. Als Cache bezeichnen wir in diesem zusammenhang die Kopie der Metadaten des Online-Repositories auf unserem lokalen System. Bei vielen Cloud-Anbietern führt dies dazu, dass ohne ein 'apt update' keine Pakete gefunden werden können.

## apt install

Wir installieren jetzt den Webserver:

````bash
apt install apache2
````

Wir starten den Web-Server:

````bash
service apache2 start
````

und überprüfen die Funktionalität in dem wir auf die Webseite wechseln: <http://[IP-Adresse]>.

## apt remove

Wir löschen den Webserver wieder:

````bash
apt remove apache2
````

Wir sehen allerdings, dass unter /etc/apache2 noch die Konfigurationsdateien vorhanden sind. Diese löschen wir mit einem anderen Befehl, der sowohl das Apache2 als auch alle Konfigurations-Dateien löscht! Dieser Unterschied ist vor z.B. im Zusammenhang mit Datenbanken nicht unerheblich: Bei remove sollten die Inhalte der Datenbank noch vorhanden sein und nach der Neuinstalaltion wieder eingebunden werden können (bitte trotzdem vorher ein Backup machen!!). Bei purge ist die Datenbank dann gelöscht.

```bash
apt purge apache2
````

oder

````bash
apt remove --purge apache2
````

Purge bedeutet übrigens Säuberung, insbesondere auch politische. 

Wir sehen den Hinweis, dass im Falle des Webservers die HTML-Seiten nicht glöscht worden sind. Das ist sehr nett von der Routine, auf dieses Verhalten sollte man sich allerdings keinesfalls verlassen, es gehört eher zu den Ausnahmen. Desweiteren sehen wir, das Abhängigkeiten istallirt wurden, die nicht mehr benötigt werden. Also deinstallieren wir diese:

````bash
apt autoremove
````

## apt upgrade

Um ein System einem Update zu unterziehen werden die Metadaten des Repositories mit denen der installierten Pakete verglichen. Bei einer Divergenz wird das aktuellere Paket installiert. Ist das installierte Paket aktueller (wurde also manuell eingespielt), gibt es eine Fehlermeldung:

````
apt upgrade
````

Upgrade installiert keine neuen Pakete (z.B. durch Abhängigkeiten) und führt keine operationen durch, die die stabilität des Systems gefährten könnte. Um alle Pakete zu installieren gibt es zwei Möglichkeiten:

````bash
apt dist-upgrade
````

oder

````bash
apt full-upgrade
````

Um auf eine neue Version zu kommen (z.B. von Ubuntu 17.04 auf Ubuntu 17.10) wird ein Skript verwendet:

````bash
do-release-upgrade
````

Das Skript hält den Upgrade-Pfad ein, also von LTS-Version zur nächsten LTS-Version oder aber von der regulären Version zur nächsten. Das Skript wird nciht zeitnah zur Veröffentlichung aktiviert, sondern hat einen Schutz dass es erst mit Verögerung das Upgrade anbietet. Dies wird gemacht um die Stabilität des Systems sicher zu stellen. bei LTS-Versionen wird es z.B. erst bei ersten Update (z.B. Ubuntu 16.04.1) freigeschaltet.

# dpkg

Um alle installierten Pakete anzeigen zu lassen:

````bash
dpkg --get-selections
````

Die Hauptbedeutung kommt dpkg allerding zu, um einzelne Pakete zu installieren. Wir möchten bespielsweise einen Webserver in einer Version installieren, der noch nicht im Repository aufgenommen wurde.

Achtung: Das manuelle installieren von Paketen ist mit einem erheblichen Risiko für das System verbunden und kann im schlimmsten Falle zu inkonsistwenten Systemen mit nicht erfüllten Abhängigkeiten führen. Die manuelle Instalaltion von .deb-Paketen ist eine Notlösung, insbesondere wenn Abhängigkeiten im Verlauf der Instalaltion manuell nachgezogen werden müssen!

Wir laden das Paket von der Seite <https://pkgs.org/download/apache2>:

````bash
wget http://archive.ubuntu.com/ubuntu/pool/main/a/apache2/apache2_2.4.18-2ubuntu3_amd64.deb
````

Wir schauen uns die Informationen zu dem Paket an:

````bash
dpkg-deb -I apache2_2.4.18-2ubuntu3_amd64.deb
````

Wir schauen nach, ob das Paket bereits installiert ist:

````bash
user@joergreuter3:~$ dpkg --get-selections apache2_2.4.18-2ubuntu3_amd64
dpkg: no packages found matching apache2_2.4.18-2ubuntu3_amd64
````

Und stellen fest, dass wir das Paket noch nicht installiert haben.

Den Inhalt des Pakets können wir uns ebenfalls anzeigen lassen:

````bash
dpkg-deb --contents apache2_2.4.18-2ubuntu3_amd64.deb
````

Wir versuchen das Paket zu installieren:

````bash
dpkg -i apache2_2.4.18-2ubuntu3_amd64.deb
````

Die installation scheitert, weil die Abhängigkeiten nicht erfüllt sind! 
Wir installieren die Abhängigkeiten:

````bash
apt -f upgrade
````

Jetzt starten wir Apache wie gewohnt und besuchen die Website:

````bash
service apache2 start
````

Um das Paket wieder zu deinstallieren:

````bash
dpkg -r apache2
````

Wir sehen, dass das Paket noch als isntalliert erscheint:

````bash
dpkg -l apache2
````

und die Start-Skripte sind ebenfalls noch vorhanden. 

Daher löschen wir das Paket mit allen Konfigurationen (purge):

````bash
dpkg -P apache2
````

Jetzt wird das Paket als gelöscht angezeigt:

````bash
dpkg -l apache2
````

# Which

Manchmal ist die Antwort auf die Frage interessant: "Wo ist dieses Programm instaliert?" Beispielsweise des Befehls "ls". Die Antwort erhalten wir mit folgendem Befehl:

````bash
which ls
````

Woher weiß jetzt das System in welchem Ordner das Programm installiert ist? Über das sogenannte "environment" also der Umgebung. Mit Umgebung bezeichnet man die Menge aller Syxstemvariablen und bekommt sie angezeigt mit:

````bash
env
````

Relativ weit unten kommt eine Zeile, die mit PATH anfängt, sie gibt alle Verzeichnisse an, in dem das Betriebssystem nach einem Befehl sucht.

````bash
PATH=/home/user/bin:/home/user/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
````

Wir können uns auch nur die Variable ausgeben lassen:

````bash
echo $PATH
````

# Das erste Script

Wir schreiben ein Script, dass den Befehl "ls -a" ausführen soll. Wir nennen die Datei ls.sh. Dazu öffnen wir die datei mit unserem Lieblingseditor und schreiben hinein:

````bash
ls -a
````

Wir können das Skript nicht ausführen, weil die entsprechende Berechtigung fehlt:

````bash
user@joergreuter3:~$ ./ls.sh
-bash: ./ls.sh: Permission denied
````

Als nächstes müssen wir die Rechte setzen. Die momentanen Rechte sehe wir mit:

````bash
user@joergreuter3:~$ ls -l
total 4
-rw-rw-r-- 1 user user 6 Feb 25 14:12 ls.sh
````

Wir sehen an erster Stelle ein "-", dies bedeutet dass es sich um eine Datei handelt. Es gibt nich viele andere mögliche Zeichen, z.B. ein d für ein Verzeichnis (directory).

Dann kommen die Buchstaben 'rw-', 'rw-' und 'r--'. 'r' steht für das Leserecht (read), 'w' für das Schreibrecht (write) und 'x' steht für das Ausführungsrecht (execute). Da das x-Recht nicht gesetzt ist, steht dort ein '-'. Die erste Gruppe von drei Zeichen beschreibt die Rechte des Eigentümers (user), die zweite Gruppe die der Benutzergruppe (group) und die letzte Gruppe die Rechte der Benutzer die weder Eigentümer der Datei noch in der entsprechenden Gruppe sind (others). Wir sehen, dass die "anderen" nur ein Leserecht haben.

Wir sehen dann eine 1, dass die Anzahl der Dateien. Bei einer einzelnen Datei steht hier immer eine 1. Danach kommt der Besitzer (user) und die Gruppe (user) und das Erstellungsdatum (6. Februar)und die Uhrzeit (14:12). Die Uhrzeit wird ausgeblendet, wenn die Datei älter als ein Jahr ist und durch die Jahreszahl ersetzt. Die Zahl zwischen Datum und Uhrzeit gibt die Größe der Datei in Bytes an.

Um die Datei auszuführbar zu machen, verwenden wir folgenden Befehl:

````bash
chmod u+x ls.sh
````

und schauen uns das Ergebnis an:

````bash
user@joergreuter3:~$ ls -l
-rwxrw-r-- 1 user user 6 Feb 25 14:12 ls.sh
````

Wir sehen, dass ein x dazu gekommen ist. Das u steht für users, dass '+' für hinzufügen und das 'x' für das Ausführrecht. 
Wir führen die Datei jetzt aus:

````bash
user@joergreuter3:~$ ./ls.sh
.   .bash_history  .bashrc  .local  .profile                   .viminfo
..  .bash_logout   .cache   ls.sh   .sudo_as_admin_successful
````

Wir müssen momentan ein './' davor schreiben, weil das System nicht weiß in welchem Verzeichnis die Datei liegt. Würden wir das './' weglassen würde das System die Pfade der Variable $PATH absuchen und unsere Datei nicht finden:

````bash
user@joergreuter3:~$ ls.sh
ls.sh: command not found
````

'./' steht für das Verzeichnis indem wir uns gerade befinden. Dieses Verzeichnis wird beim Prompt angezeigt: '~'. Das '~' steht als Abkürzung für das Heimatverzeichnes des gerade angemeldeten Benutzer.

'./ls.sh' ist der relative Pfad und wir können auch den absoluten Pfad verwenden:

````bash
/home/user/ls.sh
````

Wir können jetzt unseren Befehl in ein Directory kopieren, das in der PATH-Variable angegeben ist:

````bash
sudo cp ls.sh /bin
````

Beim ausführen bekommen wir eine Fehlermeldung:

````bash
user@joergreuter3:~$ ls.sh
-bash: /bin/ls.sh: Permission denied
````

Das liegt daran, dass beim kopieren in bin der Besitzer zu root gewechselt ist. Uns gehört die Datei nichtmehr. Also passwn wir die Rechte an:

````bash
sudo chmod a+x /bin/ls.sh
````

'a' steht für all und setzt für alle die Ausführungsrechte. Jetzt können wir den Befehl verwenden.

Wir können auch die Variable $PATH unseren Bedürfnissen anpassen. Wir möchten unser Programm ls.sh im Verzeichnis /opt haben und es soll dort auch gefunden werden:

````bash
mv /bin/ls.sh /opt/
PATH=$PATH:/opt
````

# History

Mit dem Befehl 'history' sehen wir alle Befehle die unser User auf diesem System ausgeführt hat. Die Befehle werden in der Datei .bash_history gespeichert.


````bash
user@joergreuter3:~$ history
    1  su -
    2  sudo apt update
    3  sudo apt-get install -y powershell
    4  curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    5  curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/microsoft.list
    6  sudo apt-get update
    7  sudo apt-get install -y powershell
    8  pswh
````

Um den Befehl 8 auszuführen geben wir ein:

````bash
user@joergreuter3:~$ !8
pswh
No command 'pswh' found, did you mean:
 Command 'psw' from package 'wise' (universe)
pswh: command not found
````

# Locate und find

Auch unter Linux steht eine Indexsuche zur Verfügung. Indexsuche bedeutet, dass eine Datenbank durchsucht wird anstelle der realen Dateinamen auf dem Speichermedien. Der grosse Vorteil ist die hohe Geschwindigkeit. Die Aktualität des Index bestimmt masgeblich über den Erfolg der suche. Der Befehl heißt "locate" und wir installieren ihn mit:

````bash
apt install locate
````

Und wir suchen nach ls:

````bash
locate ls
````

Dem gegenüber durchsucht der Befehl find wirklich die  Verzeichnisse auf den Laufwerken. Find muss normalerweise nicht extra installiert werden:

````bash
find / -name ls
````

Der erste Parameter gibt den Startpunkt der Suche an, hier "/" und der zweite Parameter wonach gesucht werden soll. Wollen wir nach Namen suchen, die ls enthalten, so rufen wir den Befehl wie folgt auf:

````bash
find / -name '*ls*'
````

# Man

"Man" steht für Manual und bietet Hilfen für die Benutzung von Befehlen an. Manpages (also Manual-Seiten) sind in Sektionen unterteilt und haben einen standardisierten Aufbau. Sowohl die Unterteilung wie der Aufbau der einzelnen Seiten sind standardisiert. Siehe hierzu <https://de.wikipedia.org/wiki/Manpage>.

Wir schauen uns die Hilfe zum Befehl "ls" an:

````bash
man ls
````

Die angezeigte Seite sollte in etwa der Webseite entsprechen: <http://man7.org/linux/man-pages/man1/ls.1.html>.

Links oben steht ls(1) und zeigt an, dass wir uns in der ersten Sektion befinden. In der ersten Sektion werden Befehle erklärt.

Wir schauen uns den Befehl passwd an:

````bash
man passwd
````

und erhalten eine Ausgabe ähnliche <http://man7.org/linux/man-pages/man1/passwd.1.html>. Wir sehen am Ende des Textes einen Hinweis, dass es auf einen Eintrag von passwd in Sektion 5 gibt. Es gibt einmal den Befehl passwd und auf der anderen Seite die Konfigurationsdatei passwd. Konfigurationsdateien sind in Sektion 5 abgelegt:

````bash
man 5 passwd
````

Die Ausgabe ist hier zu finden: <http://man7.org/linux/man-pages/man5/passwd.5.html>

Wenn wir uns nicht sicher sind, in welcher Sektion etwas über einem Befehl steht, können wir folgendes eingeben:

````bash
user@joergreuter3:~$ whatis passwd
passwd (1ssl)        - compute password hashes
passwd (1)           - change user password
passwd (5)           - the password file
````

Alternativ funktioniert auch:

````bash
man -f passwd
````

Wenn wir alle Seiten angezeigt bekommen möchten in denen ein bestimmter String auftaucht:

````bash
user@joergreuter3:~$ apropos passwd
chgpasswd (8)        - update group passwords in batch mode
chpasswd (8)         - update passwords in batch mode
expect_mkpasswd (1)  - generate new password, optionally apply it to a user
fgetpwent_r (3)      - get passwd file entry reentrantly
getpwent_r (3)       - get passwd file entry reentrantly
gpasswd (1)          - administer /etc/group and /etc/gshadow
grub-mkpasswd-pbkdf2 (1) - generate hashed password for GRUB
htpasswd (1)         - Manage user files for basic authentication
pam_localuser (8)    - require users to be listed in /etc/passwd
passwd (1)           - change user password
passwd (1ssl)        - compute password hashes
passwd (5)           - the password file
passwd2des (3)       - RFS password encryption
SSL_CTX_set_default_passwd_cb (3ssl) - set passwd callback for encrypted PEM ...
SSL_CTX_set_default_passwd_cb_userdata (3ssl) - set passwd callback for encry...
update-passwd (8)    - safely update /etc/passwd, /etc/shadow and /etc/group
````

Alternativ:

````bash
man -k passwd
````

Innerhalb der Manpages kann man mit "/" und dem Schlüsselwort die Hilfe durchsuchen. Das drücken der "n"-Taste springt dann zum nächsten Treffer.

# Info

Manpages hanen keine Verweise, sondern beim aufruf einer neuen Seite muss die Seite geschlossen werden und manuell eine neue geöffnet werden. Bei Info ist dies anders. Informationen zur Steuerung: <https://en.wikipedia.org/wiki/Info_(Unix)>.

Die Links sind drch "*" gekennzeichnet.























# Bind installieren

Ein ergänzender Artikel ist hier zu finden <https://wiki.ubuntuusers.de/DNS-Server_Bind/>

```bash
apt update
>apt dist-upgrade
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

