# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Game

Open `index.html` directly in a browser. No build step or server required. `Khaminai-no-bg.png` must be in the same directory as `index.html` — the game will display a Hebrew error if the image fails to load.

## Architecture

The entire game is a single self-contained HTML file (`index.html`) with inline CSS and JavaScript. No external dependencies.

**Game states:** `LOADING → START → PLAYING → GAMEOVER / WIN`
State transitions are managed via `setState(s)` / `gameState` global.

**Core classes:**
- `Player` — movement, invincibility frames, lives
- `Enemy` / `EnemyGrid` — 8×4 grid with wave-based speed scaling; enemies shoot at random intervals
- `Bullet` — used for both player (cyan) and enemy (red) projectiles
- `Particle` — explosion effect spawned on enemy/player hit
- `Star` — parallax scrolling background

**Game loop:** `requestAnimationFrame` calls `gameLoop(timestamp)`, delta-time capped at 50ms. All game logic and rendering happen in one pass.

**Audio:** Web Audio API, synthesized procedurally (no audio files). Lazy-initialized on first sound call.

**Difficulty system:** Three presets in `DIFFICULTIES[]` controlling speed multiplier, shoot interval multiplier, lives, and max simultaneous player bullets.

**Localization:** All UI text is Hebrew (`lang="he" dir="rtl"`). Canvas text rendering uses `ctx.direction = 'rtl'`.
