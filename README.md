# WahrZentrale

Alle Wahr-Apps in **einer** Web-App (PWA) fürs iPhone – ein Design, ein
Standort, eine Sicherung, ein Impressum. Komplett lokal, ohne Konto, ohne
Werbung, ohne Tracking.

## Bereiche und Apps

| Bereich | Apps |
|---|---|
| Himmel | HimmelsWahr (`hw-`), Sternenhimmel (ZeitHimmel; `sternewahr-`, Symbol `zh-`), AstroWahr (`as-`), KompassWahr (`kw-`), HorizontWahr (`hz-`) |
| Unterwegs | NaviWahr (`wk-`), Keysglade (`kg-`), WowarWahr (`wow-`), BelegParkWahr (`bp-`) |
| Alltag | AlltagWahr (`aw-`), ProduktWahr (`pw-`), VorratsWahr (`vw-`), LosDenkWahr (`ld-`), QRWahr (`qr-`), LautstärkeWahr (`lw-`), MessWahr (`mw-`) |
| Spiel & Klang | ZahlenturmWahr (`zw-`), Korvanthiel (`kv-`), HerzKaroDrei (`hk-`), PartikelWahr (`pk-`), StrömungsWahr (`st-`), BeatWahr (`bw-`), KlangWahr (`kl-`) |

`index.html` ist die Startseite mit „Heute“, Suche über alle Apps und
Einträge sowie den vier Bereichen.

## Gemeinsamer Kern

| Datei | Zweck |
|---|---|
| `wz-theme.js` | ein Hell-/Dunkel-Schalter für alles (Start immer hell), kein Zwei-Finger-Zoom; Farbwelt: „Nachthimmel“ (Standard) oder eine eigene, in den Einstellungen mit zwei Reglern (Farbton, Farbstärke) gemischte Farbwelt für alle Apps (`wz_palette`, `wz_palette_custom`). Umgefärbt werden nur die violetten Töne der Oberfläche bei gleicher Helligkeit; die Werte werden beim Laden berechnet, es gibt keine eigene Farbdatei mehr |
| `wz-core.js` | ein Standort, eine Karten-Einwilligung, Heimknopf, Hinweis auf neue Version |
| `wz-unify.css` | ein Erscheinungsbild („Nachthimmel“-Palette) für alle Apps |
| `wz-common.css`, `wz-ui.js`, `wz-onboarding.js`, `wz-shared.js` | gemeinsame Bausteine |
| `wz-apps.js`, `wz-hub.js` | Verzeichnis der Apps, Startseite, Suche |
| `wz-sichern.html`, `wz-backup.js` | eine Sicherung aller Daten (ZIP) und Wiederherstellen |
| `wz-einstellungen.html` | Darstellung, Standort, Karten, Speicher, alles löschen |
| `wz-scan.html` | **ein** Scanner für alles (Knopf im Suchfeld): Barcode → ProduktWahr, QR-Code → QRWahr, Kassenbon/Parkschein → BelegParkWahr, Etikett → VorratsWahr, Zutaten → ProduktWahr. Erkennung lokal (ZXing, Tesseract); Übergabe über `sessionStorage` (`wz_scan_handoff`, wird von `wzScanHandoff()` in `wz-core.js` abgeholt und sofort gelöscht) |
| Schnelleingabe, „Hier merken“ | in `wz-hub.js`: Der Text im Suchfeld kann als Gedanke (LosDenkWahr), Beleg/Parkschein (BelegParkWahr), Vorrat (VorratsWahr) oder wiederkehrende Ausgabe (AlltagWahr) angelegt werden; „Hier merken“ legt den Standort als Wegpunkt in KompassWahr (`kw-waypoints`) oder als Ort in WowarWahr ab. Übergabe wie beim Scanner über `wz_scan_handoff` |
| Sonnenbahn | in `hw-kompass.html`: Kamera-Ansicht mit der heutigen Sonnenbahn als Messlinie (Gradnetz, Azimut-Skala, Stundenmarken, Aufgang, Höchststand, Untergang) samt Uhrzeit im Fadenkreuz |
| Poster | in `sternewahr-app.js`: Poster des Sternenhimmels zum Teilen (2160 px breit) oder für den Druck (3240 px breit), mit Titel und Widmung |
| Unterbrochene Aufzeichnung | in `wk-app.js`: NaviWahr legt den Stand einer laufenden Aufzeichnung alle 20 Sekunden, beim Pausieren, nach jeder Notiz und beim Wechsel in den Hintergrund in der Datenbank `naviwahr` (Speicher `laufend`) ab. Schließt das System die App, bietet NaviWahr beim nächsten Öffnen „Fortsetzen“, „Beenden und ansehen“ oder „Verwerfen“ an |
| Fotos in NaviWahr | Notiz-Fotos liegen in der Datenbank `naviwahr` (Speicher `fotos`), die Tour trägt nur die Kennung `photoId`. Ältere Touren mit eingebettetem Foto werden beim Öffnen von NaviWahr einmalig umgezogen; Fotos ohne Tour werden beim nächsten Start entfernt |
| Voller Speicher | `wz-core.js` zeigt in jeder App einen Hinweis, wenn der Browser das Speichern ablehnt; `wz-einstellungen.html` zeigt den Füllstand des kleinen gemeinsamen Speichers (`localStorage`) und die größten Verbraucher |
| Zeitgrenze | `wz-core.js` bricht Abfragen bei fremden Diensten nach 12 Sekunden ab (nicht bei eigenen Dateien und nicht bei Abfragen mit eigenem Abbruch); die App zeigt dann ihre gewohnte Meldung |
| `wz-xlsx.js` | eigener Excel-Export (ersetzt SheetJS vom CDN) |
| `sw.js` | **ein** Service Worker für alles (offline); angemeldet nur in `wz-core.js` |
| `wz-onboarding.js` | eine einheitliche Einführung für alle Apps |
| `404.html` | Hinweisseite für unbekannte Adressen (GitHub Pages) |
| Sperre für fremde Server | Jede Seite trägt im Kopf dieselbe `Content-Security-Policy`: Der Browser lässt nur die eigene Adresse und die in `datenschutz.html` genannten Dienste zu. Kommt ein neuer Dienst hinzu, muss er in dieser Zeile **aller** HTML-Seiten ergänzt werden – sonst blockt der Browser ihn. |
| HorizontWahr, MessWahr, KlangWahr | Kamera-, Kompass- und Klang-Werkzeuge (Stand 9. Oktober 2026), aufgebaut wie LautstärkeWahr: eine Seite je App (`hz-index.html`, `mw-index.html`, `kl-index.html`) mit eigenen Symbolen (`FINE` in `wz-apps.js`, App-Bilder `*-icon-192/512.png` im Stil ihres Bereichs). Lage und Kamera nach derselben Rechnung wie der Himmels-Kompass (Quaternion, Kompass nur als geglätteter Versatz). HorizontWahr lädt die Umgebung auf Tipp über Overpass (OpenStreetMap, ODbL) und Geländehöhen über Open-Meteo (beides schon zuvor erlaubt, keine neuen Dienste) und merkt sie in `horizontwahr_daten`; MessWahr (`messwahr_`) und KlangWahr (`klangwahr_`) arbeiten ohne Netz |
| `impressum.html`, `datenschutz.html`, `lizenzen.html` | Rechtliches für alle Apps |

Alle Bibliotheken liegen lokal bei (`lib-*`): Leaflet, jsPDF, die Texterkennung
(Tesseract-Kern mit deutschen und englischen Sprachdaten) und die Barcode-Erkennung
(ZXing). Die großen Scan-Dateien werden erst beim ersten Scannen geladen – von
derselben Adresse, nicht von fremden Servern.

## Installation auf dem iPhone

1. Die Adresse der WahrZentrale in **Safari** öffnen.
2. Teilen-Symbol → **„Zum Home-Bildschirm“**.
3. Nur dieses eine Symbol verwenden – alle Apps teilen sich dann Speicher,
   Standort, Design und Sicherung.

## Gestaltung

- Kopf der Startseite zeigt den Himmel der Tageszeit (Tag, Abendrot, Dämmerung,
  Nacht mit Sternen), Sonne bzw. Mond in der echten Phase und die nächste Sonnenzeit –
  alles lokal berechnet, ohne Netz. Das Wetter erscheint dort nur nach Freigabe.
- „Weitermachen“ (zuletzt geöffnete App, `wz_recent`) und eine Favoriten-Leiste mit
  bis zu vier Apps (`wz_home_favs`, Stern unter „Anordnen“); der fünfte Platz ist der Knopf „Hier merken“.
- Kleiner Sonnenbogen im Kopf (tagsüber, `skyMini` in `wz-hub.js`): Weg der Sonne von Aufgang bis
  Untergang über dem Horizont, gestrichelt darunter weiterlaufend, mit der Sonne an ihrer jetzigen
  Stelle; nachts der Mond in seiner Phase. Lokal berechnet.
- „Heute“, die Bereiche und „Werkzeuge“ teilen sich dieselbe Karte mit Emblem, Titel und Trennlinie;
  die Werkzeuge stehen als Symbolreihe wie die Favoriten.
- Bereiche lassen sich einklappen (Tipp auf den Kopf, `wz_home_closed`); eingeklappt bleibt eine Reihe
  der App-Symbole, die direkt zur App führen.
- Themenbereiche als elegante Karten: Emblem, Titel, Kurzbeschreibung und ein zartes
  Ornament je Bereich (nur Systemschrift, keine Serifen).
- App-Kacheln mit feinen, detaillierten Symbolen (`FINE` in `wz-apps.js`, eigene
  Zeichnungen), Name und Beschreibung; jeder Bereich hat eine eigene Abstufung derselben
  Palette (Himmel nachtblau, Unterwegs violett, Alltag lavendel, Spiel & Klang tiefe Nacht).
- Die App-Bilder (`*-icon-192/512.png`) zeigen dieselben feinen Symbole in der Abstufung
  ihres Bereichs; sie erscheinen in den App-Köpfen, auf den Startbildschirmen der Spiele und
  in der Begrüßung der Einführung („Willkommen bei …“, `wz-onboarding.js`).
- Die Startseite öffnet immer oben (auch beim Zurückkehren aus einer App mit Sprungmarke).
- Hauptknöpfe in allen Apps kräftig violett mit weißer Schrift (`--wz-primary-bg`
  in `wz-unify.css`), Nebenknöpfe bleiben blass.
- Aufbau der Startseite frei wählbar („Startseite anpassen“ bei den Werkzeugen und in den Einstellungen): Weitermachen, Favoriten, Heute, Apps und Werkzeuge lassen sich verschieben, die ersten drei auch ausblenden (`wz_home_blocks`). Im selben Blatt stehen die Reihenfolge der Apps (Eigene, A–Z, Zuletzt genutzt), „Apps anordnen“ sowie „Farben und Schrift“. Beim Anordnen erscheint über den Apps eine Zeile mit „Fertig“.
- Startseite anpassbar: Schriftgröße, Schriftfarbe, Farbstärke und Symbolfarbe
  („Farben und Schrift“ unter „Startseite anpassen“, `wz_home_style`); unter „Anordnen“ eigene Gruppen, Gruppe und
  Symbolfarbe je App (`wz_home_layout`). Alles lokal und in der Sicherung enthalten.

- Alle App-Symbole (`*-icon-192.png`, `*-icon-512.png`) sind aus denselben Liniengrafiken
  wie die Startseite erzeugt: weiße Linie auf dem Nachthimmel-Verlauf.
- Nur Systemschriften, keine Web-Schriften.
- Volle Bildschirmhöhe über `--wz-vh` (gemessen in `wz-theme.js`) statt `100vh`,
  damit auf dem iPhone unten keine Balken bleiben.

## Lizenz

Alle Rechte vorbehalten, siehe `LICENSE.txt` und `lizenzen.html`.
