# Breakout Game — CLAUDE.md

## Project Facts

- **Repository**: https://github.com/ZoliQua/Breakout-Game (portal: https://drdul.hu/arcade/#/play/breakout)
- **Arcade game id**: `breakout` · component `BreakoutGame` · demo `BreakoutDemo`
- **Stack**: React 18, Vite 5, TypeScript 5, CSS Modules (+ the shared `src/arcade/arcade.css`)
- **Package manager**: npm
- **Node**: >=20
- **Scripts**:
  - `npm run dev` — local dev server
  - `npm run build` — `tsc -b` + production build
  - `npm run preview` — preview production build
  - `npm run typecheck` — `tsc -b`
  - `npm run lint` — ESLint
  - `npm test` — Vitest (logic + component tests, jsdom)
  - `npm run check` — lint → typecheck → test → build (same as CI)
- **Playfield**: 480 × 360 logical px (`FIELD_W`, `FIELD_H`), 12 brick columns × up to 9 rows, rendered as an SVG `viewBox`
- **Simulation**: fixed 120 Hz steps (`STEP_MS`) accumulated from `requestAnimationFrame`; frames clamped to 50 ms;
  ball movement sub-stepped to ≤ 2 px per sub-step
- **Controls**: ← → / A D, mouse / touch drag over the board; Space / Enter / click / tap launch; P / Esc pause; R restart.
  **T is reserved** for the theme toggle (handled by `ArcadeShell`) — never bind it.
- **Communication**: respond to the user in **Hungarian** in chat, even though this file and the codebase are in English

If any fact above disagrees with the real repository, verify first and flag the divergence to the user before editing code.

---

## Primary Goal

A correct, polished, type-safe Breakout that is easy to review, debug, and extend. Treat this as a maintainable project, not a one-shot script.

**Priority order**: correctness → truthfulness → verification → small diffs → maintainability → polish.

---

## Architecture

Logic lives outside presentation. Do not bury game rules inside JSX.

```
src/
  arcade/          # VENDORED shared kit from Arcade-Game-Center/packages/kit — never edit here
  logic/           # types, constants, rng, levels, game (createInitialState + step), autopilot, persist (+ *.test.ts)
  hooks/           # useFrameLoop (rAF), useBreakoutKeys (held keys + hotkeys), useSound (Web Audio)
  components/      # Playfield (SVG, full + mini; dental skin via useDental), PowerUpList, SoundToggle, DentalToggle, palette
  demo/            # BreakoutDemo — landing-page attract mode
  BreakoutGame.tsx # embeddable game, ArcadeGameProps
  App.tsx          # standalone page: <ArcadeShell gameId="breakout"><BreakoutGame/></ArcadeShell>
  main.tsx         # entry, imports arcade/arcade.css once
  index.ts         # package entry for Arcade-Game-Center
```

Stats, best score, session autosave, theme, score panel and overlays come from `src/arcade/`
(`useArcadeRun`, `useSessionAutosave`, `useRestoredSession`, `StatPanel`, `GameOverlay`, …).
Change the kit only in Arcade-Game-Center and sync it (`node scripts/sync-games.mjs kit`).
The same `src/` is mirrored into `Arcade-Game-Center/apps/breakout/` — keep them identical
(`node scripts/sync-games.mjs check` there).

The central pure function is `step(state, dtMs, input, events?) → GameState` in `logic/game.ts`.
It must stay deterministic: all randomness goes through `nextRandom(state.seed)` (`logic/rng.ts`).
`events` is an optional out-array used only for sound effects.

`BreakoutGame.tsx` keeps **one authoritative state ref** (`stateRef` + `commit`) shared by the frame loop and the input
handlers — do not reintroduce functional `setState` updaters next to the loop, or a pause pressed between two frames can be lost.

---

## Type Model (abridged — see `src/logic/types.ts`)

```ts
type GameStatus = 'idle' | 'running' | 'paused' | 'gameOver';   // = ArcadeStatus
type Brick = { col; row; hp; max: 1 | 2 | 3; steel: boolean };
type Ball = { x; y; vx; vy; speed; stuck; caught; offset; heldMs };
type Drop = { x; y; kind: 'wide' | 'multi' | 'slow' | 'life' | 'catch' };
type GameState = {
  status; level; lives; score;
  bricks: Brick[]; paddleX: number; balls: Ball[]; drops: Drop[];
  effects: { wideMs; slowMs; catchMs };
  seed: number;              // Mulberry32 state (uint32)
  banner: string; bannerMs: number;
};
type StepInput = { dir: -1 | 0 | 1; targetX: number | null; launch: boolean };
```

Serving, losing a ball and level transitions all happen inside `running` (the ball rests on the paddle). Bump
`SESSION_SCHEMA` in `BreakoutGame.tsx` whenever the persisted `GameState` shape changes, and update `isBreakoutState`.

---

## Scoring

- Brick broken: `ROW_POINTS[row] × max hits × level` (ROW_POINTS = 80, 70, 60, 50, 40, 30, 20, 10, 10); steel scores 0.
- Power-up caught: `50 × level`. Level cleared: `500 × level`.
- Score-attack: runs end only when the last life is lost → `useArcadeRun` outcome `'over'`.

---

## Working Rules

**Verify before and after every non-trivial change.** Before: read the target file and its neighbours, understand data flow and state shape. After: re-check imports, signatures, call sites, state transitions, and that no stale code remains.

**No hallucination.** Do not invent files, hooks, scripts, dependencies, or prior decisions. If something is not verified, say so — "I have not confirmed X", "I do not see evidence of Y". Never claim a build, lint, or test passed without running it.

**Small, local, reversible changes.** One fix or one feature at a time. Do not bundle refactor + feature + style. If the scope feels large, decompose and ask before proceeding.

**Preserve architecture.** Do not change Vite config, `tsconfig`, folder layout, styling approach, or rendering strategy without a stated reason and user approval.

**Explicitness over cleverness.** Small pure functions, explicit types, direct state transitions. Avoid clever one-liners and hidden side effects.

**No new dependencies** without justification. Default: React + Vite + TS only. No game engines, no physics libraries, no state libraries.

---

## Breakout-Specific Bug Checklist

Before concluding any Breakout task, confirm each item was checked — or explicitly state it was not:

- Ball movement stays sub-stepped (≤ `MAX_SUBSTEP_PX`) and resolves x and y separately — no tunnelling, correct side vs top/bottom reflection.
- Paddle bounce only from the top face with `vy > 0`; angle = `rel × MAX_BOUNCE_ANGLE`; flank contact never bounces the ball up.
- Flight angles stay at least 25° from horizontal (`normalizeVelocity`) so the ball cannot loop sideways forever.
- Steel bricks never break and never block a level clear (`isCleared` ignores them).
- All randomness uses the seeded RNG in state; `step` with the same input sequence is reproducible (there is a test).
- Losing the last ball resets balls, drops and effects; losing the last life ends the run exactly once.
- The frame loop only runs while `running`; dt is clamped; hidden tab pauses.
- Keyboard listeners are attached once per mount and cleaned up; every handler starts with `shouldIgnoreKey`.
- Held steering keys are released on window blur.
- The demo has no side effects: no listeners, no stats, no sessions, no sound, no localStorage writes.

---

## Implementation Protocol (short)

For every non-trivial task:

1. **Current state** — only what you verified from the repo.
2. **Goal** — the exact requested change.
3. **Plan** — the smallest safe path and which files you will touch.
4. **Edit** — only the scoped change.
5. **Verify** — run `npm run check` (or at least `npm run typecheck` and `npm test`); note any checks you skipped.
6. **Report** — see below.

---

## End-of-Task Report

End every meaningful change with:

- **Changed files** — each file with a one-line reason.
- **Verified** — which checks actually ran (typecheck, build, lint, tests, manual inspection).
- **Not verified** — explicit gaps.
- **Risks / follow-ups** — only real concerns, no filler.

---

**Guiding principle**: correctness over speed, verification over confidence, truth over fluency, small safe diffs over large rewrites. When in doubt — inspect more, assume less, change less, state uncertainty clearly.

---

## Git & Commit Rules

- Commit **only** as `Zoltán Dul <zoltan.dul@gmail.com>` (the global git identity).
- **Never** add `Co-Authored-By: Claude …`, "Generated with Claude Code" or any AI attribution
  to commit messages or pull-request descriptions.
- Conventional Commits (`feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:`).
