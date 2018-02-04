---
layout: page
title: Powershell
images: '/images/posts/Tux.png'
---

# Powershell installieren

Zum installieren von Powershell auf Deinem System besuche <https://github.com/PowerShell/PowerShell> und folge der Anweisung.

# Warum Powershell

Powershell ist mit der Version 6 OpenSource Software geworden und unterstützt zahlreiche Plattformen wie Windows, iOS und alle gängige Linu-Plattformen (redhat, centos, Debian, Ubuntu etc.) und auch ARM-Prozessoren und Cloud-Anbieter wie Amazon, google und Azure von Microsoft. Der Vorteil liegt jetzt darin, dass man sich nicht mehr in einer hetrogenen Systemlanschaft auf jedes System einstellen muss, sondern alle Standardaufgaben in der gleichen Umgebung ausführen kann. Ebenfalls funktionieren geschriebene Skripte von einem Windowssystem auch auf allen anderen Systemen (wenn die Abhängigkeiten erfüllt sind).

# Starten der Powershell

Mit dem Befehl
````bash
pwsh
````

wird die Powershell gestartet. Mit dem Befehl

````bash
exit
````

verlässt man die Powershell wieder. 

Eine Übersicht der verfügbaren Befehle auf dem vorhandenen System bekommt man mit

````bash
get-command
````

Beim ausführen unter Ubuntu und Windows sieht man recht klar, welche Plattform am besten unterstützt wird.

# Beispiel

Um den Inhalt eines Directory anzuzeigen, verwendet Powershell den Befehl


````bash
get-childitem
````

Powershell versucht nun die verschiedenen Befehle der unterschiedlichen Plattformen zu vereinheitlichen. Bei Windows gibt es den Befehl

````bash
dir
````

und bei Linux hingegen

````bash
ls
````

Beide Befehle funktionieren auch auf allen Plattformen unter der Powershell. Aber der eigentliche Befehl heißt

````bash
get-childitem
````

# Hilfe

Die Powershell hat ein eingebautes Hilfesystem. Wenn wir uns beispielsweise alle Befehle anschauen möchten, die etwas mit den Prozessen auf unserem System zu tun haben:

````bash
> Get-Help *process*
                                                                                Name                              Category  Module                    Synopsis  ----                              --------  ------                    --------  Debug-Process                     Cmdlet    Microsoft.PowerShell.M... ...       Get-Process                       Cmdlet    Microsoft.PowerShell.M... ...       Start-Process                     Cmdlet    Microsoft.PowerShell.M... ...       Stop-Process                      Cmdlet    Microsoft.PowerShell.M... ...       Wait-Process                      Cmdlet    Microsoft.PowerShell.M... ...       Get-PSMetaConfigurationProcessed  Function  PSDesiredStateConfigur... ...       Set-PSMetaConfigDocInsProcesse... Function  PSDesiredStateConfigur... ... 
````

Wir suchen uns den Befehl Get-Process aus und möchten zu diesem Befehl mehr Informationen:

````bash
> get-help get-process                                                                                                                                  NAME                                                                                Get-Process                                                                 
SYNTAX
    Get-Process [[-Name] <string[]>] [-Module] [-FileVersionInfo]
    [<CommonParameters>]

    Get-Process [[-Name] <string[]>] -IncludeUserName  [<CommonParameters>]
````

Am Ende steht eventuell folgender Hinweis:

````bash
REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It
    is displaying only partial help.
        -- To download and install Help files for the module that includes
    this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help
    Get-Process -Online" or
           go to https://go.microsoft.com/fwlink/?LinkID=113324.

````


gDort ist einmal der Hinweis, dass die Hilfe nicht komplett auf unserem System installiert ist und wir dies mit dem Befehl

````bash
update-help -force
````

tun können. Jetzt wird die Hilfe wesentlich ausführlicher. Des weiteren können wir den Link öffnen der unten angegeben ist und die Hilfe online lesen. Auf einem graphischen System könnten wir eingeben:

````bash
get-help get-process -online
````

Die Hilfe ist noch umfangreicher als die installierte Hilfe.

# Die 4 Arten von Befehlen

Es gibt 4 Arten von Befehlen bei der Powershell:

1.) Administrative Befehle des Betriebssystems (ifconfig, ipconfig)

2.) Aliases wie dir und ls

3.) Skripte

4.) Cmdlets

## Alias

Alias sind Verweise oder Verlinkungen auf andere Befehle. Beispielsweise verweise ls und dir auf den Befehl Get-ChildItem. Eine Liste aller Aliases kann angezeigt werden mit:

````bash
Get-Alias
````
Wir bekommen eine Liste aller Alliases und das Ziel des verweises angezeigt. Bitte beachte, dass bei Scripten auf Aliases verzichtet werden sollte, weil die Fehlersuche durch diese sehr erschwert wird.


## Cmdlets

Aussprache: Command Lets

Cmdlets sind eine neue Kategorie von Befehlen die es so bisher weder unter Windows noch Unix gab.  Sie sind daran zu erkennen, dass erst ein Verb kommt und dann ein Nomen. Wir schauen uns ein CMDlet an:

````bash
get-command
````

Wir bekommen eine Liste aller verfügbaren CMDlets in der Powershell angezeigt.

Cmdlets können von anderen Quellen installiert oder selbst programmiert werden. Sogenannte Snapins können sehr viele Cmdlets beinhalten (teilweise tausende). Verfügbar sind Snapins z.B. für VirtualBox, AWS, Azure etc.

Wir haben oben das Cmdlet Get-Process kennengelernt. Cmdlets können Parameter mit "-" angehängt werden, erfordern aber niemal zwingend einen Parameter. Wir möchten z.B. nur die Prozessinformationen über die offenen Shells wissen:

````bash
Get-Process -name bash
````

Probiere den Befehl auch mit "Bash" aus. Ist die Powershell Casesensitive? Beachtet die Linux-Konsole Groß- und Kleinschreibung?

# Verketten von Befehlen

wir haben gesehen, dass wir mit

````bash
Get-Process -name bash
````

die aktiven Konsolen angezeigt bekommen. Jetzt möchten wie diese beenden:

````bash
Get-Process -name bash | Stop-Process
````

Unsere Sitzung wird beendet, weil wir den Prozess beendet haben. Das Zeichen "|" nennt man "pipe" und bedeutet Röhre. Die Ausgabe vom ersten Befehl wird wie in einer Röhre nicht auf dem Bildschirm angezeigt sondern an dem nachfolgeneden Befehl weitergeleitet. Der Unterschied zur Linuxkonsole ist, dass dies ein Objekt ist nicht die einfache Textausgabe. Dies erhöht die Flexibilität ernorm:

````bash
Get-Process  | Select-Object -property CPU
````

Property bedeutet Eigenschaft und zeigt in diesem Fall vom Objekt Get-Process die Eigenschaft der CPU an. Um das ganze lesbarer zu gestalten lassen wir uns den Namen ausgeben:

````bash
Get-Process  | Select-Object -property name, CPU
````

Wir können auch mehere Pipes hintereinander fügen. Es soll jetzt beispielsweise die Prozesse nach der CPU-Belastung sortiert werden und nur die 10 Prozesse angezeigt werden, die die CPU am meisten belasten:

````bash
 Get-Process | Select-Object -property name, CPU|Sort-Object -descending CPU|Select-Object -first 10
````



# Variable






