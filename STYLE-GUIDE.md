# Battle Strategy — Visual Style Guide

Rules for every character battle-strategy sheet. Goal: a single-page A4 combat
flowchart that is **easy to read in low light on printed paper**, and that prints
without manual scaling. Start every new sheet from `_template.svg`.

## File naming

- Each page is its own SVG: `<Character> battle strategy (Page N).svg` — full character
  name in title case, then `(Page 1)`, `(Page 2)`, etc. Most sheets are just `(Page 1)`.
- The printable PDF is `<Character> battle strategy.pdf` (no page suffix). For multi-page
  characters it contains all pages in order, merged with `cairosvg` + `pypdf`, ready to
  print double-sided.
- Files live in a folder named after the character (e.g. `Clover Darkbloom/`).

## Canvas & output

- **viewBox `0 0 595 842`**, `width="595" height="842"`. This is A4 at 72 dpi
  (595 × 842 pt), so the exported PDF is true A4 with no scaling.
- White background (`#ffffff`). **Do not use a dark theme** — it burns printer ink.
  Low-light legibility comes from contrast, size, and weight, not a dark page.
- **Print-safe area: keep all content within y 10–822** (and x 15–580). Real A4
  printers can't print the outer ~5–6mm (~14–18pt), so anything below ~y822 gets
  clipped. The footer block sits at y801–822 with the Synced line baseline at y819 —
  do not push it lower. Leave the same clearance at the other three edges.
- Export: SVG is the source of truth. PDF via `cairosvg ... dpi=72` (gives 595×842 pt).
  Regenerate the PDF only when asked; otherwise the SVG is the live file.

## Typography

- Font: Arial, sans-serif throughout.
- **Body text floor: 9px.** Footnotes/notes: **8px minimum** (italic). Never smaller.
- Titles and labels are **bold**. Decision text bold 10px; box titles bold 10px;
  the START node bold 12px; header title bold 17px.
- **Ink is near-black** (`#111111` / `#1a1a2e`). Use grey (`#555`) only for genuinely
  secondary sub-text (e.g. legend examples). No light-grey body text.
- **An inline icon needs its title to clear it.** The action-box shape icon sits at
  the left; the centred title must not reach it. Keep titles short (≈≤14 chars in a
  ~130px box, fewer in narrower boxes) and push the descriptive detail into the body
  lines — e.g. title "CLEAVE", not "GREATAXE + CLEAVE". If a title can't shrink, move
  the icon to the far-left edge and shift the title centre right.
- **Keep every string inside its box.** Text has no wrapping, so size it to fit:
  trim or abbreviate rather than overflow. In two-column rows (label + value), start
  the value column far enough left that the *longest* value still ends ~8px inside the
  right edge — for the ~130px bottom boxes that means a value column around x+70, not
  x+90. The bottom utility boxes and narrow action boxes are the usual offenders.
  Always confirm by rendering to PNG and eyeballing the edges before finalising.

## Colour (semantic, but never the only cue)

- Header / dark chrome: `#1a1a2e`. Survival/danger: red family (`#b32330`, `#9e1b27`).
- Control/utility: purple `#6f42c1`. Attack: blue `#0069d9` / orange `#d9690a`.
- Familiar/teal: `#138d75`. Caution banners: `#fff3cd` fill, `#856404` stroke.
- Colour must always be **backed by shape + text**, because print under warm/dim light
  desaturates and red/green is the common colour-blind axis.

## Shape vocabulary (the grayscale-safe cue)

Every action box carries a small drawn **vector** icon (not a font glyph — font
symbols render as tofu boxes in some pipelines).

- **Triangle = survival / defensive** (disengage, hide, hold a defensive reaction).
  Always amber (`#ffd633`) and always contains an **"!"** (a `class="bang"` text
  element centred in the triangle).
- **Diamond = control / utility** (saves, conditions, summons, terrain).
- **Circle = attack / damage** (cantrip or slotted damage).

The diamond and circle are **white-filled** with a thin dark outline. On a coloured
action box the icon is white with the box's own dark edge colour; in the SYMBOL KEY
(white background) keep the same white fill but give it a `#333` outline so it stays
visible — this matches how the icons actually appear on the sheet. The triangle is
amber + "!" everywhere.

A **SYMBOL KEY** box (top-right of the flowchart region) explains each shape used
on the sheet, with one example. Include it on every sheet; drop a row for any shape
the sheet doesn't use (e.g. a martial with no control spell). Space the rows ~26px
apart with the italic example ~10px below its label, and keep the last example ~8px
clear of the box edge so nothing crowds or overlaps the border.

## Decision flow

- Decisions are diamonds; actions are rounded rects with a 1.5px dark outline.
- Every box has a dark outline so its role reads in grayscale without relying on fill.
- **No dead ends.** A branch that represents a sub-action (e.g. "Familiar: Help")
  must loop back into the main flow, labelled (e.g. "then act").
- Arrowheads are filled triangles; arrow lines are continuous, **stroke-width 2.5**.
  Section dividers ≥ 1px solid.

## YES / NO edge labels (chips)

- Each label is a **white rounded-rect chip** (`rx 3`, 1px coloured stroke) with bold
  10px coloured text, matching the arrow's colour.
- The chip sits **beside the centre of the arrow, never on top of the line**:
  - **Vertical arrow → chip to the LEFT or RIGHT** of the line (aligned to its centre).
  - **Horizontal arrow → chip ABOVE or BELOW** the line (aligned to its centre).
- Pick the side that avoids collisions; the line stays continuous underneath.

## Page regions

**Always present:**

1. **Header** (dark bar): NAME — BATTLE STRATEGY, plus a one-line stat strip
   (level/race/class, HP, AC, save DC, attack mod, key resources).
2. **Reference area** — either **3 boxes** (Quick Reference, Survival Priority in red
   with "!", and a class box: spells / weapons / features) **or a single full-width
   strip** for martials with little to track (e.g. Oskar). Survival Priority box always
   carries the "!" triangle.
3. **Symbol Key** (top-right of flowchart).
4. **Main flowchart**: "YOUR TURN" → survival check → positioning → resource/attack logic.
   Branches go **both left and right** of the centre line and usually end in a
   **terminal row** of side-by-side outcome boxes.
5. **Bottom utility row** (4 boxes). Labels are class-dependent — pick from: Reaction,
   Bonus Action, Rituals / Spellbook, Movement, Cantrips / At-Will, Utility, Defenses.
6. **Footer** (amber): one-line reminder + **`Synced: YYYY-MM-DD · Lv<n>`** stamp.

**Optional, use when the character needs them** (snippets in `_template.svg`):

- **Pre-combat banner** (amber, above the flow): buffs/summons/free casts to do before
  initiative (e.g. Mage Armor, Find Familiar, Rage prompt).
- **Inline mid-flow callout banner**: a reminder placed *inside* the flow, coloured by
  role (e.g. Lysander's "give Bardic Inspiration"). Distinct from the pre-combat banner.
- **Section divider + title**: dashed full-width rule + centred bold 11px title, for
  sheets organised into sections below the flow instead of one chart (e.g. Oskar's
  Bonus Action / Movement Priorities).
- **Thin summary strip**: full-width one-liner, e.g. damage-per-turn totals (Oskar).
- **Extra bottom box-row**: 1–2 boxes below the footer for "Situational Options" /
  "Weapons (last resort)" (Lysander, Clover).

Keep everything on one A4 page; add optional regions only if they earn their space.

## Data accuracy

- All stats come from **D&D Beyond via ddb-mcp** (`ddb_get_character`), never guessed.
- Only stamp `Synced: <date>` after an actual live pull. Re-sync on level-up.
- Character IDs live in `characters.md`.
