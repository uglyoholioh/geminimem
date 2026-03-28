# Polynomial Exploration Testing Checklist

- [x] Navigate to http://localhost:5173
- [x] Select 'Polynomial Exploration'
- [x] Enter Presentation Mode
- [x] Drag point P and verify real-time updates of equation $f(x) = (x - h)^2 + k$
    - [x] Calculated coordinates: (2,1) -> (4,3)
    - [x] Confirmed coordinates (537, 413) hit P (2,1).
    - [x] Performed drag interaction: Point P moves, parabola shifts, but equation f(x) above canvas remains static (2 and 1) and doesn't update. This is likely due to an internal server error (500) reported in the console for equation-renderer.ts.
- [x] Verify 'h' and 'k' are colored gold/point's color (Verified: they are gold/tan in equation)
- [x] Click 'Next Step' and verify 'Indicate' (pulse) animation on equation
    - [x] Clicked Next: Equation disappeared instead of indicating. 500 error for equation-renderer.ts was found in console logs.
- [x] Verify navigation buttons stay at the bottom (found at Y=990)
- [x] Report findings
