# Hermes DevOps Skills

🔥 **4 autonomous agent skills for DevOps pipelines** — quality gates, ratchet loops, learning pipelines, and dependency graphs.

Built for [Hermes Agent](https://hermes-agent.nousresearch.com/) by adapting battle-tested patterns from multi-agent-ralph-loop and kayba-ai/recursive-improve.

## Architecture

```
quality-gates ──→ sprint-contract, hermes-harness-bundle
     ↑
ratchet-loop ──→ auto-learning-pipeline
     │                 │
     │                 └──→ fabric
     │
skill-dependency-graph ← (analyzes all)
```

## Skills

| Skill | What It Does |
|-------|-------------|
| **quality-gates** | 4-stage validation (correctness → quality → security → consistency) + adversarial review for complex tasks |
| **ratchet-loop** | Autonomous improve → run → eval → keep/revert — agent self-improvement |
| **auto-learning-pipeline** | 5-stage curator: discover → score → rank → ingest → learn patterns |
| **skill-dependency-graph** | Visualize dependencies, detect circular refs, suggest compositions |

## Quick Start

```bash
# Install in Hermes
cp -r skills/* ~/.hermes/skills/devops/

# Or clone the repo
git clone https://github.com/bytheby72/hermes-devops-skills.git
cp -r hermes-devops-skills/skills/* ~/.hermes/skills/devops/
```

## Dependency Graph

```
ratchet-loop
├── quality-gates (for evaluation)
└── auto-learning-pipeline (for learning)

quality-gates
├── sprint-contract (planning gate)
└── hermes-harness-bundle (progress tracker)

auto-learning-pipeline
└── fabric (memory storage)

skill-dependency-graph
└── (leaf — analyzes all of the above)
```

## Gate Flow

```
CODE CHANGE
    │
    ▼
┌──────────────┐  BLOCKING
│ CORRECTNESS  │──▶ YAML valid? Required fields?
└──────┬───────┘
       ▼
┌──────────────┐  BLOCKING
│   QUALITY    │──▶ Body > 500 chars? Sections complete?
└──────┬───────┘
       ▼
┌──────────────┐  AUTO-FIX
│  SECURITY    │──▶ Secrets leaked? Auto-redact.
└──────┬───────┘
       ▼
┌──────────────┐  ADVISORY
│ CONSISTENCY  │──▶ Naming conventions? Tags present?
└──────┬───────┘
       ▼
     PASS / FAIL
```

## Recovery

If skills break during execution:
1. Check fabric for error context: `fabric_recall(query="error")`
2. Re-run quality gates on the target skill
3. Check ratchet-loop logs for revert history

## License

MIT
