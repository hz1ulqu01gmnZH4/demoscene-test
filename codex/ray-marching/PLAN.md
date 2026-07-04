# Ray-Marching Demo Plan

1. **HTML Skeleton**
   - Single-file document with `<canvas>` and inline `<script>` sets up WebGL2 context and render loop.
   - Include inline `<audio>` or Web Audio creation triggered by user gesture to satisfy autoplay policies.

2. **Shader Architecture**
   - Fragment shader handles ray setup using camera basis (position, forward, up, right) derived from uniforms.
   - Distance estimator combines primitives (spheres, boxes, repetition) plus smooth min for blending.
   - Lighting computes normals via finite differences, supports diffuse/specular, soft shadows, and fog/palette functions.

3. **JavaScript Glue**
   - Compile shader program, pass uniforms for time, resolution, camera animation, and audio-driven parameters.
   - Build camera path (e.g., orbiting spline) and update matrices each frame.
   - Implement adaptive ray-march controls: max steps, epsilon, safety bailout.

4. **Audio System**
   - Use Web Audio API oscillators/noise nodes mixed through GainNode into destination.
   - Attach AnalyserNode to extract bass/mid energy; smooth values and feed to shader uniforms for color/displacement modulation.

5. **Experience Polish**
   - Add timeline-driven transitions for scene parameters synced to audio envelope.
   - Display fallback text/instruction for browsers requiring user interaction to start audio/visuals.
   - Keep entire demo contained in one HTML file with minimal external dependencies.

6. **Validation**
   - Manually test in Chromium/Firefox ensuring shader compiles and audio plays.
   - Monitor console for precision/performance issues; tweak step counts or resolution scaling if necessary.
