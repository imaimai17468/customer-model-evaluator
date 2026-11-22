# Evaluation framework

Score **model × tactic** pairs. A model with no distinctive tactic is incomplete.

## Axes

### 説明力 (explanatory power)
Does the model organize customers in a way that matches known reality and aids briefing?

Look for: consistency with stated facts/survey; clarity of segment boundaries; usefulness for creative/sales conversation.

Not the same as predicting outcomes or improving lift.

### 予測力 (predictive power)
Does membership or score forecast the KPI (purchase, churn, visit, etc.)?

- **Hypothesis mode:** default **未検証**. Mentions of "likely accurate" without holdout evidence are invalid.
- **Empirical mode:** report metric, split method, baseline comparison, sample caveats.

### 施策価値 (intervention value)
If we act on this model, do outcomes improve vs baseline targeting?

This is the primary decision axis when axes conflict.

- **Hypothesis mode:** rate **expected** value as 高/中/低 with mechanism + required proof; keep status **仮説・未検証**.
- **Empirical mode:** incremental lift, incremental profit, or credible quasi-experimental estimate.

## Ordinal scores

Use: `高` | `中` | `低` | `未検証`

Optional confidence: `確信度: 高/中/低` based on evidence quality, not on rhetorical confidence.

## Evidence tags (required on rationales)

Every non-trivial rationale cites at least one:

| Tag | Meaning |
|-----|---------|
| 観測事実 | User-provided data or stated results |
| 文献・調査 | Cited research or attached studies |
| 仮説 | Working assumption for comparison |
| 推測 | Weak inference |

## Comparison rules

1. Prefer higher 施策価値 when 説明力 is similar.
2. High 説明力 + low expected 施策価値 → good for research/comms, weak as targeting system.
3. High hypothesized 施策価値 + 未検証 予測力 → recommend a **cheap test**, not full rollout.
4. Complex model must beat baseline on expected 施策価値 or learning speed; else recommend baseline (+ better measurement).
5. Do not average axes into one vanity score that hides 未検証 cells.

## Validation plan quality bar

Each recommended pair needs:

- Falsifiable hypothesis
- Primary metric + guardrails
- Design (A/B, holdout, switchback, geo, difference-in-differences—pick feasible)
- Rough sample or runtime needs (order-of-magnitude OK; label 仮説)
- Kill / scale criteria
- What result would demote the model

## Anti-patterns

- Invented precision ("lift +12.4%") in hypothesis mode
- Declaring a persona "correct" because it is vivid
- Conflating segment interpretability with campaign ROI
- One tactic copy-pasted under every model
- Ignoring baseline
