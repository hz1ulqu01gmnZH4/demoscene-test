# Demoscene - Audio Visual Experience

A self-contained HTML5 demoscene application featuring procedural visual effects and audio synthesis. Built with vanilla JavaScript, Canvas API, and Web Audio API—no external dependencies required.

## 🎨 Features

### Visual Effects
- **Starfield** - 3D parallax starfield with depth and audio reactivity
- **Plasma** - Multi-layered sine wave plasma with dynamic colors
- **Tunnel** - Classic tunnel effect with perspective distortion
- **Particles** - Audio-reactive particle system with gravitational attraction
- **Metaballs** - Organic metaball rendering using distance fields
- **Credits** - Smooth scrolling text with glow effects

### Audio Engine
- **Procedural Music** - Real-time audio synthesis using Web Audio API
- **Bass Line** - Kick drum pattern (4/4 beat at 130 BPM)
- **Melody** - Arpeggiated lead (A minor pentatonic scale)
- **Pad** - Detuned sawtooth wave pad for atmosphere
- **Audio Reactivity** - Visual effects respond to bass, mid, and high frequencies

### Technical Features
- 🎯 **Self-contained** - Single HTML file, works offline
- ⚡ **Performance** - Optimized for 60fps on modern hardware
- 🎬 **Scene Sequencer** - Automatic transitions between effects
- 🎮 **Interactive Controls** - Keyboard shortcuts for control
- 📱 **Responsive** - Adapts to any screen size
- 🎨 **Procedural Graphics** - All visuals generated in real-time

## 🚀 Quick Start

### Method 1: Direct Open
1. Open `demoscene.html` in a modern web browser
2. Click the **"Start Experience"** button
3. Enjoy the show!

### Method 2: Local Server (Recommended)
```bash
# Python 3
python -m http.server 8000

# Node.js
npx http-server

# Then open: http://localhost:8000/demoscene.html
```

## 🎮 Controls

| Key | Action |
|-----|--------|
| **Click "Start"** | Begin the experience (required for audio) |
| **SPACE** | Pause/Resume |
| **F** | Toggle fullscreen |
| **R** | Restart from beginning |

## 📋 Scene Timeline

| Time | Scene | Description |
|------|-------|-------------|
| 0:00 - 0:05 | **Starfield Intro** | Depth-parallax starfield with zooming |
| 0:05 - 0:15 | **Plasma Dreams** | Multi-wave plasma with color cycling |
| 0:15 - 0:25 | **Tunnel Vision** | Classic perspective tunnel effect |
| 0:25 - 0:35 | **Particle Storm** | Audio-reactive particle system |
| 0:35 - 0:45 | **Metaball Fusion** | Organic morphing metaballs |
| 0:45 - 0:55 | **Credits Roll** | Scrolling credits with effects |
| 0:55 - 1:00 | **Outro** | Return to starfield (loops) |

## 🛠️ Technical Details

### Architecture
```
├── Configuration & Constants
├── Global State Management
├── Utility Functions
│   ├── Math helpers (lerp, clamp, map)
│   ├── Easing functions
│   └── Color conversion (HSL to RGB)
├── Audio Engine
│   ├── Web Audio API setup
│   ├── Oscillator synthesis
│   ├── Frequency analysis
│   └── Beat detection
├── Visual Effects
│   ├── Starfield (3D projection)
│   ├── Plasma (sine wave composition)
│   ├── Tunnel (polar coordinates)
│   ├── Particles (physics simulation)
│   ├── Metaballs (distance fields)
│   └── Credits (scrolling text)
├── Scene Manager
│   ├── Timeline sequencing
│   ├── Scene transitions
│   └── Effect initialization
└── Main Loop
    ├── Animation loop (RAF)
    ├── Delta time calculation
    ├── FPS counter
    └── Render pipeline
```

### Performance Optimizations
- **Typed Arrays** - Uint32Array for pixel manipulation
- **Object Pooling** - Pre-allocated particle arrays
- **Lookup Tables** - Pre-calculated tunnel coordinates
- **Dirty Regions** - Trail effects for motion blur
- **Adaptive Quality** - Metaball rendering at 2x2 pixels

### Browser Compatibility
- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Opera 76+

**Requirements:**
- Canvas API
- Web Audio API
- RequestAnimationFrame
- ES6+ JavaScript

## 📊 File Size

| Metric | Value |
|--------|-------|
| **Uncompressed** | ~28KB |
| **Gzipped** | ~7KB |
| **Dependencies** | 0 |
| **Lines of Code** | ~850 |

## 🎨 Effect Algorithms

### Plasma Effect
```javascript
// Multi-layered sine wave composition
v1 = sin(x * 10 + t)
v2 = sin(10 * (x * sin(t/2) + y * cos(t/3)) + t)
v3 = sin(sqrt(100 * (x² + y²) + 1) + t)
v4 = sin(y * 10 + t)
value = (v1 + v2 + v3 + v4) / 4
```

### Tunnel Effect
```javascript
// Polar coordinate mapping
angle = atan2(dy, dx)
distance = sqrt(dx² + dy²)
u = angle / π + time
v = 1 / distance + time
```

### Metaballs
```javascript
// Distance field evaluation
sum = Σ(radius / distance)
if (sum > threshold) render pixel
```

## 🎵 Audio Synthesis

### Bass Line
- **Waveform:** Sine wave
- **Frequency:** 55 Hz (A1)
- **Pattern:** Kick on every beat (4/4)
- **Envelope:** Quick attack, exponential decay

### Melody
- **Waveform:** Square wave
- **Scale:** A minor pentatonic (A3, C4, D4, E4, A4)
- **Pattern:** 16th note arpeggio
- **Filter:** Low-pass at 1000 Hz

### Pad
- **Waveform:** Detuned sawtooth (3 oscillators)
- **Frequencies:** 220, 220.5, 219.5 Hz
- **Filter:** Low-pass at 800 Hz, Q=2

## 🔧 Customization

### Modify Scene Duration
Edit the `SCENES` array in the code:
```javascript
const SCENES = [
  { name: 'Custom Scene', duration: 8000, effect: 'plasma' },
  // Add or modify scenes
];
```

### Adjust Audio Parameters
```javascript
const audio = {
  bpm: 130,           // Change tempo
  masterGain: 0.3,    // Adjust volume
  // Modify in audio.init()
};
```

### Configure Visual Quality
```javascript
const CONFIG = {
  particleCount: 200,   // More particles = slower
  metaballCount: 8,     // More metaballs = slower
  starCount: 300        // More stars = slower
};
```

## 🐛 Troubleshooting

### No Audio
- **Issue:** Web Audio requires user interaction
- **Fix:** Click the "Start Experience" button

### Low FPS
- **Issue:** Device may be underpowered
- **Fix:** Reduce `particleCount`, `metaballCount`, or `starCount` in CONFIG

### Effects Not Visible
- **Issue:** Canvas not initialized
- **Fix:** Ensure you're using a modern browser with Canvas support

## 📝 License

This demoscene application is released as open source for educational and artistic purposes.

## 🙏 Credits

**Created with:**
- Vanilla JavaScript
- HTML5 Canvas API
- Web Audio API
- Pure web technologies

**Inspired by:**
- Classic demoscene productions
- 8-bit/16-bit era graphics
- Procedural generation techniques

## 🌟 Showcase

Perfect for:
- Web development demonstrations
- Creative coding education
- Browser capability showcases
- Audio-visual performances
- Coding interviews (procedural graphics)

---

**Enjoy the demoscene experience!** 🎨🎵✨
