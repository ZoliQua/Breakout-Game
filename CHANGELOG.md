# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

## [Unreleased]

## [1.1.0] — 2026-11-09

### Added

- Three languages: English, Hungarian and German, switchable in the top bar / settings and by `?lang=en|hu|de`; the choice is shared across the arcade (`arcade.lang`).
- Dental mode (off by default, shared `arcade.dental`, `?dental=1`): the paddle is a toothbrush, the ball a toothpaste ball and the bricks tartar and plaque with distinct brush, paste and probe marks. Presentation only: rules, scores, stats and saves are unchanged.

### Changed

- Numbers use the locale's digit grouping.
- Links now point to https://dulzoltan.hu/arcade/ (the arcade moved to its new domain).

## [1.0.2] — 2026-11-04

### Changed

- Vendored kit (`src/arcade/`) updated: the registry knows the eleventh arcade game, Texas Hold'em, with its accent colour.
- README: the Zoltan's Arcade family table lists all eleven games.

## [1.0.1] — 2026-11-01

### Changed

- Vendored kit (`src/arcade/`) updated: the registry knows the tenth arcade game, Nut Sort, with its accent colour.
- README: the Zoltan's Arcade family table lists all ten games.

## [1.0.0] - 2026-10-27

### Added
- Breakout for Zoltan's Arcade: 480 × 360 logical playfield rendered as a scalable SVG, paddle with angle control
  (up to 60° off vertical depending on the hit point), 3 lives.
- Six hand-designed walls — Classic, Pyramid, Checkers, Fortress, Invader, Bulwark — looping with a faster ball each level.
- 1-, 2- and 3-hit bricks and indestructible steel bricks, coloured by row, with damage cracks.
- Power-ups dropped by broken bricks from a seeded RNG stored in the game state: Wide paddle, Multiball, Slow ball,
  Catch and Extra life; timed effects with a countdown in the side panel.
- Fixed-timestep (120 Hz) pure engine `step(state, dtMs, input)` with sub-stepped, axis-separated collision — no tunnelling
  through bricks or the paddle at any speed, correct side vs. top/bottom reflection.
- Scoring: row value × brick hardness × level, power-up catches and a level-clear bonus; best score and run stats through
  the shared arcade kit (`arcade.stats.breakout`).
- Keyboard (← → / A D), mouse and touch-drag steering; Space / click / tap to launch; P / Esc pause; R restart.
- Autosave & resume of the complete game state (`arcade.session.breakout`) with a strict validator, *Welcome back* overlay
  and Discard; automatic pause when the tab is hidden.
- Synthesised Web Audio sound effects with a remembered on/off toggle.
- `BreakoutDemo` attract mode: the real engine driven by an autopilot that predicts the ball's landing point, with a
  deterministic aim error so it occasionally misses; auto-relaunch and auto-restart.
- `embedded`, `onGameOver`, `onRunEnd` and `onStatusChange` props (shared `ArcadeGameProps` contract).
- Vitest + Testing Library test suite, `npm run check`, GitHub Actions CI, community health files.

### Notes
- Score-attack: the levels never run out, so every run ends with outcome `over`.
