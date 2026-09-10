# Griffwerk (guitar-song-coach)

Offline-fähiger Gitarren-Lerncoach als einzelne `index.html`. Kein Server,
kein Build-Schritt, alles in `localStorage`. Details/Song-JSON-Schema stehen
in `README.md` — das hier ist nur der Schnelleinstieg für zukünftige Sessions.

## Wichtiger Stolperstein

**Dieses Repo hat keinen `main`-Branch.** Der Default-Branch heißt
`claude/griffwerk-gitarrencoach` — dort direkt committen/pushen, nicht
versuchen auf `main` zu pushen (schlägt fehl: "src refspec main does not
match any").

## Architektur

- Songs = strukturierte Akkordfolge (`progression`) + Metadaten (`bpm`,
  `capo`, `timeSignature`, `tuning`, optional `notes`, `ampSettings`).
- `sanitizeSongData()` validiert/kappt jedes Feld beim Speichern (JSON-Import
  UND manuelle Eingabe laufen beide durch diese Funktion) — neue Song-Felder
  immer dort mit ergänzen, sonst werden sie beim Speichern verworfen.
- Noten-Rendering über VexFlow (CDN), Schwierigkeitsberechnung und
  Lernplan-Generierung laufen komplett client-seitig aus der `progression`.
- Song-Transkription läuft über den Chat: Nutzer bittet Claude, einen Song
  als Griffwerk-JSON zu transkribieren, fügt es über "JSON importieren" ein.

## Testen

Änderungen mit Playwright (headless Chromium,
`executablePath: '/opt/pw-browsers/chromium'`) smoke-testen — Achtung: CDN-
Ressourcen (Google Fonts, VexFlow) sind in manchen Sandbox-Umgebungen ohne
Internetzugang nicht ladbar; das ist kein echter Bug, nur eine
Sandbox-Einschränkung.

## Zusammenspiel mit claude.ai-Projects (Chat/Cowork)

Dieses Repo ist im claude.ai-Project "Kleine Anwendungen" als Kontext verknüpft.
Das Project liest dadurch automatisch den aktuellen Repo-Stand (Commits, Dateien,
auch diese Datei) — es gibt aber keinen Weg zurück: was im Project-Chat besprochen
wird, landet nicht automatisch hier.

Deshalb: tatsächliche Arbeitsanweisungen ("bau X", "ändere Y") nur hier im
Code-Bereich geben, nicht parallel im Project-Chat, damit nicht zwei Stellen
unabhängig voneinander am selben Code arbeiten. Wurde im Project-Chat trotzdem
etwas entschieden, das den Code betreffen soll, wird es zuerst hier (in dieser
Datei oder direkt in der nächsten Anweisung) festgehalten, bevor daran
gearbeitet wird.
