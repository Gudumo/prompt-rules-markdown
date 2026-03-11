# Frontend Rules

> The LLM reads this file **only for steps that produce frontend/UI code**. For all steps, read `PROMPT_RULES.md` first.

## Claude Code: frontend-design skill

When **all of these are true**:
- The assigned model is Claude
- You are running inside Claude Code (not a plain chat)
- The step produces visible UI (a page, layout, or component from scratch)

→ Invoke the `/frontend-design` skill **before writing any code**. It produces higher-quality, more distinctive UI than standard code generation and works well with the visual QA loop below.

If any condition is false (different model, plain chat, or the step is a minor tweak to existing UI), skip this and follow the conventions below directly.

## Conventions

These rules apply to all frontend code. The LLM must follow them.

1. **Semantic HTML** — use `<nav>`, `<main>`, `<section>`, `<button>`, etc. No `<div>` soup.
2. **Accessibility** — interactive elements must be keyboard-navigable. Images need `alt`. Forms need labels. Use ARIA only when native semantics are insufficient.
3. **Responsive design** — mobile-first. Every page/component must work at 320px and up.
4. **No inline styles** — use the project's styling approach. Exception: truly dynamic values (e.g., `style={{ width: computedWidth }}`).
5. **Component naming** — PascalCase for components, camelCase for hooks/utilities. One component per file.
6. **File structure** — co-locate component, styles, and tests:
   ```
   src/components/Button/
     Button.tsx
     Button.test.tsx
     Button.module.css   (or styles within the file if using Tailwind)
   ```
7. **No hardcoded strings** — user-facing text should be extractable for i18n. Use constants or a translation function.
8. **Images and assets** — optimize images (WebP/AVIF preferred). Use lazy loading for below-the-fold content.
9. **No `any` types** — if using TypeScript, define proper types/interfaces. No `as any` escape hatches.
10. **Performance** — avoid unnecessary re-renders. Memoize expensive computations. Lazy-load routes and heavy components.

---

## Design Reference & Visual QA

### Folder structure

```
.dev-plan/
  design-reference/
    inspiration/          — user drops screenshots of sites/components they like
      README.md           — optional notes per image (what you like about each)
      landing-hero.png
      dashboard-sidebar.png
      card-layout.png
      ...
    screenshots/          — LLM-generated screenshots of its own work
      18-F1-S3-hero-v1.png
      18-F1-S3-hero-v2.png
      ...
```

The user creates `design-reference/inspiration/` when setting up the repo. The LLM creates `design-reference/screenshots/` on first use.

### Inspiration README format

The user can optionally add notes in `inspiration/README.md` to guide the LLM:

```markdown
# Design Inspiration

## landing-hero.png
- Source: linear.app
- I like: clean typography, generous whitespace, subtle gradient background
- Don't copy: the specific color palette

## dashboard-sidebar.png
- Source: notion.so
- I like: icon style, compact density, hover states
- Don't copy: the exact layout — adapt to our content

## General direction
- Minimal, modern, not corporate
- Prefer muted tones with one accent color
- Lots of whitespace
- Subtle animations, nothing flashy
```

If no README exists, the LLM must still study the images and infer style direction from them.

### LLM visual QA protocol

When implementing a frontend step that produces visible UI:

1. **Before coding** — read all images in `design-reference/inspiration/` and `inspiration/README.md` (if it exists). Note the visual patterns: spacing, typography, color usage, layout, component style.

2. **After first implementation** — take a screenshot of the result:
   - Start the dev server (follow the Port Protocol in `PROMPT_RULES.md`).
   - Use a screenshot tool to capture the page/component. Suggested command:
     ```
     npx playwright screenshot http://localhost:<port>/<route> .dev-plan/design-reference/screenshots/<step-id>-<name>-v1.png --viewport-size=1440,900
     ```
   - For responsive checks, capture a mobile viewport too:
     ```
     npx playwright screenshot http://localhost:<port>/<route> .dev-plan/design-reference/screenshots/<step-id>-<name>-mobile-v1.png --viewport-size=375,812
     ```

3. **Compare** — open the screenshot and the inspiration images side by side. Evaluate:
   - **Layout**: Does the spacing and structure match the inspiration's feel?
   - **Typography**: Are font sizes, weights, and line heights in the same ballpark?
   - **Color**: Does the palette align with the direction given?
   - **Density**: Is the whitespace generous/tight enough to match?
   - **Components**: Do buttons, cards, inputs look consistent with the reference style?

4. **Iterate** — if the result doesn't match the inspiration well enough:
   - List the specific gaps (e.g., "hero section needs more vertical padding", "buttons too rounded compared to reference").
   - Fix the issues.
   - Take a new screenshot (`-v2.png`, `-v3.png`, ...).
   - Compare again. Repeat up to **3 visual iterations** per step.

5. **Report** — include in the COMPLETED block:
   ```
   Visual QA:
     Inspiration refs: landing-hero.png, card-layout.png
     Screenshots: 18-F1-S3-hero-v1.png, 18-F1-S3-hero-v2.png (final)
     Iterations: 2
     Notes: Increased hero padding to match linear.app feel. Card border-radius reduced.
   ```

### Screenshot naming convention

```
<step-id>-<descriptor>-v<N>.png
<step-id>-<descriptor>-mobile-v<N>.png
```

Examples: `18-F1-S3-hero-v1.png`, `18-F1-S3-hero-mobile-v2.png`

### When no dev server is available

If the step cannot be previewed in a browser (e.g., SSR-only, build required), the LLM must:
1. Note this in the COMPLETED block: `Visual QA: deferred — requires build`.
2. Add a MANUAL TEST asking the user to build and screenshot, or verify visually.
