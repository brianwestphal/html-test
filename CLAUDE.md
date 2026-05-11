# Project: HTML/CSS rendering test suite

Static HTML documents used as inputs for a new rendering engine. Each file
exercises a focused subset of HTML5 + stable CSS. `work.md` is the canonical
checklist and progress ledger — all 156 items were complete as of the initial
build (see `git log` once the repo is initialized).

## Hard scope constraints

- **No JavaScript** — the rendering target doesn't execute it. Don't introduce
  `<script>` or event handlers. Attributes like `onsubmit` can be tolerated
  since they're parsed but not run.
- **No animation / no transition** — static rendering only. `@keyframes`,
  `animation`, `transition` are out of scope. Transforms, filters, gradients,
  shadows, etc. are in scope when used statically.
- **No audio, video, or iframes** — skipped entirely. `<audio>`, `<video>`,
  `<track>`, `<iframe>`, `<embed>`, `<object>` are not covered.
- **CSS scope = stable, broadly-supported** (CSS 2.1 + Flexbox, Grid incl.
  subgrid, custom properties, transforms, filters, logical properties,
  modern selectors incl. `:has`, `@layer`, nesting, `@container`,
  `color-mix`/`oklch`). Avoid experimental/draft features.

## Layout conventions

- **Flat structure**: all test files sit at the project root as
  `NN-category-name.html`. No subdirectories for tests. The user specifically
  rejected the initial `tests/` subdirectory in favor of this flat layout.
- **Asset paths are always `assets/…` with no `../`** — the user flagged
  broken relative paths early on; keep this rule strict so `file://` opens
  work cleanly.
- **Self-contained by default**: inline `<style>` blocks per file. Use
  external CSS only when the feature under test demands it (`<link
  rel=stylesheet>`, `@import`, `@font-face url()`).
- Every test has a visible `<h1>` naming the feature and a short
  pass-criteria note when the rendering intent isn't obvious.

## Assets (`assets/`)

- 5 format-labeled square images (128×128) — `img-{red.png, blue.jpg,
  green.gif, purple.webp, orange.svg}` — each is a solid color with its
  format name centered in white (SVG has black text on orange).
- `img-wide.png` (384×128, 3:1) and `img-tall.png` (128×384, 1:3) for
  object-fit / aspect-ratio tests.
- `shared.css` + `shared-import.css` exercise `<link>` + `@import` chains.
- Regenerate images with ImageMagick (`magick` is on the user's PATH at
  `/opt/homebrew/bin/magick`). See command history for the exact invocations.
  PIL is **not** installed.
- `@font-face` rules use `local()` fallbacks to system fonts (Menlo /
  Georgia / etc.) to avoid binary font dependencies. Don't introduce
  network-fetched fonts.

## Working with `work.md`

- `work.md` is the plan and the progress tracker. Update checkboxes as items
  complete. It is the source of truth for coverage — don't rely on task-tool
  state for this project.
- Categories are numbered 01–32; filenames echo the category number
  (`10-sel-*.html` for selectors, etc.).
- If adding a new test, append a bullet to the matching category in
  `work.md` first, then create the file.

## User preferences observed

- **Check in before long autonomous runs** on tasks of this scale, but don't
  over-ask once direction is set. The user prefers proceeding in batches with
  periodic progress updates over stopping for confirmation on each step.
- **Readability of test HTML isn't a priority** — brevity and feature
  coverage win. No need to format for human review.
- **Granularity**: many small focused files are preferred over fewer large
  combined files — easier to bisect rendering failures.
- **Images must be obviously identifiable** as images (distinct colors,
  labels) — we're not testing image quality, we're testing that an image
  rendered *where expected*.
- **American-English spelling and grammar** everywhere — file contents,
  code comments, prose in test pages, commit messages, and this file.
  No `colour`/`grey`/`labelled`/`centre`/`behaviour`/`organise`/etc.;
  use `color`/`gray`/`labeled`/`center`/`behavior`/`organize`. Same goes
  for grammar (e.g. American comma usage, no `whilst`/`amongst`).

## Things NOT to add without asking

- A test runner, diffing harness, or screenshot comparison tooling.
- A build step, package.json, or bundler. The suite is plain files served
  directly.
- CSS variables / theming across files — each test stands alone.
- Documentation beyond `work.md`, `index.html`, and this file.
