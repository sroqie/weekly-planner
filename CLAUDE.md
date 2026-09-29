# Football Weekend Planner

## What this is

A single-page website for planning a Notre Dame home football weekend, published on
GitHub Pages and updated every week after a game.

- The entire site is `index.html`. Nothing else is served.
- No server, no build step, no API, no runtime data fetching, no external files.
  (Google Fonts are the only permitted external resource, and the site currently uses none.)
- Unofficial. Nothing on the page may imitate an official University or Athletics mark.

## Rules that never change

1. All CSS lives in one `<style>` in `<head>`; all JS lives in one `<script>` at the end of `<body>`.
2. Never link to local files (no `style.css`, no `app.js`, no images, no second HTML page).
3. All game data lives in ONE object called `SEASON`, the first thing in the `<script>` block.
   Itinerary text lives in `ITINERARY`. Update history lives in the array `CHANGELOG`
   (newest entry first). Rendering code only reads from these three objects.
4. Weekly updates change `SEASON` and `CHANGELOG` only.
5. Every date on the page is derived from `SEASON` (changelog entries carry their own date).
   The record is always computed from `SEASON.games`; never type it.
6. Never invent a kickoff time. Unknown kickoff is the string `"TBA"`; Saturday times are then
   shown relative to kickoff ("Kickoff −3h").
7. The page must work on a phone (readable at 380px, no horizontal scroll, tap targets ≥44px)
   and print cleanly in "parents mode" (itinerary only, no controls).

## How work is done here

- **Building or restyling** `index.html`, or adding a feature → follow the
  `single-page-site` skill (`.claude/skills/single-page-site/SKILL.md`).
- **Updating after a game** (score, kickoff time, schedule change, "roll forward") → use the
  `weekly-updater` agent, which follows the `weekly-update` skill
  (`.claude/skills/weekly-update/SKILL.md`).
- **Before finishing any task**, run the `qa-reviewer` agent on `index.html` and fix what it
  reports. If an agent cannot launch another agent, the main session runs `qa-reviewer`
  after that agent returns.
- **When asked for a change from claude.ai/code**: edit `index.html`, commit, and push a new
  branch. Never merge, never push to `main`. The owner verifies and merges.
