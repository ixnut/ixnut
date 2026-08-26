# INK × smart v2 — fixes + 4 design variants (locked)

File: HZToawEyV4hNcB7C7wKq2a — **DESIGN file**, 67 frames, all now 1920×1080.
Page 1 currently holds VARIANT A (built, needs fixes below).
Fonts: Inter (Good Sans will NOT load — environment limit). Mono: Roboto Mono.

═══════════════════════════════════════════
PART 1 — FIXES TO VARIANT A (do first)
═══════════════════════════════════════════

## FIX 1 — Delete every "Accent mark"
User: "a big em dash that has no purpose, no reason to be there." Correct — it is
unmotivated decoration. Delete ALL nodes named "Accent mark" across all 67 frames.
Do not replace with anything. One line:
  page.children.forEach(f => f.children.filter(c=>c.name==="Accent mark").forEach(c=>c.remove()))

## FIX 2 — Hard rule: no partial bleed
Every element must EITHER sit fully inside x 120–1800, OR bleed to a true frame
edge (x=0 or right edge exactly 1920). A partial overhang (e.g. ending at 1980)
reads as broken, not intentional. This was the bug in the user's screenshots.

Frames to correct (right edges currently 1960–1980):
- 1:88  Creative Opportunity — imgs → x=120 w=600 | x=750 w=460 | x=1240 w=560
- 1:113 Hero Moodboard — tile D x=1320 → w=480 (ends 1800)
- 1:108 Digital Twin Aircraft — see FIX 3
- 1:237 HY11 mosaic — tiles ending 1980 → clamp to 1800
- 1:370 / 1:366 — second image x=970 → w=830 (ends 1800)
Case-study openers (1:352, 1:374, 1:72, 1:420, 1:410, 1:415): image x=880 w=1040
ends exactly at 1920 = true full bleed. KEEP — that one is legitimate.

Sweep after: any RECTANGLE/ELLIPSE with x+w between 1801 and 1919 → resize to 1800-x.

## FIX 3 — 1:108 Digital Twin Aircraft is broken
Same imageHash (deed4116…) placed twice at 900×540 with FILL — the source is
already a composite grid, so it tiles and repeats. Rebuild: ONE image, x=1040
y=120 w=760 h=840, scaleMode FIT (not FILL) so the composite is not cropped.
Move body copy up to fill the left column; drop the second image entirely.

## FIX 4 — Slide 08 (1:101) left column bottom is dead space
After the accent mark is gone, move the sub-paragraph down to y=520 so the
column reads as two weighted blocks rather than floating at the top.

═══════════════════════════════════════════
PART 2 — FOUR VARIANTS, GENUINELY DIFFERENT
═══════════════════════════════════════════
Duplicate the PAGE (figma.root.children — use page.clone() if available, else
create page + clone frames into it). One page per variant. Name pages:
  "A — Studio"  "B — Keynote"  "C — Product"  "D — Technical"
If page.clone() is unavailable in this API build, duplicate frames within Page 1
into a new column block far to the right (x offset +20000 per variant).

These must NOT look like siblings. Different alignment, weight strategy,
density, colour and image treatment — not just a palette swap.

## A — STUDIO  (built)
White-led, near-black dividers. Inter Extra Light/Thin display, left-aligned,
120px margins, Roboto Mono eyebrows with indices, hairline rules, footer with
logo + NN/67. Monochrome; colour only from photography.

## B — KEYNOTE  (radically minimal, dark)
The Apple event stage. Rules:
- EVERY frame #000000. No white frames at all.
- ONE idea per frame. If a frame has 4 bullets, it becomes 4 frames — expand the
  deck, do not compress. Target ≤ 2 text elements per frame.
- Type CENTRED, vertically and horizontally. Inter Semi Bold 120–200 for the
  statement. No thin weights — this variant is about weight, not delicacy.
- NO footers, NO page numbers, NO eyebrows, NO rules, NO mono. Nothing but the words.
- Images full-bleed 0,0,1920,1080 with the headline overlaid centred in white.
- Numbers get the hero treatment: "2027" at 400pt, label beneath at 32pt.
- Zero gradients.

## C — PRODUCT  (apple.com product page, light + warm)
- Background #FAFAFA with occasional pure-white bands; large soft-grey panels
  #F0F0F2 at cornerRadius 28.
- Type CENTRED, Inter Semi Bold 72–96 headline + Inter Regular 24 body directly
  beneath, max width 900, centred. Marketing cadence, not editorial.
- Images inside rounded cards (cornerRadius 24), generous 80px internal padding,
  never full-bleed, always centred.
- Accent colour permitted here ONLY: a single blue #0071E3 (Apple's link blue) for
  small "Learn more"-style labels and rule accents. Everything else greyscale.
- Sections separated by full-width band colour changes, not rules.
- No mono type anywhere. No eyebrow labels — use a small centred category word.

## D — TECHNICAL  (spec sheet / Pro page, dense and precise)
- White. Strict 12-column grid, 100px margins, 24px gutters.
- NO display type above 48pt anywhere. The whole variant runs small and precise.
- Inter Medium 18 headings, Inter Regular 15 body, Roboto Mono 12 for all
  numerals, labels, units and keys.
- Heavy use of tabular rows with 1px rules, key/value pairs, and left-aligned
  columns. Data-sheet feel.
- Images small, inline, aligned to grid columns — never hero-sized.
- Section numbering as "§01.02" in mono.
- Palette: white, #111 text, #767676 secondary, #E3E3E3 rules. No black frames.

═══════════════════════════════════════════
EXECUTION ORDER (budget-aware)
═══════════════════════════════════════════
1. Part 1 fixes to Variant A. Validate clean. ← highest value, do first
2. Variant B (most visually distinct, biggest payoff).
3. Variant C.
4. Variant D.
Finish each variant COMPLETELY before starting the next. Four half-built
variants are worthless; two finished ones are not.

## Validation after every batch
right-margin ≤1800 (or exact edge), no text overlap >6px, no text bottom >984
when a footer exists, no text bottom >1080, no zero-height text.

## Known content flags to carry into every variant
- NIO frame (1:415) carries Joby's copy verbatim, no imagery.
- 1:200 / 1:207 Model B slides have no real content.
- Slide 21 second "SCALE" was relabelled MOMENTUM.
- "DIGITAL & SOCIAAL" typo fixed to SOCIAL.

═══════════════════════════════════════════
PART 3 — QUALITY AUDIT (do before variants C/D)
═══════════════════════════════════════════

## BUG 1 — footer mark invisible on split-layout frames  (CONFIRMED, user-reported)
Cause: the footer replacement placed "INK x Smart" at a hardcoded x=1400 w=400
(right edge 1800) on EVERY frame. The six case-study openers (1:352 Polestar,
1:374 Air Canada, 1:72 Scout, 1:420 Lightship, 1:410 Joby, 1:415 NIO) use a split
layout — white text panel x 0–880, full-bleed image x 880–1920 — and their footer
rule only spans 120–740. So the mark landed ON the photograph in dark grey:
invisible, and pointless where it is.

FIX (general, not per-frame): for every frame, find its "Footer rule" rect and
right-align the "Footer mark" text to that rule's right edge:
  markRightEdge = rule.x + rule.width
  mark.resize(400, h); mark.x = markRightEdge - 400; mark.textAlignHorizontal="RIGHT"
Never hardcode 1400/1800 again. Same rule for the INK logo on the left:
logo.x = rule.x.

## Systematic audit to run in the same pass
1. TEXT-OVER-IMAGE CONTRAST. For every TEXT node, test whether its bbox overlaps
   any IMAGE-filled rect in the same frame. If it does and there is no scrim,
   flag it. Either move the text off the image or add a scrim. (Variant B
   deliberately overlays text on images WITH a scrim — that is fine.)
2. FOOTER GEOMETRY. logo.x == rule.x, mark right edge == rule right edge,
   both vertically consistent (logo y=1012 h=32, mark baseline aligned).
3. LEFT-EDGE ALIGNMENT. Within a frame, collect x of all left-aligned text.
   Large display type intentionally sits at 116 (optical offset vs 120 body).
   Flag any OTHER pair differing by 1–8px — that is sloppiness, not optics.
4. SAFE MARGINS. Nothing between x 1801–1919 or 1–119 unless it bleeds to a
   true frame edge (0 or 1920).
5. DEAD FRAMES. Any frame whose only content is a footer + eyebrow — it has lost
   its body copy. Report, do not silently fill.
6. ORPHANS. Zero-height text, zero-opacity nodes, nodes fully outside the frame,
   duplicate stacked nodes at identical coords.
7. VARIANT B CHECK. B has no footers/eyebrows by design — confirm none leaked in
   from clones of the image-only frames.

Report every finding with frame name + node before fixing, so the user sees the
list rather than a silent rewrite.
