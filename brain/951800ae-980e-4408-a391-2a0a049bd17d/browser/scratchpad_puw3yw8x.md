# Gram-Schmidt Visual Verification Task
- [x] Navigate to http://localhost:3001/view/02-gram-schmidt
- [x] Advance to Step 4 of Verse 1
- [x] Identify red text or KaTeX error in Step 4
- [x] Capture DOM and LaTeX for the error
- [x] Capture console log for "KaTeX parse error"
- [x] Advance to Scene 2 (3D)
- [x] Capture console log for "glTexSubImage2DRobustANGLE"
- [x] Report findings

## Final Diagnosis
1. **KaTeX Matrix Rendering Bug**: 
   - Element: `<span class="katex-error" title="ParseError: KaTeX parse error: Unexpected end of input in a macro argument, expected '}' at end of input: ….5 \end{pmatrix">`
   - Issue: The LaTeX string is being prematurely split or clipped before the final `}` of `\end{pmatrix}`. The `innerHTML` of the error span is `\displaystyle \begin{pmatrix} -0.5 \\ 1.5 \end{pmatrix` and a subsequent element contains the missing `}`.
   - Cause: Likely a regex or string splitting logic in `EquationParser.ts` that fails on multi-line or complex environments.

2. **3D Scene Rendering Bug**:
   - Error: `GL_INVALID_OPERATION: glTexSubImage2DRobustANGLE: Level of detail outside of range.`
   - Symptom: Scene 2 is completely blank.
   - Cause: Common in MathBox when textures (often for labels) are updated incorrectly or with zero dimensions.
