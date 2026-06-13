---
name: quality-gates
description: 4-stage validation pipeline (correctness → quality → security → consistency) with auto-remediation and adversarial review for complex tasks. Adapts multi-agent-ralph-loop quality gates to Hermes tools.
version: 1.0.0
author: anatoli
metadata:
  hermes:
    tags: [quality, validation, auto-fix, adversarial, pipeline]
    dependencies: [sprint-contract, hermes-harness-bundle]
    compositions: []
---

# Quality Gates Skill

4-stage validation pipeline adapted from multi-agent-ralph-loop quality gates system. Ensures every implementation passes correctness, quality, security, and consistency checks before approval.

## When to Use

- After any code generation or file modification task
- Before declaring a task "done"
- When task complexity >= 5 (multi-file, multi-step)
- After user says "проверь", "qa", "quality", "validate"

## Architecture

```
Stage 1: CORRECTNESS (BLOCKING)
  ├─ Syntax errors → auto-fix or BLOCK
  ├─ Logic errors → BLOCK with explanation
  └─ Requirements compliance → BLOCK if mismatch

Stage 2: QUALITY (BLOCKING)
  ├─ Type errors → auto-fix or BLOCK
  ├─ Test coverage → warn if <80%
  └─ Edge cases → BLOCK if unhandled

Stage 3: SECURITY (AUTO-FIX, non-blocking)
  ├─ SAST scan (semgrep if available)
  ├─ Secret leaks (gitleaks patterns)
  ├─ Auto-fix for known patterns
  └─ Advisory for unfixable issues

Stage 4: CONSISTENCY (ADVISORY)
  ├─ Code style → warn
  ├─ Pattern adherence → warn
  └─ Documentation completeness → warn

If complexity >= 7: ADVERSARIAL VALIDATION
  └─ delegate_task to 2 independent reviewers → reconcile
```

## Quick Start

```
# In any Hermes session, after completing work:
"Run quality gates on my last change"

# Or explicitly:
"Validate file.py with quality gates"
```

## Gate Actions Per Stage

### Stage 1: CORRECTNESS
- **Check**: syntax, imports, basic logic
- **Auto-fix**: formatting, missing imports
- **Block if**: syntax errors, broken logic
- **Tool**: `terminal("python3 -m py_compile ...")` or `terminal("node --check ...")`

### Stage 2: QUALITY
- **Check**: types, test coverage, edge cases
- **Auto-fix**: type annotations
- **Block if**: type errors, untested critical paths
- **Tool**: `terminal("mypy ...")` or `terminal("tsc --noEmit")` if available

### Stage 3: SECURITY
- **Check**: semgrep scan, secret patterns
- **Auto-fix**: known Docker patterns (no-new-privileges, read_only), hardcoded secrets → env vars
- **Advisory**: unfixable patterns → suggest `/loop`
- **Tool**: `terminal("semgrep --config=auto ...")` if installed, else grep-based fallback

### Stage 4: CONSISTENCY
- **Check**: style, naming, docs
- **Never blocks** — advisory only
- **Tool**: grep for patterns, manual review

## Adversarial Validation (complexity >= 7)

```python
# Pseudo-flow:
if task_complexity >= 7:
    reviewer_a = delegate_task(goal="Review for correctness and security")
    reviewer_b = delegate_task(goal="Review for quality and consistency")
    # Reconcile: merge agreements, flag disagreements for human
```

## Pitfalls

1. **Don't block on style in Stage 4** — consistency is advisory only
2. **semgrep may not be installed** — fall back to grep-based patterns
3. **Adversarial validation is expensive** — only for complexity >= 7
4. **Auto-fix must create backup** — always `cp file file.backup` before fixing
5. **Race conditions**: use atomic write pattern (write to temp → mv)

## Verification

After running quality gates, check:
- [ ] No blocking errors in Stage 1-2
- [ ] Security issues either auto-fixed or documented
- [ ] Consistency warnings reviewed (not necessarily fixed)
- [ ] If adversarial: both reviewers returned, disagreements flagged
