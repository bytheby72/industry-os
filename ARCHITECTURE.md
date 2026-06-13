# Industry OS — Architecture

Autonomous Business Operating System on Hermes Agent.
Five layers, each building on the previous.

## Layer Stack

```
                         ┌──────────────────────────────────────┐
                         │   Layer 4: INTELLIGENCE PIPELINES     │
                         │   market monitors · tender analysis   │
                         │   financial forensics · autoresearch  │
                         ├──────────────────────────────────────┤
                         │   Layer 3: BUSINESS OS                │
                         │   Drive sort · CRM · 1C pipeline      │
                         │   partner analytics · price monitor   │
                         ├──────────────────────────────────────┤
                         │   Layer 2: SWARM MANAGEMENT           │
                         │   registry · lanes · sweeper          │
                         │   control plane · human gate          │
                         ├──────────────────────────────────────┤
                         │   Layer 1: DEVOPS CORE                │
                         │   quality gates · ratchet loop        │
                         │   auto-learning · dependency graph    │
                         ├──────────────────────────────────────┤
                         │   Layer 0: HERMES AGENT               │
                         │   skills · cron · delegate_task       │
                         │   memory · tools · context compress   │
                         └──────────────────────────────────────┘
```

### Layer 0 — Hermes Agent (Foundation)
The runtime. Provides: skill system, cron scheduler, delegate_task for sub-agents, persistent memory, tool integration (terminal, web, browser, files), context compression.

Nothing in Industry OS works without this layer. Hermes is the OS kernel.

### Layer 1 — DevOps Core
Foundation skills every autonomous system needs:

| Skill | Function |
|-------|----------|
| **quality-gates** | 4-stage validation: correctness → quality → security → consistency. Blocks bad output before it reaches production. |
| **ratchet-loop** | Autonomous improvement: improve → run → eval → keep/revert. Agents self-improve over time. |
| **auto-learning-pipeline** | 5-stage curator: discover → score → rank → ingest → learn. Builds institutional knowledge from experience. |
| **skill-dependency-graph** | Visualize skill dependencies, detect circular references, suggest compositions. |

### Layer 2 — Swarm Management
Orchestrates multiple agents working in parallel:

| Component | Function |
|-----------|----------|
| **tera-harness** | Run Registry (SQLite), 4 lanes (scout/research/fulfill/delivery), completion routing, recovery sweeper, control plane. |
| **Lanes** | scout: monitors sources → research: deep analysis → fulfill: executes decisions → delivery: sends results |
| **Human Gate** | Single human decision point. Agent proposes, human approves/shelves/modifies. |
| **Hashline** | SHA256 chain of events — cryptographic audit trail for every run. |
| **Stream Rules** | Context-aware rules that trigger on events (consecutive errors → auto-fail, empty scrape → mark low quality). |
| **OMP Bridge** | Bridge to external agent runners for additional compute. |

### Layer 3 — Business OS
Domain-specific business automation:

| Subsystem | Function |
|-----------|----------|
| **Drive Sorter** | Auto-classify Google Drive files into 49-folder structure. Date-based naming, version history. |
| **Obsidian CRM** | Markdown-based people/company/meeting tracking. Auto-extracted from emails and documents. |
| **1C Pipeline** | Email → parse → SQLite pipeline for 1C financial reports (gross profit, accounts receivable, sales, cash flow). |
| **Partner Analytics** | Volume comparisons, risk detection, recommendation generation. Cross-reference 5+ data sources. |
| **Price Monitor** | Camoufox-based competitor price tracking. Self-healing selectors when websites change. |
| **Tender Intelligence** | Government tender parsing (icetrade.by, goszakupki.by). Pattern analysis, opportunity scoring. |
| **Financial Intelligence** | Belarusian accounting docs → stability ratios → A-E ratings → CFO reports. |
| **Frappe CRM** | PartnerGrid: custom DocTypes (Partner, Deal, Grid Task). Hetzner-deployed. |
| **Negotiation Prep** | Trip schedule → per-partner financials → structured negotiation plans. |

### Layer 4 — Intelligence Pipelines
Long-running autonomous research and monitoring:

| Pipeline | Function |
|----------|----------|
| **Market Monitors** | Price, competitor, catalog — continuous scraping with SQLite storage. |
| **Regulatory Watch** | Tender pattern analysis, currency tracking, policy changes. |
| **Financial Forensics** | Bank statement parsing, bankruptcy detection, related-party analysis. |
| **Autoresearch** | Iterative web research loop with scored opportunity maps. |

## Cross-Cutting Concerns

### Security (P0)
- Fail-closed: if a policy check fails, the system stops
- Local-first: sensitive business data never leaves user's Google Drive or local vault
- No cronjobs before hardening: P0 gate (loop-harden skill) mandatory before production
- Confidential data stays local: no paid Firecrawl/Tavily/Exa without explicit approval

### Privacy
- Commercial data (prices, discounts, partner terms, volumes) on user's Google Drive only
- Personal data (EKOP evaluations, PPF partner data) in local `~/fabric/` vault
- Service account for Drive (read), OAuth for Gmail (required — service account fails)

### Self-Healing
- Web scrapers auto-recover when site layouts change (LLM-powered selector repair)
- 3 failures in a week → mark "needs_manual_review"
- Cron jobs auto-restart after timeout (recovery sweeper)

### Audit Trail
- Every Tera Harness run has hashline chain (SHA256, cryptographically verifiable)
- Event log in SQLite registry: `events` table with event_type, payload, timestamp
- All state changes recorded: who, what, when, with what result

## Integration Points

```
Hermes Agent
  ├── skills/ (Layer 1 skills)
  │     ├── devops/quality-gates
  │     ├── devops/ratchet-loop
  │     ├── devops/auto-learning-pipeline
  │     └── devops/skill-dependency-graph
  ├── skills/ (Layer 2 — swarm)
  │     └── tera-harness
  ├── skills/ (Layer 3 — business)
  │     ├── data-science/iek-os-drive-sort
  │     ├── iek-price-monitor
  │     └── tender-pattern-analysis
  ├── skills/ (Layer 3.5 — methodology)
  │     └── devops/loop-engineering (+ audit, harden, deploy, security)
  └── ~/fabric/ (shared memory across all layers)
        ├── daily/
        ├── people/
        ├── companies/
        ├── projects/
        └── topics/
```

## Design Principles

1. **Namespace-first** — every skill lives in a clear namespace (devops/, data-science/, tera-harness/)
2. **Database-driven** — SQLite for monitoring, long-term data, audit trails
3. **Local-first** — minimize cloud dependencies, maximize privacy
4. **Fail-closed** — undefined state = stop, not continue
5. **Self-documenting** — every skill has SKILL.md with pitfalls, lessons learned, recovery procedures
6. **Measurable** — every autonomous loop tracks cost per accepted change

## Evolution Path

```
Phase 1 (current): DevOps Core + Business OS
  → quality gates, ratchet loop, Drive sort, 1C pipeline

Phase 2 (in progress): Swarm Management
  → Tera Harness running scout/research/fulfill/delivery cycles

Phase 3 (next): Intelligence Pipelines
  → Autonomous market monitors with human gate decisions

Phase 4 (future): Self-Improving OS
  → Ratchet loop applied to the OS itself. Agents improve agents.
```
