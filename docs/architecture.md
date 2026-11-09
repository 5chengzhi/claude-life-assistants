# Architecture

Each assistant is **one HTML file**, published as a Claude Artifact. No server, no build step, no framework.

```
┌──────────────────────── one HTML file ────────────────────────┐
│  <style>    design tokens (light + dark), layout               │
│  <script>   REFERENCE DATA (written into the page)             │
│             e.g. clothing catalog, factory specs, light guide  │
│  <script>   UI: tabs, lists, forms, checks, 3D, charts         │
│                 │                       │                      │
│                 ▼                       ▼                      │
│        claude.use("db")         claude.use("sample")           │
│        records that grow        "Ask Claude" box,              │
│        (logs, check-ins)        photo rating                   │
└────────────────┬────────────────────────┬──────────────────────┘
                 │                        │
     Artifact database            Claude, billed to the viewer
     (Claude can also read/write  (asks consent on first use)
      it from chat)
```

## Where data lives

| Kind of data | Where | Why | Example |
|---|---|---|---|
| Reference that rarely changes | A JS array in the page | Fast, versioned with the page, no setup | 99 clothing items with tags; car specs |
| Records that grow | `db` collections | Survive republishing; Claude can write them from chat | `log`, `worn`, `checkins`, `dents` |
| One "current state" object | One `db` doc | Settings and odometer in one place | `car/state`, `app/state` |
| Per-device conveniences | `localStorage` | Not worth syncing | Last open tab |
| Photos | `assets` capability, id stored in the `db` row | Images too big for a row | Outfit photos, dent photos |

**Rule:** if Claude will ever need to add or read it from chat, it goes in `db`, not in the page source.

## The runtime calls (Claude Artifacts)

Inside a published Artifact, the page reaches capabilities through `claude.use(name)`. It returns `null` when the capability isn't available (not granted, signed out, opened outside Claude), so every page must work without it.

```js
// Runs inside an async function (see template/starter.html for the full version).
// Storage: use the Artifact database when present, else the browser.
const db = window.claude?.use ? await claude.use("db") : null;
if (db) {
  const snap = await db.collection("log").get();          // read all records
  const rows = snap.docs.map(d => ({ id: d.id, ...d.data() }));
  await db.collection("log").doc("2026-11-09").set({ work: "Oil change", cost: 65 });
}

// AI: ask Claude with the page's own data attached.
const sample = window.claude?.use ? await claude.use("sample") : null;
if (sample) {
  const { text } = await sample(
    "You are my car assistant. Data: " + JSON.stringify(rows) + "\nQuestion: what's due?",
    { onText: ({ text }) => (answer.textContent = text), cache: false }
  );
}
```

Declare what the page uses when publishing, e.g. `capabilities: { db: {}, sample: {}, user: {} }`. In practice you just tell Claude "publish this with saved data and an Ask Claude box" and it declares them.

Key behaviors (from the runtime's type definitions):
- `use()` never shows a prompt; consent prompts appear on the first real call (first `sample()` asks the viewer to allow it; the viewer pays for usage).
- `sample` has no memory between calls. Every call must include instructions, the relevant page data, and the output format.
- Don't hard-code seed records in the page. Publish first, then have Claude write the starting records into `db`.

## Patterns that worked

**Live checks instead of advice.** The outfit builder and the floor plan don't tell you what to do in prose; they run rules on what you've picked and show green/amber/red dots ("belt leather doesn't match shoes", "door swing hits the dresser", "walking path under 18 in"). Rules are plain JS over the item tags.

**Status computed, not typed.** Each maintenance item stores its interval ("every 5,000 mi or 6 months") and when it was last done. The page computes whichever limit comes first against today's date and the current odometer, and shows ok / soon (≥85% of the interval) / overdue. Update the odometer or the last-done entry and every status updates.

**One "Do next" box.** Each page opens with the 1–3 most urgent things, computed from the data. This is the part I look at most.

**3D as an index, not decoration.** The car model is useful because tapping a part opens its card: what it does, how it fails, the words a mechanic uses, and *its status from my logbook*. A 3D model without that link is a toy.

**Honest labels.** Every estimate is marked: "(fabric)" for inferred care, "schematic, not CAD-accurate" on the 3D car, "my estimates from your photos" on room sizes, price sources named.

## Size and libraries

| Page | Size | Libraries |
|---|---|---|
| Car | ~200 KB | three.js r128 + OrbitControls, Chart.js 4.4.1 |
| Closet | ~170 KB (+ ~100 photo files) | Chart.js 4.4.1 |
| Room | ~130 KB | three.js r128 + OrbitControls, Chart.js 4.4.1 |

Load libraries from a CDN with an exact version pinned (e.g. `three.js/r128`, `chart.js@4.4.1`), never "latest".
