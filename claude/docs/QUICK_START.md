# Quick Start Guide

## 🚀 Launch the Demoscene

### Option 1: Double-Click (Easiest)
Simply double-click `demoscene.html` - it will open in your default browser.

### Option 2: Python Server (Recommended)
```bash
cd demoscene-test
python3 -m http.server 8000
# Open browser to: http://localhost:8000/demoscene.html
```

### Option 3: Node.js Server
```bash
cd demoscene-test
npx http-server
# Open browser to: http://localhost:8080/demoscene.html
```

## 🎮 First Time Experience

1. **Click "Start Experience"** - This is required to enable audio (browser security)
2. **Go Fullscreen** - Press `F` for the best experience
3. **Sit Back** - Enjoy the 60-second audio-visual journey
4. **Control** - Use `SPACE` to pause, `R` to restart

## 📋 What to Expect

### Timeline (60 seconds)
- **0-5s**: Starfield intro with depth parallax
- **5-15s**: Plasma effect with flowing colors
- **15-25s**: Tunnel effect with perspective
- **25-35s**: Particle system with audio reactivity
- **35-45s**: Metaballs with organic morphing
- **45-55s**: Credits scroll
- **55-60s**: Outro (then loops)

### Audio
- **Bass**: Kick drum pattern (130 BPM)
- **Melody**: Arpeggiated lead (A minor pentatonic)
- **Pad**: Atmospheric background
- All sounds generated in real-time!

### Visual Effects
All effects respond to the music:
- **Bass** → Affects particle movement, color intensity
- **Mids** → Controls plasma speed, tunnel zoom
- **Highs** → Modulates particle size, glow effects

## 🎨 Customization Tips

Want to modify the experience? Edit `demoscene.html`:

### Change Scene Duration
```javascript
// Line ~30: Find SCENES array
{ name: 'Plasma Dreams', duration: 10000, effect: 'plasma' }
// Change duration (in milliseconds)
```

### Adjust Performance
```javascript
// Line ~20: Find CONFIG object
particleCount: 200,   // Reduce for slower devices
metaballCount: 8,     // Reduce for better performance
starCount: 300        // Reduce for older hardware
```

### Modify Music Tempo
```javascript
// Line ~220: Find audio object
bpm: 130,  // Change tempo (beats per minute)
```

## 🐛 Troubleshooting

### No Sound?
- Make sure you clicked "Start Experience"
- Check browser volume
- Try Chrome/Firefox (best compatibility)

### Low FPS?
- Reduce `particleCount` and `metaballCount` in CONFIG
- Close other browser tabs
- Try a different browser

### Blank Screen?
- Check browser console (F12) for errors
- Ensure you're using a modern browser
- Try refreshing the page

## 📱 Browser Support

✅ **Best Experience:**
- Chrome 90+
- Firefox 88+
- Edge 90+

⚠️ **Limited Support:**
- Safari 14+ (some audio issues possible)
- Mobile browsers (performance may vary)

## 🎯 Pro Tips

1. **Fullscreen Mode** - Press `F` for immersive experience
2. **Share It** - The file is self-contained, just send `demoscene.html`
3. **Learn From It** - View source to see how effects are implemented
4. **Remix It** - Modify scenes, add new effects, change colors
5. **Performance** - Monitor FPS (top-left) to see real-time performance

## 📚 Next Steps

- Read `README.md` for full feature list
- Check `docs/TECHNICAL.md` for implementation details
- Experiment with the code
- Create your own effects!

---

**Enjoy the show!** 🎨✨🎵
