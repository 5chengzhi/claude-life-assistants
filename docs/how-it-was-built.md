# How these were built

All three assistants were built the same way, in conversation with Claude in the Claude desktop app, in October and November 2026. No code was written by hand. This page describes the process I actually followed, including the parts that didn't work.

## The five-step pattern

### 1. Intake: turn the messy real world into records

Claude can read photos, PDFs and screenshots, so the first session of each build was mostly uploading:

| Assistant | What I uploaded | What Claude turned it into |
|---|---|---|
| Car | A CARFAX report going back to 2015, recent shop invoices, dashboard and engine-bay photos, photos of each dent | 40 log records (29 from CARFAX, the rest from invoices) with date, mileage, shop, work, cost and what the mechanic said; a watch list of 17 problems; 4 dent records with size and repair verdict |
| Closet | One photo per item (99 items), close-ups of tags, try-on photos | One record per item: type, color, color family, formality 0–5, seasons, fit verdict, fabric, care instructions |
| Room | Corner photos, tape-measure numbers, furniture sizes | Room and furniture dimensions in inches, a floor plan, a starting layout |

**Rule that mattered:** every number gets a source. Claude marked inferred values, e.g. care instructions "(fabric)" when guessed from the material rather than read off a tag, and room sizes as "my estimates from your photos" until I measured.

### 2. Decide where each piece of data lives

This is the one technical decision that matters. See [architecture.md](architecture.md) for detail.

- **Reference data that rarely changes** (the clothing catalog, the car's factory specs, the dashboard-light guide) is written directly into the page.
- **Records that grow** (service visits, outfits worn, check-ins, dents) go in the Artifact's database (`db`), so they survive page updates and Claude can read and write them from chat.
- **Per-device conveniences** (which tab you were on) go in browser storage.

### 3. Build the page around one question per tab

Each tab answers one question I actually ask:

- Car: "What's due?" (Logbook) · "What is this part?" (Inside, 3D) · "Is this dent worth fixing?" (Outside) · "What does this light mean?" (Cabin) · "What do I learn this week?" (Learn)
- Closet: "What do I own?" · "Does this outfit work?" (builder with live checks) · "What do I wear this week?" (Planner) · "Why?" (Learn) · "What's working?" (Insights)
- Room: "What do I do today?" · "Where should the furniture go?" · "What upkeep is due?" · "Is the room getting better?" (Check-ins) · "Where does this go?"

Libraries: three.js r128 for the 3D car and room, Chart.js 4.4.1 for trends. Everything else is plain HTML, CSS and JavaScript in one file.

### 4. Feed it from chat, not just from the page

The biggest surprise: **most of the useful data entry happened in chat, not in the page.** I'd send Claude a photo of an invoice or an outfit, and Claude would read it and write the record straight into the assistant's database. Examples from the real logs:

- An outfit photo → "Black vest added as the dark anchor. Worn unzipped: about 7." logged as a worn outfit.
- A shop invoice → a log entry plus a note that the coolant and CVT service was sent to another shop and billed differently from the quote, which became a question to ask the shop.
- A tire-shop receipt → a flag that the tires were set to 38 psi when the door placard says 33.

So design the page to **show** data well, and expect Claude to do much of the **typing**.

### 5. Rate, revise, repeat

After each version I asked Claude to score the assistant out of 10 against a rubric (see [rubric.md](rubric.md)), list problems, fix them, and re-score until 9/10. This caught real issues: tabs that duplicated each other, advice without sources, a check that flagged a correct layout as blocked, and a "brief" that told me what I already knew.

## What didn't work

- **Too many tabs.** The car page grew to 5 main tabs and 6 sub-tabs (about 200 KB). Several are rarely opened. Start with 3.
- **Features that need a habit I didn't have.** The room's weekly photo check-in, humidity log and sell list had zero entries after setup. The car's 24-week learning plan has no weeks marked done. A feature without a trigger (a time, a place, an existing habit) doesn't get used.
- **"Arrives on the 1st" without automation.** The car's monthly brief is great when it exists, but nothing generates it automatically yet; I have to ask. If a feature promises a schedule, set up a scheduled task for it.
- **Empty fields that analysis depends on.** The closet has cost-per-wear and comfort/confidence ratings, but prices and ratings were never filled in, so those insights show nothing.

## How they grew (start small, merge later)

The car assistant didn't start as one page. In mid-October 2026 I had three separate artifacts: a parts explainer with the 3D model ("Rogue Anatomy", Oct 12), a learning plan (Oct 12) and a service logbook (Oct 14). Once each had proven useful on its own, they were merged into one page with tabs, so the 3D model could show each part's status from the logbook. Building small single-purpose pages first, then merging the ones you keep using, is better than designing the big page up front.

I didn't time the sessions. The closet's saved state has been rewritten 32 times since it was created, which gives a sense of how much iteration a useful assistant takes. The prompts in this repo are meant to get you to a first usable version in one sitting by skipping the dead ends above.
