# Bericht Woche 1
---

**Thema:** Digital Signage

**Datum:** 13.07.2026 - 17.07.2026


--- 

### Aufgabenstellung 
- Geeignete Open-Source-Lösungen recherchieren, die als Alternative zu Screenly oder Yodeck in Frage 
  kommen.
- Test auf Raspberry Pi oder vergleichbarer IoT-Hardware vorbereiten und die Grundfunktionen prüfen. 
- Wichtige Punkte dokumentieren: Installation, Fernverwaltung, Stabilität, Content-Ausspielung und 
  Wartbarkeit. 
- Am Ende eine kurze Empfehlung abgeben, welche Lösung für einen Pilotbetrieb am sinnvollsten wäre. 
 
--- 

### Anthias 

- Das Konzept: Es ist zu 100 % kostenlos und quelloffen. Im Gegensatz zur kostenpflichtigen Cloud-Version von Screenly verwaltest du jedes Gerät einzeln über eine lokale Weboberfläche.

- Architektur: Die Software läuft direkt auf dem Raspberry Pi (unterstützt Pi 2 bis Pi 5). Es benötigt keine dauerhafte Internetverbindung, da alle Medien lokal auf der SD-Karte gespeichert werden.

- Vorteile:

  - Extrem einfache Installation: Kann direkt über den offiziellen Raspberry Pi Imager geflasht werden.

  - Sehr stabil: Speziell für die Pi-Hardware optimiert; Videos laufen flüssig in Full HD.

  - Kein Cloud-Zwang: Läuft komplett autark im lokalen Netzwerk.

- Nachteile:

  - Kein zentrales Management: Jeder Raspberry Pi hat sein eigenes Dashboard. Wenn du 10 Bildschirme hast, musst du 10 IP-Adressen einzeln aufrufen, um Inhalte zu ändern.

 #### Inbetriebnahme (Anthias)
1. Raspberry Pi Imager downloaden
2. MircoSD Karte an den Pc anschließen
3. Raspberry Pi Imager öffnen
   1. Richtigen Pi wählen (z.B. Pi 3)
   2. OS auswählen -> Other specific-purpose Os -> Digital Signage -> Anthias
   3. SD Karte auswählen
   4. Zum Schluss auf SCHREIBEN
4. SD Karte in den Pi stecken
5. HDMI-Kabel anstecken, danach erst die Stromversorgung
6. LAN-Kabel einstecken
7. Bei hochfahren den Pi sollte nun eine IP-Adresse auf dem Bildschirm erscheinen, diese aufschreiben
8. Auf dem PC im Browser eingeben: http://"IP-Adresse"
9. Nun ist man auf dem Dashboard vom Pi und einstellen was gezeigt werden soll.


#### Praktische Testergebnisse (Anthias)

##### Zeitaufwand (Inbetriebnahme)
- Kriterium: Messung der benötigten Zeitspanne vom Flashen des Betriebssystem-Images bis zur ersten erfolgreichen Bildausgabe auf dem angeschlossenen Monitor.

- Ergebnis: Das Aufsetzen des Systems gestaltete sich durch das im Raspberry Pi Imager integrierte, vorkonfigurierte Image äußerst effizient. Der reine Schreibvorgang auf die MicroSD-Karte nahm rund 5 Minuten in Anspruch. Nach dem Einlegen der Karte und dem Bootvorgang stand die             Weboberfläche nach weiteren 3 Minuten im lokalen Netzwerk bereit. Die gesamte Inbetriebnahme bis zur ersten Content-Ausspielung war somit in unter 15 Minuten abgeschlossen.

- Fazit: Anthias eignet sich hervorragend für ein schnelles Deployment ohne tiefgehende Linux-Vorkenntnisse.

##### Format- und Medientest (Content-Ausspielung)

- Kriterium: Überprüfung der Performance und Darstellungsqualität bei verschiedenen Medientypen (lokale Videodateien vs. webbasierte Inhalte).

- Ergebnis: MP4-Videos: Lokale Videodateien im MP4-Format wurden in Full-HD-Auflösung ($1080\text{p}$) absolut flüssig und ohne sichtbare Framedrops oder Ruckler abgespielt.
  - Webseiten: Die Darstellung von Web-Assets über die integrierte Kiosk-Browser-Engine verlief plattformkonform.

- Einschränkung für die Praxis: Bei extrem JavaScript-lastigen Webseiten oder komplexen Animationen stieß die CPU des Raspberry Pi 4 vereinzelt an ihre Leistungsgrenzen, was zu verzögerten Ladezeiten führte. Für Standard-Websites und Dashboards ist die Performance jedoch             vollkommen ausreichend.

##### Härtetest (Simulierter Stromausfall & Ausfallsicherheit)
- Kriterium: Simulation eines unvorhergesehenen Spannungsabfalls durch abruptes Ziehen des Netzsteckers im laufenden Betrieb. Überprüfung, ob das System nach der Stromwiederkehr selbstständig und fehlerfrei in den Kiosk-Modus zurückkehrt.

- Ergebnis: Nach dem Trennen und anschließenden Wiederverbinden der Stromversorgung startete das zugrundeliegende Linux-Betriebssystem autonom. Anthias initialisierte sämtliche Hintergrunddienste ordnungsgemäß, rief die lokal gespeicherte Playlist ab und startete die Wiedergabe des          Contents vollautomatisch. Es war keinerlei manueller Eingriff erforderlich.

- Fazit: Das System beweist eine hohe Resilienz gegenüber netzseitigen Störungen und ist für den wartungsfreien Kiosk-Dauerbetrieb im Unternehmen qualifiziert.

--- 

### Xibo

- Das Konzept: Xibo trennt strikt zwischen dem Server (CMS), auf dem du die Inhalte verwaltest, und den Playern (Clients), die am Bildschirm hängen. Der Server wird via Docker auf einem PC/Server installiert.
  Für den Raspberry Pi gibt es mittlerweile einen offiziellen, kostenlosen Linux-Player.

- Architektur: Zentralisiert. Du verwaltest beliebig viele Bildschirme über ein einziges Web-Dashboard.

- Vorteile:

  - Echte Zentralverwaltung: Perfekt für den professionellen Einsatz. Du kannst hunderte Player von einer einzigen Oberfläche aus steuern.

  - Layout-Design: Du kannst den Bildschirm in Zonen unterteilen.

  - Umfangreiche Zeitplanung: Sehr detaillierte Steuerung, wann welche Kampagne läuft.

#### Inbetriebnahme (Xibo)
1.  Docker Desktop herunterladen
2.  Erstelle einen neuen Ordner Namens: "xibo-server"
3.  Erstelle in diesen Ordner einen neue Datei Namens: "docker-compose.yml", in diese schreibst du:
```
services:
    mysql:
        image: mysql:5.7
        volumes:
            - "./shared/db:/var/lib/mysql:Z"
        env_file: config.env
        restart: always

    cms-xmr:
        image: xibosignage/xibo-xmr:release-0.8
        ports:
            - "9505:9505"
        restart: always
        env_file: config.env

    cms-web:
        image: xibosignage/xibo-cms:latest
        volumes:
            - "./shared/cms/custom:/var/www/cms/custom:Z"
            - "./shared/cms/web/theme/custom:/var/www/cms/web/theme/custom:Z"
            - "./shared/cms/library:/var/www/cms/library:Z"
            - "./shared/cms/web/userscripts:/var/www/cms/web/userscripts:Z"
        ports:
            - "8080:80"
        restart: always
        env_file: config.env
        depends_on:
            - mysql
```
4. Danach erstellst du eine neue Datei Namens: "config.env", in diese schreibst du:
```
MYSQL_DATABASE=cms
MYSQL_USER=cms
MYSQL_PASSWORD=praktikum2026
MYSQL_ROOT_PASSWORD=praktikum2026
MYSQL_HOST=mysql
CMS_SERVER_NAME=localhost
```
5. Dann öffne deine Kommandozeile und gib ein:
```
cd "Pfad des Ordners"

docker-compose up -d
```
6. Jetzt kann man Docker Desktop öffnen und man sieht den Xibo Server
7. Öffne die Seite des Xibo-Servers jetzt mit "http://localhost:8080"
8. Login ist: xibo_admin Passwort ist: password

##### Mit dem Terminal:
9. Öffne den Raspberry Pi Imager und lade auf die SD-Karte das Pi OS (Legacy, 64-Bit) Bookworm
10. Botte den Pi jetzt mit der SD-Karte

#####  Mit Android:
9. Öffne den Raspberry Pi Imager und lade auf die SD-Karte unter Freemium and paid-for OS "Android by emteria". 
10. Botte den Pi jetzt mit der SD-Karte




#### Probleme während der Inbetriebnahme der Player/Clients

Bei der Inbetriebnahme des offiziellen Linux-basierten Xibo-Players traten gravierende Probleme auf. Nach dem Starten der Anwendung kam es zu einem ununterbrochenen Hin- und Herspringen des Anmeldefensters. 

Was wurde durchgeführt, damit man es zum laufen bekommen könnte:
* Das manuelle Erzwingen des älteren X11-Grafikprotokolls
* Das Abschalten der Chromium-Sandbox und der GPU-Beschleunigung
* Die Modifikation der Konfigurationsdateien über das Terminal

#### Ursachenanalyse:
Die anschließende tiefergehende Systemanalyse ergab zwei wesentliche Gründe, weshalb Xibo in dieser Konstellation keine praxistaugliche Option darstellt:

#### 1. Inkompatibilität der CPU-Architektur
Der offizielle Linux-Player von Xibo basiert auf einer x86_64-Architektur (Intel/AMD) und schließt ARM-Prozessoren, wie sie auf dem Raspberry Pi verbaut sind, aus. Versuche, die Software über inoffizielle Umwege auf Debian 12 (Bookworm) zu betreiben, scheitern an der Inkompatibilität mit dem modernen Wayland-Fenstermanagerk des Betriebssystems, was das unkontrollierte Flackern der Benutzeroberfläche auslöst.

#### 2. Lizenzbarrieren beim Android-Workaround
Ein alternativer Lösungsansatz über das Android-Betriebssystem emteria.OS auf dem Raspberry Pi ermöglichte zwar technisch die erfolgreiche Installation der Xibo-Android-App, scheiterte jedoch daran das man für die Lizenz zahlen müsste. Im Gegensatz zu den Desktop-Clients für Windows und Linux ist der Android-Player von Xibo ein rein kommerzielles Produkt. Nach Ablauf einer 14-tägigen Testphase entstehen pro Gerät laufende Lizenzgebühren.

#### Fazit & Ausblick

Ergebnis der Evaluation:  
Da das Ziel des Projekts eine dauerhaft kostenfreie Open-Source-Lösung auf Raspberry-Pi-Basis war, erwies sich Xibo für dieses Szenario als ungeeignet. 

---

#### PiSignage (Open-Source Server Community Edition)

- Das Konzept: Es ist zu 100 % kostenlos. Die jeweiligen Screens werden über einen Zentralen Pc gesteuert.

- Architektur: Ähnlich wie Xibo zentralisiert, aber speziell auf den Raspberry Pi und dessen Hardware-Eigenschaften zugeschnitten.

- Vorteile:

  - Zentrales Management gratis: Bietet eine zentrale Verwaltung für mehrere Pis, ohne Geld zu kosten.

  - Gute Pi-Optimierung: Unterstützt native Pi-Features wie CEC (Bildschirm über das HDMI-Kabel automatisch an-/ausschalten).

Nachteile:

 
    
#### Inbetriebnahme (PiSignage)
1.  Docker Desktop herunterladen
2.  PiSignage Image auf der Website von PiSignage herunterladen
3. Raspberry Pi Imager öffnen und unter OS "Eigenes Image" auwählen
4. Das zuvor heruntergelade Image auf die SD-Karte schreiben
5. Sd-Karte in den Pi stecken und botten lassen.
6. Es sollte nun auf dem angeschlossenem Bild des Pi´s angezeigt werden welche ID er hat.
7. Öffnen sie auf ihrem Pc den Browser und geben sie "www.pisognage.com" ein und loggen sie sich ein
8. Jetzt können sie in ihrem Dashboard, unter Player auf "Einen Player registrieren" drücken
9. Mit der ID auf dem Bildschrim des Pi´s, können sie den Player hinzufügen
10. Dann unter Inhalte die gewünschten Inhalte hochladen (Bilder, Videos, etc.)
11. Diese Anschließen zu einer Wiedergabeliste hinzufügen
12. Und diese wiederum zu einer Gruppe hinzufügen
13. Die jeweilige Gruppe Können sie jetzt bereitstellen und ihr Inhalt wird auf ihrem Player angezeigt

---

### Probleme während des Praxis Test

## Fehleranalyse und Problembehebung: Instabilitäten der Stromversorgung

Im Rahmen meines Praktikumsprojekts zur Einrichtung eines Digital-Signage-Systems auf einem Raspberry Pi 3 (RPi3) traten kritische Probleme bei der Stromversorgung auf. Diese führten im Verlauf der Inbetriebnahme zu einem temporären Systemausfall. Im Folgenden dokumentiere ich die beobachteten Fehlerbilder, meine durchgeführten Schritte zur Fehlerdiagnose sowie die erarbeitete Lösung.

### 1. Problembeschreibung und Fehlerbild

* **Mein erster Versuch (Standard-Netzteil):** Das von mir primär verwendete Netzteil lieferte eine zu geringe elektrische Leistung. Der Raspberry Pi signalisierte dies sporadisch durch das gelbe Blitz-Symbol in der oberen rechten Ecke des Bildschirms (Under-Voltage-Warnung). Das System lief dadurch extrem instabil und stürzte wiederholt ab.
* **Mein zweiter Versuch (Ersatznetzteil von zu Hause):** Um das Problem schnell zu lösen, habe ich ein privates Ersatznetzteil mitgebracht. Damit schien das System zunächst stabil zu laufen. Nach etwa drei Stunden kontinuierlichem Betrieb brach die Spannung dieses Netzteils unter Last jedoch ebenfalls komplett zusammen.
* **Aktueller Systemstatus:** Nach diesem Spannungszusammenbruch bootet mein Raspberry Pi 3 überhaupt nicht mehr. Auf der Platine leuchtet dauerhaft nur noch die rote Power-LED (PWR). Die grüne Aktivitäts-LED (ACT) zeigt keinerlei Blinksignale und es wird kein Bildsignal mehr ausgegeben.

### 2. Meine Maßnahmen

* **Lastminimierung:** Ich habe sämtliche angeschlossenen Peripheriegeräte (Maus, Tastatur, USB-Sticks) abgezogen. Damit wollte ich sicherstellen, dass der Stromverbrauch beim Startvorgang so gering wie möglich gehalten wird.
* **Software-Ausschluss:** Ich habe versucht, das System mit einer frischen Speicherkarte und einem schlanken Standard-Image (Raspberry Pi OS (Legacy, 32-Bit)) zu starten. Dadurch konnte ich ausschließen, dass eine fehlerhafte Konfiguration meiner Digital-Signage-Software das Booten blockiert. Leider schlug auch dieser Versuch fehl.
* **Hardware-Prüfung:** Nach genauerer Recherche konnte ich ausschließen, dass die SoC-Komponenten (Prozessor) dauerhaft beschädigt wurden. Der Raspberry Pi befindet sich stattdessen in einer Schutzabschaltung.

### 3. Ursache und geplante Lösung

#### Hintergrund zur Hardware-Schutzschaltung:
Der Raspberry Pi besitzt eine integrierte, selbstrücksetzende Schmelzsicherung (eine sogenannte **Polyfuse**). Bei einer gravierenden Unterspannung oder abrupten Spannungsspitzen (wie beim Einbrechen meines zweiten Netzteils) löst diese Sicherung aus, um die empfindlichen Hauptkomponenten vor dauerhaften Schäden zu schützen. Solange diese Sicherung aktiv ist, verbleibt der Pi in einem "Reset-Modus", bei dem ausschließlich die rote PWR-LED leuchtet.

#### Maßnahme zur Behebung:
Eine solche Polyfuse benötigt nach dem Auslösen ausreichend Zeit, um vollständig abzukühlen und ihren physikalischen Widerstand wieder auf den Normalwert zu senken. Ich habe den Raspberry Pi daher komplett von der Stromversorgung getrennt. Er verbleibt nun über Nacht im absolut stromlosen Zustand.

Zum weitern Testen der Open-Source Löungen wurde mir ein stärkerer Raspberry Pi 4 Model B zurverfügung gestellt mit dem originalem Netzteil (5.1V / 3A), um keine Leistungseinbrüche inder Stromversorgung zu haben. 

--- 

#### Hardware-Planung

| Komponente | Spezifikation / Empfehlung | Menge | Warum wichtig? | Preis |
| :--- | :--- | :---: | :--- | :--- |
| **Raspberry Pi** | Raspberry Pi 4 (4GB) oder Pi 5 (4GB) | 5x | Das Gehirn hinter dem Bildschirm. 4GB RAM sind wichtig für flüssige Web-Dashboards. | 575 - 675,- |
| **Netzteil** | Offizielles USB-C Netzteil | 5x | Billige Netzteile führen bei Pis unter Last schnell zu Abstürzen oder Performance-Drosselung. | 60 - 75,- |
| **Speicherkarte** | 32 GB MicroSD (z.B. SanDisk Extreme) | 5x | Schnelles Schreiben-/Leseen verhindern Ruckler im System. | 80,- |
| **Gehäuse & Kühlung** | Passives Alu-Gehäuse | 5x |Ein Gehäuse mit passiver Kühlung (ohne Lüfter) ist wartungsfrei und leise. | 75,- |
| **Display-Kabel** | Micro-HDMI auf Standard-HDMI-Kabel (ca. 1m bis 2m) | 5x | Der Pi hat Micro-HDMI-Ausgänge, Fernseher haben Standard-HDMI. | 25,- |
| **Bildschirm / TV** | 24/7 geeignete Business-Displays (z.B. von Samsung oder LG, Full HD) | 5x | Normale Consumer-TVs gehen im Dauerbetrieb (24/7) in der Produktion schnell kaputt. |  |
| **Wandhalterung** | Flache VESA-Halterung | 5x | Um die Displays sicher an der Wand oder an Produktionsmaschinen zu befestigen. | _ |

Ausgangssituation: 5x Displays





