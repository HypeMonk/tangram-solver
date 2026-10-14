# Tangram Helper

A static web page that gives TDS students their verified Tangram arrangement and an
officially signed completion token for the GA1 `q-visual-game-tangram` question.

**Live page:** https://hypemonk.github.io/tangram-solver/

**Repository:** https://github.com/HypeMonk/tangram-solver

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

All **100 targets in the Sept 2026 GA1 pool now have verified arrangements**, including
the four previously pending puzzles: tg-004, tg-060, tg-077 and tg-088. There is no
manual-solving requirement for any target in this pool. The page retains a safe
unavailable-answer fallback for future dataset changes.

The redesigned page includes numbered pieces, target/solution tabs, collapsible
state and placement details, light/dark themes, request retries, and copy fallbacks.
Changing email clears stale results before another request.

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
node make-portal.mjs     # verifies answers and regenerates portal-site/index.html
```

Copy `portal-site/index.html` over this folder's `index.html`, then publish the
updated file. The editable design lives in `portal-template.html` and the browser
logic in `portal-app.js`; both are embedded by the generator.

The build refuses to bundle any answer that doesn't pass the game's own
exact-hash verification. The page also checks the official response's email and
target claims before enabling the token-copy button. The assignment remains the
final judge of a submitted token. Currently bundled: **100 of 100** targets.

## Files

- `index.html` — the whole portal (copy of `portal-site/index.html`)
- Nothing else is required at runtime.
