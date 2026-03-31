# CLAUDE.md

This file provides guidance for AI assistants working on this repository.

## Project Overview

A self-contained Valentine's Day interactive web application. A single HTML file with embedded CSS and JavaScript — no build tools, no dependencies, no frameworks.

**File inventory:**
- `index.html` — the entire application (HTML + CSS + JS, ~289 lines)
- `specs.md` — detailed behavioral specification (source of truth for intended behavior)
- `README.md` — minimal project description

## Running the App

Open `index.html` directly in a browser, or serve it via any static HTTP server:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

No installation, no build step.

## Architecture

Everything lives in `index.html`:

| Section | Lines | Description |
|---------|-------|-------------|
| `<style>` | 8–126 | All CSS including animations |
| `<body>` HTML | 131–149 | DOM structure (question section + success message) |
| `<script>` | 151–284 | All JS logic |

### Key JavaScript components

- **`noClickCount`** (0–3): tracks how many times "No" was clicked
- **`yesBtnScale` / `noBtnScale`**: current scale multipliers for each button
- **`moveNoButton()`**: calculates a valid new position for the No button (called on hover/touch when `noClickCount >= 3`)
- Yes button click → hide question section, show success message

### Interaction flow

1. Question 1: "Will you be my valentine? 💝"
2. Click "No" → question advances, Yes grows (+0.5x scale), No shrinks (−0.25x scale)
3. Question 2: "Are you sure? 🥺" (after 1st No click)
4. Question 3: "Are you 100% sure? 😢" (after 2nd No click)
5. After 3rd No click → No button becomes "dodgeable" (moves away on hover/touchstart)
6. Click "Yes" at any point → show animated heart success screen

### Dodging logic constraints (`moveNoButton`)

- Must move ≥120px from current position
- Must stay ≥100px from the Yes button center
- Must stay within 15–85% horizontal, 20–80% vertical bounds
- Max 50 attempts to find a valid position; silently skips if none found

## Code Conventions

- **Vanilla JS/CSS/HTML only** — do not introduce frameworks, libraries, or external dependencies
- **Single-file constraint** — all code stays in `index.html`; do not split into separate files
- **4-space indentation** throughout
- **Mobile-first** — touch events use `passive: false` where `preventDefault()` is needed
- **No build tools** — the file must remain directly openable in a browser without any processing

## Specs

`specs.md` is the authoritative behavioral specification. Before making changes to interaction logic, button scaling, animations, or color values, cross-reference it. The success criteria checklist at the bottom of `specs.md` lists 11 verification points.

## No Tests / No CI

There is no test framework and no CI pipeline. Verify changes by opening `index.html` in a browser and manually testing the interaction flow on both desktop (hover) and mobile (touch).

## Git Workflow

- Development branch: `claude/add-claude-documentation-7LP6N`
- Main branch: `main`
- Push with: `git push -u origin <branch-name>`
