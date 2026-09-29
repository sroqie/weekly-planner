---
name: weekly-update
description: How to update the planner after a Notre Dame game is played. Use when the user reports a score, a kickoff time, or a schedule change, or asks to roll forward.
---

# Weekly update procedure

Weekly updates edit exactly two things in `index.html`: the `SEASON` object and the
`CHANGELOG` array. Nothing else changes unless the user explicitly asks.

## Data format reminders
- Each game in `SEASON.games`:
  `{ opponent, date: "YYYY-MM-DD", site: "home" | "away" | "neutral", venue, kickoff, played, result, score }`
- `kickoff` is `"TBA"` or 24-hour Eastern Time `"HH:MM"` (3:30 PM ET → `"15:30"`).
- `result` is `"W"`, `"L"`, or `null`. `score` is Notre Dame's points first (`"34-17"`),
  `"to confirm"` if the result is known but the score is not, or `null` if unplayed.
- There is no record field. The record is computed from `SEASON.games`. Never type it.
- There is no "next game" field. The page picks the earliest unplayed home game itself.

## Procedure
1. **Find the game** in `SEASON.games` by opponent name. If the opponent is not in the list,
   or the score or winner is missing or ambiguous, stop and ask. Do not guess.
2. **Record the result**: set `played: true`, `result: "W"` or `"L"`, `score: "ND-OPP"`.
3. **Record** is recomputed automatically; do not add or edit any record text.
4. **Next home game** becomes the earliest unplayed home game automatically. Check that the
   game you expect is indeed the earliest home game still `played: false`.
5. **Kickoff times**: only change a `kickoff` when the user states one. Convert to 24-hour ET.
   If a time is given without AM/PM, 1:00–11:59 is PM and 12:00 is noon; say so in the reply.
   If the user gives a time in another zone, convert to ET and say so. If no time is given,
   leave `"TBA"`; the page will show relative times ("Kickoff −3h"). Never invent a time.
6. **Schedule changes** (date moved, game added): edit only the affected game object. Ask
   before deleting any game.
7. **CHANGELOG**: add ONE new entry at the top (index 0) with today's ISO date and one plain
   sentence, e.g. `{ date: "2026-10-11", text: "Irish beat Stanford 34-17. Navy kickoff set for 3:30 PM ET." }`.
   Keep every earlier entry unchanged.
8. **Do not touch** layout, CSS, rendering code, `ITINERARY` text, or the footer unless asked.
9. **Run the `qa-reviewer` agent** and include its verdict.
10. **Reply** with: what changed (field by field), what is still TBA, the new computed record,
    the next home game, and this reminder: "Verify kickoff times on fightingirish.com before
    merging."

## Worked example

User: "Irish beat Stanford 34-17, kickoff vs Navy is 3:30"

Changes, and only these:

```js
// SEASON.games — Stanford entry
{ opponent: "Stanford", date: "2026-10-10", site: "home", venue: "Notre Dame Stadium",
  kickoff: "TBA", played: true,  result: "W",  score: "34-17" },   // was played:false, result:null, score:null

// SEASON.games — Navy entry
{ opponent: "Navy", date: "2026-10-31", site: "home", venue: "Notre Dame Stadium",
  kickoff: "15:30", played: false, result: null, score: null },    // was kickoff:"TBA"

// CHANGELOG — new entry added at index 0, older entries untouched
{ date: "<today's ISO date>", text: "Irish beat Stanford 34-17. Navy kickoff set for 3:30 PM ET." },
```

Not changed: Stanford's kickoff (still `"TBA"`; it was never announced here and the game is
over), the date of either game, every other game, `ITINERARY`, CSS, rendering code.

Effects the page computes on its own: record goes from 4–0 to 5–0; next home game becomes
Navy; Navy's Saturday itinerary switches from "Kickoff −3h" labels to clock times
(e.g. parking target 10:30 AM for a 5-hour lead).

Reply would say: Stanford marked W 34-17; Navy kickoff 3:30 PM ET (assumed PM); record now
5–0; next home game Navy; still TBA: Miami, Boston College, SMU; plus any "to confirm" scores;
"Verify kickoff times on fightingirish.com before merging."
