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

Dort ist einmal der Hinweis, dass die Hilfe nicht komplett auf unserem System installiert ist und wir dies mit dem Befehl

````bash
update-help -force
````

tun können. Jetzt wird die Hilfe wesentlich ausführlicher. Des weiteren können wir den Link öffnen der unten angegeben ist und die Hilfe online lesen. Auf einem graphischen System könnten wir eingeben:

````bash
get-help get-process -online
````

Die Hilfe ist noch umfangreicher als die installierte Hilfe.


