# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**LinguaAI** — An AI-powered English language learning platform. Single-file web app with zero dependencies, no build step. Gamified student experience with CEFR level tracking.

## Quick Start

```bash
open index.html          # macOS
npx serve .              # or serve locally
```

## Architecture

### Single-File SPA (`index.html`)

The entire app lives in one `index.html` file with no build tooling, frameworks, or external JS.

**Screen System:** 7 screens toggled via `.active` CSS class:
1. `#screen-login` — Name + password + age group (Kids/Tweens/Teens)
2. `#screen-welcome` — Goal picker + assessment intro (first-time users only)
3. `#screen-assessment` — 5-sentence CEFR-graded speaking test (A1-C1)
4. `#screen-assess-result` — Celebration + CEFR level assignment
5. `#screen-dashboard` — XP/CEFR progress, Word of Day, Lesson, stats
6. `#screen-practice` — Listen & Repeat flow with Sarvam STT/TTS
7. `#screen-feedback` — AI analysis with CEFR scoring + XP earned

**First-Time User Flow:** Login → Welcome (pick goal) → Assessment (5 sentences, listen+repeat) → Result (CEFR level + confetti) → Dashboard

**Returning Users:** Skipped via `localStorage` — go straight to Dashboard.

### Age-Based Theming
Three CSS theme classes (`kids-theme`, `tweens-theme`, `teens-theme`) applied to `<body>`. Each has distinct colors, typography, and personality.

### Key State Variables
- `AG` — Age group (`'kids'`, `'tweens'`, `'teens'`)
- `userName`, `userGoal`, `userXP`, `userLevel` (CEFR), `userStreak`, `userSessionCount`, `userBadges`
- `chosenLang` — 17 language options, mapped to Sarvam's 11 supported languages
- `history` — In-memory array of past analysis results
- `sarvamKey` — Single API key for both STT and TTS

### localStorage Persistence
Keys prefixed `lingua_`: `name`, `age`, `goal`, `xp`, `level`, `badges`, `sessions`, `streak`, `lastvisit`, `visited`

### CEFR Level System
- 6 levels: A1 (Beginner) → A2 → B1 → B2 → C1 → C2 (Proficient)
- XP thresholds: A1:0, A2:100, B1:300, B2:600, C1:1000, C2:1500
- Score mapping: 90%+=C1, 75%+=B2, 60%+=B1, 40%+=A2, <40%=A1
- Avatar emojis: 🌱🌿🌳🌲🌟🏆 per level
- `scoreToCEFR()`, `xpToLevel()`, `xpProgress()` utility functions

### Gamification
- **XP**: +50 base per practice, +25 at 80%+, +50 at 90%+, +200 assessment completion
- **Badges**: First Words ⭐, On Fire 🔥, Crystal Clear 💎, Dedicated 🏆, Level Up 🚀
- **Confetti**: CSS animation triggered on assessment result and level-ups
- **Streak tracking**: Based on `lastvisit` date comparison

### Speech Pipeline
1. **STT**: Sarvam AI (`saaras:v3`) via multipart POST to `api.sarvam.ai/speech-to-text`
2. **TTS**: Sarvam AI (`bulbul:v3`) via JSON POST to `api.sarvam.ai/text-to-speech`
3. **Fallbacks**: Web Speech API → simulation (STT); browser `speechSynthesis` (TTS)

### Natural Voice Configs
Per-age-group voice selection with naturalness controls:
```js
VOICE_CONFIG = {
  kids:   { speaker: 'priya',  pace: 0.78, temperature: 0.9 },
  tweens: { speaker: 'kavya',  pace: 0.88, temperature: 0.75 },
  teens:  { speaker: 'aditya', pace: 0.95, temperature: 0.65 }
}
```

### Assessment Sentences
5 CEFR-graded: A1 "The cat is on the mat" → A2 → B1 → B2 → C1 "Artificial intelligence is transforming industries worldwide"

### Content Database (`DB` object)
Age-group-specific data: themes, mascots, word of the day, lessons, practice sentences, baseline stats.

### Claude AI Integration
- `api.anthropic.com/v1/messages` with `claude-sonnet-4-20250514`
- 3 personalized tips per session, age-appropriate tone
- Falls back to `fallTips()` on failure

## Development Notes
- **No build/lint/test tooling** — zero-dependency prototype
- CSS ~1100 lines in `<style>` block; responsive at 700px breakpoint
- Keep single-file constraint — don't introduce build steps unless requested
- All Sarvam API calls are client-side; API key entered at runtime
