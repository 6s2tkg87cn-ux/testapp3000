# Bandlauf

Übe-App für Musik mit sechs Instrumenten: Gitarre, Ukulele, Bass und Klavierbegleitung
(Laufband mit Akkorden, Akkord-Baukasten), Klaviermelodie (fallende Noten, rechte und
linke Hand) und Gesang (Liedtext Zeile für Zeile mit Akkorden). Dazu Stimmgerät,
Mikrofon-Erkennung und MIDI.

**Klassenband:** 14 Lieder gibt es bei allen Instrumenten – bei Klaviermelodie die Melodie,
bei Gitarre, Ukulele, Bass und Klavierbegleitung die passenden Akkorde, jeweils in derselben
Tonart, im selben Tempo und mit einem Takt zum Einzählen (4/4, 3/4 oder 2/4). So kann eine
Klasse zusammen spielen. Neu dabei: „Die Gedanken sind frei“, „Kein schöner Land“ und „Muss i denn“
(Melodien nach der Essener Volksliedsammlung) sowie „Morgen kommt der Weihnachtsmann“.

**Übersicht:** Die Lieder jedes Instruments sind in Bereiche aufgeteilt – Übungen, Klassenband,
Popsongs und „Meine“ (bei Klaviermelodie: alle Lieder, Klassenband, „Meine“).

**Lieder importieren:** Ein Lied mit Akkorden einfügen – Akkorde in einer eigenen Zeile über
dem Text (wie auf Akkordseiten) oder im ChordPro-Format. Die App erkennt Abschnitte,
Wiederholungen, englische und deutsche Akkordnamen (B/H) und vereinfacht Sonderakkorde.
Wie lange jeder Akkord dauert, steht in keinem Akkordblatt: Das stellt man im Schritt „Takte“
ein (− / +, „Alle 2 Takte“). Unpassende Akkorde oder ganze Zeilen lassen sich entfernen, und eine
fertige Strophe bzw. ein fertiger Refrain wird mit einem Tipp auf alle gleichartigen Abschnitte
übertragen. Danach gibt es das Lied bei Gesang (aktuelle Zeile mit Akkorden, die App spielt die
Band) und bei allen Begleit-Instrumenten.

**Teilen per QR-Code:** Beim Teilen zeigt die App einen QR-Code (auch groß für den Beamer).
Schülerinnen und Schüler scannen ihn, wählen ihr Instrument und können sofort loslegen.
Liedtexte sind urheberrechtlich geschützt: importierte Lieder nur im Unterricht mit der eigenen
Lerngruppe nutzen und nur innerhalb der Lerngruppe teilen.

**Beamer:** „Vollbild“ zeigt nur Text und Akkorde ohne Bedienelemente (z. B. beim Spiegeln per
AirPlay). Am Laptop/PC mit Beamer als zweitem Bildschirm öffnet „Beamer-Fenster“ ein eigenes,
synchron laufendes Fenster für den Beamer – wie der Präsentationsmodus von PowerPoint. Hinweis:
Über AirPlay kann eine Web-App kein eigenes zweites Bild erzeugen, AirPlay spiegelt das iPad.

**Melodie-Editor (Klaviermelodie):** Eigene Melodien als Balken setzen oder über ein
MIDI-Keyboard, die Computer-Tastatur oder die Bildschirmtasten aufnehmen – erst die rechte,
dann die linke Hand. Mit Quantisieren, Rückgängig, Tempo und Taktart. Geteilt wird über einen
Link oder Code, der die ganze Melodie enthält (kein Server nötig), oder als MIDI-Datei.

Die App ist eine reine Web-Seite (HTML, CSS,
JavaScript) – sie braucht **kein PHP, keine Datenbank und keine Installation** auf dem Server.

## Dateien

```
index.html          die komplette App
fonts/              Schriften (Unbounded, Lexend) – lokal, kein Google-Abruf
.htaccess           optionale Einstellungen für Apache-Server
web.config          optionale Einstellungen für Windows-Server (IIS)
```

## Auf den Schulserver laden

1. Den ganzen Ordner (mindestens `index.html` und den Ordner `fonts/`) per FTP, WebDAV
   oder Datei-Manager auf den Webspace der Schule hochladen – z. B. in einen Unterordner
   `bandlauf/`. Alle Pfade sind relativ, die App läuft in jedem Unterordner.
2. Im Browser aufrufen, z. B. `https://www.meine-schule.de/bandlauf/`.
3. Fertig. Gibt der Server „500 Internal Server Error“ aus, die Datei `.htaccess` löschen.

Läuft der Server ohne Internetzugang (nur im Schulnetz), funktioniert die App trotzdem:
Sie lädt nichts von fremden Servern nach.

## Wichtig: HTTPS für Mikrofon und MIDI

Browser erlauben Mikrofon (Stimmgerät, „App hört zu“) und MIDI-Geräte nur auf sicheren
Adressen, also `https://…` (oder `http://localhost`). Über eine reine `http://`-Adresse
funktioniert alles andere, nur das Mikrofon nicht – die App zeigt dann einen Hinweis.

- Hat der Schulserver ein Zertifikat, in der `.htaccess` die Umleitung auf HTTPS
  aktivieren (die drei auskommentierten Zeilen am Ende).
- Hat er keins, die Schul-IT bitten, HTTPS einzurichten (z. B. kostenlos mit Let's Encrypt).

## In WordPress einbinden

Schritt-für-Schritt-Anleitung: [ANLEITUNG-WORDPRESS.md](ANLEITUNG-WORDPRESS.md)

## In Moodle, IServ & Co. einbetten

Am einfachsten verlinkt man die App (Link „in neuem Fenster öffnen“). Soll sie in einem
iframe eingebettet werden, muss das iframe das Mikrofon ausdrücklich erlauben:

```html
<iframe src="https://www.meine-schule.de/bandlauf/" allow="microphone; midi; autoplay; fullscreen"
        style="width:100%;height:90vh;border:0"></iframe>
```

Blockiert die Plattform das Mikrofon im iframe trotzdem, zeigt die App einen Hinweis mit
dem Link zum Öffnen im eigenen Tab.

## Datenschutz

- Keine Anmeldung, keine Cookies, kein Tracking, keine Daten an den Server.
- Schriften liegen auf dem eigenen Server (kein Abruf bei Google Fonts).
- Eigene Akkordfolgen und Bestwerte speichert die App nur im Browser des jeweiligen
  Geräts (`localStorage`). Auf geteilten Schulrechnern können sie nach dem Abmelden
  gelöscht sein.
- Der Mikrofon-Ton wird nur im Browser ausgewertet und weder gespeichert noch gesendet.

## Hinweise für die Schul-IT

- Reine statische Dateien; jeder Webserver (Apache, nginx, IIS, IServ-Webspace …) genügt.
- `.woff2` sollte als `font/woff2` ausgeliefert werden (nginx ab 1.21.1 und aktuelle
  Apache-Versionen tun das bereits; für ältere Apache-Server erledigt es die `.htaccess`,
  für IIS die `web.config`).
- Inline-`<script>` und `<style>` werden verwendet. Setzt der Server eine strenge
  Content-Security-Policy, braucht diese Seite `script-src 'self' 'unsafe-inline'` und
  `style-src 'self' 'unsafe-inline'`.

## Sicherheit

- Die App lädt nichts von fremden Servern und baut keine Verbindungen auf. Eine
  Sicherheitsrichtlinie (Content-Security-Policy) steht direkt in `index.html` und verbietet
  das zusätzlich; `.htaccess` und `web.config` senden sie auch als Server-Header, dazu eine
  Permissions-Policy (Mikrofon und MIDI nur für Bandlauf, Kamera, Standort usw. aus).
- MIDI-Dateien (höchstens 2 MB), Melodie-Links und -Codes sowie gespeicherte Daten werden
  geprüft, bevor die App sie verwendet. Namen werden immer als reiner Text angezeigt.
- **Empfehlung:** Bandlauf möglichst unter einer eigenen Adresse betreiben, z. B.
  `bandlauf.meine-schule.de`. Browser merken sich die Mikrofon-Freigabe und den Speicher pro
  Adresse – liegen auf derselben Adresse noch andere Web-Apps, teilen sie sich beides.
- Einbetten per iframe ist erlaubt (Moodle, WordPress). Wer das einschränken will, aktiviert
  die Zeile `X-Frame-Options` in der `.htaccess`.

## Lizenzen

Schriften Unbounded und Lexend: SIL Open Font License 1.1 (siehe `fonts/LICENSE-*.txt`).
