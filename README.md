# 🍳 YouTube Recipe Agent

Transform YouTube cooking videos into clean, Obsidian-ready markdown recipes.

**Live Demo:** [https://[username].github.io/youtube-recipe-agent/](https://[username].github.io/youtube-recipe-agent/)

---

## What It Does

Paste a YouTube cooking video URL, and the agent will:

1. Automatically fetch the video transcript
2. Extract the recipe using Claude AI
3. Format it as clean markdown for Obsidian
4. Optionally translate it to your preferred language

No more pausing videos, scribbling notes, or copy-pasting transcripts manually.

---

## Features

- **Automatic transcript fetching** — Just paste the YouTube URL
- **Manual mode** — Paste your own transcript if auto-fetch doesn't work
- **9 languages** — English, Spanish, Portuguese, French, German, Japanese, Chinese, Arabic, Hindi
- **Metric conversions** — Converts lbs → grams, oz → grams, fl oz → ml, inches → cm
- **Obsidian-ready** — Output is formatted markdown you can paste directly into your vault

---

## How to Use

1. Go to the website
2. Enter the access password
3. Paste a YouTube cooking video URL
4. Select your preferred language
5. Click "Generate Recipe"
6. Copy the markdown output to Obsidian

That's it!

---

## Example Output

```markdown
# Homemade Pancakes
For 4 servings

## Ingredients
**Prep Time:** 10 minutes
**Cook Time:** 15 minutes

- 2 cups (240g) all-purpose flour
- 2 Tbsp sugar
- 2 tsp baking powder
- 1/2 tsp salt
- 1 3/4 cups (415ml) milk
- 2 eggs
- 1/4 cup (60g) melted butter

## Directions
1. In a large bowl, whisk together flour, sugar, baking powder, and salt.
2. In a separate bowl, combine milk, eggs, and melted butter.
3. Pour wet ingredients into dry ingredients and stir until just combined.
4. Heat a non-stick pan over medium heat.
5. Pour 1/4 cup batter per pancake. Cook until bubbles form, then flip.
6. Serve warm with maple syrup and fresh berries.

Personal Note: Don't overmix the batter — lumps are okay!
```

---

## Technical Details

### Architecture

```
┌─────────────────────┐
│   GitHub Pages      │  ← Static frontend (HTML/CSS/JS)
│   (Frontend)        │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  Cloudflare Worker  │  ← Handles auth, API calls, keeps secrets safe
│  (Backend Proxy)    │
└─────────┬───────────┘
          │
    ┌─────┴─────┐
    ▼           ▼
┌────────┐  ┌────────────┐
│RapidAPI│  │ Claude API │
│(YouTube│  │ (Recipe    │
│transcripts)│ generation)│
└────────┘  └────────────┘
```

### Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | Vanilla HTML/CSS/JavaScript |
| Hosting | GitHub Pages |
| Backend | Cloudflare Workers |
| Transcripts | RapidAPI (YouTube Captions) |
| AI | Anthropic Claude API |

### Security

- API keys stored as encrypted secrets in Cloudflare (never exposed to frontend)
- Password validation happens server-side
- Session-based authentication (password stored in sessionStorage, cleared on tab close)

---

## Self-Hosting Guide

Want to run your own instance? Here's how:

### Prerequisites

- GitHub account
- Cloudflare account (free tier works)
- Anthropic API key ([console.anthropic.com](https://console.anthropic.com))
- RapidAPI key ([rapidapi.com](https://rapidapi.com) — search "YouTube Transcript")

### Step 1: Deploy the Frontend

1. Fork this repository
2. Go to Settings → Pages
3. Set source to "Deploy from branch" → `main` → `/ (root)`
4. Your site will be live at `https://[username].github.io/youtube-recipe-agent/`

### Step 2: Create the Cloudflare Worker

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com)
2. Navigate to Workers & Pages → Create Application → Create Worker
3. Name it `recipe-agent-proxy`
4. Click Deploy, then Edit Code
5. Replace the code with the contents of `worker.js`
6. Save and Deploy

### Step 3: Add Secrets

In your Worker settings (Settings → Variables and Secrets), add:

| Name | Type | Value |
|------|------|-------|
| `ANTHROPIC_API_KEY` | Secret | Your Anthropic API key |
| `RAPIDAPI_KEY` | Secret | Your RapidAPI key |
| `ACCESS_PASSWORD` | Secret | Your chosen password |

### Step 4: Update Frontend Configuration

In `index.html`, update the `WORKER_URL` constant (around line 290):

```javascript
const WORKER_URL = 'https://recipe-agent-proxy.[your-username].workers.dev';
```

Commit and push. Done!

---

## Limitations

- **Videos without captions** won't work with auto-fetch (use manual mode instead)
- **Very long videos** (2+ hours) may get truncated
- **RapidAPI free tier** is limited to 500 requests/month
- **Translation quality** varies by language (Romance languages work best)

---

## Cost Estimates

| Service | Free Tier | Paid |
|---------|-----------|------|
| GitHub Pages | Unlimited | — |
| Cloudflare Workers | 100k requests/day | — |
| RapidAPI | 500 requests/month | ~$10/month for more |
| Anthropic Claude | — | ~$0.003-0.006 per recipe |

For personal/family use, expect less than $5/month in API costs.

---

## License

MIT — Do whatever you want with it.

---

## Acknowledgments

Built with [Claude](https://claude.ai) by Anthropic.
