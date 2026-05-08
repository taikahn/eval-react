# eval-react Product Page Testing

A few notes on the testing flow for the product page.

## Step 1 — Visual parity check

After step 1 completes, it should print a URL and port. If it doesn't, run `npm run dev` to spin up the webserver yourself.

Open the page and compare it against the original HTML version. You're looking for the React render to match the HTML render — common things that break:

- Font sizes blown up larger than the original
- Product cards rendering at the wrong size
- Page-level misalignment

## Steps 2–4 — Run straight through

Run steps 2, 3, and 4 back-to-back. No input needed between them — just let them finish.

## After step 4 — Grid alignment check

Toggle the grid layer and verify the page lines up with it. Two things worth knowing:

- It's fine to resize or reposition the browser window to get the grid to line up with elements.
- You're checking that elements are *mostly* the right size and spaced correctly relative to each other — not pixel-perfect alignment.

If that looks good, you're done.
