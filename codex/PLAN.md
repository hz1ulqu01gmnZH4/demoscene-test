# Synth Mirage Demo Plan

## Goals
- Deliver a self-contained demoscene experience in one HTML document with inline CSS/JS.
- Blend retro-futuristic visuals (warp tunnel, neon skyline, particle bloom) using `<canvas>` drawing only.
- Keep runtime deterministic: scenes advance via elapsed time rather than user input.

## Scene Timeline
1. **Intro (0–5 s)** – Gradient wash, typewriter title, RGB split flicker.
2. **Warp Tunnel (5–15 s)** – Radial streaks converging to center, accelerating color cycle.
3. **Skyline Reveal (15–30 s)** – Ground grid, sun halo, scrolling mountains, scanlines.
4. **Particle Bloom (30–45 s)** – Horizon emits particles synced to faux beat cues, glitch captions.
5. **Finale (45–60 s)** – Layer convergence, solar flare, tilt-up, fade to black with “RUN / RESET” prompt.

## Technical Architecture
- **HTML skeleton**: `<canvas id="demo">` + overlay `<div id="caption">`.
- **CSS**: palette variables, fullscreen layout, grain texture background, caption styling.
- **JS**:
  - Canvas/DPI setup and resize handling.
  - `SceneController` orchestrating timed segments (array of `{ duration, render }`).
  - Utility helpers: easing, lerp, HSV→RGB, pseudo-random/noise, text glitch/typewriter.
  - Scene renderers share mutable state (particles, mountains, tunnel streaks).

## Implementation Tasks
1. Create `index.html` with inline `<style>`/`<script>` blocks and base layout.
2. Build reusable drawing helpers (gradients, stars, grid generator, text renderer).
3. Implement each scene renderer sequentially, verifying transitions before moving on.
4. Add restart handler (`r` key) and ensure RAF loop respects paused/active state.
5. Manual browser test for sizing/performance; tweak constants for smooth flow.

