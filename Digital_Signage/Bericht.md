# Bericht Woche 1
---

**Thema:** Digital Signage

**Datum:** 13.07.2026

--- 

### Aufgabenstellung 
- Geeignete Open-Source-Lösungen recherchieren, die als Alternative zu Screenly oder Yodeck in Frage 
kommen.
- Test auf Raspberry Pi oder vergleichbarer IoT-Hardware vorbereiten und die Grundfunktionen prüfen. 
- Wichtige Punkte dokumentieren: Installation, Fernverwaltung, Stabilität, Content-Ausspielung und 
  Wartbarkeit. 
- Am Ende eine kurze Empfehlung abgeben, welche Lösung für einen Pilotbetrieb am sinnvollsten wäre. 
 

### Anthias (ehemals Screenly OSE)

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


#### Evaluierung, Troubleshooting und praktische Testergebnisse (Anthias)

##### Zeitaufwand (Inbetriebnahme)
- Kriterium: Messung der benötigten Zeitspanne vom Flashen des Betriebssystem-Images bis zur ersten erfolgreichen Bildausgabe auf dem angeschlossenen Monitor.

- Ergebnis: Das Aufsetzen des Systems gestaltete sich durch das im Raspberry Pi Imager integrierte, vorkonfigurierte Image äußerst effizient. Der reine Schreibvorgang auf die MicroSD-Karte nahm rund 5 Minuten in Anspruch. Nach dem Einlegen der Karte und dem Bootvorgang stand die             Weboberfläche nach weiteren 3 Minuten im lokalen Netzwerk bereit. Die gesamte Inbetriebnahme bis zur ersten Content-Ausspielung war somit in unter 15 Minuten abgeschlossen.

- Fazit: Anthias eignet sich hervorragend für ein schnelles Deployment ohne tiefgehende Linux-Vorkenntnisse.

##### Format- und Medientest (Content-Ausspielung)

- Kriterium: Überprüfung der Performance und Darstellungsqualität bei verschiedenen Medientypen (lokale Videodateien vs. webbasierte Inhalte).

- Ergebnis: MP4-Videos: Lokale Videodateien im MP4-Format wurden in Full-HD-Auflösung ($1080\text{p}$) absolut flüssig und ohne sichtbare Framedrops oder Ruckler abgespielt. Die Hardware-Beschleunigung des Raspberry Pi 3 wird vom System optimal ausgenutzt.
  - Webseiten (z. B. Firmen-Website): Die Darstellung von Web-Assets über die integrierte Kiosk-Browser-Engine verlief plattformkonform. Textinhalte, CSS-Layouts und statische Elemente wurden fehlerfrei gerendert.

- Einschränkung für die Praxis: Bei extrem JavaScript-lastigen Webseiten oder komplexen Animationen stieß die CPU des älteren Raspberry Pi 3 vereinzelt an ihre Leistungsgrenzen, was zu verzögerten Ladezeiten führte. Für Standard-Websites und Dashboards ist die Performance jedoch             vollkommen ausreichend.

##### Härtetest (Simulierter Stromausfall & Ausfallsicherheit)
- Kriterium: Simulation eines unvorhergesehenen Spannungsabfalls durch abruptes Ziehen des Netzsteckers im laufenden Betrieb. Überprüfung, ob das System nach der Stromwiederkehr selbstständig und fehlerfrei in den Kiosk-Modus zurückkehrt.

- Ergebnis: Nach dem Trennen und anschließenden Wiederverbinden der Stromversorgung startete das zugrundeliegende Linux-Betriebssystem autonom. Anthias initialisierte sämtliche Hintergrunddienste ordnungsgemäß, rief die lokal gespeicherte Playlist ab und startete die Wiedergabe des          Contents vollautomatisch. Es traten keine korrupten Dateisystemfehler auf der MicroSD-Karte auf, und es war keinerlei manueller Eingriff (z. B. per Tastatur oder SSH) erforderlich.

- Fazit: Das System beweist eine hohe Resilienz gegenüber netzseitigen Störungen und ist für den wartungsfreien Kiosk-Dauerbetrieb im Unternehmen qualifiziert.

--- 

### Xibo (Self-Hosted CMS + Open-Source Linux Player)

- Das Konzept: Xibo trennt strikt zwischen dem Server (CMS), auf dem du die Inhalte verwaltest, und den Playern (Clients), die am Bildschirm hängen. Der Server wird via Docker auf einem PC/Server installiert.
  Für den Raspberry Pi gibt es mittlerweile einen offiziellen, kostenlosen Linux-Player.

- Architektur: Zentralisiert. Du verwaltest beliebig viele Bildschirme über ein einziges, mächtiges Web-Dashboard.

- Vorteile:

  - Echte Zentralverwaltung: Perfekt für den professionellen Einsatz. Du kannst hunderte Player von einer einzigen Oberfläche aus steuern.

  - Mächtiges Layout-Design: Du kannst den Bildschirm in Zonen unterteilen (z. B. links Video, rechts Wetter-Widget, unten Newsticker).

  - Umfangreiche Zeitplanung: Sehr detaillierte Steuerung, wann welche Kampagne läuft.


---

#### PiSignage (Open-Source Server Community Edition)

- Das Konzept: Wenn du den Server selbst auf einem eigenen System betreibst (z. B. via Docker), entfallen die Lizenzgebühren. Du nutzt das System somit völlig kostenlos.

- Architektur: Ähnlich wie Xibo zentralisiert, aber speziell auf den Raspberry Pi und dessen Hardware-Eigenschaften zugeschnitten.

- Vorteile:

  - Zentrales Management gratis: Bietet eine zentrale Verwaltung für mehrere Pis, ohne Geld zu kosten (solange du den Server selbst hostest).

  - Gute Pi-Optimierung: Unterstützt native Pi-Features wie CEC (Bildschirm über das HDMI-Kabel automatisch an-/ausschalten).

Nachteile:

  - Community-Support: Da es die kostenlose Variante des Herstellers ist, gibt es Updates und Fehlerbehebungen oft etwas verzögert über die GitHub-Community.
    
#### Inbetriebnahme (PiSignage)
1.  Docker Desktop herunterladen
2.  PiSignage Image auf der Website von PiSignage herunterladen
3.  Erstelle einen neuen Ordner: docker-pisignage-test
4.  Erstelle einen Textdatei mit Namens: docker-compose.yml mit diesem Inhalt:
      ```
      version: '3'
      
      services:
        # Das Hauptprogramm
        pisignage-server:
          image: pisignage/pisignage-server
          container_name: pisignage-server
          ports:
            - "3000:3000"
          volumes:
            - pisignage-data:/home/pi/data
          depends_on:
            - mongo
          environment:
            - MONGO_URL=mongodb://mongo:27017/pisignage
      
        # Die benötigte Datenbank
        mongo:
          image: mongo:4.4
          container_name: pisignage-mongo
          volumes:
            - mongo-data:/data/db
      
      volumes:
        pisignage-data:
        mongo-data:
      ```
5. Öffne die Eingabeaufforderung:
```
cd "z.B  C:\Users\Alexander Hauser\Documents\Pisignage-Test"

docker compose up -d
```
6. Der Server sieht man nun in Docker Desktop unter Containers
7. Mit http://localhost:3000 kann sich auf das PiSignage Dashboard verbinden

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

#### Geplante Maßnahme zur Behebung:
Eine solche Polyfuse benötigt nach dem Auslösen ausreichend Zeit, um vollständig abzukühlen und ihren physikalischen Widerstand wieder auf den Normalwert zu senken. Ich habe den Raspberry Pi daher komplett von der Stromversorgung getrennt. Er verbleibt nun über Nacht im absolut stromlosen Zustand.

Für die nächsten Projektschritte werde ich ein offizielles Raspberry-Pi-Netzteil organisieren, welches dauerhaft stabile **5,1 V und 2,5 A** liefert. Nach der nächtlichen Regenerationsphase der Sicherung werde ich den Bootvorgang mit diesem spezifikationsgerechten Netzteil erneut durchführen.






