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

##### Hardware-Troubleshooting (Stromversorgung & Thermik)
- Fehlerbeschreibung (Boot-Loop): 
  Nach dem erfolgreichen Flashen des Images startete der Raspberry Pi 3 nach jeweils ca. 1–2 Minuten Betriebszeit kontinuierlich neu. Die Weboberfläche zur Fernverwaltung war jeweils nur für wenige Sekunden erreichbar.

- Ursachenanalyse (Netzteil): Für den Erstaufbau wurde testweise ein älteres, kompaktes iPhone-Netzteil verwendet. Dieses liefert spezifikationsgemäß lediglich eine Leistung von 1A. Unter der Last des initialisierenden Digital-Signage-Betriebssystems benötigt der Raspberry Pi 3 jedoch       eine Stromstärke von mindestens 2,5A. Die Überlastung führte zu einem massiven Spannungsabfall (Under-voltage) und triggerte den automatischen Neustart aus Selbstschutz.

- Ursachenanalyse (Thermik): Ergänzend wurde festgestellt, dass Anthias die CPU beim Rendern des Dashboards direkt nach dem Booten stark beansprucht. Ohne passiven Kühlkörper stieg die Temperatur auf der Platine rasch an, was das Risiko von Thermal Throttling erhöhte.Fehlerbehebung: Das     unzureichende Netzteil wurde durch eine adäquate, stabile Stromquelle ($5\\text{V} / 2,5\\text{A}$) mit Micro-USB-Anschluss ausgetauscht. Zudem wurden alle unnötigen USB-Peripheriegeräte (Maus/Tastatur) getrennt, um den Stromverbrauch zu minimieren. Nach diesen Modifikationen lief der     Minicomputer dauerhaft stabil.

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
    
#### Inbetriebnahme (Anthias)
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





