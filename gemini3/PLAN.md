# Project: "Neon Void" - Single File Demoscene Intro

## Goal
Create a compact, impressive, and procedural 3D audio-visual experience contained entirely within a single HTML file.

## Development Sequence

### 1. File Skeleton & Canvas Setup
- [ ] Create `demo.html`.
- [ ] Set up full-screen CSS (reset margins, black background, `overflow: hidden`).
- [ ] Initialize `<canvas>` element.
- [ ] Create a "Click to Start" overlay (required for Web Audio API autoplay policies).

### 2. WebGL Boilerplate (The Engine)
- [ ] Initialize WebGL2 context.
- [ ] Create shader compilation helper functions.
- [ ] Define a full-screen quad (2 triangles) to render the fragment shader across the entire view.
- [ ] Set up the render loop (`requestAnimationFrame`) and time uniform management.

### 3. Visual Implementation (The Shader)
- [ ] Write the GLSL Fragment Shader.
- [ ] **Technique:** Raymarching (Signed Distance Fields) for infinite procedural geometry.
- [ ] **Visuals:** A neon grid/tunnel or abstract void.
- [ ] **Lighting:** Glow effects, distance fog, and palette cycling.

### 4. Procedural Audio (The Music)
- [ ] Initialize `AudioContext`.
- [ ] Implement a `SoundGen` class using only oscillators, noise buffers, and filters (no external samples).
- [ ] **Instruments:**
    - Kick (Sine sweep)
    - Bass (Sawtooth + LowPass)
    - Hi-hat (Noise burst)
    - Lead/Arp (Sine/Square with delay)
- [ ] Implement a loop sequencer (120-130 BPM).

### 5. Audio-Visual Synchronization
- [ ] Link audio timing/envelopes to shader uniforms.
- [ ] Example: `u_kick` uniform pulses the tunnel geometry; `u_highs` flashes lights.

### 6. Final Polish
- [ ] Add a classic "scroller" text or overlay credits.
- [ ] Optimize performance (resolution scaling if needed).
- [ ] Minify code (optional).
