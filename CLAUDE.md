# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file Hebrew children's learning web app called **"לומדים עם אבון"** (`index.html`). No build system, no dependencies, no server — open `index.html` directly in any browser. Published on GitHub Pages at `https://assafhs-ship-it.github.io/Math-is-fun/`.

## Architecture

Everything lives in one file: HTML structure, CSS, and JavaScript. Screens are shown/hidden via `.screen` / `.screen.active` toggled by `showScreen(id)`.

### Screens

| Screen ID | Purpose |
|---|---|
| `welcome-screen` | Name input + subject picker (Math / English / Hebrew) |
| `question-screen` | Active math Q&A with scoring, help panel, auto-advance |
| `summary-screen` | End-of-session results |
| `math-menu-screen` | Math difficulty/mode picker |
| `english-menu-screen` | English activity picker |
| `phonics-screen` | Letter & phonics tile board (click to pronounce) |
| `abc-screen` | A–Z flashcard navigator |
| `word-picture-screen` | See image → pick word |
| `missing-word-screen` | Fill-in-the-blank sentences |
| `hebrew-menu-screen` | Hebrew activity picker |
| `heb-missing-word-screen`, `heb-letter-screen`, `heb-count-screen`, `heb-syllable-screen`, `heb-sounds-screen` | Hebrew exercises |
| `fractions-menu-screen` | Fractions sub-menu |
| `frac-find-screen` | Identify fraction from circle diagram |
| `frac-as-screen` | Add/subtract fractions MCQ |
| `frac-md-screen` | Multiply/divide fractions MCQ |
| `mult-table-screen` | Interactive 10×10 multiplication table |
| `long-div-screen` | Long division with step-by-step help |

### JavaScript Sections (in order)

- **STATE** — global counters, current question, difficulty, subject
- **AUDIO** — `playSuccess()` / `playWrong()` via Web Audio API; `getAudio()` lazy-inits `AudioContext`
- **BG SYMBOLS** — floating math symbols on the welcome screen
- **CONFETTI** — canvas-based confetti on correct answers (`launchConfetti()`)
- **QUESTION GENERATOR** — `generateQuestion()`, `getSubLevel(n)` (1/2/3 based on question count), `pickOp(ops, weights)`, `buildExpl()` → delegates to `buildAddHTML` / `buildSubHTML` / `buildMultHTML` / `buildDivHTML`
- **GAME FLOW** — `selectSubject()`, `selectDiff()`, `loadQuestion()`, `checkAnswer()`, `showHelp()`, auto-advance timer (`startAutoNext` / `clearAutoNext`)
- **FIND THE FRACTION** — `startFracFind()`, `ffGenQ()`, `ffLoad()`, `ffCheck()`, `ffHelp()`, `drawFracCircle()`, `ffSlicePath()`
- **MULTIPLICATION TABLE** — `showMultTable()` builds the 11×11 grid dynamically with hover row/col highlighting
- **FRAC ADD/SUBTRACT** — `startFracAS()`, `fasGenQ()`, `fasLoad()`, `fasCheck()`, `fasHelp()`, `fasMiniSVG()`
- **FRAC MULTIPLY/DIVIDE** — `startFracMD()`, `fmdGenQ()`, `fmdLoad()`, `fmdCheck()`, `fmdHelp()`
- **LONG DIVISION** — `startLongDiv()`, `ldGenQuestion()`, `ldBuildSteps()`, step navigator (`ldStepPrev` / `ldStepNext`)
- **WORD PICTURE DATA / GAME** — `wordQueue`, `startWordPicture()`, `speakEng(word)`
- **PHONICS TILES** — `startPhonics()`, `buildPhonicsTiles()` (built once, cached), `ptSpeak(tile, sound, word)`, `PT_WORDS`, `PT_PHONICS`
- **ENGLISH / ABC** — `startABC()`, `showLetter()`, `speakLetter()`
- **MISSING WORD** — `startMissingWord()`, sentence fill-in game
- **HEBREW EXERCISES** — missing word, letter recognition, counting, syllables, sounds; `speakHebPart()`, `speakHebSyl()`, `speakHebSounds()`
- **BACKGROUND MUSIC** — Web Audio API oscillator-based music, toggled via options menu
- **FUN MODE** — `body.fun-mode` CSS class toggled on `<body>`; overrides colors/styles via `!important` rules in a dedicated CSS block
- **INIT** — event listeners, initial screen setup

### Key Shared Utilities

- `ffFracHTML(n, d, size)` — renders an inline stacked fraction as HTML (used by frac-find, frac-add/sub, frac-mul/div)
- `ffSlicePath(cx, cy, r, startDeg, endDeg)` — SVG arc path for fraction circle slices (used by `drawFracCircle` and `fasMiniSVG`)
- `gcd(a, b)` / `lcm(a, b)` — shared by frac-add/sub and frac-mul/div
- `speakEng(word)` — `speechSynthesis` with `lang='en-US'`, `rate=0.82`

### Difficulty & Progression

- **Math easy**: `+` and `−`, single → double digits; sub-level increases every 5 questions via `getSubLevel()`
- **Math medium**: `×` and `÷`, 1–2 digit operands
- **Math hard**: `×` and `÷`, 3–4 digit operands
- **Fractions**: all three fraction screens start easy (small denoms / same denom) and raise difficulty every 5 questions
- **Long division**: random within difficulty band, step-by-step help with forward/back navigation

### Layout Conventions

- App is Hebrew/RTL (`direction: rtl` on body). Math expressions and English content use `direction: ltr` or `<bdi dir="ltr">` inline.
- Fraction circles are SVG, generated in JS, not static HTML.
- The multiplication table grid is built entirely in JS on first call and cached (never rebuilt on re-entry).
- `body.fun-mode` overrides are in a single CSS block near the bottom of `<style>` — add new fun-mode overrides there.

## Publishing

```bash
git add index.html
git commit -m "description"
git push
```

GitHub Pages serves `index.html` from the `main` branch automatically.
