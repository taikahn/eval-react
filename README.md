# eval-react — 8px grid refactor eval

A self-contained eval task for measuring how well an AI coding agent can plan, execute, and self-test a small frontend refactor. The agent is handed a deliberately off-grid HTML page and asked to convert it into a React app, fix the spacing, build a verification tool, and hand off for manual check.

## What's in this directory

| File | Purpose |
|------|---------|
| `prompt.md` | The 4-step task given to the agent under test. This is the only instruction the agent should receive. |
| `product.html` | Static input. A fake "foo" shopping cart product page with intentionally inconsistent spacing (paddings/margins/font-sizes scattered across 7, 9, 11, 13, 17, 19, 22, 23, 27, 31 px etc.). The agent must port this to React and fix the spacing. |
| `README.md` | This file — setup and testing instructions for the human running the eval. |

## Prerequisites (macOS 26)

You need **Node.js 20 or newer** and **npm**. Verify:

```sh
node --version    # should print v20.x or higher
npm --version
```

If Node is missing or too old, install via Homebrew:

```sh
brew install node
```

Or use `nvm` if you prefer version management:

```sh
brew install nvm
nvm install 20
nvm use 20
```

A modern browser (Safari 18+, Chrome, or Firefox) is needed for the manual verification step.

## Running the eval

### 1. Sanity-check the input page

Before handing anything to the agent, confirm `product.html` renders as expected:

```sh
open product.html
```

You should see a "fooMart" product page for "foo" — header, product image, price, buy buttons, related products, footer. The spacing should look slightly *off* (mismatched gaps, uneven button heights, etc.). That's intentional — it's what the agent has to fix.

### 2. Hand the prompt to the agent

Open the agent under test (Claude Code, Cursor, etc.) in this directory and give it `prompt.md` as the task. For example, in Claude Code:

```sh
cd "$(pwd)"
claude
```

Then prompt: `Read prompt.md and complete the task.`

The agent should work through all four steps without further input from you until step 4.

### 3. What to watch for during the run

- **Step 1 (port to React + Vite):** Agent should run `npm create vite@latest -- --template react` (JavaScript, **not** TypeScript) and produce a project where `npm run dev` shows the same off-grid page.
- **Step 2 (8px grid refactor):** Every spacing/sizing value should become a multiple of 8 (4 allowed for tight inline gaps).
- **Step 3 (overlay toggle):** A button fixed in the top-right corner that toggles a semi-transparent grid overlay every 8px across the viewport. Overlay must use `pointer-events: none` so clicks pass through.
- **Step 4 (build + handoff):** `npm run build` should succeed. The agent should leave a dev server running and prompt **you** to verify.

### 4. Manual verification

When the agent hands off, open the dev server URL (usually `http://localhost:5173`) and check:

1. **Page renders correctly** — same content as the original, looks polished, nothing broken or missing.
2. **Toggle works** — clicking the top-right button turns the 8px overlay on and off cleanly. The toggle stays clickable while overlay is on; the rest of the page also stays clickable.
3. **Everything snaps to grid** — with the overlay on, scan the page. Text baselines, button edges, image edges, and container edges should all sit on the 8px lines. Any element that's off-grid is a fail for step 2.

Report all three results back to the agent. The task is only complete when all three pass.

## Resetting between runs

If you want to re-run the eval from scratch, delete everything the agent generated and keep only the three source files:

```sh
# from inside this directory
ls | grep -vE '^(prompt\.md|product\.html|README\.md)$' | xargs rm -rf
```

Double-check the `ls` output before piping to `rm` — that command deletes anything not in the allow-list.

## Scoring rubric (suggested)

| Step | Pass criteria |
|------|---------------|
| 1 | Vite + React (JavaScript) scaffolded; `npm run dev` shows the page; visually matches original (still off-grid). |
| 2 | All `padding` / `margin` / `gap` / `width` / `height` / `font-size` / `line-height` / `border-radius` values are multiples of 8 (4 allowed for tight inline gaps). Layout still looks intentional. |
| 3 | Top-right toggle button exists, is always visible, toggles a viewport-wide 8px grid overlay. Overlay is `pointer-events: none`. |
| 4 | `npm run build` succeeds. Agent prompts the user to verify rather than self-declaring success. All three manual checks pass. |
