---
name: industry-os
description: >-
  Industry OS — Autonomous Business Operating System on Hermes Agent.
  Five layers: DevOps Core → Swarm Management → Business OS → Intelligence Pipelines.
  Umbrella skill that coordinates all sub-skills.
version: 1.0.0
author: Anatoli Piakhouski
metadata:
  hermes:
    tags: [industry-os, operating-system, autonomous, business, umbrella]
    depends_on: []
    related_skills:
      - quality-gates
      - ratchet-loop
      - auto-learning-pipeline
      - skill-dependency-graph
      - tera-harness
      - iek-os-drive-sort
      - loop-engineering
      - loop-audit
      - loop-harden
      - loop-deploy
      - loop-security
---

# Industry OS

Autonomous Business Operating System on Hermes Agent.

## When to Use

- Designing a new autonomous agent workflow
- Adding a new skill — need to know which layer it belongs in
- Auditing the system — need the full architecture
- Onboarding — understanding how the pieces fit together

## System Overview

Industry OS is a 5-layer operating system for autonomous business agents:

```
Layer 4: Intelligence Pipelines — autonomous research, market monitoring, forensics
Layer 3: Business OS — Drive sort, CRM, 1C pipeline, partner analytics, price monitor
Layer 2: Swarm Management — agent registry, lanes, sweeper, control plane, human gate
Layer 1: DevOps Core — quality gates, ratchet loop, auto-learning, dependency graph
Layer 0: Hermes Agent — skills, cron, delegate_task, memory, tools
```

## Adding a New Skill

1. Determine which layer the skill belongs to
2. Create it in the correct namespace:
   - `devops/` for Layer 1
   - `tera-harness/` for Layer 2
   - `data-science/` or `iek/` for Layer 3
   - `data-science/` for Layer 4
3. Run quality gates on the new skill
4. Update skill-dependency-graph

## Auditing

Run the full audit:

```
# 1. Check skill dependencies
skill-dependency-graph

# 2. Audit loops
loop-audit

# 3. Security review
loop-security

# 4. Verify quality gates still pass
quality-gates (on all skills)
```

## Pitfalls

- **Don't skip layers.** Layer 1 (quality gates) must pass before Layer 2 (swarm) can spawn agents. Layer 2 must be healthy before Layer 3 (business) can trust results.
- **Human gate is mandatory for Layer 4.** Intelligence pipelines produce analysis, not decisions. Human approves before action.
- **Fail-closed everywhere.** If a policy check fails, the system stops. Never degrade gracefully on security failures.
- **Local-first data.** Confidential business data stays on user's Google Drive / local vault. Never upload to external services.

## References

- ARCHITECTURE.md — full 5-layer design with cross-cutting concerns
- https://github.com/bytheby72/industry-os — this repo
- https://github.com/bytheby72/hermes-loop-skills — loop engineering methodology
- https://github.com/bytheby72/anatoli-google-infra — Google Workspace infrastructure
- https://github.com/bytheby72/anatoli-hermes-skills — curated business skills
