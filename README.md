# LinguaAI — Digital Language Lab

> An AI-powered English language learning platform demo. Built for investor demos and rapid prototyping.

🔗 **Live Demo:** _(add your GitHub Pages URL here after deployment)_

---

## What It Does

A fully functional, single-file web app with 4 screens:

| Screen | Description |
|--------|-------------|
| 🔐 Login | Simple login (any credentials work) |
| 📊 Dashboard | Fluency score, pronunciation score, recent activity, suggested exercises |
| 🔊🎙️ Practice | **Listen** to a sentence, then **repeat** it — get a transcript & AI feedback |
| 📝 Feedback | AI-powered word-by-word analysis, scores, and improvement tips |

---

## Features

- **Listen & Repeat** — Sarvam AI generates natural-sounding audio in 11 Indian languages; learner hears the sentence first, then speaks it back
- **Real microphone recording** via Sarvam STT API (Saaras v3 model)
- **Levenshtein distance** word matching for pronunciation analysis
- **Filler word detection** (um, uh, like...) for fluency scoring
- **Claude AI** generates personalized improvement suggestions
- **Graceful fallbacks** — browser SpeechRecognition + simulation if no API key; browser speechSynthesis if Sarvam TTS unavailable
- **Live dashboard** updates after every practice session
- **Zero dependencies** — single HTML file, no build step

---

## Quick Start (Local)

```bash
# Option 1: Just open the file
open index.html   # macOS
start index.html  # Windows

# Option 2: Serve locally
npx serve .
# or
python3 -m http.server 8080
# then open http://localhost:8080
```

---

## API Keys

### Sarvam AI (Speech-to-Text + Text-to-Speech)
Get a free API key at [console.sarvam.ai](https://console.sarvam.ai) (no credit card needed).
Paste it in the app on the Practice screen — it powers both voice playback and transcription.

Without a key, the app falls back to:
- **STT:** browser SpeechRecognition → simulation
- **TTS:** browser speechSynthesis

### Claude AI (Feedback Tips)
No key needed — the app calls the Claude API directly. Tips fall back to rule-based suggestions if the call fails.

---

## Deploy to GitHub Pages (Free Hosting)

1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Set Source to `main` branch, `/ (root)`
4. Click **Save**
5. Your live URL: `https://YOUR_USERNAME.github.io/lingua-ai-demo`

---

## Deploy to Netlify (Instant, No Signup Needed)

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag `index.html` onto the page
3. Get a live URL instantly ⚡

---

## Tech Stack

- **Frontend:** Vanilla HTML/CSS/JavaScript (zero frameworks)
- **Speech-to-Text:** Sarvam AI (`saaras:v3`) with browser fallback
- **Text-to-Speech:** Sarvam AI (`bulbul:v3`) with browser speechSynthesis fallback
- **AI Feedback:** Claude API (`claude-sonnet-4-20250514`)
- **Evaluation Logic:** Rule-based (Levenshtein distance + filler detection)
- **Fonts:** Nunito + Fredoka One + Sora (Google Fonts)

---

## Demo Sentences

| Level | Sentence |
|-------|----------|
| Beginner | She enjoys reading books in the library every weekend. |
| Intermediate | The weather forecast suggests it might rain tomorrow afternoon. |
| Beginner | I would like to make a reservation for two people, please. |
| Advanced | The government announced new environmental regulations last Tuesday. |
| Intermediate | Despite the challenges, she remained optimistic about the future. |

---

## Scoring Logic

```
Accuracy  = matched words / total expected words × 100
Fluency   = penalizes filler words + slow pace
Pron      = accuracy × 0.7 + (100 - missed × 12) × 0.3
Overall   = fluency × 0.4 + pronunciation × 0.35 + accuracy × 0.25
```

---

## Folder Structure

```
lingua-ai-demo/
├── index.html     ← entire app (single file)
└── README.md
```

---

## License

MIT — free to use, modify, and deploy.
