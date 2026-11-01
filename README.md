# 🏓 Breakout

A polished, type-safe Breakout / Arkanoid-lite built with **React 18**, **Vite** and **TypeScript** — hand-designed walls, multi-hit and steel bricks, power-ups and multiball, rendered as a crisp scalable SVG with no game engine.
Runs on its own **and** as one of the games of **[Zoltan's Arcade](https://github.com/ZoliQua/Arcade-Game-Center)** — [play it in the portal](https://drdul.hu/arcade/#/play/breakout).

[![CI](https://github.com/ZoliQua/Breakout-Game/actions/workflows/ci.yml/badge.svg)](https://github.com/ZoliQua/Breakout-Game/actions/workflows/ci.yml)
![license](https://img.shields.io/badge/license-MIT-green)
![node](https://img.shields.io/badge/node-%E2%89%A520-339933?logo=node.js&logoColor=white)
![react](https://img.shields.io/badge/React-18-61dafb?logo=react&logoColor=white)
![vite](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![typescript](https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript&logoColor=white)

---

## 📑 Table of contents

- [The Zoltan's Arcade family](#-the-zoltans-arcade-family)
- [Features](#-features)
- [Quick start](#-quick-start)
- [Controls](#%EF%B8%8F-controls)
- [Bricks, levels & power-ups](#-bricks-levels--power-ups)
- [Scoring & stats](#-scoring--stats)
- [Autosave & resume](#-autosave--resume)
- [Embedding in Arcade-Game-Center](#-embedding-in-arcade-game-center)
- [How it works](#-how-it-works)
- [Scripts](#-scripts)
- [Architecture](#%EF%B8%8F-architecture)
- [Contributing](#-contributing)
- [License](#%EF%B8%8F-license)

## 🕹️ The Zoltan's Arcade family

Every game is a standalone repo that also plugs into the portal. They share one design system, one scoring model and one save format through the vendored [`src/arcade/`](./src/arcade) kit.

| Game | Repo | Genre |
| --- | --- | --- |
| 🏠 **Arcade portal** | [Arcade-Game-Center](https://github.com/ZoliQua/Arcade-Game-Center) | All games in one place, live demos, stats, leaderboard |
| 🧱 Tetris | [Tetris-Game](https://github.com/ZoliQua/Tetris-Game) | Arcade |
| 🐍 Snake | [Snake-Game](https://github.com/ZoliQua/Snake-Game) | Reflex |
| 🔢 2048 | [2048-Game](https://github.com/ZoliQua/2048-Game) | Puzzle |
| 🟡 Pac-Man | [Pac-Man-Game](https://github.com/ZoliQua/Pac-Man-Game) | Maze |
| ⚫ Gomoku | [Gomoku-Game](https://github.com/ZoliQua/Gomoku-Game) | Strategy |
| 🦷 Tic-Tac-Toe | [Tic-Tac-Toe-Game](https://github.com/ZoliQua/Tic-Tac-Toe-Game) | Duel |
| 🏓 **Breakout** (this repo) | [Breakout-Game](https://github.com/ZoliQua/Breakout-Game) | Paddle |
| 👾 Space Invaders | [Space-Invaders-Game](https://github.com/ZoliQua/Space-Invaders-Game) | Shooter |
| 🔴 Connect Four | [Connect-Four-Game](https://github.com/ZoliQua/Connect-Four-Game) | Duel |
| 🔩 Nut Sort | [Nut-Sort-Game](https://github.com/ZoliQua/Nut-Sort-Game) | Sorting |

## ✨ Features

- 🧱 **Six hand-designed walls** (Classic, Pyramid, Checkers, Fortress, Invader, Bulwark) that loop forever with a faster ball each level
- 💪 **1-, 2- and 3-hit bricks** plus **indestructible steel**, coloured by row, with visible cracks when damaged
- 🎯 **Paddle-angle control** — where the ball meets the paddle sets the bounce angle (up to 60°)
- 🎁 **Five power-ups** — Wide paddle, Multiball, Slow ball, Catch and Extra life; timed effects count down in the side panel
- 🧮 **Robust physics** — sub-stepped, axis-separated movement: the ball never tunnels through a brick or the paddle and side hits reflect correctly
- 🎲 **Deterministic** — a fixed 120 Hz simulation with a seeded RNG stored in the game state (same seed + same input = same game)
- 🖱️ **Keyboard, mouse and touch** — arrows / A D, move the pointer over the board, or drag with a finger
- 🔊 **Synthesised sound effects** (Web Audio, toggle in Settings, remembered)
- 💾 **Autosave & resume** — close the tab mid-rally and pick up exactly where you left off; the tab going hidden pauses the game
- 🏆 **Unified arcade stats** — best score, plays, total time, shared with the portal
- 🌗 **Dark / light theme** shared with every arcade game (press **T**)
- 🤖 **Attract-mode demo** (`<BreakoutDemo />`) — the real engine with an autopilot paddle, shown on the portal's landing page
- 🧪 **Tested** — engine, autopilot and save-validator unit tests plus component tests with Vitest + Testing Library

## 🚀 Quick start

Requires **Node ≥ 20** and npm.

```bash
git clone https://github.com/ZoliQua/Breakout-Game.git
cd Breakout-Game
npm install
npm run dev          # http://localhost:5173
```

Production build: `npm run build && npm run preview`. The build uses relative asset paths, so `dist/` can be served from any sub-folder.

## ⌨️ Controls

| Input | Action |
| --- | --- |
| ⬅️ ➡️ / **A D** | Move the paddle |
| 🖱️ Mouse / 👆 touch drag over the board | Paddle follows the pointer |
| **Space** / **Enter** · click · tap | Start · launch the ball · resume |
| **P** / **Esc** | Pause / resume |
| **R** | Restart |
| **T** | Toggle dark / light theme (standalone page) |

Hitting the ball with the paddle's **centre** sends it almost straight up; the **edges** send it away at up to 60°.

## 🧱 Bricks, levels & power-ups

| Brick | Hits | Notes |
| --- | --- | --- |
| Plain | 1 | Colour shows the row (red at the top → violet at the bottom) |
| Armoured | 2 | Thin white rim; fades and cracks when hit |
| Reinforced | 3 | Dashed white rim |
| Steel | ∞ | Grey; never breaks and does not count towards clearing the wall |

Clearing every breakable brick awards the level bonus and serves the next wall. After the sixth wall the cycle starts again.
The ball's base speed is **230 px/s + 18 px/s per level** (capped at 410), and every paddle hit adds 1.5 % up to 1.25 × the level's base.

Every broken brick has a **14 %** chance (seeded RNG) to drop a capsule — catch it with the paddle:

| Capsule | Power-up | Effect |
| --- | --- | --- |
| 🟦 **W** | Wide paddle | 68 → 104 px for 15 s |
| 🟪 **M** | Multiball | Every ball in play splits into three (max 8) |
| 🟩 **S** | Slow ball | All balls at 70 % speed for 12 s |
| 🟧 **C** | Catch | The paddle catches the ball for 15 s; launch it, or it releases itself after 2.5 s |
| 🟥 **+** | Extra life | +1 life (max 6) |

Losing the last ball in play costs a life; a lost ball or a new wall clears all active power-ups and falling capsules. You start with **3 lives**.

## 🎯 Scoring & stats

| Event | Points |
| --- | --- |
| Break a brick in row *r* (top = 1) | row value × hits it took × level |
| Catch a power-up | 50 × level |
| Clear the wall | 500 × level |

Row values from the top: **80, 70, 60, 50, 40, 30, 20, 10, 10**. Example: a 2-hit brick in the top row on level 3 scores 80 × 2 × 3 = **480**. Steel scores nothing.

Breakout is score-attack: the levels never run out, so a run ends only when the last life is lost (outcome `over`).
When a run ends the kit records it once in `localStorage` under `arcade.stats.breakout`
(`best`, `plays`, `wins`, `totalScore`, `totalSeconds`, `lastScore`, `lastPlayedAt`).

## 💾 Autosave & resume

While a run is in progress the **complete** game state — bricks with their remaining hits, every ball, paddle, falling capsules, active power-up timers, level, lives, score and the RNG seed — is saved about once a second and whenever the page is hidden or the game is unmounted
(key `arcade.session.breakout`, schema-versioned and strictly validated on load by `src/logic/persist.ts`). Reloading shows **Welcome back** with the restored run paused.
**Discard** next to the *Autosaved* badge throws the save away. Finished runs clear their save automatically. The sound preference is stored separately under `breakout.sound`.

## 🧩 Embedding in Arcade-Game-Center

```tsx
import { BreakoutGame, BreakoutDemo } from '@arcade/breakout';

<BreakoutGame
  embedded                                   // hide the page title; the portal shows it
  onGameOver={(score) => submitScore('breakout', score)}
  onStatusChange={({ status, score, seconds }) => updateHeader(status, seconds)}
/>
<BreakoutDemo active={cardIsVisible} />       // landing-page attract mode
```

Both components follow the shared `ArcadeGameProps` / `ArcadeDemoProps` contract from `src/arcade/types.ts`.

## 🧠 How it works

State machine: `idle → running ↔ paused → gameOver → (restart) → running`. Serving, losing a ball and clearing a level all
happen inside `running` (the ball simply rests on the paddle until it is launched), so the run timer and the kit's status stay simple.

A single pure function, `step(state, dtMs, input, events?)`, owns the simulation:

1. Count down the power-up timers and move the paddle (keyboard speed 470 px/s, pointer follow capped at 2400 px/s).
2. Balls resting on the paddle ride along; a launch (or the Catch timeout) sends them off at the paddle angle.
3. Each free ball moves in **sub-steps of at most 2 px**, resolving **x then y**: overlapping a brick while moving along x is a
   side hit (reflect `vx`), along y a top/bottom hit (reflect `vy`). Bricks are 38 × 14 px, so nothing can be skipped.
4. A downward ball meeting the paddle's top face bounces at `rel × 60°` from vertical; a ball already below the top face slides off the flank.
5. Broken bricks may roll a capsule from the seeded Mulberry32 RNG (`state.seed`); capsules fall and apply on contact.
6. No breakable bricks left → level bonus and next wall; no balls left → lose a life (or the run).

The component runs `step` at a **fixed 120 Hz** from a `requestAnimationFrame` accumulator (frames are clamped to 50 ms, so a stall
never makes the ball jump), and keeps one authoritative state ref shared with the input handlers so a pause pressed between two
frames is never lost. The playfield is a 480 × 360 SVG `viewBox` scaled to the container; every colour comes from the arcade's
CSS tokens, so both themes work without re-reading styles.

The **demo autopilot** (`logic/autopilot.ts`) predicts where the next falling ball will cross the paddle line (folding the path off the side
walls) and steers there with the keyboard speed. Its aim error is hashed from the game seed, so it is deterministic yet human: usually close
to the centre, now and then a clean miss — the demo loops through game over and restarts by itself.

## 🧪 Scripts

| Script | Purpose |
| --- | --- |
| `npm run dev` | Vite dev server |
| `npm run build` | Type-check (`tsc -b`) and production build |
| `npm run preview` | Serve the production build |
| `npm run typecheck` | `tsc -b` |
| `npm run lint` | ESLint |
| `npm test` | Vitest (logic + component tests) |
| `npm run check` | lint → typecheck → test → build (what CI runs) |

## 🏗️ Architecture

```
src/
  arcade/              vendored Zoltan's Arcade kit — do not edit here (see its README)
  logic/               pure game rules (no React)
    types.ts constants.ts rng.ts
    levels.ts          the six wall layouts + buildBricks
    game.ts            createInitialState + step (physics, bricks, power-ups, lives, levels)
    autopilot.ts       landing prediction + demo driver
    persist.ts         strict runtime validation of saved sessions
  hooks/               useFrameLoop (rAF), useBreakoutKeys, useSound (Web Audio)
  components/          Playfield (SVG, full + mini), PowerUpList, SoundToggle, palette
  demo/BreakoutDemo.tsx  landing-page attract mode
  BreakoutGame.tsx     the embeddable game (ArcadeGameProps)
  App.tsx              standalone page: <ArcadeShell><BreakoutGame/></ArcadeShell>
  main.tsx             entry; loads the shared arcade.css once
  index.ts             package entry used by Arcade-Game-Center
```

## 🤝 Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md), the [Code of Conduct](./CODE_OF_CONDUCT.md) and [SECURITY.md](./SECURITY.md).
Contributors are listed in [CONTRIBUTORS.md](./CONTRIBUTORS.md); changes in [CHANGELOG.md](./CHANGELOG.md).

## ⚖️ License

[MIT](./LICENSE) © 2026 Zoltán Dul

---

<p align="center">Made with ❤️ by Zoltán Dul · part of <a href="https://github.com/ZoliQua/Arcade-Game-Center">Zoltan's Arcade</a></p>
