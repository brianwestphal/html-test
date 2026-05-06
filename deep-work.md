# Deep coverage — follow-up rendering tests

Follow-up batch of test documents that exercise edge cases the original
`NN-…` files don't fully reach. Each file is named `NN-deep-<topic>.html`
and lives at the project root alongside the originals. Same scope rules
apply (no JS, no animation/transition, no audio/video/iframe, stable CSS
only — see `CLAUDE.md`).

The aim is to push rendering engines on subtleties the broad-coverage
suite leaves implicit: stacking-context creators, paint-order rules,
containing-block resolution under transforms, margin collapse edge cases,
flex/grid intrinsic sizing, anonymous table boxes, baseline alignment,
@layer / cascade interactions, and so on.

Update the checkboxes as files land. Append the new test to the matching
section in `index.html` under the existing category.

## 13 — Positioning, z-index, stacking (priority — user-flagged)

- [x] 13-deep-stacking-context-creators.html — every property that creates a stacking context (opacity, transform, filter, isolation, mix-blend-mode, mask, will-change, contain, perspective, position+z-index, flex/grid items with z-index, etc.)
- [x] 13-deep-stacking-paint-order.html — the seven-level paint order (background → negative-z descendants → block-level → floats → inline → z:auto/0 positioned → positive-z)
- [x] 13-deep-z-index-negative.html — negative z-index inside stacking context paints above parent background but below parent content
- [x] 13-deep-z-index-flex-grid.html — z-index on flex/grid items without position; order does NOT change paint order
- [x] 13-deep-fixed-in-transform.html — fixed becomes effectively absolute when an ancestor has transform / filter / will-change / contain:paint / perspective
- [x] 13-deep-sticky-edges.html — sticky in scrollable container, multiple stacked sticky siblings, sticky inside flex/grid, stuck-state overlap
- [x] 13-deep-containing-block.html — abs/fixed CB resolution under transform/filter/contain ancestors, percentage padding direction

## 11 — Box model & sizing

- [x] 11-deep-margin-collapse-edges.html — parent-child collapse, three-way collapse, negative margins, BFC stoppers (overflow, display:flow-root, contain), float/abs don't collapse
- [x] 11-deep-percent-resolution.html — % padding/margin always resolved against inline-size of CB even in vertical writing mode; % height on auto-height parent
- [x] 11-deep-intrinsic-sizing.html — min-content / max-content / fit-content / fit-content() keywords on width and height
- [x] 11-deep-box-sizing-mix.html — content-box vs border-box mid-cascade, interaction with min/max constraints, sizing of replaced elements

## 12 — Display

- [x] 12-deep-display-syntax.html — multi-keyword display (inline flex, block flow-root, flow vs flow-root); legacy single-keyword equivalence
- [x] 12-deep-display-contents.html — display:contents on flex/grid items, on links/buttons, with backgrounds (lost), child layout role inheritance

## 14 — Floats

- [x] 14-deep-float-bfc.html — BFC formed by overflow != visible, display:flow-root, contain:layout/content; float clearance behavior at BFC boundary
- [x] 14-deep-float-interactions.html — float beside float, negative margins, float clearing self, float wrapping inline content

## 15 — Flexbox

- [x] 15-deep-flex-min-auto.html — min-width:auto / min-height:auto default to min-content for flex items, can shrink past content
- [x] 15-deep-flex-baseline.html — baseline alignment groups, baseline of empty / text-less / wrapped items, last baseline
- [x] 15-deep-flex-aspect-ratio.html — aspect-ratio on flex items with flex-basis auto / 0 / fixed
- [x] 15-deep-flex-order-vs-z.html — order changes layout but not paint order without z-index; z-index requires positioned-or-flex/grid item

## 16 — Grid

- [x] 16-deep-grid-implicit.html — implicit tracks via grid-auto-rows/columns, dense packing reorders backfill, span beyond explicit creates implicit tracks
- [x] 16-deep-grid-min-max-content.html — auto track sizing, minmax with min/max-content, fr in min position is invalid, fit-content() track
- [x] 16-deep-grid-baseline.html — baseline groups across rows/columns, first vs last baseline alignment
- [x] 16-deep-subgrid-lines.html — line name inheritance across subgrid, gap propagation, subgrid in only one axis

## 04 — Tables

- [x] 04-deep-anonymous-boxes.html — display:table-cell without table-row, table-row without table — anonymous box generation
- [x] 04-deep-border-conflict.html — border-collapse winner rules (style > width > color > position), hidden vs none precedence

## 02 / 20 — Text & inline

- [x] 02-deep-bidi-isolate.html — unicode-bidi: isolate / plaintext / bidi-override / embed; nested bdi vs bdo; explicit RLO/LRO control chars
- [x] 02-deep-line-breaking.html — soft hyphen (&shy;), word-break (normal/break-all/keep-all), overflow-wrap (anywhere/break-word), hyphens with lang
- [x] 20-deep-line-box-baselines.html — mixed font-sizes in a single line box, half-leading distribution, line-height: normal vs number vs length
- [x] 20-deep-vertical-align.html — top/middle/bottom/text-top/text-bottom/sub/super/percentage/length on inline-block, baseline of inline-block depends on last in-flow line box

## 10 — Selectors

- [x] 10-deep-has-complex.html — :has with nested combinators, :has(+ x), :has(~ x), :has(:not(...)), interaction with :is/:where
- [x] 10-deep-nth-of-type.html — :nth-child(an+b of S) selector list, :nth-last-of-type, edge cases (n, 2n+1, even/odd)
- [x] 10-deep-attr-quoting.html — case-insensitive `i` flag, case-sensitive `s` flag, escapes inside quoted values, partial match nuances ([attr~=], [attr|=])

## 17 / 18 — Backgrounds, borders, shadows

- [x] 17-deep-bg-clip-text.html — background-clip:text with image / gradient / multiple layers; -webkit prefix fallback
- [x] 17-deep-bg-attachment-fixed.html — background-attachment:fixed inside scrollable element vs viewport; multi-layer attachment mix
- [x] 18-deep-radius-overflow.html — border-radius clipping rules: overflow:visible doesn't clip, hidden does; effect on box-shadow inset; per-corner clipping
- [x] 18-deep-shadow-stacking.html — multiple stacked box-shadows (paint order), inset+outset combinations, color-mix with currentColor, spread negative

## 19 — Colors

- [x] 19-deep-color-spaces.html — color() function with display-p3 / rec2020 / xyz / srgb-linear, fallback when unsupported
- [x] 19-deep-color-mix.html — color-mix(in <space>, …) across spaces (srgb/lab/oklch/hsl), with percentage hints, currentColor

## 20 — Typography (more)

- [x] 20-deep-font-features.html — font-feature-settings (liga, dlig, smcp, tnum, frac, ss01), font-variant-* shorthands, font-variation-settings (wght, wdth, opsz)
- [x] 20-deep-text-emphasis.html — text-emphasis style/color/position, text-emphasis-skip
- [x] 20-deep-decoration-detail.html — text-decoration thickness, offset, line styles, text-decoration-skip-ink
- [x] 20-deep-writing-mode-mixed.html — vertical-rl with text-combine-upright (tate-chu-yoko), vertical with bidi text

## 21 — Transforms

- [x] 21-deep-transform-origin.html — transform-origin with %/units/keywords, 3D origin (z component)
- [x] 21-deep-transform-3d-preserve.html — preserve-3d propagation, backface-visibility, perspective-origin, nested 3D containers

## 22 — Filters & blending

- [x] 22-deep-filter-stacking.html — filter creates stacking context and CB for fixed; backdrop-filter compositing through transparent
- [x] 22-deep-isolation.html — isolation:isolate confines blend modes; mix-blend-mode escaping vs not
- [x] 22-deep-blend-groups.html — multiple blended layers in one group with various separable/non-separable blend modes

## 23 — Clipping & masking

- [x] 23-deep-clip-path-shapes.html — polygon, path(), inset() with corner radii, ellipse() with %, geometry-box references
- [x] 23-deep-mask-composite.html — multiple mask-image layers, mask-composite (add/subtract/intersect/exclude), mask-mode

## 24 — Generated content

- [x] 24-deep-counter-scope.html — counter scope/inheritance, counter-set vs counter-reset, nested counters with counters() function

## 26 / 27 — Media

- [x] 26-deep-mq-prefers.html — prefers-color-scheme, prefers-reduced-motion (rendered statically), prefers-contrast, prefers-reduced-transparency
- [x] 27-deep-page-margin-boxes.html — @page selectors (:first, :left, :right, :blank), page margin boxes (@top-center, @bottom-right, etc.), named pages

## 28 — @-rules

- [x] 28-deep-container-types.html — container-type: size vs inline-size, nested containers, container query units (cqw/cqh/cqi/cqb/cqmin/cqmax), named containers
- [x] 28-deep-layer-import.html — @import with layer(name), anonymous layers, layer ordering at first appearance
- [x] 28-deep-nesting-complex.html — & in selector lists, nested @media/@supports, & with combinators, nested pseudo-elements

## 29 — Cascade & values

- [x] 29-deep-layer-priority.html — !important reverses layer order; unlayered > layered for normal but reversed for !important
- [x] 29-deep-where-is-specificity.html — :where() contributes 0, :is()/:not()/:has() contribute max of arg list, attribute vs class specificity
- [x] 29-deep-property-registration.html — @property with syntax/inherits/initial-value, type-checked custom properties, fallback animation behavior
