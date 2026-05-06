# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

Open `index.html` directly in any browser — no build step, no server needed:

```
open index.html
```

## Architecture

The entire game lives in a single `index.html` file with an inline `<script>` block. No dependencies, no bundler.

**Game loop:** `requestAnimationFrame` drives a fixed loop: `drawBackground → drawPipe → drawBird → drawUI → update`. Draw and update are intentionally separate — draw reads state, update mutates it.

**State machine:** `state` is one of `'waiting' | 'playing' | 'dead'`. All input (Space, click, touch) routes through `jump()`, which transitions state and applies an impulse to `bird.vy`.

**Physics:** Frame-based (not time-based). `GRAVITY` accumulates into `bird.vy` each frame; `PIPE_SPEED` is pixels per frame. Changing these constants directly affects difficulty.

**Pipe scoring:** Each pipe object gets a `scored` flag set when `p.x + PIPE_WIDTH < bird.x`, avoiding double-counting.

**Canvas dimensions:** `W = 400`, `H = 600` — referenced throughout for positioning and pipe gap bounds.

## Key constants (tuning difficulty)

| Constant | Default | Effect |
|---|---|---|
| `GRAVITY` | 0.5 | Fall acceleration per frame |
| `JUMP` | -9 | Upward impulse on flap |
| `PIPE_GAP` | 160 | Vertical opening between pipes |
| `PIPE_SPEED` | 3 | Horizontal scroll speed (px/frame) |
| `PIPE_INTERVAL` | 90 | Frames between pipe spawns |
