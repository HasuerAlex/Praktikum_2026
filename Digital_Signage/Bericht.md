# Bericht Woche 1
---

**Thema:** Digital Signage

**Datum:** 13.07.2026

--- 

## 1 Tag
Geeignete Open-Source-Lösungen recherchieren, die als Alternative zu Screenly oder Yodeck in Frage
kommen. 

### Mögliche Alternativen 
#### Anthias (ehemals Screenly OSE)

- Das Konzept: Es ist zu 100 % kostenlos und quelloffen. Im Gegensatz zur kostenpflichtigen Cloud-Version von Screenly verwaltest du jedes Gerät einzeln über eine lokale Weboberfläche.

- Architektur: Die Software läuft direkt auf dem Raspberry Pi (unterstützt Pi 2 bis Pi 5). Es benötigt keine dauerhafte Internetverbindung, da alle Medien lokal auf der SD-Karte gespeichert werden.

- Vorteile:

  - Extrem einfache Installation: Kann direkt über den offiziellen Raspberry Pi Imager geflasht werden.

  - Sehr stabil: Speziell für die Pi-Hardware optimiert; Videos laufen flüssig in Full HD.

  - Kein Cloud-Zwang: Läuft komplett autark im lokalen Netzwerk.

- Nachteile:

  - Kein zentrales Management: Jeder Raspberry Pi hat sein eigenes Dashboard. Wenn du 10 Bildschirme hast, musst du 10 IP-Adressen einzeln aufrufen, um Inhalte zu ändern.

#### Xibo (Self-Hosted CMS + Open-Source Linux Player)

- Das Konzept: Xibo trennt strikt zwischen dem Server (CMS), auf dem du die Inhalte verwaltest, und den Playern (Clients), die am Bildschirm hängen. Der Server wird via Docker auf einem PC/Server installiert.
  Für den Raspberry Pi gibt es mittlerweile einen offiziellen, kostenlosen Linux-Player.

- Architektur: Zentralisiert. Du verwaltest beliebig viele Bildschirme über ein einziges, mächtiges Web-Dashboard.

- Vorteile:

  - Echte Zentralverwaltung: Perfekt für den professionellen Einsatz. Du kannst hunderte Player von einer einzigen Oberfläche aus steuern.

  - Mächtiges Layout-Design: Du kannst den Bildschirm in Zonen unterteilen (z. B. links Video, rechts Wetter-Widget, unten Newsticker).

  - Umfangreiche Zeitplanung: Sehr detaillierte Steuerung, wann welche Kampagne läuft.



#### PiSignage (Open-Source Server Community Edition)

- Das Konzept: Wenn du den Server selbst auf einem eigenen System betreibst (z. B. via Docker), entfallen die Lizenzgebühren. Du nutzt das System somit völlig kostenlos.

- Architektur: Ähnlich wie Xibo zentralisiert, aber speziell auf den Raspberry Pi und dessen Hardware-Eigenschaften zugeschnitten.

- Vorteile:

  - Zentrales Management gratis: Bietet eine zentrale Verwaltung für mehrere Pis, ohne Geld zu kosten (solange du den Server selbst hostest).

  - Gute Pi-Optimierung: Unterstützt native Pi-Features wie CEC (Bildschirm über das HDMI-Kabel automatisch an-/ausschalten).

Nachteile:

  - Community-Support: Da es die kostenlose Variante des Herstellers ist, gibt es Updates und Fehlerbehebungen oft etwas verzögert über die GitHub-Community.
