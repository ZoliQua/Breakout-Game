# 🏓 Breakout

[English](README.md) · **Deutsch** · [Magyar](README.hu.md)

Ein sauberes, typsicheres Breakout / Arkanoid-lite mit **React 18**, **Vite** und **TypeScript** — handgestaltete Wände, Steine mit mehreren Treffern und Stahl, Power-ups und Multiball, als gestochen scharfes, skalierbares SVG ohne Game-Engine — mit Tastatur, Maus und Touch.
Läuft für sich allein **und** als eines der fünfzehn Spiele von **[Zoltan's Arcade](https://github.com/ZoliQua/Arcade-Game-Center)** — [im Portal spielen](https://dulzoltan.hu/arcade/#/play/breakout).

[![CI](https://github.com/ZoliQua/Breakout-Game/actions/workflows/ci.yml/badge.svg)](https://github.com/ZoliQua/Breakout-Game/actions/workflows/ci.yml)
![license](https://img.shields.io/badge/license-MIT-green)
![node](https://img.shields.io/badge/node-%E2%89%A520-339933?logo=node.js&logoColor=white)
![react](https://img.shields.io/badge/React-18-61dafb?logo=react&logoColor=white)
![vite](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![typescript](https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript&logoColor=white)

---

## 📑 Inhaltsverzeichnis

- [Die Zoltan's-Arcade-Familie](#-die-zoltans-arcade-familie)
- [Funktionen](#-funktionen)
- [Schnellstart](#-schnellstart)
- [Steuerung](#%EF%B8%8F-steuerung)
- [Steine, Level & Power-ups](#-steine-level--power-ups)
- [Punkte & Statistik](#-punkte--statistik)
- [Autospeichern & Fortsetzen](#-autospeichern--fortsetzen)
- [Einbindung in Arcade-Game-Center](#-einbindung-in-arcade-game-center)
- [So funktioniert es](#-so-funktioniert-es)
- [Skripte](#-skripte)
- [Architektur](#%EF%B8%8F-architektur)
- [Mitmachen](#-mitmachen)
- [Lizenz](#%EF%B8%8F-lizenz)

## 🕹️ Die Zoltan's-Arcade-Familie

Jedes Spiel ist ein eigenständiges Repo, das sich auch ins Portal einklinkt. Alle teilen ein Designsystem, ein Punktemodell und ein Speicherformat über das mitgelieferte [`src/arcade/`](./src/arcade)-Kit.

| Spiel | Repo | Genre |
| --- | --- | --- |
| 🏠 **Arcade-Portal** | [Arcade-Game-Center](https://github.com/ZoliQua/Arcade-Game-Center) | Alle Spiele an einem Ort, Live-Demos, Statistik, Bestenliste |
| 🧱 Tetris | [Tetris-Game](https://github.com/ZoliQua/Tetris-Game) | Arcade |
| 🐍 Snake | [Snake-Game](https://github.com/ZoliQua/Snake-Game) | Reaktion |
| 🔢 2048 | [2048-Game](https://github.com/ZoliQua/2048-Game) | Puzzle |
| 🟡 Pac-Man | [Pac-Man-Game](https://github.com/ZoliQua/Pac-Man-Game) | Labyrinth |
| ⚫ Gomoku | [Gomoku-Game](https://github.com/ZoliQua/Gomoku-Game) | Strategie |
| 🦷 Tic-Tac-Toe | [Tic-Tac-Toe-Game](https://github.com/ZoliQua/Tic-Tac-Toe-Game) | Duell |
| 🏓 **Breakout** (dieses Repo) | [Breakout-Game](https://github.com/ZoliQua/Breakout-Game) | Schläger |
| 👾 Space Invaders | [Space-Invaders-Game](https://github.com/ZoliQua/Space-Invaders-Game) | Shooter |
| 🔴 Vier gewinnt | [Connect-Four-Game](https://github.com/ZoliQua/Connect-Four-Game) | Duell |
| 🔩 Nut Sort | [Nut-Sort-Game](https://github.com/ZoliQua/Nut-Sort-Game) | Sortieren |
| 🃏 Texas Hold'em | [Texas-Holdem-Game](https://github.com/ZoliQua/Texas-Holdem-Game) | Karten |
| 💣 Minesweeper | [Minesweeper-Game](https://github.com/ZoliQua/Minesweeper-Game) | Logik |
| ♠️ Solitaire | [Solitaire-Game](https://github.com/ZoliQua/Solitaire-Game) | Karten |
| 🎴 Ulti | [Ulti-Game](https://github.com/ZoliQua/Ulti-Game) | Karten |
| 🂡 Snapszer | [Snapszer-Game](https://github.com/ZoliQua/Snapszer-Game) | Karten |

## ✨ Funktionen

- 🧱 **Sechs handgestaltete Wände** (Klassisch, Pyramide, Schachbrett, Festung, Invader, Bollwerk), die endlos mit jedes Level schnellerem Ball wiederkehren
- 💪 **Steine mit 1, 2 und 3 Treffern** plus **unzerstörbarer Stahl**, nach Reihe eingefärbt, mit sichtbaren Rissen bei Schaden
- 🎯 **Schläger-Winkelsteuerung** — wo der Ball den Schläger trifft, bestimmt den Abprallwinkel (bis zu 60°)
- 🎁 **Fünf Power-ups** — breiter Schläger, Multiball, langsamer Ball, Klebeschläger und Extraleben; zeitlich begrenzte Effekte laufen im Seitenpanel herunter
- 🧮 **Robuste Physik** — Bewegung in Teilschritten, achsengetrennt: der Ball tunnelt nie durch einen Stein oder den Schläger, und Seitentreffer werden richtig reflektiert
- 🎲 **Deterministisch** — feste Simulation mit 120 Hz und ein im Spielzustand gespeicherter Seeded-RNG (gleicher Seed + gleiche Eingaben = gleiches Spiel)
- 🖱️ **Tastatur, Maus und Touch** — Pfeile / A D, Zeiger über das Spielfeld bewegen oder mit dem Finger ziehen
- 📖 **Regeldialog** mit Abprallwinkel-Diagramm (beim Öffnen pausiert das Spiel)
- 🏁 **Vorzeitig beenden** — ein Spiel bewusst beenden und den aktuellen Punktestand in die Bestenliste eintragen
- 📱 **Touch-Spiel** — irgendwo auf dem Spielfeld ziehen bewegt den Schläger, Tippen startet den Ball
- ⛶ **Vollbild**-Knopf (CSS-Vollbildmodus auf dem iPhone)
- 🔊 **Synthetisierte Soundeffekte** (Web Audio, in den Einstellungen schaltbar, wird gemerkt)
- 💾 **Autospeichern & Fortsetzen** — Tab mitten im Ballwechsel schließen und genau dort weitermachen; ein verborgener Tab pausiert das Spiel
- 🏆 **Einheitliche Arcade-Statistik** — Bestwert, Spiele, Gesamtzeit, geteilt mit dem Portal
- 🌗 **Dunkles / helles Design**, geteilt mit allen Arcade-Spielen (Taste **T**)
- 🌍 **Drei Sprachen** — Englisch, Ungarisch und Deutsch; Umschalten in der oberen Leiste oder in den Einstellungen (auch `?lang=en|hu|de` zur Link-Übergabe); Zahlen folgen der Gruppierung der Sprache
- 🦷 **Zahnmodus** — ein Schalter „Fogas mód“ / „Dental mode“ / „Zahnmodus“ in den Einstellungen, standardmäßig aus (oder `?dental=1`), arcadeweit geteilt: Der Schläger ist eine Zahnbürste, der Ball ein Zahnpasta-Ball und die Steine Zahnstein und Plaque mit unterschiedlichen Bürsten-, Pasta- und Sondenmarkierungen; nur Darstellung
- 🤖 **Attract-Mode-Demo** (`<BreakoutDemo />`) — die echte Engine mit Autopilot-Schläger, auf der Startseite des Portals
- 🧪 **Getestet** — Unit-Tests für Engine, Autopilot und Speicher-Validator sowie Komponententests mit Vitest + Testing Library

## 🚀 Schnellstart

Benötigt **Node ≥ 20** und npm.

```bash
git clone https://github.com/ZoliQua/Breakout-Game.git
cd Breakout-Game
npm install
npm run dev          # http://localhost:5173
```

Produktions-Build: `npm run build && npm run preview`. Der Build nutzt relative Asset-Pfade, `dist/` lässt sich also aus jedem Unterordner ausliefern.

## ⌨️ Steuerung

| Eingabe | Aktion |
| --- | --- |
| ⬅️ ➡️ / **A D** | Schläger bewegen |
| 🖱️ Maus / 👆 Fingerziehen über das Spielfeld | Der Schläger folgt dem Zeiger |
| **Leertaste** / **Enter** · Klick · Tipp | Start · Ball starten · fortsetzen |
| **P** / **Esc** | Pause / weiter |
| **R** | Neustart |
| **T** | Dunkel / hell umschalten (eigenständige Seite) |

**Auf dem Handy:** Ziehe einen Finger irgendwo über das Spielfeld, der Schläger folgt ihm; tippe auf das Spielfeld, um den Ball zu starten. Die Knöpfe **Regeln**, **Beenden** und **Vollbild** sind im Panel „Spiel“. Auf dem iPhone legst du die Seite auf den Home-Bildschirm, um sie ohne Browserleisten zu sehen.

Triffst du den Ball mit der **Mitte** des Schlägers, fliegt er fast senkrecht nach oben; die **Ränder** schicken ihn in einem Winkel von bis zu 60° weg.

## 🧱 Steine, Level & Power-ups

| Stein | Treffer | Hinweise |
| --- | --- | --- |
| Normal | 1 | Die Farbe zeigt die Reihe (oben rot → unten violett) |
| Gepanzert | 2 | Dünner weißer Rand; verblasst und reißt bei Treffern |
| Verstärkt | 3 | Gestrichelter weißer Rand |
| Stahl | ∞ | Grau; geht nie kaputt und zählt nicht zum Räumen der Wand |

Sind alle zerstörbaren Steine weg, gibt es den Level-Bonus und die nächste Wand kommt. Nach der sechsten Wand beginnt der Zyklus von vorn.
Die Grundgeschwindigkeit des Balls beträgt **230 px/s + 18 px/s pro Level** (maximal 410), und jeder Schlägertreffer erhöht sie um 1,5 % bis zum 1,25-Fachen der Level-Basis.

Jeder zerstörte Stein lässt mit **14 %** Wahrscheinlichkeit (Seeded-RNG) eine Kapsel fallen — fang sie mit dem Schläger:

| Kapsel | Power-up | Wirkung |
| --- | --- | --- |
| 🟦 **W** | Breiter Schläger | 68 → 104 px für 15 s |
| 🟪 **M** | Multiball | Jeder Ball im Spiel teilt sich in drei (max. 8) |
| 🟩 **S** | Langsamer Ball | Alle Bälle mit 70 % Tempo für 12 s |
| 🟧 **C** | Kleben | Der Schläger fängt den Ball für 15 s; starte ihn, oder er löst sich nach 2,5 s selbst |
| 🟥 **+** | Extraleben | +1 Leben (max. 6) |

Verlierst du den letzten Ball im Spiel, kostet das ein Leben; ein verlorener Ball oder eine neue Wand beendet alle aktiven Power-ups und fallenden Kapseln. Du startest mit **3 Leben**.

## 🎯 Punkte & Statistik

| Ereignis | Punkte |
| --- | --- |
| Stein in Reihe *r* zerstören (oben = 1) | Reihenwert × benötigte Treffer × Level |
| Power-up fangen | 50 × Level |
| Wand räumen | 500 × Level |

Reihenwerte von oben: **80, 70, 60, 50, 40, 30, 20, 10, 10**. Beispiel: Ein Stein mit 2 Treffern in der obersten Reihe auf Level 3 bringt 80 × 2 × 3 = **480**. Stahl bringt nichts.

Breakout ist ein Punktejagd-Spiel: Die Level gehen nie aus, ein Spiel endet also erst, wenn das letzte Leben weg ist (Ergebnis `over`), oder wenn du es mit **Beenden** abbrichst (Ergebnis `finished`, der aktuelle Punktestand wird gewertet).
Endet ein Spiel, hält das Kit es genau einmal in `localStorage` unter `arcade.stats.breakout` fest
(`best`, `plays`, `wins`, `totalScore`, `totalSeconds`, `lastScore`, `lastPlayedAt`).

## 💾 Autospeichern & Fortsetzen

Während ein Spiel läuft, wird der **vollständige** Spielzustand — Steine mit ihren verbleibenden Treffern, jeder Ball, Schläger, fallende Kapseln, Timer aktiver Power-ups, Level, Leben, Punkte und der RNG-Seed — etwa einmal pro Sekunde gespeichert, ebenso wenn die Seite verborgen oder das Spiel entfernt wird
(Schlüssel `arcade.session.breakout`, versioniert und beim Laden von `src/logic/persist.ts` streng validiert). Beim Neuladen erscheint **Willkommen zurück** mit dem pausierten Spiel.
**Verwerfen** neben dem Hinweis *Automatisch gespeichert* wirft den Spielstand weg. Beendete Spiele löschen ihren Spielstand automatisch. Die Soundeinstellung wird getrennt unter `breakout.sound` gespeichert.

## 🧩 Einbindung in Arcade-Game-Center

```tsx
import { BreakoutGame, BreakoutDemo } from '@arcade/breakout';

<BreakoutGame
  embedded                                   // Seitentitel ausblenden; das Portal zeigt ihn
  onGameOver={(score, info) => submitScore('breakout', score)}   // info.finished: mit „Beenden“ vorzeitig beendet
  onStatusChange={({ status, score, seconds }) => updateHeader(status, seconds)}
/>
<BreakoutDemo active={cardIsVisible} />       // Attract-Mode für die Startseite
```

Beide Komponenten folgen dem gemeinsamen Vertrag `ArcadeGameProps` / `ArcadeDemoProps` aus `src/arcade/types.ts`.

## 🧠 So funktioniert es

Zustandsautomat: `idle → running ↔ paused → gameOver → (Neustart) → running`. Aufschlag, Ballverlust und Level-Abschluss
passieren alle innerhalb von `running` (der Ball liegt einfach auf dem Schläger, bis er gestartet wird), sodass Spieluhr und Kit-Status einfach bleiben.

Eine einzige reine Funktion, `step(state, dtMs, input, events?)`, besitzt die Simulation:

1. Timer der Power-ups herunterzählen und den Schläger bewegen (Tastatur 470 px/s, Zeigerfolge auf 2400 px/s begrenzt).
2. Auf dem Schläger liegende Bälle fahren mit; ein Start (oder das Klebe-Timeout) schickt sie im Schlägerwinkel los.
3. Jeder freie Ball bewegt sich in **Teilschritten von höchstens 2 px** und wird **erst x, dann y** aufgelöst: Überlappt er beim Bewegen entlang x einen Stein, ist das ein Seitentreffer (`vx` reflektieren), entlang y ein Treffer von oben/unten (`vy` reflektieren). Steine sind 38 × 14 px, es kann also nichts übersprungen werden.
4. Ein nach unten fliegender Ball, der die Oberseite des Schlägers trifft, prallt mit `rel × 60°` von der Senkrechten ab; ein Ball unterhalb der Oberseite gleitet an der Flanke ab.
5. Zerstörte Steine können aus dem Seeded-Mulberry32-RNG (`state.seed`) eine Kapsel auslosen; Kapseln fallen und wirken bei Berührung.
6. Keine zerstörbaren Steine mehr → Level-Bonus und nächste Wand; keine Bälle mehr → ein Leben (oder das Spiel) ist weg.

Die Komponente führt `step` mit **festen 120 Hz** aus einem `requestAnimationFrame`-Akkumulator aus (Frames werden auf 50 ms begrenzt, ein Hänger lässt den Ball also nie springen) und hält eine maßgebliche Zustands-Ref, die sich mit den Eingabe-Handlern teilt, damit eine zwischen zwei Frames gedrückte Pause nie verloren geht. Das Spielfeld ist eine 480 × 360 große SVG-`viewBox`, die auf den Container skaliert wird; jede Farbe stammt aus den CSS-Tokens der Arcade, beide Designs funktionieren also, ohne Stile neu auszulesen.

Der **Demo-Autopilot** (`logic/autopilot.ts`) sagt voraus, wo der nächste fallende Ball die Schlägerlinie kreuzt (den Weg an den Seitenwänden gefaltet), und steuert mit Tastaturgeschwindigkeit dorthin. Sein Zielfehler wird aus dem Spiel-Seed gehasht, ist also deterministisch und trotzdem menschlich: meist nah an der Mitte, ab und zu ein glatter Fehlschlag — die Demo durchläuft Spielende und startet von selbst neu.

## 🧪 Skripte

| Skript | Zweck |
| --- | --- |
| `npm run dev` | Vite-Dev-Server |
| `npm run build` | Typprüfung (`tsc -b`) und Produktions-Build |
| `npm run preview` | Produktions-Build ausliefern |
| `npm run typecheck` | `tsc -b` |
| `npm run lint` | ESLint |
| `npm test` | Vitest (Logik- und Komponententests) |
| `npm run check` | lint → typecheck → test → build (was die CI ausführt) |

## 🏗️ Architektur

```
src/
  arcade/              mitgeliefertes Zoltan's-Arcade-Kit — hier nicht bearbeiten (siehe dessen README)
  logic/               reine Spielregeln (ohne React)
    types.ts constants.ts rng.ts
    levels.ts          die sechs Wandlayouts + buildBricks
    game.ts            createInitialState + step (Physik, Steine, Power-ups, Leben, Level)
    autopilot.ts       Landevorhersage + Demo-Treiber
    persist.ts         strenge Laufzeitprüfung gespeicherter Sitzungen
  hooks/               useFrameLoop (rAF), useBreakoutKeys, useSound (Web Audio)
  components/          Playfield (SVG, voll + mini), PowerUpList, SoundToggle, palette
  demo/BreakoutDemo.tsx  Attract-Mode für die Startseite
  rules.tsx            Inhalt des Regeldialogs und Abprall-Diagramm
  BreakoutGame.tsx     das einbettbare Spiel (ArcadeGameProps)
  App.tsx              eigenständige Seite: <ArcadeShell><BreakoutGame/></ArcadeShell>
  main.tsx             Einstieg; lädt die gemeinsame arcade.css einmal
  index.ts             Paketeinstieg für Arcade-Game-Center
```

## 🤝 Mitmachen

Siehe [CONTRIBUTING.md](./CONTRIBUTING.md), den [Verhaltenskodex](./CODE_OF_CONDUCT.md) und [SECURITY.md](./SECURITY.md).
Die Mitwirkenden stehen in [CONTRIBUTORS.md](./CONTRIBUTORS.md); Änderungen in [CHANGELOG.md](./CHANGELOG.md).

## ⚖️ Lizenz

[MIT](./LICENSE) © 2026 Zoltán Dul

---

<p align="center">Mit ❤️ gemacht von Zoltán Dul · Teil von <a href="https://github.com/ZoliQua/Arcade-Game-Center">Zoltan's Arcade</a></p>
