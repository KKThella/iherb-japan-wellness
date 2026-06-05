# Case Study: AI Wellness Concierge — New Market Entry

**Role:** Product Manager (portfolio project)
**Built for:** Product interview · kept open as a reference
**Live prototype:** [iherb-japan-wellness.netlify.app](https://iherb-japan-wellness.netlify.app)

---

## The Problem

When a global e-commerce platform expands into a new market, it faces a fundamental personalization gap: no local purchase history, no co-purchase signals, no behavioral data to power collaborative filtering.

The specific question I wanted to answer:

> *How do you deliver a meaningfully personalized product experience on day one in a new market, when your recommendation models have nothing to learn from yet?*

This is a version of the classic cold-start problem — but with an added constraint: the market itself is new, so even your global collaborative signals carry geographic bias.

---

## Constraints I Designed Around

1. **Zero local behavioral data at launch** — no co-purchase history, no clickstream, no prior orders
2. **US-catalog bias** — global collaborative signals reflect a US-dominant user base, which can surface irrelevant or culturally misaligned picks
3. **Trust gap in wellness** — supplement recommendations require more explanation than, say, a book recommendation; users won't follow picks they don't understand
4. **Speed of intake** — long onboarding flows kill conversion; the model needs to work with minimal input

---

## Recommendation Architecture

### Phase 1 — Content-Based (t = 0)

At launch, the only viable approach is content-based filtering: map user-declared health goals to product attributes directly.

**How it works:**
- User selects up to 3 goals (immunity, sleep, energy, etc.) and dietary restrictions
- Products are pre-tagged with goal alignment, dietary flags, and synergy labels
- Claude generates ranked recommendations using goal-attribute overlap + synergy scoring

**Tradeoff acknowledged:** Content-based filtering is good at the obvious picks. It misses the non-obvious ones — the products users didn't know to ask for but would have engaged with. That's where collaborative filtering earns its value.

### Phase 2 — Collaborative Signal Emerging

After a user's 2nd purchase, local co-purchase signals start to form. The model can begin surfacing picks based on what users with similar goal profiles actually bought — not just what their goals suggest.

**Risk managed:** Until the local catalog reaches critical mass (~6 months of data), collaborative signals carry US-market bias. The hybrid graduation logic controls how quickly collaborative weight increases.

### Phase 3 — Hybrid Target State

| User Type | Content Weight | Collaborative Weight |
|---|---|---|
| New user (< 3 purchases) | 60% | 40% |
| Returning user (≥ 3 purchases) | 30% | 70% |

Graduation is triggered by purchase count, not time — this avoids promoting the collaborative signal before it has enough local behavioral grounding.

---

## Metric Definition

The key question was: *how do I know the collaborative signal is actually working?*

I chose **position-3 CTR** as the signal metric.

**Why position-3:**
- Position-1 and position-2 recommendations are the "obvious" picks — high goal-attribute overlap, easy to explain
- Position-3 is where the collaborative model earns its keep — it surfaces something the user didn't explicitly ask for
- If users click position-3, they found non-obvious value; if they don't, the collaborative signal isn't ready

**Calibration rule:**
> *If collaborative CTR on position-3 exceeds 8%, increase collaborative weight by 10%.*

8% was set as the threshold based on typical browse-to-click rates for recommendation carousels in wellness — conservative enough to avoid overfitting to early noise.

---

## Algorithm Transparency as a UX Decision

The most deliberate design choice in the prototype is the collapsible algorithm rationale panel.

Most recommendation UIs hide the model. I surfaced it — not as a debug view, but as a trust feature.

**Reasoning:**
- In wellness, recommendations have consequences (what goes into your body). Users are more skeptical than in entertainment or retail.
- Showing *why* a product was picked ("matched your sleep goal + collagen synergy") reduces the "why should I trust this?" friction
- Showing *how the algorithm evolves* ("collaborative activates after your 2nd purchase") sets expectations and creates a reason to come back

**What I'd measure:** Whether users who expand the rationale panel have higher add-to-list rates than those who don't. If yes, the panel is load-bearing UX. If not, it's complexity that should be simplified.

---

## Bilingual Product Thinking

The EN+Japanese mode isn't a translation toggle — it's a signal about locale-aware AI product design.

When Japanese mode is on, the AI is instructed to:
- Output product reasoning in both languages
- Surface synergy explanations with Japanese terminology where natural
- Adapt formality level appropriate for Japanese consumer context

**PM lesson here:** Localization isn't just string translation. For an AI-generated product, the model's output persona, formality, and cultural framing need to be part of the prompt design.

---

## What I'd Do Next (If This Were a Real Product)

1. **A/B test intake length** — 3 questions vs. 5 questions vs. progressive disclosure. Hypothesis: 3 questions converts better at top of funnel; 5 questions produces higher add-to-cart on recommendations.
2. **Instrument the rationale panel** — measure expand rate, dwell time, and downstream add-to-list correlation.
3. **Build the graduation trigger properly** — current prototype simulates the logic; in production this would be an ML pipeline decision, not a hardcoded threshold.
4. **Cold-start for new products** — the same problem applies to new SKUs added to the Japan catalog. A separate content-based bootstrapping flow would be needed for product cold-start, not just user cold-start.

---

## Skills Demonstrated

| Skill | Where It Shows |
|---|---|
| Recommendation system design | Cold-start architecture, hybrid graduation logic |
| Metric definition | Position-3 CTR as collaborative signal proxy |
| AI product design | Prompt-driven bilingual output, transparency UX |
| Tradeoff articulation | Content vs. collaborative, US-bias risk, trust vs. friction |
| Prototype-to-production thinking | "What I'd do next" section above |

---

**Kiran Thella** · [linkedin.com/in/kiran-kumar-t-18a08321](https://linkedin.com/in/kiran-kumar-t-18a08321)
