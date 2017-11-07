---
layout: page
title: Docker
image: '/images/pages/docker.png'
---

Bilfquelle: Von dotCloud, Inc. - <https://github.com/docker/docker/blob/master/docs/static_files/docker-logo-compressed.png>, Apache License 2.0, <https://commons.wikimedia.org/w/index.php?curid=28456942>

# Installation Webserver für Entwicklung

## Installation

```bash
docker pull ubuntu
```

Wir überprüfen das herunterladen des Images:

```bash
root@proxmox:~# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
ubuntu                  latest              dd6f76d9cc90        2 days ago          122MB

```
Wir erstellen aus dem Image einen Container, auf dem wir angemeldet sind, um die benötigte Software manuell zu installieren:

```bash
docker run -i -t ubuntu /bin/bash
```

Wir spielen die aktuellen Updates ein:

```bash
apt update
apt -y dist-upgrade
```
Installation der benötigten Programme:

```bash
apt install -y sudo apache2 apache2-utils php php-common openssh-server vim-nox
```

Di beiden Server openssh und apache2 sollen beim start des Systems automatisch starten. Der einfachste Weg ist der Eintrag in die Datei .bashrc. Dazu müsen wir zuerst den Installationspfad von  service herausfinden:

```bash
root@63fb5dc1ac6b:/# which service                                             
/usr/sbin/service
```

Wir merken uns dn Pfad und wechseln in das Home-Verzeichnis und öffnen dort die Datei .bashrc:

```bash
cd ~
vim .bashrc 
```

Am Ende der Datei (hinter dem #fi) tragen wir ein:

```bash
# Dienste die gestartet werden sollen
/usr/sbin/service apache2 start
/usr/sbin/service ssh start
```
Wir kopieren uns die Container-ID aus der Konsole und verlassen dn Container mit exit.

Als nächstes legen wir ein neues Image an:

```bash
docker commit 63fb5dc1ac6b ubuntu:web
```

Wir schauen uns das neue Image an, dass die installierten Programme enthält:

```bash
root@proxmox:~# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
ubuntu                  web                 571c92391ecf        47 seconds ago      405MB
ubuntu                  latest              dd6f76d9cc90        2 days ago          122MB
```

Wir stellen nun sicher, dass die Dinste beim start eines Containers starten:

```bash
docker run -i -t ubuntu:web /bin/bash
* Starting Apache httpd web server apache2                                     
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.4. Set the 'ServerName' directive globally to suppress this message
 * 
 * Starting OpenBSD Secure Shell server sshd 
```
Mit einem exit verlassen wir den Container. Wir brauchen jetzt noch einen Inhalt für unsere Seite. Wir gehen dazu auf <http://www.oswd.org/> und suchen uns ein Design aus.

Wir laden das Design herunter und gehen im Browser auf die Downloads um die URL zum Download zu bekommen. Wir legen auf unserem Server ein Verzeichnis an und laden das Design in das Verzeichnis.

```bash
cd ~
mkdir web
wget http://static.oswd.org/designs/3699/Red.zip
```

In dem Verzeichnis web legen wir den statschen Anteil unserer Webseite ab. Also entpacken wir unser Template:

```bash
unzip Red.zip
```

Wir kopieren die entpackten Dateien eine Ebene höher und löschen das Verzeichnis:

```bash
mv Red/* .
rmdir Red
```

## Test des Images mit statischem Webinhalt

Wir startn jetzt einen neuen Container:

```bash
docker run --name=webtest -p 80:80 -v /root/web:/var/www/html -i -t ubuntu:web /bin/bash
```

Mit dem Befehl binden wir das lokale Verzeichnis /root/web  in den Container unter /var/www/html ein und öffnen den Port 80.
Der Server sollte jetzt unter http://IP-Adresse die entsprechende Seite ausliefern.

