---
layout: page
title: LF6 SQL
image: '/images/pages/3656302901_af1a9987e4_n.jpg'
---
Bild von <https://www.flickr.com/photos/mucio/3656302901>

# Allgemeines

Fragen zum Text oder zu SQL: <https://discord.gg/TsaqyAQ>

Videos: <https://www.youtube.com/watch?v=XghJ8ya9frY>

Skript zum Anlegen der Schüler und Datenhbanken: <https://gist.github.com/joergre/be3e0cd6afa47215e2057af0e53e3c7a>

Passwort für einen User ändern:

```sql
mysql –u schuelerX -p
SET PASSWORD = PASSWORD('auth_string');
```
# Installation der Arbeitsumgebung

Wir installieren zuerst den SQLServer, in unserem Fall den freien SQL-Server mariadb. Als Betriebssystem verwenden wir Ubuntu 17.04.

```sql
apt install mariadb-server
```

Für Umgebung die wichtig sind oder im Internet laufen, sollte der SQl-Server auf jeden Fall abgesichert werden. Die erste Massnahme dafür ist das Skript mysql_secure_installation. 

```sql
mysql_secure_installation
```

Das Skript ist eigentlich selbsterklärend, daher machen wir hier weiter.

# Anlegen und löschen einer Datenbank

Video: <https://www.youtube.com/watch?v=EipHtnSYt-o>

Um mit der Datenbank in Verbindung zu treten, brauchen wier noch einen Client. Dafür geben wir ein:

```sql
mariadb-client
```

Mit dem folgenden Befehl können wir uns auf der Datenbank anmelden:

```sql
mysql -u root -p
```

Nach -u kommt der Benutzername und -p sorgt dafür, dass nach einem Passwort gefragt wird. Habt Ihr kein Passwort vergeben, könnt Ihr Euch auch einfach mit folgendem Befehl anmelden:

```sql
mysql
```

Wir lassen uns nun die Datenbanken anzeigen:

#+name: show databases
#+begin_example
SHOW DATABASES;
#+end_example

Wir sollten 3 Datenbanken angezeigt bekommen: information_schema, mysql und performance_schema. Diese Datenbanken sind für den reibungslosen Betrieb der Datenbank unverzichtbar.

Zum experimentieren legen wir daher eine neue Datenbank an:

```sql
CREATE DATABASE geburtsurkunde;
``

## Aufgaben

1.) Überprüfe, ob die Datenbank geburtsurkund erfolgreich angelegt wurde.

2.) Lege eine Datenbak mit dem Namen test an.

Mit dem folgenden Befehl löschen wir die Datenbank test:

```sql
DROP DATABASE test;
```

Jetzt melden wir uns ab von der Datenbank:

```sql
QUIT
```

