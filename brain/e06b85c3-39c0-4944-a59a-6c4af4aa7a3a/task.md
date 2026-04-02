# Task: Math Visualiser Fixes

- [/] useTween.ts — Fix deepLerp for new/removed elements (opacity fade in/out, no NaN)
- [ ] AnimatedGrid.tsx — Better axis visibility, adaptive grid density
- [ ] AnimatedAngle.tsx — Fix points.length < 3 guard
- [ ] AnimatedVector.tsx — Clamp minimum shaft, improve label safety
- [ ] AnimatedPolygon.tsx — Handle 2-vertex case as line, add thin stroke
- [ ] AnimatedLine.tsx — Add strokeWidth prop
- [ ] AnimatedMath.tsx — Tighter clamping, CSS position transition, freeze during camera move
- [ ] SVGCanvas.tsx — Pass camera-stable flag to math blocks, fix origin circle distortion
- [ ] Overlay.tsx — Remove re-triggering animation, add content crossfade
- [ ] MathText.tsx — Add equation fade-in on content change
- [ ] orthoLesson.ts — Fix mathBlock coords and camera positions
- [ ] qrLesson.ts — Replace 2-vertex polygon with line for projection
- [ ] index.css — Add overlay crossfade, math block transition styles
