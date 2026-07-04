# Mitwirken an Sparblick

Danke, dass du beitragen möchtest! Dieses Dokument beschreibt, wie du das Projekt
aufsetzt, welche Konventionen gelten und wie du Änderungen einreichst.

## Grundprinzipien

- **Keine Laufzeit-Abhängigkeiten im Frontend.** Die App ist bewusst reines
  HTML/CSS/JS ohne Framework und ohne externe CDN-Ressourcen, damit sie sowohl im
  Browser als auch im Tauri-Fenster unverändert läuft. Bitte keine Bibliotheken
  einführen, ohne das vorher abzustimmen.
- **Lokal & offline.** Es werden keine Daten an Server gesendet. Alles bleibt im
  `localStorage` bzw. in vom Nutzer gewählten Dateien.
- **Deutschsprachige Oberfläche.** UI-Texte auf Deutsch halten.

## Entwicklungsumgebung

Voraussetzungen: [Node.js](https://nodejs.org/) (18+), [Rust](https://www.rust-lang.org/),
und die [Tauri-Systemabhängigkeiten](https://tauri.app/start/prerequisites/).

```bash
npm install        # einmalig
npm run serve      # Frontend im Browser testen (http://localhost:5173)
npm run dev        # App als Desktop-Fenster (Tauri, Entwicklungsmodus)
npm run build      # installierbares Programm bauen
```

## Projektstruktur

```
src/                Frontend (statisches HTML/CSS/JS, kein Build-Schritt)
  index.html
  styles.css
  app.js            gesamte App-Logik + lokale Speicherung
scripts/serve.js    Static-Server für den Browsertest
src-tauri/          Tauri-/Rust-Teil (Desktop-Fenster, Datei-Dialoge)
.github/workflows/  CI (Checks) und Release (Desktop-Builds)
```

## Branch-Konventionen

Entwickle nie direkt auf `main`. Erstelle einen beschreibenden Branch:

- `feature/<kurz-beschreibung>` – neue Funktionen
- `bugfix/<kurz-beschreibung>` – Fehlerbehebungen
- `docs/<kurz-beschreibung>` – nur Dokumentation
- `chore/<kurz-beschreibung>` – Aufräumen, Tooling, CI

## Commit-Nachrichten

- Kurze, aussagekräftige Betreffzeile (≤ 72 Zeichen), im Imperativ
  (z. B. „Add German/English language switch").
- Bei Bedarf ein Fließtext, der das *Warum* erklärt.
- Ein Commit = eine logische Änderung.

## Code-Stil

- 2 Leerzeichen Einrückung in JS/CSS/HTML.
- Bestehende Struktur in `app.js` beibehalten (Render-Funktionen pro Ansicht,
  gemeinsame UI-Bausteine, State oben im Modul).
- Rust-Code mit `cargo fmt` formatieren (wird in der CI geprüft).
- Keine Inline-`onclick`-Attribute im HTML (Content-Security-Policy); Ereignisse
  über `addEventListener` bzw. die `el()`-Hilfsfunktion registrieren.

## Vor dem Pull Request

Bitte lokal prüfen:

```bash
node --check src/app.js
node --check scripts/serve.js
( cd src-tauri && cargo fmt --check )
```

Und die Änderung im Browser testen (`npm run serve`). Bei UI-Änderungen hilft ein
Screenshot in der PR-Beschreibung.

## Pull Requests

- Klein und fokussiert halten – lieber mehrere kleine PRs als einen großen.
- Beschreibe, *was* und *warum* sich ändert.
- Die CI (`.github/workflows/ci.yml`) muss grün sein.
- Keine Zugangsdaten, Tokens oder persönlichen Daten committen.

## Release

Desktop-Builds für macOS, Windows und Linux entstehen über
`.github/workflows/release.yml` – ausgelöst durch einen Tag `v*`
(z. B. `git tag v0.2.0 && git push --tags`) oder manuell über „Run workflow".
