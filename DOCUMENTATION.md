# Hunch — Technical Documentation

Developer-facing reference for how Hunch is built. For the player-facing overview see
[README.md](README.md); for backend setup see [FIREBASE_SETUP.md](FIREBASE_SETUP.md).

## What it is

Hunch is a *blind top-ranking* game. A setter defines a secret ordered list; the player
is shown the items one at a time and must lock each into a rank before seeing the next.
Score is based on how close each placement is to the real position.

The entire application — markup, styles, data and logic — lives in a single file,
[`index.html`](index.html) (~950 lines). There is **no build step, no bundler and no npm
dependency**. It runs by opening the file in a browser. Firebase is loaded at runtime as an
ES-module import from a CDN, and only when a project config is present.

```
Hunch/
├── index.html         The whole app: <style>, <body> markup, <script> logic
├── firestore.rules    Server-side Firestore security rules (the real backend guard)
├── firebase.json      Points the Firebase CLI at firestore.rules
├── .firebaserc        Default Firebase project id (hunch-game-79416)
├── README.md          Player-facing overview
├── FIREBASE_SETUP.md  How to stand up the Firebase project
└── DOCUMENTATION.md   This file
```

## Architecture at a glance

`index.html` is organised into labelled sections inside one `<script>`:

| Section | Lines (approx.) | Responsibility |
|---|---|---|
| Data | 248–430 | `CATS` (categories), `PRESETS` (ready-made rankings), `FACTS` (home banner) |
| Helpers | 432–454 | `store` (localStorage), escaping, base64 encode/decode, shuffle, scoring |
| Online sharing | 456–506 | Firebase init, auth, `publish()` with the daily-limit batch |
| Leaderboards | 508–558 | Per-ranking boards and the all-time players board |
| Sound / motion / effects | 560–627 | `SFX`, `confetti`, `countUp`, animated home hero |
| Router | 629–635 | `go(view)` / `view()` scroll + state helpers |
| Views | 637–938 | `renderHome`, `renderCreate`, `renderPlay`, `renderRead` |
| Boot | 939–945 | Parse the URL hash, pick the first view, start Firebase |

### Rendering model

There is no framework. Each "view" function builds an HTML string and assigns it to
`$app.innerHTML` (`$app` is the single `#app` container). Event handlers are wired by
grabbing elements with `document.getElementById(...)` right after the string is injected.
All user-supplied strings pass through `esc()` before entering HTML.

State is a handful of module-level variables:

- `user` — the signed-in Firebase user (or `null`).
- `community` — the live array of published rankings (kept current by a Firestore snapshot listener).
- `current` — coarse view name used to decide whether a background update should re-render home.
- `filter` — the active category filter on the home page.

## Data shapes

A **ranking** is the core object, stored and transmitted as:

```js
{ t: 'Planets by size', c: 'Science', i: ['Jupiter', 'Saturn', /* #1 downward */] }
```

- `t` — title (≤ 80 chars)
- `c` — category key (must exist in `CATS`)
- `i` — items **in correct order, #1 first** (3–10 entries)

Published rankings add `uid`, `name`, `createdAt`, and a Firestore doc `id`.

A **Mind-Reader payload** extends a ranking with `g` — the player's guessed order — and is
validated by `decRead()` to ensure `g` is a permutation of `i`.

### localStorage keys

- `blindrank_mine` — the user's locally created rankings (array).
- `blindrank_stats` — `{ played, best, points, bests }` where `bests` maps a ranking key to its best percentage.

`store` (index.html:435) wraps both keys; every read/write is in a try/catch so private-mode
or disabled storage degrades silently rather than throwing.

## Scoring

Scoring is positional distance, computed in `renderPlay`'s `result()` (index.html:832):

```
distance 0 (exact)      → 100 points
distance 1 (off by one) →  60 points   ← ptsFor(d) = [100,60,30][d] || 0
distance 2 (off by two) →  30 points
distance ≥ 3            →   0 points
```

Percentage is `round(totalPts / (N * 100) * 100)`, where `N` is the item count. Titles are
assigned by percentage: Rookie → Challenger (≥40) → Expert (≥60) → Master (≥80) → Legend (100).
A Wordle-style 🟩🟨🟥 grid is produced from the same per-item distances.

## Routing and link sharing

Routing is hash-based and parsed once at boot (index.html:941):

- `#play=<base64>` → decode a shared ranking and go straight to play (`renderPlay`).
- `#read=<base64>` → decode a Mind-Reader payload and play the predict-the-setter mode (`renderRead`).
- no hash → `renderHome`.

Payloads are JSON → UTF-8-safe base64 via `encObj` / `decObj`
(`btoa(unescape(encodeURIComponent(...)))`). Because the whole ranking travels in the URL,
shared links work with **no backend at all** — this is how "Challenge a friend" and the
Mind-Reader round trip function in local-only mode.

## Firebase integration (optional)

Firebase is entirely optional. `FB_ON` is true only when `FIREBASE_CONFIG.projectId` is set
(index.html:459). With it empty, every Firebase code path is skipped and the app is purely
local. The SDK is lazy-imported from `gstatic.com` inside `initFirebase()`, so there is no
cost when offline or unconfigured.

What Firebase adds:

- **Google sign-in** (`signInWithPopup`), surfaced in the top bar by `renderAuth()`.
- **Community rankings** — `publish()` writes to the `rankings` collection; a `onSnapshot`
  listener (newest 40) keeps `community` live and re-renders home on change.
- **Leaderboards** — a top-10 board per ranking (`scores/{rankingId}/entries/{uid}`) and an
  all-time board (`players/{uid}`).
- **Daily publish limit** — 5 per UTC day, enforced *both* in the client (`DAILY_LIMIT`) and in
  the rules.

### The daily-limit batch

`publish()` (index.html:499) writes the ranking and bumps the per-user/per-day counter in a
single `writeBatch`, so the two can't drift:

```
limits/{uid}_{year}_{month}_{day}.count += 1   (must end ≤ 5)
rankings/{autoId} = { t, c, i, uid, name, createdAt: serverTimestamp() }
```

### Security rules

[`firestore.rules`](firestore.rules) is the real authority — the client checks are only for UX.
Key guarantees:

- `rankings` are world-readable, but **create** requires auth, self-authorship (`uid ==
  auth.uid`), the exact field set, size bounds matching the data shapes above, and a matching
  `+1` bump of today's `limits` doc capped at 5. Rankings are **immutable** (`update: false`);
  only the creator may delete their own.
- `scores/{rid}/entries/{uid}` — a player may only write their own entry, and only to a
  **strictly higher** `pct` than before (best-score-only).
- `players/{uid}` — lifetime totals; each update must add exactly one to `played` and a
  non-negative, ≤1000 delta to `points`.

Keep `DAILY_LIMIT` in index.html in sync with the `<= 5` checks in the rules.

### Known limitation

Scores are submitted from the browser, so a determined cheater could POST a fake (but
in-range) score; the rules bound the values but can't verify the gameplay. Preventing that
needs Cloud Functions (paid plan) and is out of scope for the free Spark setup.

## Running and deploying

Open `index.html` directly, or serve the folder (needed for the Firebase module imports to
behave consistently):

```sh
python3 -m http.server 8000   # then http://localhost:8000
```

Deployment is static hosting — the live site runs on GitHub Pages. To change the backend,
follow [FIREBASE_SETUP.md](FIREBASE_SETUP.md) and paste the new config into `FIREBASE_CONFIG`.
The config values are public identifiers, not secrets; access is governed by the Firestore
rules, so update and publish `firestore.rules` (`firebase deploy --only firestore:rules`) when
you change the data model.

## Extending the game

- **Add a ranking topic** — append an object to `PRESETS` (index.html:264) with `t`, `c`
  (must be a key in `CATS`), and `i` ordered #1 first, 3–10 items.
- **Add a category** — add an entry to `CATS` (index.html:249) with an `icon` and `color`
  (a `--var` from the stylesheet).
- **Add a fun fact** — append a string to `FACTS` (index.html:325); the home banner rotates
  through them and avoids immediate repeats via `pickFact()`.
- **Change scoring** — edit `ptsFor` (index.html:451) and the title/message thresholds in the
  two `result()` blocks.
