# Griffwerk – Gitarrencoach

Eine einzelne HTML-Datei (`index.html`) als leichte, offline-fähige Gitarren-Lern-App im Browser. Kein Server, kein Build-Schritt – alle Songs und dein Übungsfortschritt werden lokal im `localStorage` des Browsers gespeichert. Im selben Stil wie [Zeiterfassung](https://github.com/luizz28-goat/zeiterfassung).

## Funktionen

- **Song-Transkription strukturiert erfassen**: Akkordfolge mit Abschnitten (Strophe, Refrain, …), Tempo, Taktart, Capo und Stimmung
- **Griffbild** für jeden verwendeten Akkord/Ton: obere Reihe = Saiten von der dicksten (E, links) zur dünnsten (e, rechts), untere Reihe **x** = nicht spielen bzw. **Zahl** = zu greifender Bund
- **Normale Notenschrift** (Notenlinien) über [VexFlow](https://www.vexflow.com/), automatisch aus der Akkord-/Tonfolge erzeugt
- **Automatische Schwierigkeitsbewertung 0–100** anhand von sieben nachvollziehbaren Faktoren (Akkordvielfalt, Wechseltempo, Barré-/Stretch-Anteil, Techniken, Rhythmus, BPM, Songlänge) – Formel ist in der App unter "Wie wird die Schwierigkeit berechnet?" einsehbar
- **Automatisch generierter Lernplan**: neue Akkorde einzeln lernen → wichtigste Wechsel im Metronomtempo üben → Abschnitte einzeln üben → ganzer Song → Feinschliff, verteilt auf mehrere Übungstage je nach Schwierigkeit
- **Übungsmodus**: Metronom (Web Audio, präziser Lookahead-Scheduler) und Übungs-Stoppuhr je Song/Tag, Fortschritt wird gespeichert
- **JSON-Import/-Export** der eigenen Songs zum Sichern/Teilen

## Wie kommt ein Song in die App?

Eine automatische Erkennung "Song rein, Griffe raus" per Audioanalyse ist mit einer reinen Offline-Browser-App (ohne Server/ML-Backend) nicht sauber umsetzbar – das bleibt Zukunftsmusik (siehe Roadmap). Aktuell gibt es zwei Wege:

1. **Per Chat transkribieren lassen**: Claude im Chat bitten, einen Song als Griffwerk-JSON zu transkribieren (Schema siehe unten) und das Ergebnis im Dialog "Neuer Song" → "JSON importieren" einfügen. Der Button "Beispiel-Schema laden" zeigt ein vollständiges Beispiel.
2. **Manuell anlegen**: Im Dialog "Neuer Song" → "Manuell anlegen" Akkord für Akkord aus der eingebauten Akkordbibliothek oder mit eigenen Bünden zusammenstellen.

### Song-JSON-Schema

```json
{
  "title": "Songtitel",
  "artist": "Interpret",
  "bpm": 96,
  "capo": 0,
  "timeSignature": 4,
  "tuning": "standard",
  "notes": "Freier Hinweistext, optional",
  "progression": [
    {
      "section": "Strophe",
      "chord": "Am",
      "shape": ["x", 0, 2, 2, 1, 0],
      "beats": 4,
      "technique": "strum"
    }
  ]
}
```

- `shape`: genau 6 Werte für die Saiten **tief → hoch** (E A D G B e). `"x"` = nicht spielen, `0` = leer, Zahl = Bundnummer (0–24).
- `beats`: Länge in Viertelschlägen – erlaubt sind `4` (ganze), `2` (halbe), `1` (viertel), `0.5` (achtel), `0.25` (sechzehntel).
- `technique`: eine von `strum`, `zupfen`, `einzelton`, `hammer-on`, `pull-off`, `slide`, `bend`, `palm-mute`.
- `tuning`: `"standard"` (E A D G B E) oder `"dropD"`.
- Für Melodien/Riffs: einfach pro Ton einen Eintrag mit nur einer belegten Saite in `shape` anlegen.

Alle importierten Daten werden beim Speichern validiert (Zahlenbereiche, erlaubte Werte) – ungültige oder unerwartete Felder werden abgewiesen bzw. auf sichere Standardwerte zurückgesetzt.

## Nutzung

1. `index.html` im Browser öffnen (lokal per Doppelklick oder über GitHub Pages gehostet).
2. Auf dem Handy: Seite öffnen und über "Zum Home-Bildschirm hinzufügen" wie eine App installieren.
3. Song auswählen oder neu anlegen, Griffbilder & Noten ansehen, Lernplan abarbeiten.

## Daten und Datenschutz

Alle Daten (Songs, Lernfortschritt, Übungszeiten) liegen ausschließlich im `localStorage` des jeweiligen Geräts/Browsers. Es gibt keinen eigenen Server. Externe Verbindungen bestehen nur zu den fest eingebundenen CDN-Bibliotheken (Google Fonts, VexFlow für den Notensatz) – es werden keine Songdaten an Dritte gesendet.

## Enthaltene Demo-Songs

- **Amazing Grace** – traditionelles, gemeinfreies Kirchenlied, eigene vereinfachte Begleitung zum Einstieg in Akkordwechsel.
- **Aufwärm-Riff in E** – eine selbst komponierte kurze Übung für Einzeltöne, Hammer-Ons und Palm Mute.

## Roadmap

- Automatische Song-Erkennung und Transkription aus einer Audiodatei (benötigt ein separates Analyse-Backend, z. B. für Beat-/Akkord-/Tonhöhenerkennung – aktuell bewusst nicht Teil dieser reinen Offline-App)
- Alternative Stimmungen (Open Tunings) als feste Presets
- Beat-übergreifende Notengruppierung (Balken über Taktgrenzen, Punktierungen)

## Technik

- Reines HTML/CSS/JavaScript, keine Build-Abhängigkeiten.
- [VexFlow](https://cdnjs.cloudflare.com/ajax/libs/vexflow/4.2.3/vexflow-min.js) (CDN) für den Notensatz.
- Kein Build-Prozess, keine Installation nötig.
