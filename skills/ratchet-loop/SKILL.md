---
name: ratchet-loop
description: Autonomous improvement loop — improve → run → eval → keep/revert. Adapts kayba-ai/recursive-improve ratchet engine to Hermes delegate_task + quality-gates.
version: 1.0.0
author: anatoli
metadata:
  hermes:
    tags: [ratchet, improvement, autonomous, evolution, loop]
    dependencies: [quality-gates, auto-learning-pipeline]
    compositions: []
---

# Ratchet Loop

Autonomous improvement loop adapted from kayba-ai/recursive-improve. Each iteration: improve the target → run it → evaluate → keep (if better) or revert (if worse). Runs until plateau or max iterations.

## When to Use

- "improve my agent overnight"
- "run the ratchet loop on X"
- "autonomous improvement cycle"
- After deploying a skill — let it self-improve

## Architecture

```
┌────────────────────────────────────────┐
│           RATCHET LOOP                  │
│                                         │
│  ┌──────────┐    ┌──────────┐          │
│  │ IMPROVE  │───▶│   RUN    │          │
│  │ delegate │    │ terminal │          │
│  └──────────┘    └──────────┘          │
│       │               │                │
│       ▼               ▼                │
│  ┌──────────┐    ┌──────────┐          │
│  │  REVERT  │◀───│   EVAL   │          │
│  │ git reset│    │quality-  │          │
│  │  --hard  │    │ gates    │          │
│  └──────────┘    └──────────┘          │
│       │               │                │
│       │    ┌──────────┐                │
│       └───▶│   KEEP   │                │
│            │git commit│                │
│            └──────────┘                │
│                                         │
│  Stopping conditions:                   │
│  - max_iterations reached               │
│  - plateau (N iterations no improvement)│
│  - max_duration exceeded                │
└────────────────────────────────────────┘
```

## Quick Start

```
# 1. Configure target
"Set up ratchet loop to improve skill X"

# 2. Run
"Start ratchet loop — 10 iterations max, stop after 3 plateau"

# 3. Check results
"Show ratchet log"
```

## Configuration

Before starting, define in `program.md`:

```markdown
## Objective
Improve error recovery in the price-monitor skill

## Target
Skill: price-monitor (path: ~/.hermes/skills/data-science/price-monitor/)

## Metrics
- success_rate: maximize (weight: 1.0)
- error_rate: minimize (weight: 0.5)
- latency_ms: minimize (weight: 0.3)

## Stopping Conditions
- max_iterations: 10
- max_duration_hours: 2
- plateau_patience: 3

## Time Budget
- minutes_per_iteration: 5
```

## Iteration Steps

### 1. IMPROVE
```python
delegate_task(
    goal="Improve skill X to reduce error_rate and increase success_rate",
    context="Current metrics: success_rate=0.7, error_rate=0.15. Target: success_rate>0.85",
    toolsets=["terminal", "file", "web"]
)
```

### 2. RUN
```python
terminal("python3 -m pytest tests/test_skill_X.py -v")
```

### 3. EVAL
```
Run quality-gates skill on the changed file
Compute composite score from metrics
```

### 4. DECIDE
```python
if new_score > best_score:
    # KEEP: commit changes
    terminal("git add -A && git commit -m 'ratchet: iteration N, score X → Y'")
    best_score = new_score
else:
    # REVERT
    terminal("git checkout -- .")
```

### 5. LOG
Append to `ratchet_log.jsonl`:
```json
{"iteration": N, "score": 0.85, "decision": "keep", "delta": +0.12}
```

## Evolution Mode (N > 1)

For parallel improvement (multiple islands):

```python
# Create N parallel worktrees, each with different improvement strategy
for i in range(n_islands):
    delegate_task(goal=f"Improve skill X using strategy {strategies[i]}")

# After all complete: pick best, merge, repeat
```

## Pitfalls

1. **Don't skip baseline**: always measure before first improvement
2. **Revert properly**: `git checkout -- .` not `git reset` (preserves history)
3. **Plateau detection**: track rolling window of last 3 scores, not absolute
4. **Time budget**: each iteration has a hard timeout — respect it
5. **Isolated worktrees**: evolution mode uses git worktrees to avoid conflicts

## Verification

After ratchet loop completes:
- [ ] `ratchet_log.jsonl` has N entries
- [ ] `ratchet_summary.md` shows score progression
- [ ] Best score > baseline score
- [ ] No uncommitted changes remain
