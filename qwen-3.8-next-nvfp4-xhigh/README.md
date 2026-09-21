# ZERO-POINT

A demoscene production in **one self-contained HTML file** — `zero-point.html`, ~164 KB,
no external assets, no libraries, no network. Everything you see and hear is generated
at runtime: WebGL2 shaders for the graphics, Web Audio for the music.

## Run it

Open `zero-point.html` in a browser (Chrome/Edge/Firefox) and press any key or click —
audio needs a user gesture. Full screen, sound up.

| key | action |
|-----|--------|
| space / enter | start (also unlocks audio) |
| P | pause / resume |
| ← / → | seek back / forward one bar |
| PgUp / PgDn | seek one section (8 bars) |
| Home / End / R | restart / last scene |
| 1 .. 9, 0 | jump straight to scene 1..10 |
| M | mute |
| H | HUD: fps, render resolution + scale, scene, bar, time |
| ↑ / ↓ | render scale down / up (pins it - `AUTO` becomes `LOCK`) |
| V | hand the render scale back to the adaptive resolver |
| F | fullscreen |

`?hud=1` in the URL starts with the HUD on. The show runs 3:23 and loops.

## The show

12 scenes, cut to a 132 BPM track in A minor. Titles appear as the scenes do:

```
BOOT  SIGNAL  LATTICE  CHROME  TUNNEL  PARTICLES
VECTOR  OVERLOAD  JULIA  CATHEDRAL  ASCENT  CREDITS
```

| scene | technique |
|-------|-----------|
| BOOT | CRT terminal, scanlines, rolling text, phosphor bloom |
| SIGNAL | plasma field + spectrum analyser, kaleidoscopic folds |
| LATTICE | raymarched SDF corridor, AO, speculars, volumetric streaks |
| CHROME | analytic chrome spheres, fresnel mirrors, procedural env |
| TUNNEL | tunnel SDF with twist, beat-driven fly-through |
| PARTICLES | 12k GPU point sprites, 4 formations morphing on the bar |
| VECTOR | vector wireframe polytope, 4D rotation, edge depth cueing |
| OVERLOAD | framebuffer feedback, block glitch, datamosh, RGB split |
| JULIA | distance-estimated filled Julia set, `c` walks the cardioid boundary |
| CATHEDRAL | kaleidoscopic/box-fold volumetric march + 4-cube wireframe (16V/32E) |
| ASCENT | volumetric starfield, nebula fbm, beat warp, dissolve to white |
| CREDITS | aurora curtains, sine-wave vector scroller, CRT collapse power-down |

Post pipeline: HDR (RGBA16F) scene target → bright-pass bloom (3 levels) → ACES-ish
tonemap → chromatic aberration, film grain, vignette, per-scene `look` values
(exposure, bloom, grain, aberration, saturation) interpolated across hard cuts.

## Build

```
node build.mjs zero-point.html      # concatenates src/*.js into the single file
```

Modules are plain IIFE-free script chunks evaluated in order:

```
src/00_util.js     math, SDF library, noise, GL helpers, shader error reporting
src/10_audio.js    the tracker: score (patterns, chords, arrangement, dynamic arc) + DSP voices
src/20_gl.js       GL state, programs, uniform auto-binding (incl. samplers), FBOs, point sprites
src/30_font.js     procedural 5x7 / mono / display glyph atlas + text mesh batcher
src/35_fxlib.js    shared fullscreen-quad harness, palette, fog/spec shading helpers
src/40_fx_a.js     BOOT  SIGNAL  LATTICE
src/41_fx_b.js     CHROME  TUNNEL  PARTICLES
src/42_fx_c.js     VECTOR  OVERLOAD  JULIA  CATHEDRAL  ASCENT  CREDITS
src/90_main.js     scene table, transport, transitions, HUD, resize + adaptive resolution
```

Rebuild after editing anything in `src/`.

### Adaptive resolution

The renderer measures frame time and walks the render scale (1.0 → 0.5) to hold frame
rate; ↑/↓ pins it. Under software rasterisation (SwiftShader, headless CI) it lands at
640×360 and still runs ~30 fps; on a real GPU it stays at native.

## Music

`src/10_audio.js` compiles the whole track into a sorted event list at load: 112 bars at
132 BPM, patterns written as 16-step token strings, chords named, arrangement as section
objects. Voices are synthesised live (kick with pitch-drop + click, 6-osc pad, 3-osc saw
lead with detune spread, acid bass, noise percussion, gated reverb, stereo delay, sidechain
ducking, bus compression into a soft-clip curve).

A per-bar **dynamic arc** scales every event velocity by section and instrument: thin intro,
a break that is 13 dB below the main section, a build, and a climax that carries the rest
of the show. Offline mix check: peak 0.82, no clipped samples.

## Dev harness (`dev/`)

All headless (Playwright + Chromium with `--enable-unsafe-swiftshader`), no display needed.

| script | what it tells you |
|--------|-------------------|
| `err.mjs` | boot state, shader compile errors, fps, renderer |
| `meter.mjs 118 124 ...` | raw HDR scene-target stats per time (mean/max/NaN/saturation) |
| `look.mjs out/*.png` | tonemapped PNG stats: mean, highlight %, shadow %, edge energy, histogram, ASCII map |
| `shot.mjs pre --w 960 --h 540 6 20 36` | screenshots at given times; `--fps` for a frame-time run |
| `shad.mjs julia 120 --re "old=>new"` | single-shader A/B patch loop with an ASCII preview |
| `mix.mjs` | offline render of the whole track: peak/RMS per 12 s and per section |
| `probe.html`, `uwarn.mjs`, `eval.mjs` | uniform warnings, arbitrary in-page evaluation |

## Notes

* Text is drawn from a procedurally rasterised glyph atlas (three faces: 5x7 bitmap, mono,
  display) — no font files, and the vector-ish glyphs get glow/wave/modulation effects.
* `D.setU` binds `WebGLTexture` values handed to `SAMPLER_2D` uniforms to auto-assigned
  texture units, which is how the feedback/glitch passes get their history buffer.
* Every shader is compiled through a path that reports the failing line with context, so a
  typo fails loudly at build time rather than rendering black.
* NaN guards (`finite3`) sit at every scene output: one bad pixel in an HDR target would
  otherwise smear black across the whole bloom chain.
