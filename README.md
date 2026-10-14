# Tangram Helper

A static web page that gives TDS students their verified Tangram arrangement and an
officially signed completion token for the GA1 `q-visual-game-tangram` question.

**Live page:** `https://<your-username>.github.io/<repo-name>/` (enable GitHub Pages after pushing)

## What it does

1. Student enters their registered exam email.
2. The page derives which of the 100 silhouettes is assigned to that email
   (same deterministic assignment the exam uses).
3. It shows the **verified piece arrangement**: a colored diagram with per-piece
   rotation/flip, the exact state JSON, and a placement table.
4. It requests a **signed token from the official game server**
   (`tds-network-games.sanand.workers.dev/api/visual/attest`) — the same endpoint the
   game board itself uses — and displays the receipt only if the server confirms
   `invalid:false` and the arrangement's `match_key` equals the target's expected hash.
5. The student copies the token into the exam answer box themselves.

The server holds the signing key; this page only supplies verified geometry.
The email is used solely for that attestation request and is never stored.

## Deploying

Everything is bundled into a single `index.html` (answer table, target pool,
seedrandom, exact geometry). Host it as a static page:

- **GitHub Pages:** push `index.html` to a repo, then Settings → Pages →
  Deploy from branch → select branch/root. Done.
- Any other static host (Netlify, Cloudflare Pages) works identically.

## Rebuilding after new verified answers

The page is generated — don't edit it by hand. From the project folder
(`tangram-solver/`):

```sh
node make-portal.mjs     # regenerates portal-site/index.html from solutions.json
```

The build refuses to bundle any answer that doesn't pass the game's own
exact-hash verification, so the page can never serve a wrong arrangement.
Currently bundled: **96 of 100** targets.

## Files

- `index.html` — the whole portal (copy of `portal-site/index.html`)
- Nothing else is required at runtime.
