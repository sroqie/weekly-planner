# Football Weekend Planner

An unofficial, single-page planner for a Notre Dame home football weekend: the season record,
a countdown to the next home game, and an hour-by-hour itinerary from arrival to Sunday
morning that you can print for parents.

**Live site:** https://USERNAME.github.io

The whole site is one file, `index.html`. No server, no build step.

## Updating after a game

Open the repo in Claude Code (or claude.ai/code) and send one sentence:

> Use the weekly-updater agent. Notre Dame beat X 34-17 on DATE. Kickoff against Y is 3:30 PM ET. Roll the planner forward.

It updates the score and kickoff, adds a "This week" entry, and runs a QA review. Check
kickoff times on fightingirish.com, then merge.

## Upload the `.claude` folder too

The `.claude` folder holds the skills and agents that make the weekly update work
(`.claude/skills/` and `.claude/agents/`). Upload it with `index.html`, `CLAUDE.md`, and this
README, or future sessions won't know the rules.
