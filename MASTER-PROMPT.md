# Master Prompt — Rebuild Zakat Quest From Nothing

**Use this if you must build inside the committee's own environment.** Pasting *code* is
forbidden there; pasting a *prompt* is not. So paste the block below into the AI assistant in
that environment and it will generate the game from scratch.

**Two things to know before you start.**

1. The result will be a working game faithful to this specification, but it will not be
   byte-identical to the file in this repository. Different assistants write different code.
   That is fine — and it is honest: it was prompted, not copied.
2. If the assistant stops partway through (long files often get truncated), do not start
   over. Say exactly this: **"Continue from where you stopped. Output only the remaining
   part of the file, no repetition, no summary."** Repeat until the closing `</html>` appears.

Recommended order if the assistant struggles with one shot: paste the whole thing, and if it
produces only a skeleton, follow with *"Now write Section 5 (all eight chapters) in full"*,
then *"Now write Section 6 (the calculator) in full"*.

---
## ⬇ PASTE EVERYTHING BELOW THIS LINE ⬇
---

Build a complete HTML5 educational game about **zakat** for secondary school students aged
13–18. Write it in English. Deliver it as **one single `index.html` file** containing all
HTML, CSS and JavaScript — no build step, no npm, no frameworks, no external libraries, and
no image or audio files. It must run by opening the file in a browser, and keep working
with no internet connection.

The game is called **Zakat Quest**, subtitle **The Path of Prosperity**.

## 1. Technical constraints

- One file. Inline `<style>` and `<script>`. Plain JavaScript, no modules, no bundler.
- The only permitted network request is Google Fonts (`Baloo 2` for display, `Plus Jakarta
  Sans` for body text). If it fails, system fonts must take over cleanly.
- All illustrations are **inline SVG written by you**. Do not use an icon library, do not
  link to stock images, do not use placeholder image services. Emoji are acceptable for
  small labels only.
- All sound is **synthesised with the Web Audio API**. No audio files.
- Progress saves to `localStorage`. Every read and write is wrapped in try/catch and the
  game must still run when storage is unavailable.
- Layout works from 390 px to 1440 px wide. Fixed viewport, no page scrolling — individual
  panels scroll internally. Use `100dvh`, not `100vh`.

## 2. Design system

Deep Islamic green and gold, modelled on Indonesian Ramadan calendar posters. Flat colour
fills, no gradients on components, soft green-tinted shadows on cards only.

Define these as CSS custom properties on `:root` and use them everywhere:

```
green   #0e6b46   main background
green-light #169160
green-dark  #0a5238
green-deep  #063b28
green-pale  #dcefe4
gold    #d4a94e   accents and primary buttons
gold-dark   #9a7726
gold-pale   #f6e7bc
cream   #fdf8ee
ink     #10352a   body text
red     #d9534f   wrong answers only
```

Visual signatures, all of which matter:

- **Background:** a radial green gradient, darker at the edges, overlaid with a **thin gold
  calendar grid** (vertical and horizontal hairlines forming cells).
- **Oversized numerals 1–8 sitting inside the grid cells** at about 10% opacity, the way
  dates sit on a calendar poster.
- **Three gold lanterns hanging on chains from the top edge**, swaying slowly, each with a
  soft glow. Different chain lengths.
- A crescent moon and a star in one top corner.
- A faint **eight-point star (girih)** motif tiling the background at very low opacity, and
  a girih star ring rotating slowly around the game emblem.
- A fine **paper grain** over the whole surface: a fixed, `pointer-events:none` overlay
  using an inline SVG `feTurbulence` data URI at about 5% opacity.
- **Arabesque divider** ornaments (a thin line with a small knot in the middle) between
  sections on the title screen and results card.
- Cards are white with a soft shadow tinted dark green, so they read as pinned paper.
- A faint eight-point star watermark in the top-right corner of each white panel.

Corner radius rule, followed everywhere: pill for controls (buttons, tabs, chips, map
nodes), large (26px) for cards and panels, standard (18px) for elements inside a card.

Typography: display type gets tight negative letter-spacing (about `-0.02em`) and
`text-wrap: balance`. Body prose is capped near 65–70 characters a line. Any number that
changes on screen — clock, score, statistics, currency — uses
`font-variant-numeric: tabular-nums` so digits stop jittering.

## 3. Screens and flow

Title → Map → (per chapter: Story → Quiz → Mini-game → Results) → back to Map.
Plus a Calculator screen and a Progress screen, reachable from the title and the map.

Transitions between screens use an **iris shaped like an eight-point star** (a full-screen
element with a star `clip-path`, scaling up then down). Motion uses a custom cubic-bezier
with slight overshoot, never the browser default easing.

**Title screen:** the emblem with its rotating girih ring, the title, the subtitle
"The Path of Prosperity", an arabesque divider, a row of fact chips reading
"8 chapters · 52 questions · 8 mini-games · 7 zakat calculators", a one-sentence blurb, and
buttons for Start, Calculator, Progress, How to Play, Settings. If progress exists, show a
saved-progress line and change Start to "Continue the Journey".

**Map screen:** a green calendar panel with a gold grid, chapter numerals in its cells, date
palm fronds in the corners, and the heading THE PATH OF PROSPERITY. Eight chapter markers
sit along a zig-zag path: cream circles with gold borders, a locked padlock for chapters not
yet reached, a red border on the current one, and earned stars under each label. The path is
drawn in gold — solid for the distance already travelled, dotted ahead. Chapter 1 is always
open; each chapter unlocks when the previous one is completed.

Tapping a marker opens a card showing the chapter title, a one-line description, question
count, stars earned and best score, with Start or Replay.

**Story screen:** a full-bleed illustrated backdrop with a dialogue box over it. Text types
out character by character and a tap completes it instantly. A dot for each line, a Skip
button, and named speakers with an emoji avatar.

**Quiz screen:** a white card with the question and four options labelled A–D, shuffled each
time. Three lives shown as hearts, a countdown timer bar, and a running score. Answering
correctly bursts small gold stars from the chosen option and floats the points gained
upward. Answering wrong nudges the option and removes a heart. Either way, an explanation
panel opens, with a scriptural reference where one applies. A Hint button reveals a clue and
removes one wrong option, at a cost of 30 points; hints are disabled in the final chapter.

Scoring: 100 base, plus up to 60 for answering quickly, plus 15 per consecutive correct
answer capped at 75. Losing all three lives ends the chapter.

**Important:** after answering, the Next button must be briefly disabled (about 300 ms) so
the same keypress that submitted the answer cannot also skip the explanation.

**Results screen:** three stars — three for a flawless run, two if one answer was missed,
one otherwise — with the score counting up, an arabesque divider, and a grid of statistics
(correct, wrong, accuracy, best streak, time, mini-game bonus). Buttons for the next
chapter, replay, and back to the map.

**Keyboard:** A–D or 1–4 answers, H is a hint, Enter continues, Escape closes a dialog.

## 4. Mini-games — write three engines, each driven by data

**Sort.** Cards must be placed into labelled baskets. Support drag-and-drop on desktop
**and** tap-card-then-tap-basket on touch, because drag-and-drop does not work on phones.
When a card is selected on a narrow screen, scroll the baskets into view. Check marks each
card green or red, lists what belongs where, and awards 60 points per correct card.

**Scale.** A balance beam tips according to how the wealth compares against the nisab —
**the heavier side must drop**. The player judges "Zakat is due" or "Not yet due" for each
case and gets an explanation. 80 points per correct call.

**Calc.** A number pad, also usable from the keyboard, for working out a zakat amount.
Accepts an optional hint. 90 points per correct answer.

## 5. The eight chapters

Each chapter needs: a title, a topic label, an emoji, a one-line description, six to ten
lines of illustrated story dialogue, six or more multiple-choice questions each with four
options, a hint, an explanation and a source where relevant, and one mini-game.

Write a distinct flat SVG backdrop for each chapter, all sharing the green-and-gold palette,
with gentle motion: a pulsing sun, swaying rice stalks, a bird crossing, village windows
glowing in turn, lanterns drifting.

**1. Gate of Knowledge** — what zakat means (to grow, purify, bless), that it is the third
pillar of Islam, the difference between obligatory zakat and voluntary infaq/sadaqah,
muzakki versus mustahik, that it is fard 'ayn, and what it purifies in the giver.
Sources: Qur'an 2:43, Qur'an 9:103. Mini-game: sort cards into Obligatory versus Voluntary.

**2. Barokah Market** — nisab and haul. Gold nisab **85 grams**, silver **595 grams**, rate
**2.5%** (one fortieth), haul is **one lunar year** of full ownership. Cash and savings
follow the gold nisab. Conditions for zakat on wealth: full ownership, growth potential,
reaching nisab, exceeding basic needs, free of due debt, haul complete. Mini-game: the nisab
scale, with five cases including one below nisab and one where the haul is incomplete.

**3. Green Fields** — crops, livestock and rikaz. Nisab **5 wasaq ≈ 653 kg** of dried grain.
**10%** where watering costs nothing (rain, spring), **5%** where irrigation is paid for.
Due **at every harvest**, no haul. Livestock nisab: camels 5, cattle 30, sheep and goats 40.
**Rikaz** — buried or mined wealth — is **20%**, immediately, with no nisab and no haul.
Sources: Qur'an 6:141; the hadith "no zakat below five wasaq"; "on rikaz, a fifth".
Mini-game: calculation.

**4. The Amil's Office** — income and trade. Zakat on professional income rests on
**MUI Fatwa No. 3 of 2003**, nisab **85 grams of gold per year**, rate 2.5%, payable monthly
as an advance on the annual duty. Trade formula: **(working capital + profit + collectible
receivables) − (debt falling due + losses), × 2.5%**, with inventory valued at selling price
when the haul completes. Mini-game: calculation.

**5. Asnaf Village** — the eight recipients from **Qur'an 9:60**: **fakir** (no wealth or
income at all), **miskin** (income that does not suffice), **amil** (administrators),
**muallaf** (new Muslims, hearts being reconciled), **riqab** (freeing from bondage),
**gharim** (crushed by lawful debt), **fi sabilillah** (in the path of Allah), **ibn sabil**
(stranded traveller). Not entitled: the wealthy, the able-bodied who can earn, and the
parents, children and spouse the giver already supports. Mini-game: sort villagers into
Fakir/Miskin, Gharim, Ibn Sabil, and Not Entitled.

**6. Night of Takbir** — zakat al-fitr. **One sa' ≈ 2.5 kg or 3.5 litres** of the local
staple food **per person**. Due from **every Muslim soul**, including a baby born before
sunset on the last day of Ramadan. Permitted from early Ramadan, obligatory at sunset on the
final day, **best before the Eid prayer**; paid after that prayer it counts as ordinary
sadaqah. It purifies the fast from idle talk and feeds the poor on the day of Eid.
Mini-game: sort payment moments into Best, Permitted, and Too Late.

**7. World of Giving** — zakat as a global instrument. This chapter carries the competition
theme, so give it weight. Cover: transferring zakat abroad (**naql al-zakat**) is generally
permitted, local need normally first but many scholars prefer sending it where need is far
more severe; refugees and displaced families qualify as **fakir, miskin and ibn sabil**
because the categories are defined by **circumstance, not nationality**; zakat's structural
advantage is that it is **obligatory and annual**, so unlike emergency appeals its flow is
predictable enough to fund multi-year programmes; and zakat maps onto the **Sustainable
Development Goals** — SDG 1 no poverty, 2 zero hunger, 3 health, 4 education, 6 clean water,
8 decent work. Mention **UNHCR's Refugee Zakat Fund**, launched 2019.

On the size of global zakat: say studies estimate it in the **hundreds of billions of US
dollars a year, with estimates varying widely**. Do not state a single precise figure as
fact — the methods behind those estimates genuinely differ.

Backdrop: a flat globe with dotted flight arcs between points, and four labelled aid icons
around it — CLEAN WATER, EDUCATION, HEALTH CARE, FOOD RELIEF. Mini-game: sort real zakat
programmes under the goals they serve.

**8. Final Test** — ten mixed questions drawing on every chapter, including arithmetic and
two on the global material. Shorter clock, no hints. Mini-game: four closing calculations.

## 6. Zakat calculator — seven types, genuinely usable

Tabs: **Income, Gold & Silver, Savings & Cash, Trade, Crops, Zakat al-Fitr, Rikaz.**
Results update live as the user types, showing the amount, a verdict of due or not due, a
line-by-line breakdown, and a note explaining the outcome.

- **Income:** monthly main and other income, with a Gross or Net method; Net deducts basic
  needs and that field is disabled under Gross. Compare against the monthly nisab
  (85 g of gold ÷ 12).
- **Gold & Silver:** grams of each, checked against 85 g and 595 g separately.
- **Savings & Cash:** savings, cash, investments and receivables, minus debt falling due.
- **Trade:** the formula from chapter 4.
- **Crops:** yield in kg, watering method choosing 10% or 5%, optional price per kg.
- **Zakat al-Fitr:** number of people × 2.5 kg, payable in food or its cash value.
- **Rikaz:** value × 20%.

A collapsible **Reference prices** panel holds an editable **currency label** (Rp, $, RM…)
and editable gold, silver and staple-food prices, all saved. State plainly that the shipped
figures are examples and must be updated to today's local prices.

**Number handling — get this right.** Use English convention: comma for thousands, dot for
decimals. Add thousands separators **while the user types**, but only when the caret is at
the end of the field, so editing mid-string is not disrupted. Write the parser and the
formatter as an exact pair so anything formatted reads back to the same value — do not use
heuristics that guess whether a dot is a separator or a decimal point.

## 7. Progress, settings and accessibility

- **24 stars** (three per chapter), points, six ranks from Beginner to Zakat Expert.
- **Nine badges:** first chapter, a flawless chapter, five correct in a row, using the
  calculator, four chapters done, finishing the global chapter, all 24 stars, beating the
  final test, finishing a chapter without hints.
- A local scoreboard of the ten best runs, and accuracy statistics.
- A progress screen listing every chapter with its stars and best score, plus the badges.
- **Settings:** sound effects on/off; ambient music on/off; **animation at three levels —
  Full, Light, Off**; larger text; high contrast. All saved.
- Respect `prefers-reduced-motion`. Visible focus rings. Ask the player's name once, when
  they first start, not on load.

## 8. Sound

Synthesise everything from oscillators. Tune the effects to the **maqam Hijaz** scale
(D, E♭, F♯, G, A, B♭, C) so they carry a Middle Eastern colour: short tones for clicks and
coins, a rising three-note figure for a correct answer, a falling one for a wrong answer, a
longer flourish on finishing a chapter.

Add **ambient background music**: a quiet sustained drone plus a slow melody drawn from the
same scale, low-pass filtered, fading in gently. Browsers block audio until the first user
gesture, so start it on the first tap rather than on load. Give it its own switch.

## 9. Before you finish

Check each of these against what you wrote:

- Every screen reachable, every button wired, no dead controls.
- The `hidden` attribute actually hides things — a CSS `display` rule will override it, so
  add `[hidden] { display: none !important; }`.
- Buttons never show white text on a white surface. Check every one.
- The balance beam tips with the heavier side dropping.
- Typing `18000000` into a currency field produces `18,000,000`, not something mangled.
- Pressing Enter to answer does not also skip the explanation.
- Nothing overflows or is clipped at 390 px wide.
- No console errors.

Write the complete file. Do not abbreviate, do not leave `// ...` placeholders, and do not
stop at a skeleton — the chapters and their questions must be written out in full.
