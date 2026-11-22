---
name: customer-model-evaluator
description: >
  Compare and evaluate customer-understanding models (personas, attribute segments,
  behavioral clusters, RFM, baselines) against marketing tactics, then recommend
  which model is most useful for which intervention. Use whenever the user mentions
  customer models, personas, segments, RFM, clustering for marketing, comparing
  targeting approaches, marketing hypothesis evaluation, campaign targeting design,
  churn/retention modeling for CRM, or asks which customer understanding method
  is best for a promotion, purchase-rate lift, repeat purchase, lead conversion,
  store visit, or AOV goal—even if they only paste personas or product briefs and
  do not say "CME" or "Customer Model Evaluator". Prefer this skill over generic
  persona writing; the goal is model×tactic evaluation and decision support, not
  prettier personas alone.
---

# Customer Model Evaluator (CME)

Evaluate **which customer-understanding model is useful for which marketing tactic**, then help the marketer decide what to run and what to validate next.

Do not optimize for "the best persona." Optimize for better marketing decisions under uncertainty.

## Role split

| Layer | Responsibility |
|-------|----------------|
| This skill (analysis) | Clarify goals, select/build models, derive tactics, score evidence, recommend, plan validation |
| Interactive Artifact (UI) | Visualize comparisons, let users explore models/tactics, show uncertainty, export |

Always produce an interactive Artifact as the primary deliverable. Keep chat commentary short: mode used, key recommendation, and where the Artifact lives.

## Language

- Skill instructions: English (this file).
- User-facing Artifact / exports: **Japanese by default**.
- If the user specifies another language, use that for all user-facing content.

## Modes

Detect from available inputs. State the mode explicitly in the Artifact.

| Mode | When | What you may claim |
|------|------|--------------------|
| **Hypothesis evaluation** | Personas, surveys, product briefs, past campaign anecdotes—but no reliable purchase/behavior datasets or experiment results | Compare model×tactic hypotheses; produce validation plans. Mark predictive power and intervention value as **未検証** unless grounded in cited results |
| **Empirical evaluation** | Transaction, CRM, event, or experiment data is present and usable | Quantify predictive metrics and/or lift where the data supports it; still separate explanation vs prediction vs intervention value |

Never present LLM guesses as measured predictive accuracy or measured incremental lift. If a cell cannot be evidenced, label it **未検証** (or the user's language equivalent).

## Design principles

1. Do not privilege personas, RFM, or any single method a priori.
2. Compare **models and the tactics they imply**, not models in isolation.
3. Keep **説明力 / 予測力 / 施策価値** separate. Intervention value is the decision criterion when tradeoffs appear.
4. Label every material claim as one of: **観測事実 / 文献・調査 / 仮説 / 推測**.
5. Prefer a simpler model unless complexity clearly buys decision or validation value.
6. Always end with: recommended model(s), recommended tactic(s), rationale, and the next experiment.

## Workflow

Follow these steps. Skip nothing material; compress prose, not rigor.

### 1. Frame the marketing problem

Extract or infer:

- Business objective and primary KPI (e.g. purchase rate, repeat rate, opportunity rate, churn, store visits, AOV)
- Offer / product context
- Channel constraints and budget/time if given
- Audience already in mind (if any)
- Available evidence types (personas, survey, CRM fields, experiments)

Write a short **評価条件** block (goal, KPI, constraints, mode, evidence inventory).

If inputs are thin: proceed with explicit **仮説** rather than blocking on questions. Ask at most 1–2 optional clarifying questions after delivering the Artifact.

### 2. Select and build customer-understanding models

Target **about 4 models**, **at most 6**. Choose adaptively from the catalog in `references/model-catalog.md` based on goal + data.

Always include when feasible:

1. **Existing-persona-derived model** (restructure provided personas; do not invent them as "validated")
2. **Needs / attribute segment model**
3. **Behavior-oriented model** (cluster or journey stages—even if behavior must be hypothesized from thin data, label as 仮説)
4. **Challenge-fit dynamic model** (RFM, lifecycle, intent, firmographic stages, store catchment—pick what fits)

Also include a **simple baseline** (e.g. "all customers / recent buyers / high spenders only") so sophistication is stress-tested.

For each model document: definition, segments/types, data requirements, strengths, failure modes, evidence labels.

Read `references/model-catalog.md` when choosing variants.

### 3. Derive tactics per model

For each model, propose 1–3 concrete tactics answering: **who / what offer or message / which channel / what success metric**.

Tactics must **differ meaningfully** across models. If two models imply the same campaign, either merge models or sharpen the tactical contrast—otherwise comparison is theater.

### 4. Evaluate on three axes

Use the rubric in `references/evaluation-framework.md`.

| Axis | Question | Hypothesis mode | Empirical mode |
|------|----------|-----------------|----------------|
| 説明力 | Does the model organize customer reality usefully? | Qualitative fit to known facts/survey; confidence + evidence tags | Same + quantitative association if data allows |
| 予測力 | Does it predict future purchase/churn/visit/etc.? | **未検証** unless holdout-quality evidence exists | Holdout / time-split metrics vs baseline |
| 施策価値 | Does using it improve marketing outcomes? | Plausible mechanism + what experiment would prove lift; score only as hypothesized priority | A/B or quasi-experiment incremental effect |

Score with transparent ordinal levels (e.g. 高 / 中 / 低 / 未検証) and short rationale per cell. Prefer matrices of **model × tactic**, not model-only averages.

### 5. Recommend and plan validation

Output:

- Primary recommended **model × tactic** pair (and a runner-up)
- Why intervention value (or expected value under uncertainty) wins
- What to stop doing or de-prioritize
- A **検証計画**: hypothesis, metric, design (A/B, holdout, geo, etc.), sample/time needs, kill criteria, what would falsify the model

### 6. Build the interactive Artifact

Primary deliverable: a single self-contained HTML Artifact (or environment-equivalent interactive canvas) per `references/artifact-spec.md`.

Embed the analysis JSON (or equivalent structured data) in the page so the UI can filter, compare, and export without another model call.

Also offer Markdown/CSV export from the Artifact when practical; if the runtime cannot download files, provide copyable Markdown and CSV text panels.

## Evidence discipline

Prefix or tag claims:

- **観測事実** — from user data or clearly stated results
- **文献・調査** — cited market research or provided studies
- **仮説** — structured assumption used for comparison
- **推測** — weak inference; never treat as measured performance

Forbidden in hypothesis mode: fabricated AUC/accuracy/lift percentages presented as results.

## Thin-input behavior

Proceed. Mark gaps in an **データギャップ** section. Downgrade predictive power and intervention value to **未検証**. Expand the validation plan instead of inventing precision.

## Chat response shape

After the Artifact:

1. Mode + language
2. One-sentence recommendation (model × tactic × KPI)
3. Top risk / uncertainty
4. Optional clarifying questions (non-blocking)

Do not dump the full report into chat unless the user asks or Artifacts are unavailable—in that case, fall back to a structured Markdown report using the same sections as the Artifact.

## References

- `references/model-catalog.md` — model types and when to select them
- `references/evaluation-framework.md` — scoring rules and anti-patterns
- `references/artifact-spec.md` — dashboard IA, interactions, data schema, export
