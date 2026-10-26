# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Pr
Classic Asteroids clone in plain HTML5 Canvas + vanilla JS (ES6+). No dependencies, no bundler, no build step, no tests, no linter. UI text and code comments are in Spanish — keep that convention.

## Running

Open `index.html` directly in a browser, or serve the folder:

```bash
npx serve .   # then visit http://localhost:3000
```

Deployed demo: https://klerith.github.io/claude-asteroids/ (GitHub Pages, served as static files).

## Architecture

All game logic lives in `game.js`, loaded by `index.html` as a classic (non-module) script with globals. `W`/`H` (800×600) are hardcoded in `game.js` and must match the `<canvas>` size in `index.html`.

- **Entities** — `Ship`, `Asteroid`, `Bullet`, `Particle` classes, each with `update(dt)` and `draw()`. They draw through the global `ctx`. Removal is done by setting `dead = true` and filtering the arrays afterwards, not by splicing during iteration.
- **Toroidal space** — positions wrap via `wrap()`. Particles are the exception (they don't wrap).
- **Asteroid sizes** — `size` is 3 (large) → 2 → 1 (small). `RADII`, `SPEEDS`, `POINTS` are arrays indexed by size (index 0 unused). `split()` returns two asteroids of `size - 1`.
- **Game state** — module-level globals (`ship`, `bullets`, `asteroids`, `particles`, `score`, `lives`, `level`, `state`). `state` is `'playing' | 'dead' | 'gameover'`; `update()` branches on it. `initGame()` resets everything; `nextLevel()` runs when no asteroids remain and spawns `3 + level` large asteroids away from the center.
- **Loop** — `requestAnimationFrame` → `update(dt)` then `draw()`. `dt` is in seconds and capped at 0.05, so all speeds are in px/s.
- **Input** — `keys[code]` holds held keys (rotate/thrust); `pressed(code)` is a one-shot edge trigger (consumes `justPressed`), used for shooting and restarting.

## Notes

- The README still mentions power-ups and a "shooting star" asteroid; those were removed from the code (commit `13e713f`). Don't assume they exist.


## Testing:
Vitest = usar la skill y la informacion de: @