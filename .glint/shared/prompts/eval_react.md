# Eval task: 8px grid refactor

You are given a single static page, `product.html`, for a fictional product called **foo**. The spacing is deliberately off — paddings, margins, gaps, and font-sizes do not sit on any consistent grid. Your job is to turn it into a React + Vite app, fix the spacing to a strict 8px grid, build a tool that proves the fix, and verify the result.

Work through the four steps in order. Do not skip ahead.

## Step 1 — Port `product.html` into a Vite + React single-page app

- Scaffold a fresh Vite + React project in this directory using the **JavaScript** template (not TypeScript). Use `npm create vite@latest -- --template react` and pick the non-interactive flags so no prompts are shown to the user during setup.
- Move all of `product.html`'s markup into React components (`.jsx` files only). You may split into multiple components or keep it as one — whatever is cleanest.
- Inline CSS from the HTML should become real styles (CSS file, CSS modules, or styled JSX — pick one and stay consistent). Do **not** preserve the inline `style="..."` attributes verbatim; they should become regular CSS rules.
- At the end of this step, `npm run dev` should serve a page that is **visually identical** to `product.html` — same off-grid spacing and all. Do not start fixing spacing yet.

## Step 2 — Refactor every spacing and sizing value onto a strict 8px grid

- Every `padding`, `margin`, `gap`, `width`, `height`, `font-size`, `line-height`, `border-radius`, and absolute position offset must be a multiple of **8px**. 4px is allowed only for tight inline spacing (e.g. icon-to-text gap inside a button) and only if you can justify it.
- Adjust the layout holistically — do not just round each value to the nearest 8. Make the page look intentional and well-proportioned on the grid.
- The page should still look like the same product page (same content, same general layout), just cleaner and properly aligned.

## Step 3 — Add an 8px grid overlay toggle

- Add a small toggle button **fixed to the top-right corner** of the viewport (above all other content, visible on every scroll position).
- When toggled on, render a semi-transparent overlay that draws horizontal and vertical lines every 8px across the entire viewport. The overlay must not block clicks on the page underneath (`pointer-events: none`).
- The toggle itself remains clickable while the overlay is on.
- The overlay state can be local component state — no need for routing or persistence.

## Step 4 — Build, run, and hand off for manual verification

- Run `npm run build` and confirm it succeeds with no errors.
- Run `npm run dev` (or `npm run preview` after the build) and leave the dev server running.
- Then prompt the user (the tester) to manually check the following and report back:
  1. Does the page render correctly and look like a polished version of the original?
  2. Does the top-right toggle turn the 8px grid overlay on and off?
  3. With the overlay on, do **all** element edges (text baselines, button edges, image edges, container edges) sit on the 8px lines?

Do not declare the task done until the user has confirmed all three.
