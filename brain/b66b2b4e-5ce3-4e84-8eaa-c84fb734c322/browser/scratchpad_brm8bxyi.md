# Task: Find a simple TypeScript example for `manim-js`

## Plan:
1. Navigate to `https://maloyan.github.io/manim-js/`.
2. Find a TypeScript example of initializing a scene and playing an animation.
3. Look for documentation on using it in a web application (e.g. Vite).
4. Return the code snippet and setup instructions.

## Findings:
- **Project Name**: `manim-web` (formerly or related to `manim-js`).
- **Official Documentation**: `https://maloyan.github.io/manim-web/`
- **Installation**: `npm install manim-web`
- **Setup with Vite**:
  ```bash
  npm create vite@latest my-animation -- --template vanilla-ts
  cd my-animation
  npm install manim-web
  ```
- **Basic TypeScript Example**:
  ```typescript
  import { Scene, Circle, Create, FadeOut, BLACK } from 'manim-web';

  const container = document.getElementById('container')!;
  const scene = new Scene(container, {
    width: 800,
    height: 450,
    backgroundColor: BLACK,
  });

  const circle = new Circle({ radius: 1.5 });
  await scene.play(new Create(circle));
  await scene.wait(1);
  await scene.play(new FadeOut(circle));
  ```
- **React Integration**:
  ```typescript
  import { ManimScene } from 'manim-web/react';
  import { Circle, Create } from 'manim-web';

  function App() {
    return (
      <ManimScene
        width={800}
        height={450}
        setup={async (scene) => {
          const circle = new Circle({ radius: 1.5 });
          await scene.play(new Create(circle));
        }}
      />
    );
  }
  ```
- **HTML Container**:
  ```html
  <div id="container"></div>
  <script type="module" src="./your-scene.ts"></script>
  ```
