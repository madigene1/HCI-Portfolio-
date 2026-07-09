# DESIGN.md — "Tactile Editorial" System

Implementation guide for consistently generating UI screens. Tokens live in `design-system/tokens.css` (CSS vars) and `design-system/tokens.json` (source). **Always reference tokens by name — never hardcode hex/px when a token exists.**

**How to use this doc:** every rule is deterministic. Where a choice exists, the default is stated first. If a value isn't specified here, round to the nearest spacing/type token rather than inventing one.

---

## Brand Personality

**Direction:** Tactile · Realistic · Messy · Professional.

An editorial, craft-driven aesthetic that pairs analog imperfection with disciplined structure — like a design studio's working desk: printed matter, ink, hand-lettering, and technical print ephemera on an underlying grid. Human and imperfect, but credible and considered. The type voice is a **skinny, square monospace display** (à la "BIG BIRD" — terminal/technical, not rounded) set against **heavy grotesque headlines and monospace lists** (à la a "CODE CRAFTED" book cover). Warm paper base, but punctuated with **clear, vivid color pops** — not muted washes.

---

## Design Principles

1. **Build on warm paper, not white.** Page = `--color-bg-base`; never `#FFFFFF`. Text = ink tokens; never `#000000`.
2. **One vivid pop per screen.** Pick **one** brand accent family (blue / red / yellow / green) per screen and use it boldly — it may fill a single CTA, mark, or indicator. Functional colors (danger/warning/success/info) are **not** counted against this and appear whenever semantically required. Keep the paper+ink field dominant so the pop reads as a pop.
3. **Three type voices, one job each.** `--font-display` (skinny square monospace) for expressive hero moments; `--font-heading` (heavy grotesque, 800–900, uppercase) for structural h1/h2; `--font-sans` for all UI/body; `--font-mono` for labels, metadata, and lists. Never two expressive/display strings competing in one view.
4. **Force scale contrast, skip the middle.** Body/functional text lives at 12–17px; headings jump to 48px+. The 20–44px range is reserved and used **only** via the defined h3/title step (17px bold) — never for body copy.
5. **Keep corners sharp.** `--radius-none` is the default for all UI. Softness comes from texture/content, not geometry.
6. **Depth through overlap + soft physical shadow.** Layer and lightly tilt decorative elements; elevation uses the low, diffuse shadow scale only.
7. **Dense detail inside generous whitespace.** Components are information-dense; the page around them stays open (macro margins).
8. **Grid first, then break deliberately.** Align to the 12-col grid, then offset/rotate at most 1–2 **decorative** elements per screen. Favor off-center, left-weighted balance. Never tilt interactive or form elements.

---

## Color Tokens

### Surface hierarchy (deterministic mapping)
| Role | Token | Rule |
|---|---|---|
| Page background | `--color-bg-base` #F4F1EA | Default for every screen. |
| Card / raised surface | `--color-bg-raised` #FAF8F2 | Any container sitting on the page. |
| Inset well / input / code | `--color-bg-sunken` #EBE7DC | Recessed areas, input fills, wells. |
| Dark block | `--color-bg-inverse` #1A1A1A | Footers, feature blocks, primary buttons. |

Do not nest the same surface on itself (e.g. `raised` on `raised`). Step one level: base → raised → sunken.

### Text-on-surface pairings (contrast-safe)
| Surface | Primary text | Secondary text |
|---|---|---|
| `bg-base` / `bg-raised` | `--color-ink-strong` | `--color-ink-muted` |
| `bg-sunken` | `--color-ink-body` | `--color-ink-muted` |
| `bg-inverse` | `--color-ink-inverse` | `--color-ink-inverse` @ 70% |

`--color-ink-faint` #9C988E is for placeholders/disabled **only** — never body copy.

### Pop accents (choose ONE family per screen)
`--color-accent-blue` #1E3AE6 · `--color-accent-red` #EE3A20 · `--color-accent-yellow` #FFC61A · `--color-accent-green` #1F7A4D. Each has a `-soft` tint (`blue-soft` #D9DEFB, `red-soft` #FBD9D2, `yellow-soft` #FFF0C2, `green-soft` #D2ECDD) for fills behind accented content.
- Applied to: links, active indicators, marks, one accent-filled CTA per view.
- Focus ring is **always `--color-accent-blue`** regardless of the screen's chosen accent.
- Yellow is a mark/emphasis color — never use yellow for text on light surfaces (poor contrast); pair yellow-soft fills with `--color-ink-strong` or `--color-warning` text.
- Legacy aliases `--color-accent-ink-blue`/`-amber`/`-olive` still resolve (→ blue/yellow/green) but prefer the new names.

### Functional colors (always available, not counted as "the pop")
danger `--color-danger` #EE3A20 · warning `--color-warning` #D98A00 · success `--color-success` #1F7A4D · info `--color-info` #1E3AE6. Use strictly for status (errors, validation, badges, alerts), never for decoration.

### Borders (deterministic usage)
| Token | Use |
|---|---|
| `--color-border-subtle` #E2DDD1 | Default dividers, card outlines, table rules. |
| `--color-border-default` #CFC9BC | Input borders, interactive outlines at rest. |
| `--color-border-strong` #1A1A1A | Artifact/frame edges, selected states, emphasis. |

### Interaction color derivation (deterministic)
- **Hover on filled:** overlay `#1A1A1A` at 8% (darken) — or for dark fills, overlay `#FFFFFF` at 8% (lighten).
- **Pressed/active:** overlay at 14%.
- **Disabled:** element opacity `--opacity-disabled` (0.4); do not recolor individually.

---

## Typography

**Families:** `--font-sans` (Archivo/Inter) — all UI/body. `--font-heading` (Archivo 800–900, uppercase) — structural headings. `--font-display` (Share Tech Mono, skinny square monospace) — expressive hero moments only; set at weight 400 with `+0.04em` tracking, uppercase. `--font-mono` (Space Mono/IBM Plex Mono) — code, metadata, technical lists.

### Semantic hierarchy (map every text element to one row)
| Element | Token / size | Family | Weight | Line-height | Case/tracking |
|---|---|---|---|---|---|
| Hero (expressive) | `--font-size-display-lg` 104 (fluid) | display (square mono) | 400 | 1.05 | UPPERCASE +0.04em |
| h1 (page) | `--font-size-display-md` 72 (fluid) | heading | 900 | `--line-height-tight` 0.95 | UPPERCASE, −0.01em |
| h2 (section) | `--font-size-display-sm` 48 (fluid) | heading | 900 | 0.95 | UPPERCASE, −0.01em |
| h3 / card title / modal title | `--font-size-lead` 17 | sans | 700 | `--line-height-snug` 1.2 | none |
| Overline / label / nav / tab / badge | `--font-size-caption` 12 | sans | 500 | 1.2 | UPPERCASE +0.08em |
| Body (default) | `--font-size-body` 15 | sans | 400 | `--line-height-body` 1.5 | none |
| Body small / dense | `--font-size-body-sm` 13 | sans | 400 | 1.5 | none |
| Lead paragraph | `--font-size-lead` 17 | sans | 400 | 1.5 | none |
| Caption / helper / metadata | `--font-size-caption` 12 | sans | 400 | 1.4 | none |

**Rules**
- Hero square-monospace display (`display-lg` 104) is opt-in for landing/marketing heroes only; use the `-fluid` variants so it scales down instead of overflowing mobile. Keep display type to one string per screen.
- Subsection titles use the **h3 step (17px bold sans)** — this is the only permitted use of "heading" styling in the 20–44px void. Do not create intermediate sizes.
- Prose measure: cap paragraph width at `--container-narrow` (680px).
- Only one h1 per screen; heading levels never skip (h1→h2→h3).

---

## Spacing

4px base scale: `1`=4 · `2`=8 · `3`=12 · `4`=16 · `6`=24 · `8`=32 · `12`=48 · `16`=64 · `24`=96 · `32`=128.

### Deterministic application
| Context | Value |
|---|---|
| Inline gap (icon↔label, chip internal) | `--space-2` (8) |
| Compact component padding (badge, tab) | `--space-2`/`--space-3` |
| Default component padding (button, input, list row) | `--space-3` vert / `--space-4` horiz |
| Card / panel padding | `--space-6` (24) |
| Modal padding | `--space-8` (32) |
| Label→field | `--space-2` · field→helper | `--space-1` |
| Stacked form fields | `--space-6` |
| Grid gutter between cards | `--space-8` |
| Between subsections | `--space-12` (48) |
| Between major page sections | `--space-24` (96) |
| Empty-state vertical padding | `--space-24` |

Default vertical stack rhythm is `--space-4`; only sections use `--space-12`/`--space-24`.

---

## Layout

- **Grid:** 12 columns, gutter `--grid-gutter` (32). Content max width `--container-max` (1200px), centered; prose max `--container-narrow` (680px).
- **Page inline padding (responsive):** `--page-pad-inline` 24 (mobile) → `--page-pad-inline-md` 64 (≥768) → `--page-pad-inline-lg` 96 (≥1024).
- **Breakpoints (min-width):** sm 480 · md 768 · lg 1024 · xl 1280.
- **Responsive column behavior:** card grids collapse 4→2 cols at `md`, 2→1 at `sm`. Never fewer than the content needs.
- **Decorative offset/tilt:** apply `--rotate-scrap-*` and manual offsets at `lg`+ only; reset to 0°/aligned below `md` for legibility.
- **Z-index (use only these):** `--z-base` 0 · `--z-raised` 10 · `--z-sticky` 100 · `--z-dropdown` 200 · `--z-scrim` 300 · `--z-modal` 310 · `--z-toast` 400. Never invent ad-hoc z-index values.

---

## Radius

`--radius-none` 0 (default, all UI) · `--radius-sm` 2px (maximum softening) · `--radius-pill` 999px (rare stamp-style badges only). Nothing else.

---

## Shadows

`--shadow-paper` `0 1px 2px rgba(26,22,18,.06)` — resting surfaces · `--shadow-raised` `0 2px 12px rgba(26,22,18,.08)` — hover/interactive lift · `--shadow-object` `0 8px 28px rgba(26,22,18,.12)` — modals/real-object overlays · `--shadow-inset` — sunken wells. Elevation ladder is paper → raised → object; no heavy, dark, or glossy shadows, no colored shadows.

---

## Components

Global rules: `--radius-none`, ink borders, one brand accent per view, uppercase `+0.08em` for all labels/nav/tabs/badges, shared focus ring (`--color-accent-blue`, 2px, 2px offset). **Every interactive component must define all applicable states below.**

### Buttons
- **Variants:** Primary (`--color-bg-inverse` fill + `--color-ink-inverse` text) · Accent (the screen's pop accent fill + white text — the one bold CTA) · Secondary (transparent + `--border-width-default` `--color-border-strong` + `--color-ink-strong`) · Ghost (no border, accent text, underline on hover).
- **Padding:** `--space-3`/`--space-6`; min-height 44px; icon gap `--space-2`. Type: sans 15 / weight 500 / uppercase +0.08em (primary, accent & secondary).
- **States:** default · hover (8% overlay / accent `brightness(1.08)`) · active (translateY(1px), `--shadow-none`, 14% overlay) · focus (shared ring) · disabled (`--opacity-disabled`, no shadow) · loading (mono spinner, label retained, disabled interaction).
- **Rules:** at most one Primary **and** one Accent CTA per view; the Accent fill is the *only* place accent color may fill a large area. Destructive → Secondary with `--color-danger` text/border.

### Inputs (text, select, textarea)
- **Style:** `--color-bg-sunken` fill, `--border-width-hairline` `--color-border-default`, `--radius-none`; caption label above (12 uppercase +0.08em `--color-ink-muted`); value text sans 15 `--color-ink-body`; placeholder `--color-ink-faint`.
- **Padding:** `--space-3`/`--space-4`; label→field `--space-2`; field→helper `--space-1`; stacked fields `--space-6`.
- **States:** default · hover (border `--color-border-strong`) · focus (border `--color-accent-blue`, `--border-width-default`, shared ring) · filled · error (border + helper `--color-danger`) · disabled (`bg-sunken`, `--opacity-disabled`) · read-only (no border, `--color-ink-body`).
- **Rules:** persistent visible label always; helper/error line is always reserved to prevent layout shift.

### Cards
- **Style:** `--color-bg-raised` on base; `--color-border-subtle` hairline OR `--shadow-paper` (not both heavy). `--radius-none`/`--radius-sm`.
- **Padding:** `--space-6`; grid gutter `--space-8`. Title = h3 step; one brand accent element max.
- **States:** default (`--shadow-paper`) · hover if interactive (`--shadow-raised`, translateY(−2px), `--duration-base`/`--ease-entrance`) · focus (shared ring) · selected (`--border-width-heavy` `--color-border-strong`) · disabled (`--opacity-disabled`).
- **Rules:** decorative `--rotate-scrap-sm` allowed on gallery/collage cards at `lg`+ only; never on cards containing inputs.

### Navigation (top bar)
- **Style:** `--color-bg-base`, hairline bottom rule `--color-border-default`; `--font-display` wordmark (~24) left; links sans 12 uppercase +0.08em `--color-ink-muted` right. Height 64; item gap `--space-6`; `--z-sticky`.
- **States:** link default (`--color-ink-muted`) · hover (`--color-ink-strong`, underline offset) · current (`--color-ink-strong` + 1.5px accent underline) · focus (shared ring) · sticky-scrolled (add `--shadow-paper`, solid `bg-base`).
- **Responsive:** below `md`, collapse links into a menu (disclosure button, ≥44px target). Left-weighted; ≤6 top-level items; never centered.

### Tabs
- **Style:** underline tabs; labels sans 12 uppercase +0.08em; full-width baseline rule `--color-border-subtle`; active = 2px bottom border `--color-border-strong` (or brand accent). No filled pills, `--radius-none`.
- **Padding:** `--space-3`/`--space-4`; tab gap `--space-6`; panel top margin `--space-8`.
- **States:** default (`--color-ink-muted`) · hover (`--color-ink-strong`) · active · focus (shared ring) · disabled (`--color-ink-faint`, `--opacity-disabled`).
- **Rules:** indicator animates `--duration-base`/`--ease-standard`; left-aligned; arrow-key navigable.

### Badges
- **Variants:** Tag (hairline `--color-border-default`, `--color-ink-muted`, `--radius-none`) · Status (`-soft` fill + matching accent/functional text). `--radius-pill` reserved for rare stamp style.
- **Padding:** `--space-1`/`--space-2`; inline gap `--space-2`; ≤3 per row.
- **Status mapping:** success→olive · info→ink-blue · warning→amber · danger→`--color-danger`.
- **States:** static default; removable (trailing × `--color-ink-muted` → `--color-ink-strong` on hover, focusable).
- **Rules:** never color-only — include text/icon; one short word/label.

### Modals
- **Style:** panel `--color-bg-raised`, `--radius-none`, hairline `--color-border-default`, `--shadow-object`, `--z-modal`; scrim `rgba(26,22,18,.4)` (`--opacity-scrim`) at `--z-scrim`. Title = h2 (small modal) or h3 step; body sans 15.
- **Sizes:** dialog ~560px max; content-dense ~720px max; full-bleed sheet on mobile (<`sm`).
- **Padding:** `--space-8`; title→body `--space-4`; body→actions `--space-8`; actions gap `--space-3`, right-aligned, primary rightmost.
- **States/behavior:** enter (scrim fade `--duration-base`; panel 0.98→1 + fade `--ease-entrance`) · open (focus trap, scroll lock) · exit (`--duration-fast`/`--ease-exit`). Dismiss via scrim click, ×, and Esc.

### Empty states
- **Style:** narrow left-aligned column (≤`--container-narrow`); headline = h3 step or h2; body sans 15 `--color-ink-muted`; exactly one CTA (Primary or Ghost); monochrome/ink illustration or single mono glyph only.
- **Padding:** vertical `--space-24`; headline→body `--space-3`; body→CTA `--space-8`.
- **Variants:** first-run (encouraging + CTA) · no-results (restate query + "clear filters" Ghost) · error (`--color-danger` accent + retry).
- **Rules:** always exactly one clear next action; the emptiness is intentional, not broken.

---

## Motion

**Duration:** instant 80 · fast 160 · base 240 · slow 400 (ms).
**Easing:** `--ease-standard` (general UI) · `--ease-entrance` (content in) · `--ease-exit` (out) · `--ease-physical` (tactile overshoot — rare, celebratory only).
**Expressive:** `--rotate-scrap-sm` −1.5° / `--rotate-scrap-md` 2.5° for layered decorative "scrap" tilt (≥`lg` only).
**Defaults:** hover/state changes use `--duration-fast`; enter/exit use `--duration-base`. **Honor `prefers-reduced-motion`:** disable tilt/overshoot/translate, keep opacity fades ≤160ms.

---

## Accessibility

- **Contrast:** body/ink on paper meets WCAG AA 4.5:1; large display meets 3:1. Re-verify any accent/functional text sitting on `-soft` fills.
- **Focus:** single shared visible ring (`--color-accent-blue`, 2px, 2px offset) on every interactive element. Never remove outlines.
- **Targets:** interactive elements min 44×44px.
- **Color independence:** status/errors always pair color with text or icon — never color alone.
- **Labels:** every input keeps a persistent visible label; helper/error text reserves its line.
- **Keyboard:** complete tab order; modals trap focus + close on Esc; tabs and nav operable via arrow keys; disclosure menus reachable.
- **Motion:** honor `prefers-reduced-motion`.
- **Semantics:** correct landmarks, non-skipping heading order (h1→h2→h3), and ARIA roles for nav/tabs/modals/toasts.

---

## Do / Don't

**Do**
- Map every surface to the base→raised→sunken hierarchy; step one level when nesting.
- Treat brand accent (one family/screen) and functional color (as needed) as separate systems.
- Assign every text element to a row in the typographic hierarchy table.
- Use deterministic spacing values from the application table.
- Keep corners sharp; create depth via overlap + soft physical shadows.
- Use the fixed z-index scale and defined breakpoints.
- Reference tokens by name.

**Don't**
- Use `#FFFFFF` backgrounds or `#000000` text.
- Fill more than one CTA (the single Accent button) with accent color, or use functional color decoratively.
- Use more than one pop-accent family, one display string, or competing heavy headings per view.
- Set yellow as text on light surfaces (contrast fail) — reserve it for marks/fills.
- Create type sizes in the 20–44px range (use the h3 17px-bold step instead).
- Round UI beyond `--radius-sm`, or use heavy/dark/colored shadows.
- Tilt or offset interactive/form elements, or apply decorative tilt below `md`.
- Invent ad-hoc z-index, spacing, or color values when a token exists.
- Center-align nav/tabs or build rigid symmetrical layouts.
