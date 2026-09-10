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
