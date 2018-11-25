---
layout: page
title: Git Einführung
---
# Vorbereitung

## Git installieren

````
sudo apt update
sudp apt install git
````

## Anpassung an die Schulinfrastruktur

In der Schule müssen wir die Überprüfung des SSL-Zertifikats ausschalten (zu Hause **nicht** nötig):

```
git config http.sslVerify false
```

# Einen Account bei github erstellen

Bittte erstelle, falls nicht bereits vorhanden, einen Account bei GitHub.

# Ein Repository anlegen mit Name Stunde3

Führe bitte folgende Befehle aus:

```
git clone
cd Stunde3
ls
```
Bei dem befehl "ls" sollten keine Dateien angezeigt werden.

Wir legen eine leere Datei an:

```
touch hello.html
```

Und fügen in die Datei "hello.html" mit unserem Lieblingseditor folgenden Code ein:

```
<!Doctype html>
<html>
    <head>
        <title>FBS </title>
    </head>
    <body>
        Hallo FBS!
    </body>
</html>
```

Danach führen wir folgende Befehle aus:

```
git add hello.html
git commit -m "Nachricht"
git status
git push
```

Der Befehl "git pull" lädt alle Dateien herunter inklusive der gesamten History der Daetein. Es kann also jeder vergangene Zustand ohne Internet wieder hergestellt werden.


Wir verändern jetzt den Code der Datei "hello.html" und führen folgende Befehle aus:

```
git pull
```

Wir sehen, dass die Datei auch lokal verändert wurde. Wir verändern die Datei jetzt **lokal und online** auf der Webseite von github und führen den Befehl "git pull" aus:

```
git commit -am "Lokale Änderung!"
git pull
```

Es kommt die Fehlermeldung "merge conflict". Dieser Fehler tritt auf, wenn die Datei im zentralen Repository verändert wurde und lokal. Wir öffnen die Datei lokal und schauen uns den Inhalt an.

In der Datei steht "<<<<< HEAD" und dann "=====". Die Zeichen dazwischen wurden an dem lokalen Computer verändert. Die Zeichen zwischen "===== und ">>>>>" und einer Zeichenfolge wurden remote verändert. Die Zeichen stellen den Commit dar, der das Problem auslöste. Die Zeilen, die nicht erwünscht sind, müssen händisch entfernt werden bis zu dem Zustand einer Datei, die aussieht wie sie sein sollte.

```
git log
```

Mit git log können wir die Veränderungen nachvollziehen. Wir möchten jetzt einen alten Zustand des Repositories wieder herstellen:

```
git reset --hard <commit>
```

# Branch

Alle verfügbaren Branches anzeigen:

```
git branch
```

Wir legen einen Branch mit dem Namen "erweiterung an:

```
git branch erweiterung
```

Und überprüfen den Erfolg des Befehls:

```
git branch
```

Wir stellen unser lokaes Repositories aus den Branche um:

```
git checkout erweiterung
git branch
```
Wir verändern unsere Datei hello.html und checken sie ein:

```
git commit -am "erweiterung"
```

Wechseln zum Branch "master" und überprüfen die Datei:

```
git checkout master
git cat index.html 
```

=> Alte Datei
Unterschied wird deutlich mit
git log in beiden Branches
Beide Branches zusammenführen:
In den MasterBranche wechseln
git merge erweiterung
git branch feature
git checkout feature
index.html anlegen
git push
Fehlermeldung, weil Branch nicht existiert
git push --set-upstream origin feauture
github-Seite: Compare&pull request