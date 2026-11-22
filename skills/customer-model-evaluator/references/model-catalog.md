# Customer-understanding model catalog

Use this catalog to **select** models for a CME run. Prefer fit to KPI and data over covering every type. Typical set: ~4 models + 1 baseline, max 6 including baseline.

## Selection heuristic

1. Start from the primary KPI and channel.
2. Prefer models whose segments map to **actionable targeting** (who can be reached and treated differently).
3. Include at least one model **not** derived from the user's existing personas.
4. Include a **simple baseline** to challenge complexity.
5. Drop models that would produce identical tactics; keep tactical contrast.

## Catalog

### Existing-persona restructuring
- **Idea:** Reorganize provided personas into decision-useful types with explicit jobs/needs/barriers.
- **Good for:** Teams with research-heavy personas; communication and creative briefing.
- **Weak when:** Personas are aspirational or unlinked to reachable CRM fields.
- **Data need:** Persona docs / interviews (thin OK if labeled 仮説).

### Attribute / needs segments
- **Idea:** Segment by demographics, firmographics, needs, jobs-to-be-done, price sensitivity.
- **Good for:** Offer design, messaging, category entry.
- **Weak when:** Attributes poorly predict response; privacy or missing fields.
- **Data need:** Profile fields, survey, or hypothesized attribute map.

### Behavioral clusters / journey stages
- **Idea:** Group by observed or hypothesized behaviors: browse→cart, feature adoption, store visit cadence, content engagement.
- **Good for:** Trigger campaigns, lifecycle CRM, on-site personalization.
- **Weak when:** Behavior logs absent and proxies are vague.
- **Data need:** Events/transactions ideal; else journey hypotheses with 仮説 tags.

### RFM / value-recency dynamics
- **Idea:** Recency, frequency, monetary (or B2B analogs: recency of usage, seat expansion, contract value).
- **Good for:** Repeat purchase, win-back, VIP treatment, churn triage.
- **Weak when:** First-purchase acquisition is the only goal; cold audiences.
- **Data need:** Purchase/usage history; without it, define RFM rules as 仮説 only.

### Lifecycle / lifecycle-risk
- **Idea:** New, active, at-risk, dormant, won-back (or trial→paid→expand→renew).
- **Good for:** Retention, churn prevention, onboarding.
- **Data need:** Tenure + activity signals.

### Intent / propensity tiers (conceptual)
- **Idea:** High/med/low likelihood of conversion or churn—only score empirically when data exists.
- **Good for:** Budget allocation, sales prioritization.
- **In hypothesis mode:** Describe features that *would* enter a propensity model; do not invent accuracy.

### Context / catchment (retail)
- **Idea:** Distance, trade area, occasion, daypart, weather/occasion proxies.
- **Good for:** Store visit and local promotion.
- **Data need:** Location, visit logs, or catchment hypotheses.

### Firmographic + buying-committee (B2B)
- **Idea:** Company size/industry + roles (champion, economic buyer, user).
- **Good for:** Pipeline conversion, expansion, multi-thread outreach.
- **Weak when:** SMB self-serve with no sales motion.

### Baseline (required stress test)
Examples:
- Treat everyone the same
- Target only recent buyers / openers / high AOV
- Target only the largest segment by headcount

If a sophisticated model cannot beat baseline on expected 施策価値 or learning value, say so.

## Domain nudges (not hard rules)

| Domain / KPI | Lean toward |
|--------------|-------------|
| D2C·EC purchase rate | Attribute/needs + behavior (+ baseline); persona for creative |
| D2C·EC repeat | RFM / lifecycle + behavior |
| B2B opportunity rate | Firmographic/committee + intent/lifecycle |
| B2B churn | Usage lifecycle + value tiers + baseline |
| Retail visits | Catchment/occasion + RFM-like visit cadence |
| Retail AOV | Basket behavior + value tiers |

## Anti-patterns

- Six near-duplicate persona variants
- Models that cannot change targeting or creative
- "AI cluster" names with no operational definition
- Declaring a model winner solely because it sounds sophisticated
