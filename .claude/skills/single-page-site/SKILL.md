---
name: single-page-site
description: Standards for building or changing this project's single-file website. Use whenever index.html is created, restyled, or given a new feature.
---

# Single-page site standards

The whole site is `index.html`. These rules apply to every change to it.

## 1. One file
- All CSS in one `<style>` element in `<head>`.
- All JS in one `<script>` element placed at the end of `<body>`.
- No `<link>` or `src` pointing at local files. No images (no `<img>`, no image files, no
  background images, no data-URI pictures). Google Fonts is the only allowed external resource.
- No runtime fetching (`fetch`, XHR, imports). The page works opened straight from disk.

## 2. Mobile first
- Base CSS targets a phone; widen with `min-width` media queries.
- Readable at 380px wide with no horizontal scroll (no fixed widths wider than the viewport;
  long words and URLs wrap).
- Every tap target (buttons, selects, radio rows, primary links) is at least 44px tall.

## 3. Parents mode (print)
- A `@media print` stylesheet hides every control, button, header, tip box, changelog and
  footer, and shows only the generated itinerary.
- The printed itinerary's heading is the game name and date (e.g. "Notre Dame vs. Stanford",
  "Saturday, October 10, 2026").
- Black text on white, no backgrounds required to read it, items do not split across pages.
- The "Print for parents" button calls `window.print()`.

## 4. Visual style
- Colors are CSS custom properties on `:root`. Base palette: navy `#0C2340`, gold `#C99700`,
  generous white space.
- Gold on white fails contrast for text; use gold for rules, borders and accents, or on navy.
- Nothing imitating an official University mark: no monograms, logos, leprechauns, seals,
  official fonts, or "official" wording.

## 5. Data separated from presentation
- `SEASON` (game data), `ITINERARY` (itinerary templates and copy), `CHANGELOG` (update
  history, newest first) are plain objects at the top of the script. Rendering reads from them.
- `SEASON` is the first declaration in the script so weekly updates find it immediately.
- Every date on the page is derived from `SEASON` (weekday names, "Friday, October 9", the
  countdown, the picker labels). Never type a date twice. Changelog entries carry their own
  ISO date.
- The record is computed from `SEASON.games`.

## 6. Remembered choices
- The visitor's choices (game, arrival) go in `localStorage`.
- Every read and write is wrapped in `try/catch`. When storage is empty, blocked, or holds a
  stale value (e.g. a game that has since been played), the page falls back to defaults and
  renders correctly.

## 7. Accessibility
- Every control has a visible `<label>` (or `<fieldset>` + `<legend>` for radio groups).
- Text contrast at least 4.5:1. Visible focus styles. Fully usable with keyboard only.
- No emoji anywhere in the UI. No images.

## Definition of done
1. `index.html` is the only site file, with one `<style>` and one `<script>` at the end of body.
2. Works at 380px: no horizontal scroll, all tap targets ≥44px, every control labeled.
3. Print preview shows only the itinerary, headed by game name and date, with no controls.
4. Every visible date and the record come from `SEASON`; kickoff "TBA" renders sensibly.
5. The `qa-reviewer` agent reports "Ready to publish".
