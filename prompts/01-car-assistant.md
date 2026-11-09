# Car assistant

**Usefulness in my build: 8/10.** The part that pays for itself is the logbook plus Claude reading every invoice. Start there; add the 3D model later.

## Gather first (20 min)

- [ ] Year, make, model, trim, engine, transmission, drivetrain (on the registration or the door-jamb sticker)
- [ ] Service history: a CARFAX/AutoCheck report if you have one, plus every invoice or receipt you can find (photos or PDFs are fine)
- [ ] Current odometer reading (photo of the dash)
- [ ] Tire placard photo (driver door jamb): tire size and cold pressure
- [ ] Optional: photos of any warning lights you've seen, dents, rust spots
- [ ] Your owner's manual maintenance schedule (PDF from the manufacturer's site), so intervals come from a source

Don't include the VIN or plate in anything you plan to share (see [docs/privacy.md](../docs/privacy.md)).

## Build prompt (paste with the files attached)

```text
[paste prompts/00-shared-rules.md here]

Build me a car assistant for my [YEAR MAKE MODEL TRIM, engine, transmission, drivetrain].

Version 1 has 3 tabs:
1. LOGBOOK — "What's due, and what's been done?"
   - Read every attached invoice/report. One db record per visit in collection "log":
     {date, mi, shop, work, cost, said} where "said" is what the mechanic recommended or warned about, word for word where possible.
   - Collection "maint": one record per maintenance item with interval in miles and/or months FROM MY OWNER'S MANUAL (cite the page),
     plus lastMi and lastDate taken from the log. Compute ok / soon / overdue against today and my odometer.
   - Odometer in one doc "car/state" that I can update in one tap.
   - "Do next" at the top: overdue items first, then open watch-list items marked "act".
2. WATCH LIST — "What problems am I tracking?"
   - Collection "watch": {name, level: act|watch|resolved, opened, signs, prevent, source}.
   - Seed it from anything a mechanic said that wasn't fixed yet.
3. LIGHTS — "What does this dashboard light mean?"
   - Group my car's warning lights into: stop safely / get checked soon / fix yourself / normal.
   - Use only my owner's manual for this; link the page. Tell me which lights you couldn't confirm.

Also: a cost summary (total per year from the log) on the Logbook tab.
Ask me any questions before building. Then publish, then write my records into db and show me what you wrote.
```

## Add-ons once v1 is in use (one at a time)

- **3D "inside" view:** *"Add a tab with a schematic three.js r128 model of my car's main systems. Tapping a part shows: what it does, how it fails, words a mechanic uses, and its status from my logbook and watch list. Label it schematic, not CAD-accurate."*
- **Dent / body tracker:** *"Add a tab with a 3D exterior where I tap to mark a dent. For each: size vs a quarter, paint broken?, crease?, near an edge? Give a verdict (leave / DIY / paintless dent repair / body shop / seal it now) with a price range and its source."*
- **Monthly brief:** *"Each month write a doc in collection 'briefs' with the 5–8 things that matter this month, from my data."* Then create a scheduled task so it actually arrives on the 1st; without one it only appears when you ask.

## Weekly loop (this is what makes it useful)

After any shop visit, tire shop, fill-up with a warning light, or inspection, send Claude a photo in chat:

```text
Here's today's invoice for my car assistant [artifact link]. Add it to the log, update any maintenance items and watch-list entries it affects,
and tell me if anything on it doesn't match the quote, my manual's spec, or earlier records (fluid type, pressures, mileage).
```

Monthly (5 min, same day each month):

```text
Monthly check for my car assistant: odometer is [X]. Update it, then give me the "Do next" list and anything that changed since last month.
```
