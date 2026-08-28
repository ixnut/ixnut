# INK × smart — "Studio" design language
Definitive spec. Reproduce exactly; do not reinterpret.

## CANVAS
1920 × 1080 (16:9). Anything else letterboxes in Present — resize and reflow.
Margins 120 left/right. Content lives strictly within x 120–1800.

## PALETTE  (Figma uses 0–1; hex given for reference)
white / bgL   #FFFFFF   1, 1, 1
ink           #08080A   0.031, 0.031, 0.039     dark backgrounds + text on white
muteL         #6E6E76   0.431, 0.431, 0.463     secondary text on white
ruleL         #E0E0E3   0.878, 0.878, 0.890     hairlines on white
tile          #F2F2F4   0.949, 0.949, 0.957     placeholder panels on white
muteD         #9A9AA3   0.604, 0.604, 0.639     secondary text on dark
dimD          #666671   0.400, 0.400, 0.443     tertiary text on dark
ruleD         #2A2A30   0.165, 0.165, 0.188     hairlines on dark
ghost         #131317   0.075, 0.075, 0.090     giant section numerals on dark
footerMark    #2E2E33   0.180, 0.180, 0.200     "INK x Smart"

Monochrome only. The sole colour in the deck comes from photography.

## TYPE
Inter + Roboto Mono. (Good Sans is the brand font but CANNOT be loaded — see gotchas.)

role            family / style              size    lineHeight   letterSpacing
eyebrow         Roboto Mono Regular         15      —            3.4
index / micro   Roboto Mono Regular         13      —            2.4
caption         Roboto Mono Regular         12      —            2.2
display XL      Inter Extra Light           132–176 size × 1.06  —
display L       Inter Thin                  88–112  size × 1.09  —
display M       Inter Thin                  64–80   size × 1.10  —
lead            Inter Light                 24–30   36–44        —
title           Inter Medium                24–30   32–38        —
body            Inter Light                 17–19   27–30        —
footer mark     Inter Regular               20      —            —
section numeral Inter Extra Light           400     400          —

HARD RULE: nothing between 32 and 80pt. That dead zone is deliberate — the whole
system depends on a ~10× jump from mono label to display.

## VERTICAL GRID
eyebrow            y = 96
display            y = 170–260 (content) / 430 (dividers)
footer rule        y = 990
footer logo        y = 1012, w 68, h 32, scaleMode FIT
footer mark        y = 1015
Body content must end above y = 984 on any frame carrying a footer.

## FOOTER  (content frames only — never on cover, dividers, or closer)
rule (1px, ruleL) + INK logo left + "INK x Smart" right.
Derive positions from the rule, NEVER hardcode:
  logo.x = rule.x
  mark.textAlignHorizontal = "RIGHT"
  mark.resize(400, h); mark.x = (rule.x + rule.width) - 400
On split layouts the rule spans only the text panel, so the mark follows it.
Hardcoding put the mark on top of photography — a real shipped bug.

## EYEBROW
Uppercase, mono, letterspaced. PLAIN LABEL ONLY.
No index numbers. No em dashes.   `OUR ROLE`  not  `03 — OUR ROLE`
Dividers use the single word `SECTION`; the giant numeral states which one.

## TONAL ARC
Dark (ink) is reserved for: cover, every section divider, and the closer.
Everything else is white. Dark is for emotion, white is for information.

## GRADIENT  (dark frames only, never on white)
ONE radial ellipse per frame. Never two.
  size ~5200 × 4400  (≈3× canvas)
  GRADIENT_RADIAL, gradientTransform [[1,0,0],[0,1,0]]
  stops: 0 → white a 0.05 · 0.5 → white a 0.0175 · 1 → white a 0
  centre anchored OFF-canvas, so only the falloff is visible
Test: if you can see where the ellipse ends, it is too small or too strong.

## LAYOUT PATTERNS  (vary the anchor slide to slide — never march down one template)

THREE COLUMN — cols x 120 / 700 / 1280, w 500. Vertical rules x 670 & 1250, y 606–906.
  index y 608 · title Medium 29 y 646 · body Light 19 y 750

SPLIT ⅓:⅔ — left x 120 w 520 (eyebrow, display Thin 112 y 196, sub Light 21 y 372).
  right x 800 w 1000, rows y = 170 + i×196: rule w 1000 · index y+28
  · title Medium 27 x 878 y+20 · body Light 18 w 880 x 880 y+66

FULL-BLEED TIMELINE — rule x 0 w 1920 y 640. Columns x 120 / 760 / 1400, w 400.
  index y 594 · dot ⌀15 y 633 · title Medium 30 y 692 · body Light 19 y 750

INDEX LIST — left x 120 w 560 (display Thin 88 y 196, sub Light 20 w 490 y 520).
  right x 760 w 1040, rows y = 178 + i×136: rule w 1040 · index y+42
  · label Extra Light 52 x 858 y+30.  Closing row: rule y 858, label Medium 28 y 892.

IMAGE BAND — images y 468 h 322 at x 120 w 600 / x 750 w 460 / x 1240 w 560.
  index y 822 · title Medium 26 y 852 · body Light 17 y 894

CASE-STUDY SPLIT — image x 880 w 1040 h 1080, true full bleed right.
  left: eyebrow y 96 · brand Thin 96 w 700 y 330 · desc Light 22 w 620 y 512
  · rule x 120 w 620 y 656 · "CAPABILITY" mono 12 y 676 · list Light 19 y 710
  footer rule x 120 w 620.

FOUR-STEP ROW — cols x = 120 + i×440, w 360. rule at y · index y+22
  · title Medium 24 y+60 · body Light 17 w 340 y+110

DIVIDER (dark) — one glow · numeral Extra Light 400 in `ghost` at x 1620 y 10
  (crops off the right edge) · eyebrow `SECTION` at 120, 96
  · display Extra Light 132–172, w 1400, x 116, y 430
  · sub Light 26–30, w ≤1250, x 120, y = 430 + displayHeight + 60
  · NO footer

PLACEHOLDER GRID — tiles fill `tile`, 1px `ruleL` stroke,
  caption mono 13 at tile.x + 32, tile.y + 30

## OPTICAL DETAIL
Display type sits at x = 116, body at x = 120. That 4px offset compensates for
side bearing on large thin type so the left edge reads flush. Only for display
sizes — at body sizes it just looks misaligned.

## ANTI-RULES  (each one learned from a rejected version)
- NO decorative accent marks. A 140×2 rule above headings was added as ornament;
  the client called it "a big em dash with no purpose". All 16 were deleted.
  If a mark does not carry meaning, it does not go on the slide.
- NO partial bleed. Every element sits fully inside 120–1800 OR bleeds to a true
  frame edge (0 / 1920). Anything ending at 1801–1919 reads as broken.
- NO numbers or em dashes in eyebrows.
- NO type between 32 and 80pt.
- NO second gradient on a frame.
- NO text on imagery without a scrim.
- NO template repetition — vary the anchor and composition frame to frame.

## VALIDATION  (run after every batch; fix before continuing)
fail if:
  two TEXT nodes overlap > 6px on both axes
  text bottom > 984 on a frame with a footer
  text bottom > 1080, or text y < 0
  text x + width > 1815 while x < 1500
  any non-bleed node with right edge in 1801–1919
  text height < 2
Screenshot only when validation fails, plus once per section as a visual check.

## FIGMA API GOTCHAS  (these will break you)
1. Existing text nodes report fontName.family "Good Sans" even though they render
   Inter. "Good Sans" cannot be loaded. So you CANNOT set .characters, .fontSize,
   .fontName, .lineHeight or .letterSpacing on existing text — it throws.
   You CAN set .x, .y, .name, .fills and call .resize().
   To change text: read .characters → delete node → create new in Inter.
   Never call frame.rescale() on a frame containing text.
2. Wrapping text: set textAutoResize = "HEIGHT" THEN resize(w, h). FILL alone
   collapses the node to near-zero width.
3. Colors are 0–1. Paint color objects take {r,g,b} only — opacity sits on the
   paint, not the color. Gradient stops DO take RGBA.
4. Fills are read-only arrays — clone, mutate, reassign.
5. Composite source images must use scaleMode FIT, not FILL, or they tile.
6. Identify nodes by CONTENT, not by font family — the family is unreliable.

## KNOWN CONTENT FLAGS  (carry forward, never silently fix)
- The NIO slide carries Joby's copy verbatim and has no imagery.
- Model B slides have no real content.
- Source listed "SCALE" twice; the second was relabelled MOMENTUM.
- Source typo "DIGITAL & SOCIAAL" corrected to "DIGITAL & SOCIAL".
- The INK logo asset is black-on-transparent — invisible on dark. On dark frames
  set "INK" in Inter Semi Bold instead of placing the logo image.
