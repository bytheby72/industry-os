# Industry OS 🔥

**Autonomous Business Operating System on Hermes Agent.** Five layers of production-ready automation — from DevOps core to swarm management to business intelligence.

Built on [Hermes Agent](https://hermes-agent.nousresearch.com/). Battle-tested in production across price monitoring, partner analytics, financial forensics, and tender intelligence.

## The Stack

```
Layer 4: Intelligence Pipelines   ← autonomous market research, tender analysis, financial forensics
Layer 3: Business OS              ← Drive sort, CRM, 1C pipeline, partner analytics, price monitor
Layer 2: Swarm Management         ← agent registry, 4 lanes, recovery sweeper, human gate
Layer 1: DevOps Core              ← quality gates, ratchet loop, auto-learning, dependency graph
Layer 0: Hermes Agent             ← skills, cron, delegate_task, memory, tools
```

## What's Included

### DevOps Core (this repo)
| Skill | What It Does |
|-------|-------------|
| **quality-gates** | 4-stage validation (correctness → quality → security → consistency). Blocks bad output before production. |
| **ratchet-loop** | Autonomous improve → run → eval → keep/revert. Agents self-improve overnight. |
| **auto-learning-pipeline** | 5-stage curator: discover → score → rank → ingest → learn patterns. |
| **skill-dependency-graph** | Visualize skill dependencies, detect circular refs, suggest compositions. |

### Swarm Management → [tera-harness](https://github.com/bytheby72/tera-harness)
| Component | What It Does |
|-----------|-------------|
| Run Registry | SQLite-backed agent run tracking with hashline audit chain |
| 4 Lanes | scout (monitor) → research (analyze) → fulfill (execute) → delivery (send) |
| Recovery Sweeper | Auto-restart stuck agents, escalate after 3 failures |
| Human Gate | Single decision point — agent proposes, human approves |

### Business OS → [anatoli-google-infra](https://github.com/bytheby72/anatoli-google-infra)
| Subsystem | What It Does |
|-----------|-------------|
| Drive Sorter | Auto-classify files into 49-folder structure with version history |
| Obsidian CRM | Markdown-based people/company/meeting tracking |
| 1C Pipeline | Email → parse → SQLite for financial reports |
| Partner Analytics | Volume comparisons, risk detection, recommendations |
| Price Monitor | Camoufox-based competitor tracking with self-healing selectors |

### Intelligence Pipelines
| Pipeline | What It Does |
|----------|-------------|
| Market Monitors | Continuous competitor price/catalog scraping |
| Tender Intelligence | Government tender parsing and opportunity scoring |
| Financial Forensics | Bank statement parsing, bankruptcy detection |

### Methodology → [hermes-loop-skills](https://github.com/bytheby72/hermes-loop-skills)
Loop engineering with 4 hardening skills: audit, harden, deploy, security.

## Quick Start

```bash
# Clone Industry OS
git clone https://github.com/bytheby72/industry-os.git

# Install DevOps Core
cp -r industry-os/skills/* ~/.hermes/skills/devops/

# Verify installation
hermes skill list | grep quality-gates
hermes skill list | grep ratchet-loop

# Run your first quality gate
# (in any Hermes session after code changes)
"Run quality gates on my last change"
```

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for the full 5-layer design, cross-cutting concerns (security, privacy, self-healing, audit), and integration points.

## Design Principles

1. **Namespace-first** — every skill in a clear namespace
2. **Database-driven** — SQLite for all long-term state
3. **Local-first** — sensitive data never leaves user's infrastructure
4. **Fail-closed** — undefined state stops the system
5. **Self-documenting** — every skill has pitfalls, lessons, recovery
6. **Measurable** — cost per accepted change tracked for every loop

## Evolution

- **Phase 1 (done):** DevOps Core + Business OS
- **Phase 2 (in progress):** Swarm Management via Tera Harness
- **Phase 3 (next):** Autonomous intelligence pipelines with human gate
- **Phase 4 (future):** Self-improving OS — ratchet loop applied to the OS itself

## License

MIT
