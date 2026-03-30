# QR Factorization & Least Squares

## Goal
The goal of this task is two-fold:
1. **Redesign the JSON Editor UI** so that it doesn't persistently occupy screen real-estate via a sidebar. We will switch to an invisible trigger (e.g., a top-right action icon) that opens a fully featured, centered dark-mode Modal dialogue.
2. **Implement a new Math Lesson (`qrFactorization.ts`)** that visualizes the Gram-Schmidt process resulting in a $QR$ factorization, and explaining its usage in solving $Ax = b$ via Ordinary Least Squares algorithm. 

## Proposed Changes

### UI & UX: JSON Editor Redesign
- **[MODIFY] `src/components/JSONPanel.tsx`**: Change the implementation from a collapsible absolute sidebar (`w-96` / `w-12`) to a massive, centered, glassmorphic Modal component. The modal will take up the majority of the screen when active to give plenty of room for pasting complex lesson schemas.
- **[MODIFY] `src/components/Player.tsx`**: Replace the current `<JSONPanel />` sidebar invocation with a minimalistic floating action button in the top right corner (e.g., a `<FileJson />` icon or gear `⚙️`) that controls the visibility of the new Modal.

<hr>

### Content: QR + Least Squares Lesson
- **[NEW] `src/data/qrLesson.ts`**: Create a new declarative JSON lesson. The scene will feature two vectors $a_1$ and $a_2$ and guide the user through deriving $q_1$ and $q_2$ (the orthonormal basis forms $Q$). 
  - **Step 1:** Establish linearly independent columns of an overdetermined system $A = [a_1, a_2]$.
  - **Step 2:** Normalize $a_1$ to find our first orthogonal vector $q_1$.
  - **Step 3:** Project $a_2$ mathematically onto $q_1$ and subtract the projection to reveal $u_2$, then normalize to $q_2$.
  - **Step 4:** Construct the dense lower-dimensional $R$ matrix by reflecting projections ($R = Q^T A$).  
  - **Step 5:** Final explainer showing how replacing $Ax = b$ with $Rx = Q^T b$ drastically reduces computational complexity to a simple back-substitution because $R$ is perfectly upper-triangular. 
- **[MODIFY] `src/components/Player.tsx`**: Swap the hardcoded `sampleLesson` reference to load the new `qrLesson` by default.

> [!NOTE] 
> Because the SVG canvas intrinsically interpolates vector coordinates `[x, y]`, the projection animations will automatically transform smoothly across the screen mathematically without us needing to keyframe them!

## Open Questions

> [!WARNING]
> While we have a 2-dimensional system $A \in \mathbb{R}^{2 \times 2}$, typically Least Squares uses a tall-and-skinny matrix ($A \in \mathbb{R}^{m \times n}$) leading to a projection onto a subspace. Since our engine operates on a strict geometric visually 2D plane `[x,y]`, my script mathematically computes exactly how $Gram$-$Schmidt$ factors a $2\times 2$ invertible matrix $A$ into orthogonal matrices. Is a 2D $2\times 2$ visualisation of the matrix $R$'s upper-triangular components an acceptable substitute to convey the concept without needing a full blown 3-Dimensional $XYZ$ visualizer for a plane projection?

### Verification Plan
1. **Manual Visual Testing**: Check the new JSON modal opens cleanly, overlays the background blur, and correctly applies stringified JSON to the timeline.
2. **Animation Tracking**: Seek through the Gram Schmidt process backwards and forwards and confirm the resulting vectors perfectly orthogonally intersect visually.
