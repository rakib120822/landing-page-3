# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Global Bioenergy Watch" — a marketing/landing site made of two hand-written, self-contained HTML files (CSS and JS inline in each):

- [index.html](index.html) (~1.4 MB) — the landing page.
- [dataset.html](dataset.html) (~27 KB, currently untracked) — a standalone "Renewable LPG suppliers" data page with its own `<style>`/`<script>` and a mailto "share summary" action. `index.html` does not link to it yet (the nav entry "Policy and market data" is marked `soon: 1` in `NAV`).

There is no package.json, build step, bundler, linter, or test suite; no README. To develop, open the file in a browser (or serve the folder with any static server) and refresh.

## Structure of index.html

Line numbers drift as the file is edited; re-Grep for anchors (`<style>`, `<script>`, `id="..."`, `(function name()`) rather than trusting them.

- `<style>` (~lines 11–1952): all CSS, driven by `:root` design tokens (`--ink`, `--leaf`, `--market`, `--policy`, `--display`/`--body` fonts, radii). Reuse tokens rather than hard-coding colors. `dataset.html` has its own, different token set (`--mist`, `--paper`, …).
- Markup (~lines 1958–2600): navbar (`#nav`; the list and mobile nav are empty and filled by JS), `.hero`, `#markets` (ticker/benchmarks), `#updates`, `#map` (`.jmap`, globe), `#industries` (slider), `#intelligence`, footer. Sections use `aria-labelledby`; keep the accessibility patterns (skip link, focus-visible, reduced-motion handling).
- **Two `<script>` IIFEs** (~2609–4098):
  1. Main IIFE (~2609–3959), organized under banner comments. Page content is **data-driven**: constants at the top (`MARKETS`, `UPDATES`/`FEED`, `SLIDES`, `PLACES`, `WTI`/`SPX`/`NG`/`BRENT`/`NYSE` series, `CARDS`, `NAV`, `SPY` scrollspy map) are rendered into the DOM by the named sub-IIFEs below (`navbar`, `buildTicker`, `marketsSection`, `updatesSection`, `sliderSection`, `initializeIntelligenceSection`, `footerExtras`, `globe3d`, …). To change copy or items, edit these arrays instead of the markup. Includes a canvas-rendered rotating globe (`LAND` polygons, `frame`/`rotate`/`focusOn`), a market chart with count-up animation, and an updates grid with kind filters (`All`/`Market`/`Policy`) and SVG fallbacks for failed images.
  2. "Jurisdiction map" IIFE (~3962–4097): a separate SVG world map. `PATHS` (country name → SVG path), `LISTS` (country names per mandate status `M`/`m`/`P`/`D`, pipe-separated), `ST` (status → label/color), `D` (per-country blurb + blend details) and `GEN` (generic fallback copy). Country names must match the keys in `PATHS` exactly (e.g. "United States of America", "Dem. Rep. Congo").

## Gotchas

- Several lines are enormous: base64 PNG logos (~296 KB each, in `.site-logo-img` ~line 1961 and `.footer-logo-img` ~line 2494) and the `PATHS` line (~581 KB, ~line 3964). Don't Read or print the whole file — use Grep with line numbers (add `| cut -c1-120` in shell), or Read with `offset`/`limit` that avoids those lines, and avoid `cat`/`head` on them.
- External dependencies are only Google Fonts (Bricolage Grotesque, Instrument Sans) and Unsplash image URLs in `UPDATES`; there are no JS libraries.
- Edit with targeted string replacements; the base64 lines make whole-file rewrites hazardous.
