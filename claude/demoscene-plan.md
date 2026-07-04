# Demoscene Application Plan - Self-Contained HTML

## Project Overview
A single-file HTML demoscene application showcasing visual effects, animations, and audio-visual synchronization using modern web technologies (Canvas API, WebGL, Web Audio API).

## Technical Architecture

### Core Technologies
- **HTML5 Canvas/WebGL** - Graphics rendering
- **Web Audio API** - Audio synthesis and visualization
- **Vanilla JavaScript** - No external dependencies
- **CSS3** - UI styling (inline)
- **RequestAnimationFrame** - Smooth 60fps animation loop

## Feature Sequence & Implementation Plan

### Phase 1: Foundation (Core Engine)
1. **HTML Structure**
   - Single `<!DOCTYPE html>` file
   - Embedded CSS in `<style>` tags
   - Embedded JavaScript in `<script>` tags
   - Fullscreen canvas element

2. **Animation Loop**
   - RequestAnimationFrame-based main loop
   - Delta time calculation
   - FPS counter
   - Scene timing and sequencing system

3. **Audio Engine**
   - Web Audio API oscillators
   - Procedural music generation
   - Beat detection/sync system
   - Audio-reactive parameters

### Phase 2: Visual Effects Library
4. **2D Canvas Effects**
   - Plasma effect
   - Tunnel effect
   - Starfield/particle systems
   - Metaballs
   - Fire effect
   - Water ripples

5. **WebGL Shaders (Optional Advanced)**
   - Fragment shader framework
   - Raymarching scenes
   - Post-processing effects
   - Feedback loops

### Phase 3: Scene Composition
6. **Scene Manager**
   - Timeline-based scene sequencer
   - Smooth transitions between effects
   - Camera/viewport controls
   - Effect parameter interpolation

7. **Text Engine**
   - Scrolling text/credits
   - Text effects (waving, scaling, rotating)
   - Vector text rendering
   - Greetings section

### Phase 4: Audio-Visual Integration
8. **Sync System**
   - Music timeline markers
   - Effect trigger on beats
   - Parameter modulation by frequency bands
   - Audio spectrum visualization

9. **Procedural Music**
   - Chiptune-style synthesis
   - Bass line generation
   - Melody patterns
   - Drum patterns

### Phase 5: Polish & Optimization
10. **Performance**
    - Object pooling for particles
    - Efficient rendering techniques
    - Conditional quality settings
    - Memory management

11. **UI/UX**
    - Loading screen
    - Start button (audio context requires user interaction)
    - Fullscreen toggle
    - Keyboard controls (space to pause, etc.)

## Scene Sequence Example

```
0:00 - 0:05  | Intro: Fade in with starfield
0:05 - 0:15  | Plasma effect with audio sync
0:15 - 0:25  | Tunnel effect with pulsing
0:25 - 0:35  | Particle system showcase
0:35 - 0:45  | Metaballs with morphing
0:45 - 0:55  | Combined effects montage
0:55 - 1:05  | Credits scroll with fire effect
1:05 - 1:10  | Outro: Fade to logo/greetings
```

## Code Structure (Single File)

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    /* Embedded CSS - fullscreen, no margins */
  </style>
</head>
<body>
  <canvas id="canvas"></canvas>
  <div id="ui"><!-- Start button, controls --></div>

  <script>
    // 1. Configuration & Constants
    // 2. Utility Functions
    // 3. Audio Engine
    // 4. Effect Implementations
    // 5. Scene Manager
    // 6. Main Loop
    // 7. Initialization
  </script>
</body>
</html>
```

## Implementation Strategy

### Development Approach
1. **Incremental Building**
   - Start with basic canvas setup
   - Add one effect at a time
   - Test each effect independently
   - Integrate into scene sequence

2. **Testing Checkpoints**
   - Verify animation loop smoothness
   - Test audio playback and sync
   - Check cross-browser compatibility
   - Measure performance (aim for 60fps)

3. **Optimization Focus**
   - Minimize DOM access
   - Pre-calculate static values
   - Use typed arrays where applicable
   - Implement quality fallbacks

## Technical Constraints

### File Size Target
- **Goal**: < 50KB uncompressed
- **Stretch**: < 20KB (competitive demoscene size)
- **Techniques**: Minification, code golfing, procedural generation

### Browser Compatibility
- Modern browsers (Chrome, Firefox, Safari, Edge)
- ES6+ JavaScript
- No polyfills (keep it small)

### Performance Targets
- 60fps on mid-range hardware
- Responsive to window resize
- No memory leaks during runtime

## Effect Parameters for Audio Reactivity

Each effect should expose parameters that can be modulated:
- **Color** - Hue rotation, saturation, brightness
- **Speed** - Animation speed multiplier
- **Scale** - Zoom level, particle size
- **Intensity** - Effect strength, blur amount
- **Frequency** - Pattern repetition, wave frequency

## Deliverables

1. **demoscene.html** - Single self-contained file
2. **README.md** - Instructions and effect descriptions
3. **TECHNICAL.md** - Implementation notes and algorithms

## Success Criteria

✓ Runs entirely offline (no external dependencies)
✓ Smooth 60fps animation
✓ Audio-visual synchronization
✓ Multiple distinct effects
✓ Professional presentation
✓ Cross-browser compatible
✓ Under 50KB file size

## Future Enhancements (Post-MVP)

- WebGL 2.0 advanced shaders
- Touch/mobile support
- URL parameter configuration
- Export to video functionality
- Community effect submissions

---

**Next Steps**:
1. Set up basic HTML canvas boilerplate
2. Implement animation loop with timing
3. Create first effect (starfield recommended)
4. Add audio engine foundation
5. Build scene sequencer
