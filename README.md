# Number Generator — offline, single file

A tiny 100% offline tool to generate number sequences between two bounds, with step, order, parity, separators, grouping, prefix/suffix, and padding. Zero dependencies, a single `index.html` you open in your browser.

- Self‑contained, free, no tracking
- Compatible with recent Chrome, Edge, Firefox
- Automatic light/dark theme
- Works locally (file://) or via GitHub Pages

---

## Table of contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick start](#quick-start)
- [Fields and options](#fields-and-options)
- [Examples](#examples)
- [Ergonomics and shortcuts](#ergonomics-and-shortcuts)
- [Clipboard](#clipboard)
- [Accessibility](#accessibility)
- [Performance and decimal precision](#performance-and-decimal-precision)
- [Limits and warnings](#limits-and-warnings)
- [Browser compatibility](#browser-compatibility)
- [Repository structure](#repository-structure)
- [Development](#development)
- [Roadmap](#roadmap)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This project ships a single `index.html` file containing all HTML, CSS, and JavaScript. Open it locally and generate flexible, formatted numeric sequences for scripts, CSV, lists, tests, or content in one click.

---

## Features

- Inputs
  - Bounds From (x) and To (y) — integers or decimals
  - Step — strictly positive (decimals allowed)
  - Order — ascending or descending
  - Include bounds — checkboxes to include x and/or y
  - Parity — all, even, odd (auto‑disabled when decimals are used)
  - Output format — comma, space, semicolon, newline, or custom separator (supports escapes `\n`, `\t`, `\r`)
  - Grouping — N values per line (0/empty = no grouping)
  - Per‑value prefix and suffix
  - Padding (minimum length) to add leading zeros (e.g., 0037)
- Generation
  - Correctly handles both x <= y and x > y according to the chosen order
  - Clear validation with error messages under the corresponding fields
  - Warning + confirmation beyond a threshold (defaults to 100,000 items)
  - Efficient string building (`join`), no dependencies
  - Controlled rounding to avoid obvious floating‑point accumulation issues
- Output
  - Result area (read‑only textarea) for easy copying
  - Counter of generated items
  - Buttons: Generate, Reset, Copy (with fallback if Clipboard API is unavailable)
- UX/UI
  - Responsive (mobile/desktop)
  - Light/dark via `prefers-color-scheme`
  - Clean card design, subtle shadows, micro‑animations
  - Keyboard navigation and focus states
  - System fonts, inline SVG icons

---

## Requirements

- No server, no installation
- Recent browser (Chrome, Edge, Firefox)
- Optional: a text editor (e.g., VS Code) if you want to tweak the file

---

## Installation

1) Download or copy the `index.html` file into a local folder.  
2) Double‑click it to open in your browser (file:// scheme).  
3) That’s it 🎉

Tip: you can also publish this file on GitHub Pages (main branch, repository root) to use it online over HTTPS.

---

## Quick start

- Enter x, y, and the step; choose order, whether to include bounds, and formatting options.
- Click Generate or press Enter.
- Use Copy to send the output to the clipboard.
- Grouping “N values per line” forces a newline between groups.
- Padding adds leading zeros to the integer part (e.g., 37 → 0037).

---

## Fields and options

- From (x) / To (y)
  - Accept integers or decimals (dot or comma)
- Step
  - Strictly positive; decimals allowed
- Order
  - Ascending or Descending; the tool automatically handles x <= y and x > y
- Include bounds
  - Include x and/or y in the output
- Parity
  - All, Even, Odd
  - Automatically disabled if x, y, or step involve decimals
- Output format
  - Separator: comma, space, semicolon, newline, or custom
  - Custom supports `\n` (newline), `\t` (tab), `\r` (carriage return)
- Grouping
  - N values per line; 0 or empty = no grouping
- Prefix / Suffix
  - Strings added around each value
- Padding
  - Minimum length of the integer part with leading zeros (keeps sign and decimals)

---

## Examples

- x=1, y=10, step=2, ascending, separator “,”  
  Output: `1,3,5,7,9`
- x=10, y=1, step=3, descending, separator “; ”  
  Output: `10; 7; 4; 1`
- x=-3, y=3, step=1, parity=even, separator “\n”  
  Output:
  ```
  -2
  0
  2
  ```

---

## Ergonomics and shortcuts

- Enter: triggers Generate when the focus is within the form
- Focus management: logical tab order; visible focus states
- After generation: focus moves to the result area for quick copying

---

## Clipboard

- Uses `navigator.clipboard` when available in a secure context (HTTPS)
- Automatic fallback: selection + `document.execCommand('copy')` (useful with `file://`)
- Displays “Copied ✔” on success

---

## Accessibility

- Explicit labels, semantic structure, ARIA for error messages
- `aria-invalid` state on invalid fields
- AA color contrast (light/dark themes)
- Smooth keyboard navigation (radios, checkboxes, inputs, buttons)

---

## Performance and decimal precision

- Efficient generation via arrays + `join`, avoiding repeated DOM manipulation
- Decimals handled with a “common scale”:
  - Convert to integers on a 10^d base (d = max decimals, capped at 12)
  - Do comparisons and increments as integers to avoid binary float issues
  - Final formatting removes trailing decimal zeros
- Safety guard: stops if more than 5,000,000 items are being produced (hard cap)

---

## Limits and warnings

- Warning/confirmation above roughly 100,000 items
- Decimal precision stable up to 12 decimal places (maximum scaling)
- Depending on the browser, Clipboard API may require HTTPS; under `file://`, the fallback is used
- Very large outputs can consume considerable CPU and memory

---

## Browser compatibility

- Tested on: Chrome, Edge, Firefox (recent versions)
- No frameworks, libraries, bundling, or ES modules required
- Dark/light via `prefers-color-scheme`

---

## Repository structure

```
.
├─ index.html        # Complete app (HTML + CSS + JS inline)
└─ README.md         # This document
```

Optional: add a `LICENSE` file if you publish the repo.

---

## Development

- 100% of the code lives in `index.html`
  - `<style>` for theming (light/dark), responsive layout, focus states
  - `<script>` for logic (validation, generation, formatting, copy, UX)
- Code style
  - Comments and UI are in French in the source (by design)
  - No external dependencies, no analytics
  - Inline SVG icons
- Suggested tweaks
  - Warning threshold: constant `BIG_THRESHOLD` (defaults to 100000)
  - Max decimal precision: capped at 12 (scaling function)
  - UI messages: directly editable in HTML/JS
- Manual tests
  - Reuse the examples in the [Examples](#examples) section
  - Check combinations: bounds inclusion, parity, grouping, custom separators

---

## Roadmap

- Option “exclude duplicates” if parity + step + bounds might lead to duplicates
- Direct export to .txt or .csv (via Blob)
- Presets stored in localStorage (still 100% offline)
- Optional header/footer per group line in grouping mode

---

## FAQ

- Why is parity disabled with decimals?
  - Even/odd only makes sense for integers; to avoid ambiguity, the option is grayed out as soon as any field uses decimals.
- Clipboard API doesn’t work locally?
  - Some browsers require HTTPS for `navigator.clipboard`. The fallback (selection + `execCommand`) is used automatically on `file://`.
- Can I use a comma as a decimal separator in input?
  - Yes, commas are accepted and converted before computation.
- Why the warning above 100,000 items?
  - To help prevent tab freezes or heavy memory usage. You can confirm or reduce the volume (increase step, narrow bounds).

---

## Contributing

Suggestions are welcome:
- Open an issue (bug, enhancement, question)
- Submit a PR:
  - Keep the app 100% offline, single file, no dependencies
  - Follow the style (French comments, readability, accessibility)
  - Test on recent Chrome/Edge/Firefox
- Please describe the use‑case and UI/UX impact clearly

---

## License

MIT — free to use, modify, and distribute.  

Happy number generation 🔢✨
