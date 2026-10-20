# Bandlauf in WordPress einbinden

WordPress kann die App nicht selbst „enthalten“. Die App liegt deshalb als eigener Ordner
auf dem Webspace, und WordPress zeigt sie an (eingebettet) oder verlinkt sie.

```
https://www.meine-schule.de/            ← WordPress
https://www.meine-schule.de/bandlauf/   ← die App (eigener Ordner)
```

Du brauchst:

- **Zugang zum Webspace:** per FTP/SFTP oder über den Datei-Manager des Hosters
  (z. B. Plesk, cPanel, All-Inkl KAS, IONOS). Hast du ihn nicht, frag die Person, die die
  Schul-Website betreut.
- **Ein WordPress-Konto mit der Rolle „Administrator“ oder „Redakteur“.** Nur diese Rollen
  dürfen iframes in Seiten einfügen. In einer WordPress-Multisite darf das nur der
  Netzwerk-Administrator.

> **Hinweis zu WordPress.com:** Auf den günstigen Tarifen von WordPress.com gibt es keinen
> Dateizugang, und eigenes HTML wird gefiltert. Dort geht nur Variante C (Link auf einen
> anderen Server, auf dem die App liegt).

---

## Schritt 1: App hochladen

1. Die ZIP-Datei `bandlauf.zip` auf dem Computer entpacken. Es entsteht ein Ordner
   `bandlauf` mit `index.html`, dem Ordner `fonts` und weiteren Dateien.
2. Mit dem FTP-Programm (z. B. FileZilla) oder dem Datei-Manager des Hosters den
   **ganzen Ordner `bandlauf`** in das Hauptverzeichnis der WordPress-Installation
   hochladen. Das ist der Ordner, in dem auch `wp-admin`, `wp-content` und `wp-config.php`
   liegen.
   - **Nicht** in `wp-content/uploads` oder über die Mediathek hochladen. WordPress
     blockiert dort `.html`- und Schrift-Dateien.
   - Die Datei `.htaccess` ist auf Mac und Windows oft unsichtbar (der Punkt am Anfang
     macht sie „versteckt“). Sie ist nicht zwingend nötig. Fehlt sie, läuft die App trotzdem.
   - **Wichtig:** Die `.htaccess` von WordPress selbst (im Hauptverzeichnis) **nicht**
     überschreiben. Die App-`.htaccess` gehört nur in den Ordner `bandlauf`.
3. Im Browser testen: `https://www.meine-schule.de/bandlauf/`. Die App sollte erscheinen.
   Bei „500 Internal Server Error“ die `.htaccess` im Ordner `bandlauf` löschen.

Kein Plugin nötig: WordPress lässt echte Ordner und Dateien automatisch in Ruhe und
liefert sie direkt aus.

---

## Schritt 2: In WordPress anzeigen

Es gibt drei Varianten. Am zuverlässigsten ist **Variante A + C kombiniert**: App einbetten
und darunter einen Link zum Öffnen im eigenen Tab anbieten.

### Variante A: Eingebettet in eine Seite (iframe)

1. In WordPress **Seiten → Neue Seite erstellen**, Titel z. B. „Bandlauf – Musik üben“.
2. Auf **+** klicken, den Block **„Individuelles HTML“** (engl. „Custom HTML“) suchen und
   einfügen.
3. Diesen Code hineinkopieren und die Adresse anpassen:

   ```html
   <iframe
     src="https://www.meine-schule.de/bandlauf/"
     title="Bandlauf – Übe-App für Musik"
     allow="microphone; midi; autoplay; fullscreen"
     allowfullscreen
     loading="lazy"
     style="display:block;width:100%;height:85vh;min-height:560px;border:0;border-radius:12px">
   </iframe>
   <p><a href="https://www.meine-schule.de/bandlauf/" target="_blank" rel="noopener">Bandlauf im eigenen Fenster öffnen</a> (empfohlen auf Handy und Tablet)</p>
   ```

4. Den Block in der Werkzeugleiste auf **„Volle Breite“** stellen (falls das Theme das
   anbietet). Das gibt der App mehr Platz.
5. Auf **Vorschau** klicken, testen, dann **Veröffentlichen**.

Wichtig ist `allow="microphone; midi; …"`. Ohne diese Angabe darf die App im iframe weder
das Mikrofon (Stimmgerät, „App hört zu“) noch MIDI-Geräte benutzen.

### Variante B: Eigene Seite ohne Theme-Rahmen

Soll die App die ganze Seite füllen, ohne Kopfzeile und Menü der Schul-Website: Manche
Themes bieten beim Bearbeiten der Seite rechts unter **Seite → Template** eine Vorlage wie
„Leere Seite“, „Ohne Kopf- und Fußzeile“ oder „Canvas“. Diese auswählen und den Code aus
Variante A einfügen. Die Höhe dann auf `height:100vh` setzen.

### Variante C: Nur verlinken (am einfachsten)

1. In einer Seite oder im Menü (**Design → Menüs** bzw. **Design → Editor → Navigation**)
   einen Link auf `https://www.meine-schule.de/bandlauf/` setzen.
2. Oder einen Block **„Buttons“** einfügen, Text „Bandlauf öffnen“, Link eintragen und
   „In neuem Tab öffnen“ aktivieren.

So läuft die App im ganzen Browserfenster. Mikrofon und Vollbild funktionieren dabei am
zuverlässigsten, besonders auf iPad und iPhone.

---

## Wenn etwas nicht klappt

| Problem | Lösung |
|---|---|
| Nach dem Speichern ist der iframe verschwunden | Dein Konto darf kein HTML einfügen. Ein Administrator muss den Block einfügen, oder nimm Variante C. |
| Das iframe bleibt leer oder zeigt „Verbindung abgelehnt“ | Adresse im `src` prüfen (mit `https://`). Ein Sicherheits-Plugin oder der Server verbietet evtl. das Einbetten (`X-Frame-Options: DENY`). Dann auf `SAMEORIGIN` stellen lassen oder Variante C nehmen. |
| Mikrofon geht im iframe nicht, im eigenen Tab schon | `allow="microphone; midi"` fehlt im iframe-Code, oder ein Sicherheits-Plugin bzw. der Server setzt den Header `Permissions-Policy: microphone=()`. Dort das Mikrofon für die eigene Domain erlauben. |
| Mikrofon geht gar nicht | Die Seite muss über `https://` laufen, und zwar die WordPress-Seite **und** die App-Adresse im iframe. |
| App sieht im iframe gequetscht aus | Höhe im `style` erhöhen (z. B. `height:95vh`), Block auf „Volle Breite“ stellen oder Variante B/C nehmen. |
| 404 unter `/bandlauf/` | Der Ordner liegt am falschen Ort. Er muss neben `wp-admin` und `wp-content` liegen, und die Datei darin muss genau `index.html` heißen. |
| Caching-Plugin zeigt alte Version | Nach einem App-Update den Cache des Plugins (z. B. WP Rocket, W3 Total Cache) leeren und die Seite im Browser neu laden. |

## Datenschutz-Hinweis für die Website

Die App lädt nichts von fremden Servern, setzt keine Cookies und sendet keine Daten.
Eigene Übungen speichert sie nur im Browser des Geräts. Das Mikrofon wird nur nach
Zustimmung im Browser benutzt und lokal ausgewertet. Für die Datenschutzerklärung reicht
in der Regel ein Satz wie:

> „Die Übe-App Bandlauf läuft vollständig in Ihrem Browser. Sie speichert Einstellungen
> nur lokal auf Ihrem Gerät und überträgt weder Audio noch andere Daten an unseren Server
> oder an Dritte.“
