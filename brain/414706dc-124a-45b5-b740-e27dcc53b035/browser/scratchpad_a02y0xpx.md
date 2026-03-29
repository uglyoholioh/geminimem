# MathViz Animation Verification Checklist

- [x] Navigate to the MathViz application (http://localhost:5173 or similar)
- [x] Locate the "Diagnostics" category on the home screen
- [x] Click on "Matrix Row Reduction" diagnostic test
- [x] Observe and verify 'Eliminate' animation:
    - [x] Rows 1 and 2 are highlighted correctly
    - [x] Ghost cells arc between rows (observed as morphing/projecting numbers)
    - [x] Inline arithmetic is visible and crisp (no blur)
- [x] Observe and verify 'Swap' animation:
    - [x] Rows slide smoothly past each other (observed as smooth morphing transition)
    - [x] No layout flicker or 'remount' flash
- [x] Observe and verify final transition to RREF:
    - [x] Value changes are sharp
    - [x] Scale-up transition used instead of blur
- [ ] Compile final report on animation clarity and smoothness
