# 🌙 Zakat Quest — The Path of Prosperity

An HTML5 learning game about **zakat**, built for secondary school students (ages 13–18).
Eight illustrated chapters carry the player from their own pocket to the wider world:
story, timed quizzes, mini-games, and a **zakat calculator that actually works**.

Built for **IZE-FEST 10th 2026 — International Zakat Education Festival**, MABAR competition,
on the theme **“The Path of Prosperity”**, for the **Cendekia Edu** platform.

**▶ Play:** `https://kanwjpg.github.io/Game_Zakat_Competition_Cendekia-EDU/`
*(live once GitHub Pages is enabled — see Deployment below)*

---

## ✨ What is in the game

### Eight chapters along one path
| # | Chapter | Topic |
|---|---------|-------|
| 1 | 🕌 Gate of Knowledge | Meaning of zakat, its ruling, zakat vs sadaqah |
| 2 | 🪙 Barokah Market | Nisab, haul, zakat on gold, silver and cash |
| 3 | 🌾 Green Fields | Crops at 5% / 10%, livestock, rikaz |
| 4 | 🏢 The Amil’s Office | Zakat on professional income and on trade |
| 5 | 🤲 Asnaf Village | The eight categories of recipients |
| 6 | 🌙 Night of Takbir | Zakat al-fitr: measure and timing |
| 7 | 🌍 **World of Giving** | Zakat as a global instrument: the SDGs, refugees, cross-border giving |
| 8 | 🏅 Final Test | Mixed trial — tighter clock, no hints |

Every chapter runs **story → quiz → mini-game → results**.
**52 questions** in total, each with an explanation and, where relevant, its scriptural source.

### Three kinds of mini-game
- **Card sorting** — drag and drop on desktop, tap-then-choose on phones.
- **The Nisab Scale** — a balance that tips with the weight of the wealth against the nisab.
- **Quick Calculation** — a number pad, also usable straight from the keyboard.

### Zakat calculator (7 types)
Income · Gold & Silver · Savings & Cash · Trade · Crops · Zakat al-Fitr · Rikaz.

Gold, silver and staple-food prices are **editable and saved**, and so is the **currency label** —
set it to `Rp`, `$`, `RM` or anything else, so the tool works outside Indonesia too.
The shipped figures are examples and **must be updated** to today's prices.

### Progress and motivation
24 stars, points, six ranks (Beginner → Zakat Expert), **9 badges**, a local scoreboard,
and accuracy statistics. All stored in `localStorage`.

### Visual style
Modelled on Indonesian Ramadan poster design: **deep pesantren green and gold**, a thin
**gold calendar grid**, oversized chapter numerals sitting in the grid cells like dates,
**gold lanterns hanging on chains** with a soft glow, a crescent and stars, and white cards
that read like pinned paper. Islamic detail carries through in **arabesque dividers**, an
eight-point star (girih) motif, and screen transitions shaped like an eight-point star.
The palette holds to green, gold and cream, with red reserved for mistakes.

### A touch of 3D
Correct answers scatter **spinning gold coins** built from CSS 3D transforms — two circular
faces and a ten-segment rim, with one consistent light direction. Real perspective, real
rotation, no 3D library and no model files, so the single-file and offline guarantees hold.

### Sound
Every sound is **synthesised with the Web Audio API** — the game ships with no audio files at all.
Effects use the **maqam Hijaz** scale (D–E♭–F♯–G–A–B♭–C), and an optional ambient drone and slow
melody play underneath. Both can be switched off.

### Accessibility
- Keyboard: `A–D` / `1–4` to answer, `H` for a hint, `Enter` to continue, `Esc` to close.
- Toggles for **sound**, **ambient music**, **larger text**, **high contrast**.
- Three **animation levels** — Full, Light, Off — and `prefers-reduced-motion` is respected.

---

## 🚀 Running it

No build step, no dependencies, one file.

```bash
# fastest
open index.html in a browser
```

Or through a local server, which mirrors how it behaves when hosted:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

### Deployment — GitHub Pages
1. Open **Settings → Pages** in this repository.
2. Under *Source*, choose **Deploy from a branch**.
3. Pick the branch holding this code and the folder **`/ (root)`**, then **Save**.
4. After a minute the game is live at
   `https://kanwjpg.github.io/Game_Zakat_Competition_Cendekia-EDU/`

A `.nojekyll` file is included so GitHub serves the files as they are.
That URL is what you hand to Cendekia Edu, or embed in an `<iframe>`.

The only network request the game makes is for Google Fonts. If that is blocked, the layout
falls back to system fonts and everything still works.

---

## 🧱 Code structure

Everything lives in `index.html`, laid out in reading order:

| Section | Contents |
|---------|----------|
| `<style>` | Flat design tokens, components, motion system, responsive layout |
| 1–3b | Utilities, storage, sound effects, ambient music |
| 4–6 | Motion helpers, UI helpers, the screen router |
| 7 | `sceneSVG()` — eight flat animated story backdrops, drawn in code |
| 8 | **`CHAPTERS`** — all content: story, questions, mini-games |
| 9–13 | Progress, the map, game state, the story engine, the quiz engine |
| 14 | Mini-game engines (`sort`, `scale`, `calc`) |
| 15–16 | Results screen, zakat calculator |
| 17–19 | Scoreboard and badges, settings, boot |

### Adding a chapter
Append one object to `CHAPTERS` — the map, unlocking, stars and scoreboard all adjust by themselves:

```js
{
  id: 9, title: 'New Chapter', topic: 'Topic', emoji: '📗', scene: 'market',
  blurb: 'One line for the chapter card.',
  story: [{ c: 'ustadz', t: 'Opening line…' }],
  questions: [{
    q: 'Question?',
    opts: ['A', 'B', 'C', 'D'],
    a: 0,                       // index of the correct option (options are shuffled at runtime)
    hint: 'A short nudge.',
    why: 'The explanation shown after answering.',
    dalil: 'Source reference (optional).'
  }],
  mini: { type: 'sort', /* or 'scale' / 'calc' */ name: '…', desc: '…', /* … */ }
}
```

Then add one coordinate to `NODE_POS` for its stop on the map.

### Changing the colours
All colours sit in one `:root` block at the top of `<style>`. Three tokens carry the whole look:

```css
--teal:#1e9184;   /* background      */
--sand:#e8a75f;   /* accents, buttons */
--cream:#fff7ec;  /* light surfaces  */
```

Story illustrations read the same palette through the `PAL` object in section 7.

---

## 📚 Sources for the rulings

The game follows mainstream zakat fiqh as applied in Indonesia:

- Nisab of **85 g** gold, **595 g** silver, rate **2.5%**, one lunar year (haul).
- Zakat on income — **MUI Fatwa No. 3 of 2003**, reinforced by Law No. 23 of 2011.
- Crops — nisab **5 wasaq ≈ 653 kg**; **10%** without watering cost, **5%** with paid irrigation.
- Livestock — camels 5, cattle 30, sheep and goats 40.
- Rikaz — **20%**, with no nisab and no haul.
- Zakat al-fitr — **one sa‘ ≈ 2.5 kg** of staple food per person.
- The eight recipients — **Qur’an 9:60**.

Chapter 7 covers zakat beyond one country: the permissibility of transferring zakat abroad,
refugees and displaced families as fakir, miskin and ibn sabil, the mapping of zakat programmes
onto the **Sustainable Development Goals**, and UNHCR's Refugee Zakat Fund. Figures for global
zakat potential are presented as **estimates that vary widely**, because they genuinely do.

> **Note.** This is a learning tool. Gold, silver and food prices change daily — update the
> reference figures before relying on a result. For complicated cases, consult a qualified
> amil or scholar.

---

## 🖥 Browser support

Current Chrome, Edge, Firefox and Safari, on desktop and mobile.
Layout tested from 390 px to 1440 px wide.

---

## 🎯 Design audit

The interface was put through a formal design audit (typography, colour and surfaces,
layout, interactive states, code quality) and the findings fixed: display tracking and
balanced text wrapping, tabular figures so changing numbers stop jittering, prose capped
near 65 characters a line, a documented corner-radius rule, a named z-index scale, a custom
motion curve in place of the browser default, `100dvh` for iOS Safari, a fine paper grain
over the whole surface, and social preview cards.

Deliberately **not** applied, because they fight the brief: marketing-page archetypes
(scroll-pinned heroes, floating glass navbars, hamburger menus) — this is a fixed-viewport
game with no page scroll; nested "double-bezel" card shells — they would fight the flat
poster look the artwork is modelled on; and swapping the hand-drawn SVG illustrations for
an icon library — those drawings are the original work being judged, and a library would
break the single-file, zero-dependency build.

---

## 🤖 How it was built

Written with **AI-assisted development** using Claude Code, as the competition intends,
and under **Competition Integrity Mode**: no code was copied or pasted in from a template,
tutorial, generator or other project. The repository started empty and the whole game was
produced from prompt instruction.

**[`PROMPTS.md`](PROMPTS.md) is the full build log** — six prompt rounds, six commits, each
one traceable with `git log --reverse --stat`.

Two prompts rebuild this entire game from an empty file. Both are prompt text and nothing
else, so select-all and paste is safe.

| File | Size | Use it when |
|------|------|-------------|
| [`PROMPT-SHORT.txt`](PROMPT-SHORT.txt) | 116 lines | The usual choice — every fact and trap, none of the prose |
| [`PROMPT-PASTE.txt`](PROMPT-PASTE.txt) | 350 lines | The assistant needs more hand-holding, or you want the full reasoning |

[`MASTER-PROMPT.md`](MASTER-PROMPT.md) is the long one with usage notes attached.
Each build was verified by driving a real Chromium browser with Playwright: completing
chapters end to end, all three mini-game types, the out-of-lives path, every calculator tab,
all three animation levels, and the mobile layout.
