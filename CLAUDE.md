# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Global Bioenergy Watch" — a single-page marketing/landing page. The whole site is one hand-written file, [index.html](index.html) (~945 KB). There is no package.json, build step, bundler, linter, or test suite; no README. To develop, open `index.html` in a browser (or serve the folder with any static server) and refresh.

## Structure of index.html

- `<style>` (starts ~line 11): all CSS, driven by `:root` design tokens (`--ink`, `--leaf`, `--market`, `--policy`, `--display`/`--body` fonts, radii). Reuse tokens rather than hard-coding colors.
- Markup (~line 2100–2810): sections in page order — navbar, `.hero`, `#markets` (ticker/benchmarks), `#updates`, `#map` (`.jmap`, globe), `#industries` (slider), `#intelligence`, footer. Sections use `aria-labelledby`; keep the accessibility patterns (skip link, focus-visible, reduced-motion handling).
- One `<script>` IIFE (starts ~line 2814), organized under banner comments. Page content is **data-driven**: constants at the top (`MARKETS`, `UPDATES`/`FEED`, `SLIDES`, `PLACES`, `WTI`/`SPX` series, `CARDS`, `NAV`, `SPY` scrollspy map) are rendered into the DOM by the code below. To change copy or items, edit these arrays instead of the markup. Later sections include a canvas-rendered rotating globe (`LAND` polygons, `frame`/`rotate`/`focusOn`), a market chart with count-up animation, and an updates grid with kind filters (`All`/`Market`/`Policy`) and SVG fallbacks for failed images.

## Gotchas

- Several lines are enormous (base64 PNG logos at ~lines 2130 and 2699, ~300 KB each; a ~29 KB line at 2327; a ~120 KB line at 4117). Don't Read or print the whole file — use Grep with line numbers, or Read with `offset`/`limit`, and avoid `cat`/`head` on those lines.
- External dependencies are only Google Fonts (Bricolage Grotesque, Instrument Sans) and Unsplash image URLs in `UPDATES`; there are no JS libraries.
- Edit with targeted string replacements; the base64 lines make whole-file rewrites hazardous.
