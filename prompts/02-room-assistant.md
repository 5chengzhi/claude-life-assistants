# Room assistant

**Usefulness in my build: 4/10 so far.** The floor planner was worth it for one layout decision. The weekly features went unused because they needed a new habit. Build the planner first; add a weekly feature only if you name the trigger (e.g. "Sunday after laundry").

## Gather first (15 min)

- [ ] Tape-measure the room: wall lengths in feet, door width and which way it swings, window and closet positions
- [ ] Measure each big piece of furniture in inches (bed frame, desk, dresser, fridge)
- [ ] One photo from each corner and one from the doorway
- [ ] What you want: e.g. "more floor space", "a spot to sit with a guest", "a projector wall"

## Build prompt (paste with the photos attached)

```text
[paste prompts/00-shared-rules.md here]

Build me a room planner for my room. Measurements: [walls in feet; door position and swing; windows; closets].
Furniture in inches: [list]. What I want from the layout: [goals].

Version 1 has 2 tabs:
1. LAYOUT — "Where should the furniture go?"
   - Top-down SVG floor plan to scale. Drag pieces to move them, tap then Rotate. Snap to 1 inch and to walls and other pieces' edges.
   - Side-by-side three.js r128 3D view of the same layout, with a few camera presets (from the door, from the bed, top down).
   - Live checks, shown as green/amber/red: overlapping pieces, door swing blocked, closet doors blocked, walking path from the door under 18 in.
   - Let me save named arrangements and reset to the original. Inputs for room and furniture sizes so I can correct your estimates.
   - Label every size you estimated from photos as an estimate until I type a measured number.
2. UPKEEP — "What do I clean today/this week?"
   - Daily, weekly and monthly tasks for a room this size, as tick boxes that reset on their own schedule.
   - Store ticks in db so they sync across my phone and laptop.

Ask me questions before building. Then publish.
```

## Add-ons (only with a named trigger)

- **Weekly check-in:** *"Add a check-in: I upload one photo from the door, Claude scores it 1–10 against this scale (5 lived-in, 6 tidy, 7 put together, 8 clean and intentional, 9 guest-ready), names the zones that are clear, and gives one fix for next week. Chart the scores."* Trigger it from an existing routine, and set up a scheduled reminder.
- **Where things go:** *"Add a searchable index: zone → what lives there, so tidying is putting back, not deciding."* Useful the week you reorganize; lower value after.
- **Guest-ready checklist:** a short list to run 20 minutes before someone comes over.

## Weekly loop

```text
Room check-in for [artifact link]: here's a photo from the door. Score it on the room scale, list which zones are clear, give one fix for next week, and log it.
```

If after 4 weeks you haven't logged a check-in, cut the feature and keep the Layout tab. That's not a failure; the planner did its job.
