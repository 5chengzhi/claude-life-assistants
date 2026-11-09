# Wardrobe & stylist assistant

**Usefulness in my build: 8/10.** The most-used of the three: getting dressed happens every day, and sending Claude an outfit photo is easy. The catalog takes the longest to build, so do it in batches.

## Gather first (30–60 min, can be split)

- [ ] One photo per item, laid flat or hung, on a plain background, in daylight. Do one category at a time (outerwear, knits, shirts, pants, shoes, accessories).
- [ ] A close-up of each tag: brand, size, fabric, care symbols. These make the care and fit notes real instead of guessed.
- [ ] 2–3 photos of you in outfits you like and 2–3 you don't.
- [ ] Your occasions: e.g. campus, work, church, dates, events, gym.
- [ ] Optional: what you paid for your main pieces (enables cost-per-wear later).

## Build prompt (paste with one batch of photos)

```text
[paste prompts/00-shared-rules.md here]

Build me a wardrobe assistant. My occasions are: [list]. My style goal in one line: [e.g. "smart casual that looks put together without trying"].

From the attached photos, create one record per item in the page's reference data:
{id, name, category (outer|knit|shirt|pants|shoes|acc|lounge), color, color family, formality 0–5, seasons, texture, pattern,
 fit verdict (core|keep|later|lounge|go), brand, size, fabric, care}.
Read brand/size/fabric/care from the tag photos. If a value is inferred rather than read, mark it "(inferred)".

Version 1 has 3 tabs:
1. CLOSET — "What do I own?" Grid of items with search by any tag and filters. Tap an item for its card.
2. OUTFITS — "Does this outfit work?" Pick layer/top/bottom/shoes/belt/extra. Show live checks with green/amber/red dots:
   formality spread, one statement piece at most, belt leather matches shoes, season fits today, a dark/light anchor.
   Explain each check in one line. Save outfits; "Log as worn" writes {date, fit, occ, notes, comfort 1–5, confidence 1–5} to db collection "worn".
3. INSIGHTS — "What's working?" Most and least worn items, items never worn in 30 days, wear count per occasion.

Ask me questions before building. Then publish. I'll send more photo batches after.
```

## Add-ons

- **Planner:** *"Plan my outfits for the week from my calendar's occasions and the forecast I give you."*
- **Learn tab:** short lessons tied to my own pieces (why the check flagged something), not generic style tips.
- **Photos on items:** *"Store each item's photo with the assets capability and show it in the grid."*

## Daily / weekly loop

When you get dressed for anything that matters, send a mirror photo:

```text
Outfit for [occasion] — rate it 1–10 for that occasion, name the one change that would raise it most using only pieces I own,
and log it as worn in [artifact link] with comfort [1–5] and confidence [1–5].
```

Weekly: *"From my worn log, what did I over-rely on, what didn't I wear, and plan 5 outfits for next week."*

Fill in comfort, confidence and prices from the start. My build skipped them, and the insights that depend on them stayed empty.
