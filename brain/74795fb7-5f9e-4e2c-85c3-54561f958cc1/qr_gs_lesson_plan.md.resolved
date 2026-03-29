# Lesson: QR Factorization & Gram-Schmidt Process

This plan outlines the addition of a new interactive lesson teaching the Gram-Schmidt process for QR factorization. It will leverage the newly implemented synchronized highlighting feature to link mathematical formulas directly to high-quality vector visualizations in the graph.

## Proposed Changes

### Example Definition

#### [MODIFY] [app.ts](file:///Users/oli/Desktop/lectvideoanalyser/MathViz/src/app.ts)
- Add a new entry to the `EXAMPLES` array titled **"QR Factorization & Gram-Schmidt"**.
- This example will include:
  - **Narrative Story-telling**: Step-by-step guidance using the `{{id:content}}` syntax.
  - **Mathematical Visualization**: A coordinate system showing:
    - Original vectors $v_1, v_2$.
    - Projection of $v_2$ onto $v_1$ to show the orthogonal component.
    - Resulting orthonormal vectors $q_1, q_2$.
  - **Synchronized Colors**: 
    - {{v1: Teal}} for the first vector.
    - {{v2: Gold}} for the second vector.
    - {{proj: Blue}} for the projection.
    - {{u2: Purple}} for the orthogonal vector.
    - {{q: White/Special}} for the final orthonormal basis.
  - **Animations**: Using `Indicate` to draw focus to each step as it's explained.

## Lesson Structure

1. **Introduction**: Show $v_1 = [3, 0]$ and $v_2 = [1, 2]$.
2. **First Vector**: Identify $u_1 = v_1$.
3. **Projection**: Calculate and show $proj_{u_1}(v_2)$.
4. **Orthogonalization**: Subtract projection to find $u_2$.
5. **Normalization**: Show $q_1, q_2$ (the columns of Q).
6. **QR Matrix**: Display the final $A = QR$ result.

## Verification Plan

### Manual Verification
- Render the new "QR Factorization & Gram-Schmidt" example.
- Step through the lesson in **Presentation Mode**.
- Verify that clicking each step highlights the corresponding text, math terms, and graph vectors in the same color.
- Check that the projection vector and orthogonal subtraction are clearly visualized on the grid.
- Ensure the final matrix $A = QR$ is correctly displayed in the math area.
