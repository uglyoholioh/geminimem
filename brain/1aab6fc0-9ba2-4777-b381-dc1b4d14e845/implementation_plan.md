# MathViz — AI-Powered Math Visualization Tool

A web-based tool that turns structured JSON specs into animated, step-by-step mathematical visualizations. Designed so AI assistants can generate visualization specs that produce rich, animated visual explanations of linear algebra, calculus, and more.

## Core Concept

**The Problem**: AI responses to math questions are text-based, making it hard to visualize matrices transforming, vectors in space, or the steps of a derivation.

**The Solution**: A declarative visualization language (JSON specs) + a beautiful web renderer. You (or an AI) describe *what* to visualize, and MathViz animates it step by step.

### How It Works (User Flow)
1. Ask an AI a math question (e.g., "Solve this system using RREF")
2. The AI generates a **MathViz spec** (structured JSON) alongside its explanation
3. Paste the spec into MathViz (or the AI outputs it directly)
4. MathViz renders an animated, step-by-step visualization
5. Use playback controls to go forward/backward through steps

---

## User Review Required

> [!IMPORTANT]
> **Technology Choices** — I'm planning to build this with **Vite + Vanilla JS** (no heavy frameworks), using **KaTeX** for math rendering and the **HTML5 Canvas API** for vector/graph visualizations. Matrices and step-by-step displays will use DOM elements with CSS animations for smoother, more styleable results. This keeps the project lean and fast. Does this work for you, or would you prefer a React-based approach?

> [!IMPORTANT]
> **3D Visualization** — For vector spaces, I'll start with **2D visualization** with an option for isometric pseudo-3D projection (no Three.js dependency). Full 3D with rotation/zoom can be added later. Is 2D-first acceptable?

> [!IMPORTANT]
> **Scope for v1** — I'm planning to build all five visualization modules below in v1. This is substantial but each module is self-contained. Should I prioritize any specific modules?

---

## Architecture

### Tech Stack
| Layer | Technology | Why |
|-------|-----------|-----|
| Build | Vite | Fast HMR, ES modules, minimal config |
| Rendering (Math) | KaTeX | Fast LaTeX rendering, smaller than MathJax |
| Rendering (Graphics) | Canvas 2D API | Precise geometric rendering for vectors/graphs |
| Rendering (Matrices) | DOM + CSS Grid | Easy styling, smooth CSS transitions |
| Computation | math.js | Matrix operations, expression parsing |
| Animation | Custom engine + CSS | requestAnimationFrame for canvas, CSS transitions for DOM |
| Fonts | Inter + JetBrains Mono | Clean sans-serif + monospace for math |

### File Structure
```
MathViz/
├── index.html
├── package.json
├── vite.config.js
├── src/
│   ├── main.js                    # Entry point, initializes app
│   ├── style.css                  # Global styles, design tokens, animations
│   ├── app.js                     # App shell, routing between visualizers
│   │
│   ├── components/
│   │   ├── sidebar.js             # Navigation + example gallery
│   │   ├── toolbar.js             # Playback controls (play/pause/step/speed)
│   │   ├── input-panel.js         # JSON spec input with syntax highlighting
│   │   └── info-panel.js          # Current step explanation display
│   │
│   ├── visualizers/
│   │   ├── matrix-viz.js          # Matrix operations (REF/RREF/multiplication)
│   │   ├── vector-viz.js          # 2D vector space renderer
│   │   ├── graph-viz.js           # Function/data plotter
│   │   ├── step-viz.js            # Step-by-step math solutions
│   │   └── system-viz.js          # Systems of equations (geometric view)
│   │
│   ├── engine/
│   │   ├── animation.js           # Animation timeline & playback engine
│   │   ├── canvas-renderer.js     # Canvas drawing utilities (grid, arrows, etc.)
│   │   └── math-engine.js         # Matrix/vector computation helpers
│   │
│   ├── api/
│   │   ├── schema.js              # MathViz Spec validation
│   │   └── parser.js              # Parse + normalize incoming specs
│   │
│   └── utils/
│       ├── colors.js              # Color palette + auto-assignment
│       └── katex-helpers.js       # KaTeX rendering utilities
│
├── public/
│   └── examples/                  # Built-in example specs
│       ├── rref.json
│       ├── vectors.json
│       ├── linear-transform.json
│       ├── graph.json
│       └── step-by-step.json
│
└── AI_PROMPT.md                   # Prompt template that teaches AI to use MathViz
```

---

## Proposed Changes

### Phase 1: Project Foundation

#### [NEW] package.json
- Vite as dev dependency
- KaTeX and math.js as dependencies

#### [NEW] vite.config.js
- Basic Vite configuration

#### [NEW] index.html
- App shell HTML with sidebar, main canvas area, input panel, toolbar

#### [NEW] src/style.css
Complete design system:
- **Dark theme**: Deep navy background (#0a0e1a), glassmorphic cards
- **Color palette**: Electric blue (#4f8ff7), violet (#7c5cfc), amber (#f7b74f) accents
- **Typography**: Inter for UI, JetBrains Mono for code/math
- **Animations**: Defined keyframes for fade, slide, pulse, highlight
- **Layout**: CSS Grid for main layout, flexbox for components
- **Matrix cells**: Grid layout with transition support for row operations
- **Playback toolbar**: Sleek bottom bar with glass effect

#### [NEW] src/main.js
- Initialize the app, load fonts, set up KaTeX

#### [NEW] src/app.js
- App router: load the right visualizer based on spec type
- Manage state: current spec, current step, playback state
- Wire up sidebar, input panel, toolbar

---

### Phase 2: Core Engine

#### [NEW] src/engine/animation.js
Animation timeline system:
- `Timeline` class with steps, durations, easing
- Playback controls: play(), pause(), stepForward(), stepBack(), setSpeed()
- Event callbacks: onStepChange, onComplete
- Interpolation helpers for smooth transitions

#### [NEW] src/engine/canvas-renderer.js
Canvas drawing utilities:
- `drawGrid()` — coordinate grid with labels
- `drawVector()` — vector with arrowhead and label
- `drawLine()` — line from equation
- `drawFunction()` — plot a function curve
- `drawPoint()` — labeled point
- `animateVector()` — smoothly draw a vector
- `animateTransform()` — morph grid under linear transformation

#### [NEW] src/engine/math-engine.js
Math computation helpers:
- `computeRREF(matrix)` — returns steps array
- `applyRowOp(matrix, operation)` — apply one row operation
- `vectorAdd()`, `scalarMult()` — vector arithmetic
- `matrixMultiply()` — with intermediate step tracking
- `eigenvalues()`, `eigenvectors()` — computation
- `evaluateFunction(expr, x)` — evaluate math expression

---

### Phase 3: UI Components

#### [NEW] src/components/sidebar.js
- Category list: Matrices, Vectors, Graphs, Steps, Systems
- Built-in example gallery (clickable thumbnails)
- Collapsible on mobile

#### [NEW] src/components/toolbar.js
- Playback controls: ⏮ ⏪ ▶️/⏸ ⏩ ⏭
- Step counter: "Step 3 of 7"
- Speed slider: 0.5x — 3x
- Progress bar with clickable step indicators

#### [NEW] src/components/input-panel.js
- Collapsible panel with JSON editor (textarea with basic syntax highlighting)
- "Load Example" dropdown
- "Render" button
- Validation status indicator
- Formatted error messages

#### [NEW] src/components/info-panel.js
- Current step title and description
- KaTeX-rendered math explanation
- Before/after comparison
- Expandable detailed explanation

---

### Phase 4: Visualization Modules

#### [NEW] src/visualizers/matrix-viz.js
**Matrix Operations Visualizer**
- Render matrix as CSS Grid of cells with borders and brackets
- Row operation animations:
  - **Scale**: Highlight row, animate cell values changing
  - **Swap**: Animate rows sliding past each other
  - **Add**: Highlight source row, show scalar multiply, add to target, animate result
- REF/RREF mode: Auto-compute all steps from initial matrix
- Manual mode: Follow provided operations list
- Features:
  - Augmented matrix support (vertical line separator)
  - Color-coded pivots
  - Zero cells dimmed
  - Step annotations with KaTeX

#### [NEW] src/visualizers/vector-viz.js
**2D Vector Space Visualizer**
- Canvas-based coordinate grid with axis labels
- Draw vectors as arrows from origin with labels
- Animations:
  - **Vector addition**: Show parallelogram method
  - **Scalar multiplication**: Stretch/shrink vector
  - **Linear combination**: Build result from components
  - **Span**: Shade region covered by span of vectors
  - **Linear transformation**: Morph the entire grid, show how basis vectors transform
  - **Projection**: Show projection of one vector onto another
- Interactive: Hover to see components, click to highlight

#### [NEW] src/visualizers/graph-viz.js
**Function & Data Plotter**
- Canvas-based with auto-scaling axes
- Plot multiple functions with different colors
- Animated curve drawing (progressive reveal)
- Features:
  - Function intersection highlighting
  - Tangent line at a point
  - Area under curve (shaded)
  - Derivative visualization
  - Data point scatter plots
  - Regression line fitting

#### [NEW] src/visualizers/step-viz.js
**Step-by-Step Math Display**
- Vertical timeline of math steps
- Each step rendered with KaTeX
- Highlight what changed between steps (diff-style)
- Expandable explanation for each step
- Auto-scroll to current step during playback
- Support for:
  - Equation solving
  - Derivatives/integrals
  - Proof steps
  - Any sequential math process

#### [NEW] src/visualizers/system-viz.js
**Systems of Equations Visualizer**
- Split view: equations + geometric interpretation
- 2D: Show lines and their intersection
- Animate adding each equation/line
- Show augmented matrix alongside
- Color-code each equation and its corresponding line
- Handle: unique solution, no solution (parallel), infinite solutions (same line)

---

### Phase 5: API & AI Integration

#### [NEW] src/api/schema.js
- JSON schema definitions for each visualization type
- Validation function with helpful error messages

#### [NEW] src/api/parser.js
- Parse incoming specs
- Normalize formats (e.g., accept both `[[1,2],[3,4]]` and `"1 2; 3 4"` for matrices)
- Auto-detect visualization type if not specified

#### [NEW] AI_PROMPT.md
Markdown document containing:
- Complete MathViz Spec reference
- Examples for each visualization type
- Instructions an AI should follow to generate specs
- Template that users can paste into their AI conversations

#### [NEW] public/examples/*.json
Built-in example specs demonstrating each visualizer

---

### Phase 6: Utility Modules

#### [NEW] src/utils/colors.js
- Predefined color palettes for vectors, functions, matrix highlights
- Auto-assign distinct colors to unnamed elements
- Color interpolation for smooth transitions

#### [NEW] src/utils/katex-helpers.js
- Render math string to HTML element
- Matrix-to-LaTeX converter
- Vector notation formatter
- Step diff highlighter

---

## MathViz Spec Format (JSON API)

This is the structured format that AI generates:

### Matrix Operations
```json
{
  "viz": "matrix",
  "title": "Row Reduce to RREF",
  "matrix": [[2,1,-1,8], [-3,-1,2,-11], [-2,1,2,-3]],
  "augmented": 3,
  "mode": "auto",
  "steps": []
}
```
When `mode: "auto"`, MathViz computes all REF/RREF steps automatically from the initial matrix.

### 2D Vectors
```json
{
  "viz": "vectors2d",
  "title": "Linear Combination",
  "gridRange": [-6, 6],
  "vectors": [
    {"id": "v", "components": [3, 1], "color": "#4f8ff7", "label": "v⃗"},
    {"id": "w", "components": [1, 3], "color": "#f74f8f", "label": "w⃗"}
  ],
  "animations": [
    {"type": "show", "targets": ["v", "w"]},
    {"type": "linearCombination", "scalars": [2, -1], "vectors": ["v", "w"], "resultLabel": "2v⃗ - w⃗"}
  ]
}
```

### Function Graph
```json
{
  "viz": "graph",
  "title": "Quadratic vs Linear",
  "xRange": [-5, 5],
  "yRange": [-2, 25],
  "functions": [
    {"expr": "x^2", "color": "#4f8ff7", "label": "f(x) = x²"},
    {"expr": "2*x + 3", "color": "#f74f8f", "label": "g(x) = 2x + 3"}
  ],
  "animations": [
    {"type": "drawFunction", "target": 0},
    {"type": "drawFunction", "target": 1},
    {"type": "showIntersections"}
  ]
}
```

### Step-by-Step
```json
{
  "viz": "steps",
  "title": "Solving a Quadratic Equation",
  "steps": [
    {"math": "2x^2 + 5x - 3 = 0", "explanation": "Start with the equation"},
    {"math": "x = \\frac{-5 \\pm \\sqrt{25 + 24}}{4}", "explanation": "Apply quadratic formula"},
    {"math": "x = \\frac{-5 \\pm 7}{4}", "explanation": "Simplify the discriminant"},
    {"math": "x = \\frac{1}{2} \\text{ or } x = -3", "explanation": "Two solutions"}
  ]
}
```

### Systems of Equations
```json
{
  "viz": "system",
  "title": "Two Lines Intersecting",
  "equations": [
    {"a": 1, "b": -1, "c": 1, "label": "x - y = 1"},
    {"a": 2, "b": 1, "c": 5, "label": "2x + y = 5"}
  ],
  "showAugmented": true
}
```

---

## Design Direction

### Visual Theme
- **Background**: Deep space navy (#0a0e1a) with subtle gradient
- **Cards**: Glassmorphic panels with `backdrop-filter: blur(20px)` and soft borders
- **Accents**: Electric blue (#4f8ff7) → violet (#7c5cfc) gradients
- **Matrix cells**: Dark cards with amber (#f7b74f) highlights on active operations
- **Vectors**: Vibrant, auto-assigned colors with glow effects
- **Typography**: Inter 400/500/600 for UI, JetBrains Mono for code/values
- **Animations**: Smooth 400-600ms transitions, spring-like easing

### Key UI Elements
- Floating glassmorphic sidebar with category icons
- Central visualization area with subtle grid background
- Bottom toolbar with playback controls and progress indicator
- Slide-up input panel for spec editing
- Step info cards that animate in/out

---

## Open Questions

> [!IMPORTANT]
> **Deployment**: Would you like this deployed somewhere (e.g., Cloud Run, Vercel) or is running locally with `npm run dev` sufficient for now?

> [!IMPORTANT]
> **3D Vectors**: Is 2D visualization sufficient for your linear algebra studies, or do you need 3D vector space visualization (e.g., R³ with rotation)? This would add significant complexity with Three.js.

> [!IMPORTANT]
> **Interactivity Level**: Should vectors/graphs be interactive (draggable, zoomable, hoverable) or is playback-only animation sufficient?

---

## Verification Plan

### Automated Tests
```bash
npm run dev          # Verify dev server starts
npm run build        # Verify production build succeeds
```

### Manual Verification
- Load each built-in example and verify it renders correctly
- Test playback controls (play through, step forward/back, speed change)
- Test JSON input: paste a spec and verify rendering
- Test matrix RREF auto-computation against known results
- Verify KaTeX rendering of all math expressions
- Test responsive layout at different viewport sizes
- Verify animations are smooth (60fps)

### Browser Testing
- Use browser subagent to navigate to the running app
- Load each visualization type
- Verify visual quality meets design standards
- Test playback interaction
