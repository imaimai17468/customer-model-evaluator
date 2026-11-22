# Interactive Artifact specification

Primary user deliverable: one self-contained interactive HTML dashboard (Claude Artifact / local HTML file). Analysis happens in the skill; the Artifact is for exploration and decision support.

## Goals

Users should be able to:

1. Compare customer-understanding models side by side
2. Inspect tactics implied by each model
3. See 説明力 / 予測力 / 施策価値 with evidence and uncertainty
4. Filter or highlight by model, tactic, or axis
5. Adjust simple assumptions (e.g. priority weights, target focus) and see recommendation shift at a **simulation** level—not fake precision
6. Read recommendation + validation plan
7. Export Markdown and CSV (download or copy panels)

## Technical constraints

- Single HTML file when possible: inline CSS + JS (vanilla JS or a single CDN React/Vue build is fine if the host allows it)
- Japanese UI copy by default
- Works in a narrow Artifact iframe: responsive layout, no broken horizontal scroll on mobile
- No backend. All data embedded as JSON in a `<script type="application/json" id="cme-data">...</script>` block
- Do not call external analytics. CDN UI libraries are OK if needed; prefer zero-dependency if feasible

## Information architecture

Suggested tabs or sections (names can vary; keep the jobs):

1. **概要** — mode, KPI, evidence inventory, one-line recommendation
2. **モデル比較** — cards/table of models (definition, segments, data needs, risks)
3. **施策マトリクス** — model × tactic list with who/what/channel/metric
4. **評価** — heatmap or table for three axes; click cell → rationale + evidence tags
5. **シミュレーション** — lightweight controls (e.g. weight on 施策価値 vs 説明力; focus KPI; exclude a model). Recompute a **recommended pair** from embedded scores only—no invented metrics
6. **推奨と検証計画** — primary + runner-up, stop-doing, experiment design
7. **エクスポート** — Markdown report + CSV of models/tactics/scores

Show a persistent banner: `モード: 仮説評価` or `モード: 実証評価`, and a legend for 観測事実 / 文献・調査 / 仮説 / 推測 / 未検証.

## Visual design (practical, not decorative)

- Clear hierarchy; one primary recommendation callout
- Heatmap colors must remain readable; include text labels, not color-only encoding
- Avoid dashboard clutter: no vanity KPI tiles disconnected from evidence
- Mark 未検証 cells distinctly (e.g. dashed border / muted) so they never look like measured wins

## Simulation rules

Allowed:

- Re-rank by weighting axes
- Filter models/tactics
- Toggle "require actionable CRM fields"
- Show sensitivity: which runner-up wins if 施策価値 weight drops

Forbidden:

- Generating new lift % or accuracy from sliders without empirical data
- Presenting simulated ranks as measured ROI

Label simulation outputs: `シミュレーション（仮説スコアの再重み付け）`.

## Embedded data schema (minimum)

```json
{
  "meta": {
    "title": "string",
    "mode": "hypothesis" | "empirical",
    "language": "ja",
    "kpi": "string",
    "domain": "string",
    "generatedAt": "ISO-8601",
    "evidenceInventory": ["string"]
  },
  "problem": {
    "objective": "string",
    "constraints": ["string"],
    "dataGaps": ["string"]
  },
  "models": [
    {
      "id": "m1",
      "name": "string",
      "family": "persona|attribute|behavior|rfm|lifecycle|baseline|other",
      "summary": "string",
      "segments": [{"name": "string", "definition": "string"}],
      "dataRequirements": ["string"],
      "strengths": ["string"],
      "failureModes": ["string"],
      "evidenceTags": ["観測事実", "仮説"]
    }
  ],
  "tactics": [
    {
      "id": "t1",
      "modelId": "m1",
      "name": "string",
      "who": "string",
      "what": "string",
      "channel": "string",
      "successMetric": "string",
      "evidenceTags": ["仮説"]
    }
  ],
  "evaluations": [
    {
      "tacticId": "t1",
      "explanatory": {"score": "高|中|低|未検証", "rationale": "string", "evidenceTags": ["仮説"], "confidence": "低|中|高"},
      "predictive": {"score": "高|中|低|未検証", "rationale": "string", "evidenceTags": ["仮説"], "confidence": "低|中|高"},
      "intervention": {"score": "高|中|低|未検証", "rationale": "string", "evidenceTags": ["仮説"], "confidence": "低|中|高"}
    }
  ],
  "recommendation": {
    "primary": {"modelId": "m1", "tacticId": "t1", "rationale": "string"},
    "runnerUp": {"modelId": "m2", "tacticId": "t2", "rationale": "string"},
    "deprioritize": ["string"],
    "validationPlan": {
      "hypothesis": "string",
      "metric": "string",
      "design": "string",
      "sampleOrDuration": "string",
      "killCriteria": "string",
      "falsification": "string"
    }
  }
}
```

Extend fields if useful; keep IDs stable for UI binding.

## Export contents

**Markdown:** problem framing, model list, tactics, evaluation table, recommendation, validation plan, evidence legend.

**CSV:** at least `model,tactic,explanatory,predictive,intervention,evidence_notes`.

## Host fallbacks

| Host | Behavior |
|------|----------|
| Claude.ai Artifact | Render interactive Artifact; brief chat summary |
| Claude Code / Cursor | Write `cme-dashboard.html` (and optional `.md` / `.csv`) to the working directory; tell the user the path |
| Artifact unavailable | Full Markdown report in chat using the same section order |

## Quality checklist before delivery

- [ ] Mode banner correct
- [ ] ≥1 baseline model
- [ ] Tactics differ across models
- [ ] 予測力 is 未検証 in hypothesis mode unless real evidence exists
- [ ] No fabricated lift/accuracy numbers
- [ ] Evidence tags visible on detail panes
- [ ] Recommendation + validation plan complete
- [ ] Export path works or copy panels exist
- [ ] UI language matches user preference (default JA)
