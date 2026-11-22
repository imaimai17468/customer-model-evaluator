# Customer Model Evaluator (CME)

マーケティング施策に対して、どの顧客理解モデルが有効かを比較・評価し、施策と検証計画を提案する Claude Agent Skill。

## Layout

```
skills/customer-model-evaluator/
├── SKILL.md
├── references/
│   ├── model-catalog.md
│   ├── evaluation-framework.md
│   └── artifact-spec.md
└── evals/
```

## MVP scope

- **Hypothesis evaluation mode:** personas / product context → multiple models, tactics, validation plan
- **Primary output:** interactive HTML Artifact dashboard (Japanese UI by default)
- Empirical scoring (CSV/CRM, A/B lift) is Phase 02–03

## Install

Point Claude at `skills/customer-model-evaluator/` (Claude Code project skill, or upload packaged `.skill`).
