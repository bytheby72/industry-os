---
name: auto-learning-pipeline
description: 5-stage autonomous learning pipeline — discover → score → rank → ingest → learn patterns from codebases. Adapts multi-agent-ralph-loop curator pipeline to Hermes fabric storage.
version: 1.0.0
author: anatoli
metadata:
  hermes:
    tags: [learning, curator, patterns, fabric, auto-learn]
    dependencies: [fabric]
    compositions: []
---

# Auto-Learning Pipeline

5-stage pipeline that autonomously discovers, evaluates, and learns patterns from codebases — adapted from multi-agent-ralph-loop curator system to Hermes fabric storage.

## When to Use

- After completing a complex task — extract learnings automatically
- When user says "what did we learn", "extract patterns", "learn from this"
- Periodic: run after every 5+ complex sessions
- When onboarding a new project — learn its conventions

## Architecture

```
Stage 1: DISCOVER
  └─ Scan recent sessions via session_search()
  └─ Identify: decisions made, errors fixed, patterns applied

Stage 2: SCORE
  └─ Rate each candidate pattern by:
      - frequency (how often used)
      - outcome (was the fix successful)
      - generality (applies to other projects)

Stage 3: RANK
  └─ Select top N (default 5) patterns
  └─ Diversity filter: avoid repeating same domain

Stage 4: INGEST
  └─ Write to fabric: fabric_write(type='decision' or 'pattern')
  └─ Tag by domain: backend, frontend, devops, security, data
  └─ Link to source session

Stage 5: LEARN
  └─ Extract rules from fabric entries
  └─ Update skills with new pitfalls
  └─ Write learned patterns to .hermes/skills/*/SKILL.md
```

## Quick Start

```
# Manual trigger:
"Extract learnings from our last 3 sessions"

# Auto-trigger (in hooks):
# After SessionEnd — run learning pipeline
```

## Stage Details

### Stage 1: DISCOVER
```python
# Search recent sessions for patterns
session_search(query="fixed error pattern", sort="newest", limit=5)
session_search(query="decision architecture", sort="newest", limit=5)

# Identify:
# - Decisions: when user said "use X instead of Y"
# - Errors: when terminal returned non-zero, then fixed
# - Patterns: recurring code structures across sessions
```

### Stage 2: SCORE
Score each candidate 0-1 on:
- **frequency** (0-0.4): how many sessions show this pattern
- **outcome** (0-0.3): did the fix work on first try
- **generality** (0-0.3): does this apply beyond current project

### Stage 3: RANK
- Sort by score descending
- Apply diversity filter: max 2 per domain
- Select top 5

### Stage 4: INGEST
```python
fabric_write(
    type="pattern",
    content="Use atomic write pattern (temp file + mv) to avoid race conditions",
    summary="Atomic writes prevent data corruption in concurrent hooks",
    tags="devops, hooks, concurrency",
    training_value="high",
    verified="true",
    evidence="All 14 hooks in ralph-loop use mktemp + mv pattern"
)
```

### Stage 5: LEARN
- Update relevant skills with new pitfalls
- Add missing edge cases to existing skills
- Create new skills for novel patterns

## Integration with Quality Gates

After learning completes, run quality-gates on new/modified skills:
```
Stage 1 (CORRECTNESS): validate YAML frontmatter
Stage 2 (QUALITY): check required fields present
Stage 4 (CONSISTENCY): verify naming conventions
```

## Pitfalls

1. **Don't over-learn**: max 5 patterns per run to avoid noise
2. **Verify before ingesting**: check pattern exists in at least 2 sessions
3. **fabric_write needs accurate tags**: bad tags = hard to recall later
4. **Session data expires**: older sessions may have been compacted
5. **Not every session has learnings**: empty result is OK — skip silently

## Verification

After running pipeline:
- [ ] 3-5 new fabric entries created
- [ ] At least 1 skill updated with new pitfall
- [ ] All entries have training_value="high" and verified="true"
- [ ] No duplicate patterns ingested
