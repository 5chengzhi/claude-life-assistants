# Scoring rubric for a personal assistant

Use this after **4 weeks** of real use, not on the day you build it. Score from the data the assistant holds, not from how it looks. Each line is worth 1 point.

| # | Check | Pass if… | How to check |
|---|---|---|---|
| 1 | **Fed by an existing habit** | New records arrive from something you already do (bills, getting dressed, a shop visit) | Count records added in the last 4 weeks: at least 4 |
| 2 | **Changed a decision** | You can name one thing you did differently because of it | Write it down. If you can't, 0 |
| 3 | **Saves time or money** | One concrete instance with a number (minutes, dollars) | e.g. "caught a $X overcharge", "picked an outfit in 2 min" |
| 4 | **Opens on what matters** | First screen shows the 1–3 things to do next, computed from your data | Open it cold: do you know what to do in 10 seconds? |
| 5 | **Claude can update it from chat** | Sending a photo or receipt in chat results in a saved record | Try it once |
| 6 | **Honest numbers** | Every estimate is labeled as an estimate; sources named for prices/specs | Pick 3 numbers at random and find their source |
| 7 | **No dead tabs** | Every tab opened at least once in 4 weeks | Cut or merge the rest |
| 8 | **Empty states are useful** | Blank sections say what to add and why | Look at any empty list |
| 9 | **Works on your phone** | Usable at phone width, no sideways scrolling | Open it on your phone where you'd actually use it |
| 10 | **Survives your absence** | If you skip 2 weeks, it tells you what's overdue instead of silently going stale | Check the "Do next" box after a gap |

**Reading the score:** 9–10 keep and extend · 7–8 keep, fix the failing lines · 5–6 cut to the one tab you use · ≤4 archive it or rebuild around a habit you actually have.

**Rate-and-revise loop with Claude:** paste this rubric and say *"Score my assistant against this rubric using its saved data. List each failing line with a fix. Apply the fixes, re-score, and repeat until 9/10 or until what's left is outside the page's control (e.g. I haven't logged anything yet). Tell me which."*
