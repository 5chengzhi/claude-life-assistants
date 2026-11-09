# Shared rules (paste at the top of every build prompt)

These rules are the difference between a page that looks good and one you keep using. Copy everything in the box.

```text
RULES FOR THIS BUILD
1. One HTML file, published as a Claude Artifact. Pin library versions (three.js r128, Chart.js 4.4.1). No other dependencies.
2. Data placement:
   - Reference data that rarely changes goes in the page as a JS array.
   - Records that grow (logs, check-ins, wear history) go in the Artifact database (db), so you can add them from chat later.
   - Do not hard-code my starting records in the page. Publish first, then write them into db.
   - Make every db collection readable and writable only by editors of the page.
3. The first screen shows "Do next": the 1–3 most urgent things, computed from my data. No generic tips.
4. At most 3 tabs in version 1. Each tab answers one question I actually ask (I'll tell you which).
5. Every number is labeled: measured, from a document (say which), or estimated. Never invent specs, prices or intervals; if you don't know, say so and leave it blank.
6. Every empty list says what to add and why, in one sentence.
7. Works at phone width with no sideways scrolling. Light and dark mode.
8. If an "Ask Claude" box is included, each call sends: the instructions, the relevant records from db, and the format I want back. It has no memory.
9. Before publishing, check and fix: the "Do next" box works with empty data and with real data; every estimate is labeled; every empty list explains itself; nothing scrolls sideways at 375 px wide.
10. Ask me before adding any feature that needs me to do something on a schedule. Tell me what the trigger will be.
```
