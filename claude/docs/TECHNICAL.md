# Technical Documentation - Demoscene Application

## Architecture Overview

This document provides detailed technical information about the implementation of the demoscene application.

## Core Systems

### 1. State Management

```javascript
const state = {
  canvas: null,           // Canvas element reference
  ctx: null,              // 2D rendering context
  width: 0,               // Canvas width
  height: 0,              // Canvas height
  time: 0,                // Current timestamp (ms)
  deltaTime: 0,           // Frame delta time (ms)
  lastTime: 0,            // Previous frame timestamp
  fps: 0,                 // Current FPS
  fpsCounter: 0,          // Frame counter for FPS calculation
  fpsLastUpdate: 0,       // Last FPS update timestamp
  running: false,         // Animation loop active
  paused: false,          // Pause state
  audioContext: null,     // Web Audio context
  audioStarted: false,    // Audio playback state
  currentScene: 0,        // Current scene index
  sceneStartTime: 0,      // Scene start timestamp
  sceneProgress: 0        // Scene progress (0-1)
};
```

### 2. Animation Loop

The main loop uses `requestAnimationFrame` for smooth 60fps animation:

```javascript
function loop(timestamp) {
  // Calculate delta time for frame-rate independent movement
  state.deltaTime = timestamp - state.lastTime;
  state.lastTime = timestamp;
  state.time = timestamp;

  // Update FPS counter
  state.fpsCounter++;
  if (timestamp - state.fpsLastUpdate > 1000) {
    state.fps = state.fpsCounter;
    state.fpsCounter = 0;
    state.fpsLastUpdate = timestamp;
  }

  // Update game state
  update(state.deltaTime);

  // Render frame
  render();

  // Request next frame
  requestAnimationFrame(loop);
}
```

**Key Features:**
- Frame-rate independent timing using delta time
- FPS monitoring for performance tracking
- Separate update/render phases for clean architecture

### 3. Utility Functions

#### Interpolation Functions

```javascript
// Linear interpolation
const lerp = (a, b, t) => a + (b - a) * t;

// Smooth interpolation (ease in/out)
const smoothstep = (t) => t * t * (3 - 2 * t);

// Cubic ease in/out
const easeInOut = (t) => t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t;
```

#### Color Conversion

```javascript
// HSL to RGB conversion for procedural colors
const hslToRgb = (h, s, l) => {
  // Hue: 0-360, Saturation: 0-1, Lightness: 0-1
  const c = (1 - Math.abs(2 * l - 1)) * s;
  const x = c * (1 - Math.abs((h / 60) % 2 - 1));
  const m = l - c / 2;

  // Convert to RGB based on hue sector
  // Returns [r, g, b] in range 0-255
};
```

#### Math Helpers

```javascript
const clamp = (val, min, max) => Math.min(Math.max(val, min), max);

const map = (val, inMin, inMax, outMin, outMax) => {
  return outMin + (outMax - outMin) * ((val - inMin) / (inMax - inMin));
};

const randomRange = (min, max) => Math.random() * (max - min) + min;
```

## Audio Engine

### Web Audio API Architecture

```
AudioContext
    ├── Bass Oscillator (Sine, 55Hz)
    │   └── Gain Node (envelope)
    ├── Melody Oscillator (Square)
    │   └── BiquadFilter (lowpass)
    │       └── Gain Node
    ├── Pad Oscillators (3x Sawtooth, detuned)
    │   └── BiquadFilters
    │       └── Gain Nodes
    └── Analyser Node (frequency analysis)
        └── Master Gain
            └── Destination (speakers)
```

### Bass Line Synthesis

```javascript
createBass() {
  const bass = this.ctx.createOscillator();
  const bassGain = this.ctx.createGain();

  bass.type = 'sine';
  bass.frequency.value = 55; // A1 note

  // Kick drum envelope (ADSR)
  const trigger = () => {
    const now = this.ctx.currentTime;
    bassGain.gain.cancelScheduledValues(now);
    bassGain.gain.setValueAtTime(0.4, now);              // Attack
    bassGain.gain.exponentialRampToValueAtTime(0.01, now + 0.2); // Decay
  };

  // Trigger on every beat (4/4 time)
  const beatInterval = 60 / this.bpm; // seconds per beat
  setInterval(trigger, beatInterval * 1000);
}
```

### Frequency Analysis

```javascript
getFrequencyData() {
  // Get frequency data from analyser
  this.analyser.getByteFrequencyData(this.dataArray); // 0-255 per bin

  // Split spectrum into three bands
  const third = Math.floor(this.bufferLength / 3);

  // Bass: 0-170Hz (approx)
  this.bassFreq = average(dataArray[0...third]) / 255;

  // Mids: 170-1400Hz
  this.midFreq = average(dataArray[third...third*2]) / 255;

  // Highs: 1400Hz+
  this.highFreq = average(dataArray[third*2...end]) / 255;

  // Simple beat detection
  this.beat = this.bassFreq > 0.3 ? 1 : 0;
}
```

## Visual Effects

### 1. Starfield Effect

**Algorithm:** 3D to 2D projection with perspective

```javascript
// Each star has 3D coordinates (x, y, z)
// z ranges from 0 (camera) to 1 (far)

update() {
  star.z -= speed * deltaTime;
  if (star.z <= 0) resetToBack(star);
}

render() {
  const scale = 1 / star.z;  // Perspective division
  const screenX = centerX + star.x * maxSize * scale;
  const screenY = centerY + star.y * maxSize * scale;
  const size = scale * 2;    // Stars larger when closer
}
```

**Performance:** O(n) where n = number of stars

### 2. Plasma Effect

**Algorithm:** Multi-layered sine wave composition

```javascript
// Four sine wave layers create organic patterns
const v1 = sin(x * 10 + t);                            // Horizontal waves
const v2 = sin(10 * (x * sin(t/2) + y * cos(t/3)) + t); // Rotating pattern
const v3 = sin(sqrt(100 * (x² + y²) + 1) + t);         // Circular ripples
const v4 = sin(y * 10 + t);                            // Vertical waves

const value = (v1 + v2 + v3 + v4) / 4; // Average all layers

// Map value (-1 to 1) to hue (0 to 360)
const hue = (value * 180 + t * 50) % 360;
```

**Performance:** O(w × h) - every pixel calculated per frame

**Optimization:** Uses `Uint32Array` for direct pixel manipulation

### 3. Tunnel Effect

**Algorithm:** Polar coordinate mapping with lookup tables

```javascript
// Pre-calculate lookup tables (once on init)
init() {
  for (each pixel) {
    const dx = x - centerX;
    const dy = y - centerY;
    const distance = sqrt(dx² + dy²);
    const angle = atan2(dy, dx);

    lookupX[idx] = angle / π;      // Normalized angle
    lookupY[idx] = 1 / distance;   // Inverse distance
    lookupD[idx] = distance;       // For brightness
  }
}

// Render using pre-calculated values (fast)
render() {
  const u = lookupX[i] + shiftX;  // Texture U coordinate
  const v = lookupY[i] + shiftY;  // Texture V coordinate

  const hue = (u + v) * 180 + time;
  const brightness = 0.5 + sin(v * 20) * 0.3;
}
```

**Performance:**
- Init: O(w × h) once
- Render: O(w × h) with cache-friendly access

### 4. Particle System

**Algorithm:** Newtonian physics with gravitational attraction

```javascript
update(deltaTime) {
  particles.forEach(p => {
    // Calculate force toward center
    const dx = centerX - p.x;
    const dy = centerY - p.y;
    const distance = sqrt(dx² + dy²);

    // Apply force (F = ma, assuming m=1)
    const force = audioReactivity * 200;
    p.vx += (dx / distance) * force * deltaTime;
    p.vy += (dy / distance) * force * deltaTime;

    // Apply damping (air resistance)
    p.vx *= 0.99;
    p.vy *= 0.99;

    // Integrate velocity (Euler method)
    p.x += p.vx * deltaTime;
    p.y += p.vy * deltaTime;

    // Wrap around screen edges
    wrapPosition(p);
  });
}
```

**Performance:** O(n) where n = particle count

**Optimization:** Object pooling (pre-allocated array, reuse particles)

### 5. Metaballs Effect

**Algorithm:** Distance field evaluation with implicit surfaces

```javascript
// For each pixel, sum influence from all metaballs
render() {
  for (each pixel) {
    let sum = 0;

    balls.forEach(ball => {
      const dx = pixelX - ball.x;
      const dy = pixelY - ball.y;
      const dist = sqrt(dx² + dy²);

      // Add ball's influence (inverse distance)
      sum += ball.radius / (dist + epsilon);
    });

    // Render if above threshold (inside surface)
    if (sum > threshold) {
      renderPixel(hue, brightness);
    }
  }
}
```

**Performance:** O(w × h × b) where b = ball count

**Optimization:**
- Render at 2×2 pixel blocks
- Early termination when sum exceeds threshold
- Spatial hashing for large ball counts (not implemented)

### 6. Credits Effect

**Algorithm:** Scrolling text with alpha blending

```javascript
update() {
  scrollY -= speed * deltaTime;
  if (scrollY < -totalHeight) scrollY = screenHeight; // Loop
}

render() {
  lines.forEach((line, i) => {
    const y = scrollY + i * lineHeight;

    // Calculate alpha based on distance from top/bottom
    const alpha = min(
      (y + margin) / fadeDistance,        // Fade in from top
      (screenHeight - y + margin) / fadeDistance, // Fade out at bottom
      1.0
    );

    renderText(line, y, alpha);
  });
}
```

## Scene Management

### Scene Sequencer

```javascript
const sceneManager = {
  update(deltaTime) {
    const scene = SCENES[currentScene];
    const elapsed = time - sceneStartTime;
    const progress = elapsed / scene.duration;

    // Check if scene complete
    if (elapsed >= scene.duration) {
      loadScene(currentScene + 1); // Auto-advance
    }

    // Update current effect
    currentEffect.update(deltaTime);
  }
};
```

### Scene Definition

```javascript
const SCENES = [
  {
    name: 'Scene Name',      // Display name
    duration: 10000,         // Duration in milliseconds
    effect: 'effectName'     // Effect identifier
  }
];
```

## Performance Considerations

### Memory Management

1. **Object Pooling**
   - Pre-allocate arrays for particles
   - Reuse objects instead of creating new ones
   - Prevents garbage collection pauses

2. **Typed Arrays**
   ```javascript
   // Standard array (slow)
   const buffer = new Array(width * height);

   // Typed array (fast, cache-friendly)
   const buffer = new Uint32Array(width * height);
   ```

3. **Lookup Tables**
   - Pre-calculate expensive operations (tunnel coordinates)
   - Trade memory for speed

### Rendering Optimization

1. **Dirty Regions**
   ```javascript
   // Clear entire canvas (slow)
   ctx.clearRect(0, 0, width, height);

   // Fade with alpha (trail effect + performance)
   ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
   ctx.fillRect(0, 0, width, height);
   ```

2. **Batch Operations**
   - Minimize state changes (fillStyle, font, etc.)
   - Group similar draw calls

3. **Adaptive Quality**
   ```javascript
   // Render metaballs at 2×2 blocks instead of 1×1
   for (let y = 0; y < height; y += 2) {
     for (let x = 0; x < width; x += 2) {
       calculatePixel(x, y);
       // Fill 2×2 block with same color
     }
   }
   ```

### Frame Rate Independence

```javascript
// Bad: depends on frame rate
position += 5; // Moves 5 pixels per frame

// Good: independent of frame rate
position += speed * deltaTime; // Moves at constant rate
```

**Delta Time Calculation:**
```javascript
const deltaTime = currentTime - lastTime; // milliseconds
const deltaSeconds = deltaTime / 1000;    // seconds

// Use deltaSeconds for physics calculations
velocity += acceleration * deltaSeconds;
position += velocity * deltaSeconds;
```

## Browser Compatibility

### Feature Detection

```javascript
// Check for Web Audio API
const AudioContext = window.AudioContext || window.webkitAudioContext;
if (!AudioContext) {
  console.error('Web Audio API not supported');
}

// Check for Canvas API
const canvas = document.createElement('canvas');
if (!canvas.getContext) {
  console.error('Canvas API not supported');
}

// Check for RequestAnimationFrame
if (!window.requestAnimationFrame) {
  // Fallback to setTimeout
  window.requestAnimationFrame = (callback) => {
    return setTimeout(callback, 1000 / 60);
  };
}
```

### Vendor Prefixes

```javascript
// Fullscreen API
const requestFullscreen =
  element.requestFullscreen ||
  element.webkitRequestFullscreen ||
  element.mozRequestFullScreen ||
  element.msRequestFullscreen;
```

## Debugging Tips

### Performance Profiling

```javascript
// Measure effect render time
console.time('plasma');
plasma.render(ctx);
console.timeEnd('plasma');

// Log FPS
if (fps < 55) {
  console.warn('Low FPS:', fps);
}
```

### Visual Debugging

```javascript
// Draw bounding boxes for particles
ctx.strokeStyle = 'red';
ctx.strokeRect(p.x - p.size, p.y - p.size, p.size * 2, p.size * 2);

// Display frequency data
ctx.fillText(`Bass: ${audio.bassFreq.toFixed(2)}`, 10, 30);
```

## Future Enhancements

### Possible Improvements

1. **WebGL Implementation**
   - Use fragment shaders for effects
   - 10-100× performance boost
   - More complex effects possible

2. **Post-Processing**
   - Bloom/glow effects
   - Chromatic aberration
   - Motion blur

3. **Interactive Mode**
   - Mouse/touch interaction
   - MIDI controller support
   - VR support

4. **Export Functionality**
   - Record to video (MediaRecorder API)
   - Screenshot capture
   - GIF export

5. **Advanced Audio**
   - Multiple music tracks
   - Audio file loading
   - Microphone input for visualization

## References

- [Web Audio API Specification](https://www.w3.org/TR/webaudio/)
- [Canvas API Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [RequestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)
- [Demoscene History](https://en.wikipedia.org/wiki/Demoscene)

---

**End of Technical Documentation**
