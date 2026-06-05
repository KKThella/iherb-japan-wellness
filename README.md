# AI Wellness Concierge — New Market Entry Prototype

> A functional PM portfolio project exploring how to personalize supplement recommendations in a new market with zero purchase history.

🔗 **Live Demo:** [iherb-japan-wellness.netlify.app](https://iherb-japan-wellness.netlify.app) · 📄 [Case Study](CASE_STUDY.md)

---

## The Problem This Solves

When an e-commerce platform enters a new market, it has no local behavioral data — no co-purchase signals, no purchase history, no collaborative filtering baseline. How do you still deliver personalized recommendations on day one?

This prototype explores that cold-start problem end-to-end: user intake → AI-powered recommendations → algorithm transparency → a graduation path from content-based to collaborative filtering as data accumulates.

Built for a real product interview. Kept open as a reference for how I think about recommendation systems, market-entry product strategy, and AI-native UX.

---

## Features

| Feature | Description |
|---|---|
| **3-Question Intake** | Users select up to 3 health goals (immunity, sleep, energy, etc.) and dietary restrictions — under 60 seconds |
| **AI Recommendations** | Claude (Haiku) returns 3 ranked products with match scores, per-product reasoning, and stack synergy explanation |
| **Bilingual Mode** | Toggle EN-only vs. EN+Japanese — AI responses adapt language accordingly |
| **Algorithm Transparency** | Collapsible rationale panel explains which strategy is active (content-based / collaborative / hybrid) and why |
| **Conversational Chat** | Multi-turn "Ask Anything" flow for natural recommendation refinement |
| **Profile Mode** | Extended intake (age, notes, strategy selector) → 4-card grid with confidence scores |
| **My List** | Floating cart panel with one-click bulk add |

---

## PM Design Decisions

See the full [Case Study](CASE_STUDY.md) for the complete reasoning. Short version:

### Cold-Start Architecture

| Phase | Strategy | Trigger |
|---|---|---|
| **t = 0** | Content-based: goal → product attribute mapping | Always (new user) |
| **Early signal** | Collaborative: local co-purchase patterns | After 2nd purchase |
| **Target state** | Hybrid 60/40 → 30/70 (content/collab) | Purchase count ≥ 3 |

### Key Metric
> *"If collaborative CTR on position-3 recommendation exceeds 8%, increase collaborative weight by 10%."*

Position-3 CTR isolates the collaborative signal — it's the pick least explained by obvious goal-matching. If users click it, the model is finding non-obvious value.

### Algorithm Transparency as a Trust Feature
The rationale panel is intentional UX, not a debug view. In wellness, users follow recommendations they understand. Surfacing *why* a product was chosen — and how the algorithm evolves with their behavior — builds trust and sets accurate expectations.

---

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | React 18 (CDN), Tailwind CSS v3, Babel |
| AI | Anthropic Claude Haiku 4.5 |
| API Proxy | Netlify serverless function (API key stays server-side) |
| Fonts | Zen Maru Gothic, Nunito |
| Hosting | Netlify (static + functions) |

Single `index.html` + one serverless function. No build step — intentionally minimal for a portfolio prototype.

---

## Running Locally

```bash
git clone https://github.com/KKThella/iherb-japan-wellness.git
cd iherb-japan-wellness

# Option 1 — Mock mode (no API key needed)
open index.html

# Option 2 — Live AI via Netlify CLI
npm install -g netlify-cli
# Set ANTHROPIC_API_KEY in .env or Netlify dashboard
netlify dev
```

The app falls back to mock recommendations without a key — full UX explorable without credentials.

---

## What This Demonstrates

- **Recommendation system design** — cold-start problem, content/collaborative tradeoffs, hybrid graduation logic
- **Metric definition** — using position-3 CTR as the signal for collaborative model readiness
- **Algorithm transparency as UX** — model reasoning surfaced as a trust feature, not an afterthought
- **Locale-aware AI product thinking** — bilingual output, Japan-specific catalog, cultural framing

---

## Author

**Kiran Thella** — Lead PM at Nike · [linkedin.com/in/kiran-kumar-t-18a08321](https://linkedin.com/in/kiran-kumar-t-18a08321)
