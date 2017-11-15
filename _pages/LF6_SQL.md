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
# Anlegen und löschen einer Datenbank

Das Skript ist eigentlich selbsterklärend, daher machen wir hier weiter.

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
CREATE TABLE tblGeburtsurkunde (NameStandesAmt varchar(50), lfdNr varchar(50) PRIMARY KEY, VorNameKind varchar(50),  GeburtsnameKind varchar(50), Geschlecht varchar(1), Geburtsdatum varchar(50), Geburtsort varchar(50), VornameMutter varchar(50), NachnameMutter varchar(50), VornameVater varchar(50), NachnameVater varchar(50), ReligionKind varchar(50), ReligionMutter varchar(50), ReligionVater varchar(50), OrtStandesamt varchar(50), DatumAusstellung varchar(50), NameStandesbeamter varchar(50));
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

## INSERT einfach

```sql
INSERT INTO tblGeburtsurkunde VALUES('Bad Tölz','9/1988','Robert Siegfried','Lehmann','m','18.08.1988','Bad Tölz', 'Ilse', 'Lehmann', 'Christoph','Lehmann', 'katholisch','katholisch','katholisch','Bad Tölz','18.08.1988','Windmann');
```

und überprüfen den Eintag mit dem Befehl

```sql
select * from tblGeburtsurkunde;
```

## INSERT

Wir geben Jetzt beim INSERT-Befehl die einzelnen Spalten an. Vorteil bei dieser Schreibweise ist, dass die Werte für die Tabelle in beleibiger Reihenfolge stehen können. Auserdem ist es einfacher, bestimmte Felder nicht auszufüllen.

```sql
INSERT INTO tblGeburtsurkunde VALUES('Bad Tölz','9/1988','Robert Siegfried','Lehmann','m','18.08.1988','Bad Tölz', 'Ilse', 'Lehmann', 'Christoph','Lehmann', 'katholisch','katholisch','katholisch','Bad Tölz','18.08.1988','Windmann');
```

## Aufgabe

1.) Sache Dir den Inhalt der Tabelle an. Gibt es unterschiede? Warum?

2.) Setze das Feld lfdNr auf PRIMARY KEY. Versuche jetzt eine Zeile einzufügen mit gleicher lfdNr. Was passiert?

# SELECT

Vevor wir einen sinnvollen SELECT-Befehl schreiben können, müssen wir immer die Datenbankstruktur kennen:

```sql
SHOW FIELDS FROM tblGeburtsurkunde;
```

## Aufgabe
Wir möchten unsere CD-Sammlung digitalisieren. Dieses Projekt führen wir Schritt für Schritt durch.
1.) Lege eine Tabelle CD_Lied mit folgenden Spalten an: CD_ID, Album, Erscheinungsjahr, Titelliste. Wähle sinnvolle Einstellungen für die jeweiligen Spalten.

2.) Trage in die Tabelle bitte folgende Daten ein:

4711 	Anastacia – Not That Kind 	2000 	{1. Not That Kind, 2. I’m Outta Love, 3. Cowboys & Kisses}

4712 	Pink Floyd – Wish You Were Here 	1975 	{1. Shine On You Crazy Diamond}

4713 	Anastacia – Freak of Nature 	2001 	{1. Paid my Dues}


Wir können uns alle Einträge in einer Tabelle anschauen:

```sql
SELECT * FROM CD_Lied;
```

Oder nur bestimmte Spalten:

```sql
SELECT  Album, Titelliste FROM CD_Lied;
```

Wir können auch bestimmte Einträge selektieren:

```sql
SELECT Album, Titelliste FROM CD_Lied WHERE Album='Anastacia – Not That Kind';
```

Wenn wir nicht den genauen Eintrag kennen, dann können wir mit LIKE nach Teilen eines Eintags suchen:

```sql
SELECT Album, Titelliste FROM CD_Lied WHERE Album LIKE 'Anastacia';
```

Wir können auch suchen nach Einträgen die nicht gleich sind:

```sql
SELECT Album, Titelliste FROM CD_Lied WHERE Album!='Anastacia – Not That Kind';
```

Ungleich kann man auch schreiben als 

```sql
SELECT Album, Titelliste FROM CD_Lied WHERE Album<>'Anastacia – Not That Kind';
```

# Alter TABLE

Die Tabelle oben hat einige Probleme:

- Das Feld Album enthält die Attributwertebereiche Interpret und Albumtitel.
- Das Feld Titelliste enthält eine Menge von Titeln.

Dadurch hat man ohne Aufspaltung folgende Probleme bei Abfragen:

- Zur Sortierung nach Albumtitel muss das Feld Album in Interpret und Albumtitel aufgeteilt werden.
- Die Titel können (mit einfachen Mitteln) nur alle gleichzeitig als Titelliste oder gar nicht dargestellt werden.

Wir fügen als erstes eine Spalte 'Interpret' der Tabelle hinzu:

```sql
ALTER TABLE CD_Lied ADD Interpret varchar(50);
```

## Aufgabe

Füge bitte ncoh die Spalte 'Track' der Tabelle hinzu varchar(10).

Wir möchten die Spalte Track jetzt auf den Variablentyp int ändern:

```sql
ALTER TABLE CD_Lied MODIFY Track int;
```

oder

```sql
ALTER TABLE CD_Lied ALTER COLUMN Track int;
```

Die Befehle hängen von der eingesetzten MySQL-Version ab. 

## Aufgabe

Ändere den Spaltentyp bei Erschinungsjahr von var auf year um. Was fällt Dir auf? Warum reagiert die Datenbank so?

# DELETE FROM

Um alle Daten aus einer Tabelle zu löschen könne wir folgenden Befehl ausführen (bitte nicht wirklich ausführen):

```sql
DELETE * FROM CD_Lied
```

Der Befehl löscht nur die Daten, nicht aber die Tabelle an sich. Dieser Befehl wird sehr selt genutzt, meist wird ein Drop table verwendet.

Meistens wird der Delte-Befehl eingeschränkt mit Hilfe von Bedingngen:

```sql
DELETE FROM CD_Lied WHERE Erscheinungsjahr = '2000';
```

Der Befehl löscht 'Anastacia – Not That Kind'.

Wir können mit AND und OR auch komplexere Befehle aufbauen:

```sql
DELETE FROM CD_Lied WHERE Erscheinungsjahr = '2001' AND Album = 'Anastacia – Freak of Nature';
```

# CREATE INDEX

Ein Index ist eine Möglichkeit, den Zugriff einer Datenbank auf einzelne Felder zu beschleunigen. Allerdings werden einige Befehle dadurch verlangsamt wie z.B. update. Das liegf daran, dass das Index mit aktualisiert werden muss.

Anlegen eines Index aus einer Spalte mit Zahlen:

```sql
CREATE INDEX CDIndex ON CD (CD_ID)
```

Erstellen eines zusammengestzten Index:

```sql
CREATE INDEX Index_Namen ON CD (CD_ID, Track)
```

# INDEX löschen

Bitte beachte, dass die Wiederherstellung eines Index bei großen Datenbanken sehr lange dauern kann!

Ein Index kann auch gelöscht werden. Der Befehl ist allerdings etwas länger, ein DROP INDEX reicht nicht. Wir legen eines Test-Tabelle mit dem Index idx an um die funktionsweise besser zu verstehen:

```sql
CREATE TABLE tbltest (col1 varchar(2), col2 varchar(2));
CREATE INDEX idx ON tbltest (col1, col2);
```

Der Index idx soll nun wieder entfernt werden:

```sql
ALTER TABLE tbltest DROP INDEX idx;

```

Bei einem nochmaligen Aufruf des Befehls bekommen wir eine Fehlermeldung (der Index wurde ja bereits gelöscht):

```sql
MariaDB [test]> ALTER TABLE tbltest DROP INDEX idx;
ERROR 1091 (42000): Can't DROP 'idx'; check that column/key exists
```

Alternativ funktioniert auch folgender Befehl:

```sql
CREATE INDEX idx ON tbltest (col1, col2);
DROP INDEX idx ON tbltest;
```
# TRUNCATE TABLE

Truncate wird eingesetzt, wenn der Inhalt einer Tabelle gelöscht werden soll, aber nicht Die Tabelle und Ihre Struktur wie Spaltennamen etc. an sich.

```sql
SELECT * FROM tblGeburtsurkunde; SELECT * FROM tblGeburtsurkunde;
```
Hier die Ausgabe:

```sql
MariaDB [schueler22]> SELECT * FROM tblGeburtsurkunde;
+----------------+--------+------------------+-----------------+------------+--------------+--------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
| NameStandesAmt | lfdNr  | VorNameKind      | GeburtsnameKind | Geschlecht | Geburtsdatum | Geburtsort   | VornameMutter | NachnameMutter | VornameVater | NachnameVater | ReligionKind | ReligionMutter | ReligionVater | OrtStandesamt | DatumAusstellung | NameStandesbeamter |
+----------------+--------+------------------+-----------------+------------+--------------+--------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
| Bad Tllllllz   | 9/1988 | Robert Siegfried | Lehmann         | m          | 18.08.1988   | Bad Tllllllz | Ilse          | Lehmann        | Christoph    | Lehmann       | katholisch   | katholisch     | katholisch    | Bad Tllllllz  | 18.08.1988       | Windmann           |
+----------------+--------+------------------+-----------------+------------+--------------+--------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
1 row in set (0.00 sec)
```

Wir mlöschen jetzt den Inhalt mit TRUNCATE:

```sql
MariaDB [schueler22]> TRUNCATE TABLE tblGeburtsurkunde;
Query OK, 0 rows affected (0.03 sec)
```

Und führen den obigen SELECT-Befehl nochmals aus:

```sql
MariaDB [schueler22]> SELECT * FROM tblGeburtsurkunde;
Empty set (0.00 sec)
```

# Auto Increment

Wir schauen uns jetzt noch einmal den Primary key an und versuchen ihn ein wenig komfortabler zu gestalten. Dazu legenw ir eine neu Tabelle an:

```sql
CREATE TABLE tblGeburtsurkunde2 (NameStandesAmt varchar(50), lfdNr int AUTO_INCREMENT PRIMARY KEY, VorNameKind varchar(50),  GeburtsnameKind varchar(50), Geschlecht varchar(1), Geburtsdatum varchar(50), Geburtsort varchar(50), VornameMutter varchar(50), NachnameMutter varchar(50), VornameVater varchar(50), NachnameVater varchar(50), ReligionKind varchar(50), ReligionMutter varchar(50), ReligionVater varchar(50), OrtStandesamt varchar(50), DatumAusstellung varchar(50), NameStandesbeamter varchar(50));
```

Wir tragen jetzt einen Datensatz ein:

```sql
INSERT INTO tblGeburtsurkunde2 (VorNameKind, GeburtsnameKind) VALUES ('Kasper','Mueller');
```

Wir schauen uns den Eintrag an:

```sql
MariaDB [schueler22]> SELECT * FROM  tblGeburtsurkunde2;
+----------------+-------+-------------+-----------------+------------+---------                                                                             -----+------------+---------------+----------------+--------------+-------------                                                                             --+--------------+----------------+---------------+---------------+-------------                                                                             -----+--------------------+
| NameStandesAmt | lfdNr | VorNameKind | GeburtsnameKind | Geschlecht | Geburtsd                                                                             atum | Geburtsort | VornameMutter | NachnameMutter | VornameVater | NachnameVate                                                                             r | ReligionKind | ReligionMutter | ReligionVater | OrtStandesamt | DatumAusstel                                                                             lung | NameStandesbeamter |
+----------------+-------+-------------+-----------------+------------+---------                                                                             -----+------------+---------------+----------------+--------------+-------------                                                                             --+--------------+----------------+---------------+---------------+-------------                                                                             -----+--------------------+
| NULL           |     1 | Kasper      | Mueller         | NULL       | NULL                                                                                      | NULL       | NULL          | NULL           | NULL         | NULL                                                                                       | NULL         | NULL           | NULL          | NULL          | NULL                                                                                          | NULL               |
+----------------+-------+-------------+-----------------+------------+---------                                                                             -----+------------+---------------+----------------+--------------+-------------                                                                             --+--------------+----------------+---------------+---------------+-------------                                                                             -----+--------------------+
1 row in set (0.00 sec)
```

und sehen, dass das Feld 'lfdNr' automatisch mit dem Wert 1 versehen worden ist.

Wir schauen uns die Struktur der Tabelle an:

```sql
MariaDB [schueler22]> SHOW FIELDS FROM tblGeburtsurkunde2;
+--------------------+-------------+------+-----+---------+----------------+
| Field              | Type        | Null | Key | Default | Extra          |
+--------------------+-------------+------+-----+---------+----------------+
| NameStandesAmt     | varchar(50) | YES  |     | NULL    |                |
| lfdNr              | int(11)     | NO   | PRI | NULL    | auto_increment |
| VorNameKind        | varchar(50) | YES  |     | NULL    |                |
| GeburtsnameKind    | varchar(50) | YES  |     | NULL    |                |
| Geschlecht         | varchar(1)  | YES  |     | NULL    |                |
| Geburtsdatum       | varchar(50) | YES  |     | NULL    |                |
| Geburtsort         | varchar(50) | YES  |     | NULL    |                |
| VornameMutter      | varchar(50) | YES  |     | NULL    |                |
| NachnameMutter     | varchar(50) | YES  |     | NULL    |                |
| VornameVater       | varchar(50) | YES  |     | NULL    |                |
| NachnameVater      | varchar(50) | YES  |     | NULL    |                |
| ReligionKind       | varchar(50) | YES  |     | NULL    |                |
| ReligionMutter     | varchar(50) | YES  |     | NULL    |                |
| ReligionVater      | varchar(50) | YES  |     | NULL    |                |
| OrtStandesamt      | varchar(50) | YES  |     | NULL    |                |
| DatumAusstellung   | varchar(50) | YES  |     | NULL    |                |
| NameStandesbeamter | varchar(50) | YES  |     | NULL    |                |
+--------------------+-------------+------+-----+---------+----------------+
17 rows in set (0.04 sec)
```

Und sehen bei 'lfdNr' den PRIMARY_KEY und das auto_increment. Der Eintag auto_increment dient dazu, dass dieses Feld automatisch hochgezählt wird.

Wir tragen noch einen Datensatz ein:

```sql
INSERT INTO tblGeburtsurkunde2 (VorNameKind, GeburtsnameKind) VALUES ('Martin','Neuer');
```

und schauen uns das Ergebnis an:

```sql
MariaDB [schueler22]> SELECT * FROM tblGeburtsurkunde2;
+----------------+-------+-------------+-----------------+------------+--------------+------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
| NameStandesAmt | lfdNr | VorNameKind | GeburtsnameKind | Geschlecht | Geburtsdatum | Geburtsort | VornameMutter | NachnameMutter | VornameVater | NachnameVater | ReligionKind | ReligionMutter | ReligionVater | OrtStandesamt | DatumAusstellung | NameStandesbeamter |
+----------------+-------+-------------+-----------------+------------+--------------+------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
| NULL           |     1 | Kasper      | Mueller         | NULL       | NULL         | NULL       | NULL          | NULL           | NULL         | NULL          | NULL         | NULL           | NULL          | NULL          | NULL             | NULL               |
| NULL           |     2 | Martin      | Neuer           | NULL       | NULL         | NULL       | NULL          | NULL           | NULL         | NULL          | NULL         | NULL           | NULL          | NULL          | NULL             | NULL               |
+----------------+-------+-------------+-----------------+------------+--------------+------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
2 rows in set (0.00 sec)

```

Und sehen, wie erwartet, beim zweiten Eintag die 2 bei lfdNr.

Wir setzen den Startwert des auto_inkrement auf 10000:

```sql
ALTER TABLE tblGeburtsurkunde2 AUTO_INCREMENT=10000;
```

und tragen einen weiteren Datensatz ein:

```sql
INSERT INTO tblGeburtsurkunde2 (VorNameKind, GeburtsnameKind) VALUES ('Norbert','Obama');
```

Und sehen, das der Ein tag in der Spalte 'lfdNR' bei 1000 weiterzählt:

```sql
MariaDB [schueler22]> SELECT * FROM tblGeburtsurkunde2;
+----------------+-------+-------------+-----------------+------------+--------------+------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
| NameStandesAmt | lfdNr | VorNameKind | GeburtsnameKind | Geschlecht | Geburtsdatum | Geburtsort | VornameMutter | NachnameMutter | VornameVater | NachnameVater | ReligionKind | ReligionMutter | ReligionVater | OrtStandesamt | DatumAusstellung | NameStandesbeamter |
+----------------+-------+-------------+-----------------+------------+--------------+------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
| NULL           |     1 | Kasper      | Mueller         | NULL       | NULL         | NULL       | NULL          | NULL           | NULL         | NULL          | NULL         | NULL           | NULL          | NULL          | NULL             | NULL               |
| NULL           |     2 | Martin      | Neuer           | NULL       | NULL         | NULL       | NULL          | NULL           | NULL         | NULL          | NULL         | NULL           | NULL          | NULL          | NULL             | NULL               |
| NULL           | 10000 | Norbert     | Obama           | NULL       | NULL         | NULL       | NULL          | NULL           | NULL         | NULL          | NULL         | NULL           | NULL          | NULL          | NULL             | NULL               |
+----------------+-------+-------------+-----------------+------------+--------------+------------+---------------+----------------+--------------+---------------+--------------+----------------+---------------+---------------+------------------+--------------------+
3 rows in set (0.00 sec)
```

# Zählen

Oft müssen Sachen gezählt werden. Wir sehen bei 'Select * From' unten die Anzahl der Zeilen, aber wir möchtend iese jetzt exlizit mit der Count-Funktion ghezählt bekommen:

```sql
MariaDB [schueler22]> SELECT COUNT(*) FROM tblGeburtsurkunde2;
+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
1 row in set (0.05 sec)
```

Wir fügen inn unserer Tabelle einen weiteren Datensatz hinzu:

```sql
INSERT INTO tblGeburtsurkunde2 (VorNameKind, GeburtsnameKind) VALUES ('Kasper','Neuer');
```

Und zählen jetzt die vornamen ohne Dopplung (DISTINCT):

```sql
MariaDB [schueler22]> SELECT COUNT(DISTINCT VorNameKind) FROM tblGeburtsurkunde2;
+-----------------------------+
| COUNT(DISTINCT VorNameKind) |
+-----------------------------+
|                           3 |
+-----------------------------+
1 row in set (0.00 sec)
```

Wir schauen uns die Tabelle an:

```sql
MariaDB [schueler22]> SELECT VorNameKind FROM tblGeburtsurkunde2;
+-------------+
| VorNameKind |
+-------------+
| Kasper      |
| Martin      |
| Norbert     |
| Kasper      |
+-------------+
4 rows in set (0.00 sec)
```

Wir sehen 4 Einträge, aber Kasper ist doppelt. Durch den DISTINCT-Befehl werden die doppelten Einträge herausgefildert. Wir können die Anzahl der Einträge auch mit COUNT zählen:

```sql
MariaDB [schueler22]> SELECT COUNT(VorNameKind) FROM tblGeburtsurkunde2;
+--------------------+
| COUNT(VorNameKind) |
+--------------------+
|                  4 |
+--------------------+
1 row in set (0.00 sec)
```

Wir möchten die Anazhl der Einträge zählen bei denen der Vorname des Kindes 'Kasper' lautet:

```sql
MariaDB [schueler22]> SELECT COUNT(VorNameKind) FROM tblGeburtsurkunde2 WHERE VorNameKind = 'Kasper';
+--------------------+
| COUNT(VorNameKind) |
+--------------------+
|                  2 |
+--------------------+
1 row in set (0.00 sec)
```

# Durchschnitt und Summe


Wir legen eine neue Tabelle für diesen Abschnitt an:

```sql
CREATE TABLE tblKlasse (lfdNr int AUTO_INCREMENT PRIMARY KEY, Klassen_Bezeichnung varchar(6), Anzahl_Schueler int);
```

und fügen ein paar Einträge hinzu:

```sql
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('11B380', 22);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('11B391', 24);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('11B392', 15);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('10B391', 24);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('10B380', 29);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('10B392', 18);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('12B380', 24);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('12B391', 22);
INSERT INTO tblKlasse (Klassen_Bezeichnung, Anzahl_Schueler) VALUES ('12B392', 22);
```
