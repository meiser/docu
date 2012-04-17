# __Netzwerkansteuerung für die Erodiermaschine CHARMILLES ROBOFIL 510__

## Idee

Die Erodiermaschine `CHARMILLES ROBOFIL 510` verfuegt lediglich ueber eine serielle Schnittstelle (RS-232/RS-422), die einen Austausch zwischen Maschine und PC gewaehrleistet. Mit Hilfe eines RS232 over Ethernet-Servers soll das Geraet zusaetzlich im Netzwerk zur Verfuegung gestellt werden.
Die folgende Anleitung beinhaltet alle wichtigen Informationen zur Einrichtung des RS232 over Ethernet-Servers und der Softwarekonfiguration auf den Clientrechnern.

## Verwendete Hardware
* [Roline Serial over Ethernet Device Server](https://www.secomp.de "Secomp Computer Products") 1x Port RS-232 [Artikelnummer 15.06.0015](https://www.secomp.de/roline_rs232_uber_ethernet_adapter/15060015.html "Produktinformation")
* Netzwerkkabel Cat 5 Kabel
* Datenkabel RS-232 9 pol. female auf RS-232 25 pol. male nicht gedreht

## Verwendete Programme, Bibliotheken usw.
* [Cimco Edit V6 Professional](http://http://www.cimco.com/ "Informationen zu Cimco Edit V6 Professional") - Datenuebertragung zum PC (loest altes Kommandozeilentool ab)
* `SerRedir.exe` - Original Software von CD, erstellt virtuellen COM Port und verknuepft diesen mit dem RS232 over Ethernet-Server, bis Windows XP SP3
* [com0com](http://com0com.sourceforge.net/ "Erstellt virtuelle COM-Port-Paare") - Optional, erzeugt virtuelle COM Port Paare, Windows 7 Unterstuetzung, Opensource
* [com2tcp](http://sourceforge.net/projects/com0com/files/ "COM Port zu TCP Redirector") - Alternative zu COM Port Redirector, benoetigt com0com )

## Softwaredownloads

Die fuer den Roline Serial over Ethernet Device Server benoetigten Programme und Treiber befinden sich auf der mitgelieferten Treiber CD im Ordner `SLAN`.
Zu naechst muessen folgende Programme in der exakten Reihenfolge installiert werden:

* `PortSetup.exe` - installiert die Treiber fuer den virtuellen COM Port und das Konfigurationstool `SLANUtility.exe` (Konfiguration des Servers)
* Installation des `SerialRedirector` unter `SLAN\WIN2KXP\SerialRedirector`
* Anschließend Neustart des Computers
* AUF AUSREICHENDE ADMINRECHTE ACHTEN!!!!!!!

Der Inhalt der Treiber CD ist auch im [Downloadbereich](http://dl.rotronic.ch/?user=anonymous "SECOMP DOWNLOAD BEREICH") des Roline-Partners `SECOMP` verfuegbar. Es wird kein Passwort benoetigt.
Die Ordnerstruktur entspricht der Artikelnummer des Geraetes (hier 15060015)


## Konfiguration Server

Der Roline Serial over Ethernet Device Server kann mit Hilfe des Konfigurationstool `SLANUtility.exe` konfortabel eingestellt werden.
Die Bedienung der Software ist intuitiv. Folgend werden nur einige wichtige Konfigurationsschritte festgehalten:

* Im Reiter Server Manager werden alle im Netzwerk befindlichen Roline Geraete angezeigt. Wird das Geraet nicht gefunden kann es hilfreich sein, die IP-Adresse bei der Suche fest einzugeben.
* Im Reiter Server Manager wird die Konfiguration des Servers (vor allem Netzwerkkonfiguration und Verbindungsparameter am physischen COM-Port des Servers) eingestellt.
* Im Reiter Port Mapping wird die Verknuepfung zwischen virtullem COM Port und Server eingestellt (gilt nur bei Verwendung des originalen Treibers `SerRedir.exe`)

## COM Porteinstellungen am Anschluss des Servers:

* Baudrate `9600`
* Datenbits `7`
* Stopbits `2`
* Paritaet `gerade`
* Flusskontrolle `Xon/Xoff`
* Interface `RS232`

## Konfiguration Charmilles Robofil 510

Im Geraet muessen folgende Einstellungen vorgenommen werden:

### Spalte DNC

* Channel (Kanal) `1`
* Baud `9600`
* Protocol `6`

### Spalte PP

* Baudrate 9600
* Protokoll `6`


## Nutzung vom com0com und com2tcp

Das OpenSource-Projekt `com0com` ermoeglicht die Virtualisierung von COM-Ports und die Erstellung von virtuellen COM-Port-Paaren, die gegenseitig untereinander kommunizieren.
Alle Daten des einen COM-Port-Partners werden automatisch an den anderen COM-Port weitergeleitet und umgekehrt.

Zur Benutzung von `com0com` und `com2tcp` muessen folgende Schritte vollzogen werden:

1. Installation von `com0com` und Erzeugung eines virtuellen COM Port-Paares z.B. `COM9` und `COM10`
2. Start der Windows-Konsole (`cmd`)
3. Ausfuehrung des com2tcp-Befehls nach folgendem Schema (eckige Klammern symbolisieren Platzhalter):

	com2tcp.exe \\.\<COM-Port> <IP-Adresse TCP-Server> <Port TCP-Server>
	
	com2tcp.exe \\.\COM10 192.168.1.2 1235


Die wichtigsten Verbindungsparameter von `com2tcp.exe` sind u.a.:

* `--baud, --b` Baudrate
* `--data, --d` Datenrate
* `--parity, --p` Paritaet
* `--stop, --s` Anzahl der Stopbits

Der vollstaendige Kommandoaufruf fuer die korrekte Ansteuerung des `Charmilles Robofil 510` ist somit:

	com2tcp.exe --baud 9600 --data 7 --stop 2 --parity even --ignore-dsr \\.\COM10 192.168.1.2 1235
	

Solang dieses Programm aktiv ist werden alle Daten des COM-Port `COM10` an den Roline Serial over Ethernet Device 
Server mit der angegebenen Netzwerkadresse und Port geschickt.

Der Nutzer muss nun nur noch das Programm `Cimco Edit V6` starten und fuer den Austausch von CNC-Dateien (Senden und Empfangen) mit der `Charmilles Robofil 510` COM-Port `COM9` auswaehlen.


## Kabel RS-232 25 pol. auf 9 pol.

Das Kabel zur Verbindung von `Charmilles Robofil 510` mit dem RS232 over Ethernet-Server benoetigt einen RS 232 - Adapter von DB9 nach DB25.
Je nach Schnittstellenkonfiguration der `Charmilles Robofil 510` (Cable switched oder Cable normal, Jumper Settings im Geraet) gibt sich folgende Belegung.

### Cable normal ###

	DB9 FEMALE	--------->		DB 25 male
	PIN 1 		--------->		PIN 8
	PIN 2 		--------->		PIN 2
	PIN 3 		--------->		PIN 3
	PIN 4 		--------->		PIN 20
	PIN 5 		--------->		PIN 7
	PIN 6 		--------->		PIN 6
	PIN 7 		--------->		PIN 4
	PIN 8 		--------->		PIN 5
	PIN 9 		--------->		PIN 22
	
### Cable switched ###

	DB9 FEMALE	--------->		DB 25 male
	PIN 1 		--------->		PIN 8
	PIN 2 		--------->		PIN 3
	PIN 3 		--------->		PIN 2
	PIN 4 		--------->		PIN 20
	PIN 5 		--------->		PIN 7
	PIN 6 		--------->		PIN 6
	PIN 7 		--------->		PIN 4
	PIN 8 		--------->		PIN 5
	PIN 9 		--------->		PIN 22

