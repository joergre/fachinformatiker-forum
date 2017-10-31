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

```sql
SHOW DATABASES;
```

Wir sollten 3 Datenbanken angezeigt bekommen: information_schema, mysql und performance_schema. Diese Datenbanken sind für den reibungslosen Betrieb der Datenbank unverzichtbar.

Zum experimentieren legen wir daher eine neue Datenbank an:

```sql
CREATE DATABASE geburtsurkunde;
```

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

# Tabelle anlegen 

YouTube-Video: <https://www.youtube.com/edit?video_id=8QFks70RsU4>

Anmelden an der Datenbank (wir haben kein PÜasswort für root gesetzt):

```sql
mysql
```

Mit dem Befehl

```sql
SHOW DATABASES;
```

sehen wir die verfügbaren Datenbanken. In unserem Fall sollten wir die drei System-Datenbanken information_schema, performance_schema und mysql sehen. Desweiteren unsere Datenbank geburtsurkunde.

Wir wählen die Datenbank geburtsurkunde aus um mit Ihr zu arbeiten:

```sql
USE geburtsurkunde;
```

Wir sehen uns die vorhandenen Tabellen an:

```sql
SHOW TABLES;
```

Leider ist noch keine Tabelle vorhanden. Dies ändern wir mit dem nächsten Befehl:

```sql
CREATE TABLE tblGeburtsurkunde (NameStandesAmt varchar(50), lfdNr varchar(50), VorNameKind varchar(50),  GeburtsnameKind varchar(50), Geschlecht varchar(1), Geburtsdatum varchar(50), Geburtsort varchar(50), VornameMutter varchar(50), NachnameMutter varchar(50), VornameVater varchar(50), NachnameVater varchar(50), ReligionKind varchar(50), ReligionMutter varchar(50), ReligionVater varchar(50), OrtStandesamt varchar(50), DatumAusstellung varchar(50), NameStandesbeamter varchar(50));
```

Der Befehl legt die Tabelle für Geburtsurkunden an (<http://www.der-postillon.com/2015/10/eigentlich-in-bad-tolz-geboren-dfb.html>). Der Befehl innerhalb der Klammer ist immer gleich aufgebaut: Spaltenname, Variablentyp und ev. ein Constraint (kommt später). Danach folgt ein Komma und die nächste Spalte. Die Informationen, die in einer Geburtsurkunde erfasst werden, stehen bei Wikipedia: <https://de.wikipedia.org/wiki/Geburtsurkunde>. Sicherlich ist der imme rgleiche Datentyp diskussionswürdig, aber hier geht es erst einmal darum, dass wir überhaupt Daten specihern können.

Die Struktur unserer Tabelle kann mit verschiedenen Befehlen dargestellt werden. Hier eine kleine Auswahl:

```sql
SHOW CREATE TABLE tblGeburtsurkunde;
SHOW COLUMNS FROM tblGeburtsurkunde;
SHOW FIELDS FROM tblGeburtsurkunde;
```

```sql
SHOW TABLES;
```

zeigt uns jetzt unsere neue Tabelle und mit

```sql
DROP TABLE tblgeburtsurkunde;
```

löschen wir die TAbelle wieder.

Wir legen die Tabelle wieder an, mit einem Primary Key:

```sql
CREATE TABLE tblGeburtsurkunde (NameStandesAmt varchar(50), lfdNr varchar(50) PRIMARY KEY VorNameKind varchar(50),  GeburtsnameKind varchar(50), Geschlecht varchar(1), Geburtsdatum varchar(50), Geburtsort varchar(50), VornameMutter varchar(50), NachnameMutter varchar(50), VornameVater varchar(50), NachnameVater varchar(50), ReligionKind varchar(50), ReligionMutter varchar(50), ReligionVater varchar(50), OrtStandesamt varchar(50), DatumAusstellung varchar(50), NameStandesbeamter varchar(50));
```

Der Primary Key referenziert eine  Eintrag inder Tabelle eindeutig. Der Primary Key muss  bei jedem Eintrag einen anderen Wert haben und darf nicht NULL sein (NULL darf nicht mit Null verwechselt werden. 0 ist ein Wert und NULL ist eben kein Wert.)

## Aufgabe

1.) Schaue Dir bitte die Struktur der Tabelle mit den obigen Befehlen an. Was fällt Dir auf?

2.) Schaue Dir bitte die folgende Seite an und lese Dir durch, welche Constraints es bei SQL gibt:  <http://wikis.gm.fh-koeln.de/wiki_db/Datenbanken/CONSTRAINT>


# INSERT

Mit dem Befehl INSERT können Daten in eine Tabelle eingetragen werden. Dazu sollte die Struktur der Tabelle bekannt sein. 

```sql
MariaDB [geburtsurkunde]> SHOW FIELDS FROM geburtsurkunde;
ERROR 1146 (42S02): Table 'geburtsurkunde.geburtsurkunde' doesn't exist
MariaDB [geburtsurkunde]> SHOW FIELDS FROM tblGeburtsurkunde;                                                               
+--------------------+-------------+------+-----+---------+-------+
| Field              | Type        | Null | Key | Default | Extra |
+--------------------+-------------+------+-----+---------+-------+
| NameStandesAmt     | varchar(50) | YES  |     | NULL    |       |
| lfdNr              | varchar(50) | YES  |     | NULL    |       |
| VorNameKind        | varchar(50) | YES  |     | NULL    |       |
| GeburtsnameKind    | varchar(50) | YES  |     | NULL    |       |
| Geschlecht         | varchar(1)  | YES  |     | NULL    |       |
| Geburtsdatum       | varchar(50) | YES  |     | NULL    |       |
| Geburtsort         | varchar(50) | YES  |     | NULL    |       |
| VornameMutter      | varchar(50) | YES  |     | NULL    |       |
| NachnameMutter     | varchar(50) | YES  |     | NULL    |       |
| VornameVater       | varchar(50) | YES  |     | NULL    |       |
| NachnameVater      | varchar(50) | YES  |     | NULL    |       |
| ReligionKind       | varchar(50) | YES  |     | NULL    |       |
| ReligionMutter     | varchar(50) | YES  |     | NULL    |       |
| ReligionVater      | varchar(50) | YES  |     | NULL    |       |
| OrtStandesamt      | varchar(50) | YES  |     | NULL    |       |
| DatumAusstellung   | varchar(50) | YES  |     | NULL    |       |
| NameStandesbeamter | varchar(50) | YES  |     | NULL    |       |
+--------------------+-------------+------+-----+---------+-------+
17 rows in set (0.00 sec)
```

Für den INSERT-Befehl gibt es jetzt zwei Möglichkeiten:

1.) Wir schreiben die Werte für die Felder genau in der Reihenfolge auf wie sie in der Tabelle vorhanden sind.
2.) Wir geben für jeden Wert das dazugehörige Feld an, dann brauchen wir auf die Reihenfolge keine Rücksicht nehmen,

** INSERT einfach

#+name: insert_simple
#+begin_example
INSERT INTO tblGeburtsurkunde VALUES('Bad Tölz','9/1988','Robert Siegfried','Lehmann','m','18.08.1988','Bad Tölz', 'Ilse', 'Lehmann', 'Christoph','Lehmann', 'katholisch','katholisch','katholisch','Bad Tölz','18.08.1988','Windmann');
#+end_example

und überprüfen den Eintag mit dem Befehl

#+name: insert_simple
#+begin_example
select * from tblGeburtsurkunde;
#+end_example

** INSERT

Wir geben Jetzt beim INSERT-Befehl die einzelnen Spalten an. Vorteil bei dieser Schreibweise ist, dass die Werte für die Tabelle in beleibiger Reihenfolge stehen können. Auserdem ist es einfacher, bestimmte Felder nicht auszufüllen.

#+name: insert_simple
#+begin_example
INSERT INTO tblGeburtsurkunde VALUES('Bad Tölz','9/1988','Robert Siegfried','Lehmann','m','18.08.1988','Bad Tölz', 'Ilse', 'Lehmann', 'Christoph','Lehmann', 'katholisch','katholisch','katholisch','Bad Tölz','18.08.1988','Windmann');
#+end_example

** Aufgabe

1.) Sache Dir den Inhalt der Tabelle an. Gibt es unterschiede? Warum?
2.) Setze das Feld lfdNr auf PRIMARY KEY. Versuche jetzt eine Zeile einzufügen mit gleicher lfdNr. Was passiert?

