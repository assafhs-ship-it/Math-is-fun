# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file Hebrew math practice web app for children called **"Learning is Fun"** (`index.html`). There is no build system, no dependencies to install, and no server required — open `index.html` directly in any browser.

## Architecture

Everything lives in one file (`index.html`): HTML structure, CSS styles, and JavaScript logic.

**Screens** (shown/hidden via `.screen` / `.screen.active`):
- `#welcome-screen` — name input + difficulty picker
- `#question-screen` — active question, scoring, help, and finish
- `#summary-screen` — end-of-game results

**Key JavaScript components:**
- `generateQuestion()` — builds a question based on `selectedDiff` ('easy' / 'medium' / 'hard') and `getSubLevel(questionCount)` (1/2/3), which controls how hard numbers get within each difficulty
- `buildExpl(op, a, b, answer)` — returns an HTML string with a stacked visual + step-by-step explanation; delegates to `buildAddHTML`, `buildSubHTML`, `buildMultHTML`, `buildDivHTML`
- `pickOp(ops, weights)` — weighted random operation selector
- `launchConfetti()` / `playSuccess()` / `playWrong()` — feedback on correct/wrong answers using Canvas and Web Audio API

**Difficulty modes:**
- Easy: `+` and `−` only, single → double digits
- Medium: mostly `×` and `÷`, 1–2 digit operands
- Hard: `×` and `÷` only, 3–4 digit operands

**Language:** Hebrew (RTL). Math question text uses `direction: ltr` so numbers display left-to-right.

## Publishing

The file is published as `index.html` on GitHub Pages at:
`https://<username>.github.io/learning-is-fun/`
