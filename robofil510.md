# __Netzwerkansteuerung für die Erodiermaschine CHARMILLES ROBOFIL 510__

## Idee

Die Erodiermaschine `CHARMILLES ROBOFIL 510` verfuegt lediglich ueber eine serielle Schnittstelle (RS-232/RS-422), die einen Austausch zwischen Maschine und PC gewaehrleistet. Mit Hilfe eines RS232 over Ethernet-Servers soll das Geraet zusaetzlich im Netzwerk zur Verfuegung gestellt werden.
Die folgende Anleitung beinhaltet alle wichtigen Informationen zur Einrichtung des RS232 over Ethernet-Servers und der Softwarekonfiguration auf den Clientrechnern.

## Verwendete Hardware
* [Roline Serial over Ethernet Device Server] 1x Port RS-232 Artikelnummer 15.06.0015 (https://www.secomp.de "Secomp Computer Products")
* Netzwerkkabel Cat 5 Kabel
* Datenkabel RS-232 9 pol. female auf RS-232 25 pol. male nicht gedreht

## Verwendete Programme, Bibliotheken usw.
[Cimco Edit V6 Professional](http://http://www.cimco.com/ "Informationen zu Cimco Edit V6 Professional")
* [com0com](http://http://com0com.sourceforge.net/ "Erstellt virtuelle COM-Port-Paare")
* [com0com](http://http://com0com.sourceforge.net/ "Erstellt virtuelle COM-Port-Paare")
* [com2tcp](http://sourceforge.net/projects/com0com/files/ "COM Port zu TCP Redirector")
* Com Port Redirector

## Softwaredownloads

Die fuer den Roline Serial over Ethernet Device Server benoetigten

Das Programm `zwfwaage.exe` kann einfach per Mausklick oder über die Kommandozeile gestartet werden. Zur Nutzung mit der Baan-Session `tibde9115m900` muss der GESAMTE Inhalt des Ordners `waage\bin\Release` bzw. `waage\bin\Debug` nach `C:\waage` kopiert werden.

## Nutzung des Programms

Ein Beispielaufruf des Programms kann wie folgt aussehen:

	zwfwaage.exe

Dieser Aufruf erzeugt im Hauptordner des Programms die Datei `last`, die die Messdaten der Rhewa 82basic beinhaltet.
Je nachdem ob mit Tara gearbeitet wurde koennen die entstehenden Dateien wie folgt aussehen:

### COM-Port-Ausgabe ohne Tara

	Nr.        508
	Bereich                                1
	Brutto                          439,0 kg
	----------------------------------------


### COM-Port-Ausgabe bei Verwendung Tara

	Nr.        508
	Bereich                                1
	Brutto                          439,0 kg
	Tara                            164,5 kg
	Netto                           274,5 kg
	----------------------------------------


Die Werte der letzten Messung werden standardmaessig in der Datei `last` gespeichert. Wird beim Start des Programms das Kommandozeilenargument `--name` bzw. `n` verwendet. wird die Datei im Ordner `data/{name}` angelegt.


## Notwendige Einstellungen an der Waage

Zur Benutzung des Programms `zwfwaage.exe` muessen folgende Einstellungen an der Konfiguration der Waage vorgenommen werden:

* COM-Port Einstellungen des Programms entsprechend der Geraetekonfiguration unter `G.KONFI` -> `INTERFACE` -> ... einstellen
* Handshake fuer COM-Port-Verbindung auf `none` setzen (im Menue `G.KONFI` -> `INTERFACE` -> `HANDSH`)
* Format des Drucks auf `UNIVER` (Universell) stellen (im Menue `G.KONFI` -> `DRUCK` -> `DRUCK.EI` -> `FORMAT`)
* Anzahl der Datensatzwiederholungen auf `1` setzen (im Menue `G.KONFI` -> `DRUCK` -> `DRUCK.EI` -> `ANZAHL`)
* EDV-Kommunikation des Geraetes auf `aktiv` setzen (im Menue `G.KONFI` -> `DRUCK` -> `EDV.KOM` -> `AKTIV`)

## Anforderungen

* Rechner mit verfuegbarem seriellen COM-Port
* Linux ab Kernel 2.6.x, Windows XP, Windows Vista, Windows 7, Mac OSX 10.5.x oder hoeher
* Bearbeitung der Projektdatei mit Codeblocks (IDE) und Kompilierung mit der C++ Boost Bibliothek

## Kommandozeilenargumente

Das Programm `zwfwaage.exe` kann mit folgenden Startparametern ausgefuehrt werden:

* `--help, --h` Listet alle zulaessigen Kommandozeilenargumente und ihre Funktionsbeschreibung auf
* `--name, --n` Angabe des Dateinamens, in dem die Daten der Messung gespeichert werden.
* `--console, --c` Startet des Programm im interaktiven Konsolenmodus
* `--p, --p` Angabe des COM-Ports z.B. --p COM1 oder --p /dev/ttyS0, Standard ist COM1
* `--baudrate, --b` Angabe der Uebertragungsgeschwindigkeit des COM-Ports, Standard ist 9600
* `--file, --f` Angabe des Pfades zu einer Konfigurationsdatei

## Interaktiver Konsolenmodus

Durch den Start des Programms im interaktiven Konsolenmodus kann ueber spezifische COM-Port-Befehle eine direkte Kommunikation mit der `Rhewa 82basic` hergestellt werden.
Die Rueckantwort des Geraetes wird direkt in der Konsole ausgegeben.

Momentan stehen folgende Befehle zur Verfuegung:

* `<FP>` Gibt die aktuellen Messwerte der Waage in der Konsole aus
* `exit` Beendigung des Programms

##Konfigurationsoptionen

Zur Konfiguration der COM-Port Verbindungsdaten bietet `zwfwaage.exe` verschiedene Moeglichkeiten:


### Erstellung einer Autokonfigurationsdatei

Die Konfigurationsdatei muss den Namen `config` haben und sich im gleichen Ordner wie die Datei `zwfwaage.exe` befinden. Der Aufbau der Datei sieht dabei folgende Struktur vor:
	
	port:COM3
	baudrate:115200

Die Reihenfolge von Port- und Baudratenangabe kann beliebig vertauscht werden.


### Verwendung des Kommandozeilenarguments `--f`

Mit Hilfe des Kommandozeilenarguments `--f` kann ein individueller Pfad zu einer Konfigurationsdatei angegeben werden (siehe Abschnitt Kommandozeilenargumente).
Die Struktur der Konfigurationsdatei enspricht dabei der Struktur der Autokonfigurationsdatei.

### Verwendung der Kommandozeilenargumente `--p` und `--b`

Mit Hilfe der Kommandozeilenargumente `--p` und `--b` koennen die Verbindungsparameter des Programms direkt beim Aufruf angegeben werden (siehe Abschnitt Kommandozeilenargumente).



### ANMERKUNGEN

Wenn die Kommandozeilenargumente `--p` oder `--b` nicht verwendet werden bezieht das Programm die Verbindungsdaten aus der Konfigurationsdatei. Dabei hat die Konfigurationsdatei, die mit dem Kommandozeilenargument `--f` angegeben wurde Vorrang vor der Autokonfigurationsdatei im Hauptornder des Programms.
Existiert keine Autokonfigurationsdatei greifen die Standardeinstellungen (`COM1` mit einer Baudrate von `9600`)


## Kompilierung

Das `zwfwaage.exe` Repository beinhaltet vorkompilierte Versionen der Anwendungen im bin-Ordner. Dort kann zwischen einer Debug und Release-Version entschieden werden.
