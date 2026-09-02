# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static, hand-authored site of interactive AI workshops by Kevin Phillips. No build step, no framework, no tests, no lint. Each page is a single self-contained `index.html` with an inline `<style>` block and inline `<script>`; the only external requests are Google Fonts and (in some workshops) the Anthropic API. Served as static files from `https://github.com/kpphillips/ai-workshops` (relative links only — `./fable-5.1-workshop/`, etc.).

Tracked content is the root `index.html` (landing page) plus one folder per published workshop. `docs/`, `archive/`, `working/`, and `.cursor/` are gitignored — `working/` is where WIP workshops live before they are promoted, `docs/workshop-platform-prd.html|md` is a longer-term product vision that is NOT the current state.

## Repository layout

- `index.html` — landing page. Lists published workshops, grouped by recency (see below).
- `<topic>-<version-or-date>-workshop/index.html` — one published workshop per folder. Naming: `fable-5.1-workshop`, `mcp-07-28-workshop`, `opus-4.8-workshop`.
- `working/` — unpromoted drafts (gitignored). A workshop is usually drafted/edited in another app, dropped here or straight into a new top-level folder, then the landing page is updated.

## Publishing a new workshop / "run an update on the landing page"

When asked to add or update a workshop on the landing page, do all of this before finishing:

1. **Confirm the workshop folder exists** at repo root with an `index.html`, and that the landing-page card's `href` points to it (`./<folder>/`).
2. **Add/refresh the card** in the correct section of `index.html`:
   - `<section class="grid" aria-label="Latest workshops">` — current-generation model workshops and the current protocol/spec workshop.
   - `<section class="grid" aria-label="Earlier workshops">`, under the `<h2 class="section-head">Earlier workshops</h2>` divider — workshops whose subject has been superseded (e.g. an older Claude model). Still linked, just visually demoted via `class="card card--earlier"` and `<span class="tag tag--earlier">`.
3. **Every card carries a real date**, not a "Latest" word-label alone: `<span class="date">Mon YYYY</span>` (month the workshop was published, from git history). Dates are the durable recency signal; the section placement + tag is the editorial layer on top.
4. **Tags**: `tag--latest` ("Latest" for the newest, "Current" for the still-current spec) in the Latest section; `tag--earlier` ("Foundational") in the Earlier section.
5. **Re-check categorization** of every existing card while you're in there — a workshop that was "Latest" may need to move down to "Earlier" when a newer one lands. Ask the user if a move is ambiguous.
6. Keep the footer line (`More workshops will appear here…`) last.

The landing page has its own small token set in `:root` (`--ink`, `--muted`, `--bg`, `--line`, `--blue`, `--violet`) and its own card system — it does NOT use the Field Guide system the workshop pages use. Match the incumbent landing-page CSS, don't import workshop tokens.

## Workshop page conventions

`fable-5.1-workshop` and `mcp-07-28-workshop` use the **Field Guide design system**: `<html data-palette="tide|ember|iris" data-theme="dark|light">`, CSS custom properties defined per `html[data-palette=...]` selector (`--bg`, `--surface`, `--ink`, `--accent`, `--green`, `--clay`, `--red`, `--alt`, …), fonts Fraunces / Public Sans / JetBrains Mono, and a left nav + right rail layout. `opus-4.8-workshop` is older and uses a different, standalone token set (Space Mono / Bebas Neue / Inter) — leave its system as-is unless asked to rebuild it.

New workshops should follow the Field Guide system for consistency with the two current ones. Preserve palette-switching and theme-switching when editing.

## Design standard for all work in this repo

Hold every change — landing page and workshop pages — to the `/impeccable` design standard. When doing non-trivial UI work, run the skill (`/impeccable`, or a specific sub-command like `/impeccable critique`, `/impeccable audit`, `/impeccable polish`).

"Run a scan on the workshop layouts" means the Impeccable detector:

```
node ~/.claude/skills/impeccable/scripts/detect.mjs --json <path-to-index.html>
```

Run it on the changed page(s) after any UI edit. Exit 0 = clean, 2 = findings. For a deeper review use `/impeccable audit <path>` (technical: a11y, perf, responsive) or `/impeccable critique <path>` (design review).

## Git

Remote `origin` is `github.com/kpphillips/ai-workshops`. Commit or push only when asked. `docs/`, `archive/`, `working/` are intentionally gitignored — do not `git add -f` them.
