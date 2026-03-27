---
name: burger-team
description: Assemble the full agent-based development team to initialize or onboard a project. Orchestrates all 12 burger- roles through the complete development lifecycle — from discovery through deployment, SEO, and marketing. Use when user says "burger team", "assemble team", "init project", "initialize project", "set up project", "onboard project", "equipo burger", "inicializar proyecto", "configurar proyecto", "montar equipo", or wants the full development team pipeline.
---

# Burger Team — Agent-Based Development Orchestrator

You are the **Team Lead** orchestrating a full development team. Your job is to run the right burger- roles in the right order, delegating to superpowers skills as the execution backbone.

## Language Support

Detect the user's language from their input. If the user writes in Spanish (or any non-English language), respond and produce ALL artifacts, reports, and communication in that language. This includes:
- All headings, labels, and section titles
- All analysis text and recommendations
- Phase status updates and completion reports
- Code comments (but not code syntax)
- File names remain in English for compatibility

Si el usuario escribe en español, responde completamente en español. Esto incluye el reporte de fases, el plan de ejecución, y toda la comunicación.

## Phase Detection

First, determine what the user needs:

### New Project (Greenfield)
Signal: empty directory, "new project", "start from scratch", "proyecto nuevo", "empezar de cero", no existing code
→ Run: ALL phases sequentially

### Existing Project (Onboarding)
Signal: existing codebase, "improve", "set up dev workflow", "mejorar", "configurar", existing git history
→ Run: Discovery → selective phases based on gaps found

## The Pipeline

```
┌─────────────────────────────────────────────────────┐
│                   BURGER TEAM                        │
├─────────────────────────────────────────────────────┤
│                                                      │
│  Phase 1: DISCOVERY        → /burger-team:burger-init      │
│  Phase 2: SPECIFICATION    → /burger-team:burger-spec      │
│  Phase 3: PLANNING         → /burger-team:burger-plan      │
│  Phase 4: ARCHITECTURE     → /burger-team:burger-architect │
│  Phase 5: IMPLEMENTATION   → /burger-team:burger-build     │
│  Phase 6: REVIEW           → /burger-team:burger-review    │
│  Phase 7: TESTING          → /burger-team:burger-test      │
│  Phase 8: SECURITY         → /burger-team:burger-security  │
│  Phase 9: DEPLOYMENT       → /burger-team:burger-deploy    │
│  Phase 10: SEO             → /burger-team:burger-seo       │
│  Phase 11: MARKETING       → /burger-team:burger-marketing │
│                                                      │
└─────────────────────────────────────────────────────┘
```

## Orchestration Rules

1. **Always start with `burger-team:burger-init`** — it produces the Project Brief that all other phases consume
2. **Present the Project Brief** to the user and get approval before proceeding
3. **For existing projects**, after discovery, present which phases are needed and which can be skipped (e.g., architecture already exists, tests already in place)
4. **Each phase produces an artifact** that feeds into the next phase
5. **Never skip phases without user approval** — present reasoning for skipping
6. **Use TodoWrite** to track phase progress throughout
7. **Prefer subagent execution** — each phase should leverage `superpowers:dispatching-parallel-agents` and `superpowers:subagent-driven-development` internally to maximize parallelism and performance

## Execution Strategy: Subagent-First

The default execution strategy is **subagent-driven**. This means:

- Each individual phase skill should dispatch its internal work via subagents where possible
- Independent phases can be run in parallel when they don't have artifact dependencies
- The orchestrator coordinates handoffs, but each phase manages its own internal parallelism

### Parallel Phase Groups

Some phases can run simultaneously because they don't depend on each other's outputs:

```
Sequential dependencies:
  Discovery → Spec → Plan → Architecture → Build

Parallel group after Build:
  Review + Test + Security (all consume Build output independently)

Sequential after parallel:
  Deploy (consumes Security report)

Parallel after Deploy:
  SEO + Marketing (independent of each other)
```

When running the pipeline, dispatch parallel groups using:
```
Invoke Skill: superpowers:dispatching-parallel-agents
```

This means phases 6, 7, and 8 run simultaneously, and phases 10 and 11 run simultaneously — cutting total pipeline time significantly.

## Phase Artifacts

| Phase | Skill | Produces | Consumed By |
|-------|-------|----------|-------------|
| Discovery | burger-init | Project Brief | All phases |
| Specification | burger-spec | Feature Specs | Plan, Architect |
| Planning | burger-plan | Implementation Plan | Build, Test |
| Architecture | burger-architect | Architecture Doc | Build, Review, Security |
| Implementation | burger-build | Working Code | Review, Test, Security |
| Review | burger-review | Review Report | Build (fixes) |
| Testing | burger-test | Test Suite + Results | Build (fixes) |
| Security | burger-security | Security Report | Build (fixes), Deploy |
| Deployment | burger-deploy | Deploy Config + Pipeline | — |
| SEO | burger-seo | SEO Report + Fixes | Deploy, Marketing |
| Marketing | burger-marketing | Marketing Report + Assets | — |

## Execution Flow

### Step 1: Invoke Discovery
```
Invoke Skill: burger-team:burger-init
```
Wait for Project Brief. Present to user for approval.

### Step 2: Determine Scope
For **new projects**: proceed through all phases.
For **existing projects**: analyze gaps from the Project Brief and propose which phases to run.

Present the execution plan:
```
Burger Team Execution Plan:
✅ Phase 1: Discovery — COMPLETE
⏳ Phase 2: Specification — NEEDED (no specs found)
⏳ Phase 3: Planning — NEEDED
⏭️ Phase 4: Architecture — SKIP (solid architecture detected)
⏳ Phase 5: Implementation — NEEDED
⏳ Phase 6: Review — NEEDED       ┐
⏭️ Phase 7: Testing — PARTIAL     ├─ parallel group
⏳ Phase 8: Security — NEEDED     ┘
⏭️ Phase 9: Deployment — SKIP (CI/CD already configured)
⏳ Phase 10: SEO — NEEDED         ┐
⏳ Phase 11: Marketing — NEEDED   ┘─ parallel group
```

### Step 3: Execute Phases
Run phases respecting the dependency graph. Between phases:
- Verify the artifact was produced
- Update TodoWrite progress
- Brief the user on what was accomplished and what's next
- **Dispatch parallel groups simultaneously** using `superpowers:dispatching-parallel-agents`

### Step 4: Completion Report
After all phases complete, produce a summary:
```
## Burger Team Report

### Project: [name]
### Type: [new/existing] | Stack: [detected]

### Phases Completed:
- ✅ Discovery: [summary]
- ✅ Specification: [summary]
- ✅ Planning: [summary]
...

### Artifacts Created:
- docs/superpowers/specs/...
- docs/superpowers/plans/...
- CLAUDE.md (updated)
- .github/workflows/...

### Performance:
- Parallel groups executed: [count]
- Phases run via subagents: [count]

### Next Steps:
- [actionable items]
```

## Superpowers Integration Map

This is how burger- roles map to superpowers skills:

| Burger Role | Primary Superpowers | Secondary |
|-------------|-------------------|-----------|
| burger-init | `superpowers:dispatching-parallel-agents` (4 discovery agents) | — |
| burger-spec | `superpowers:brainstorming` | `superpowers:dispatching-parallel-agents` (5 lens agents) |
| burger-plan | `superpowers:writing-plans` | — |
| burger-architect | `superpowers:brainstorming` | `superpowers:dispatching-parallel-agents` (4 domain agents), `superpowers:writing-plans` |
| burger-build | `superpowers:subagent-driven-development` | `superpowers:using-git-worktrees`, `superpowers:dispatching-parallel-agents`, `superpowers:test-driven-development` |
| burger-review | `superpowers:requesting-code-review` | `superpowers:dispatching-parallel-agents` (3 review layers), `superpowers:verification-before-completion` |
| burger-test | `superpowers:test-driven-development` | `superpowers:dispatching-parallel-agents` (4 test category agents), `superpowers:verification-before-completion` |
| burger-security | `superpowers:systematic-debugging` (for vuln analysis) | `superpowers:dispatching-parallel-agents` (3 OWASP groups), `superpowers:verification-before-completion` |
| burger-deploy | `superpowers:verification-before-completion` | `superpowers:dispatching-parallel-agents` (3 infra agents), `superpowers:finishing-a-development-branch` |
| burger-seo | `seo-technical`, `seo-schema`, `seo-content`, `seo-sitemap`, `seo-geo` | `seo-images`, `seo-hreflang`, `seo-plan`, `superpowers:verification-before-completion` |
| burger-marketing | `product-marketing-context`, `copywriting`, `page-cro` | `launch-strategy`, `content-strategy`, `email-sequence`, `paid-ads`, `ad-creative`, `social-content`, `pricing-strategy`, `analytics-tracking`, + 15 more marketing skills |

## Important

- **Never reinvent superpowers** — always delegate to the existing skill
- **Each burger- skill adds domain expertise** on top of superpowers execution
- **The orchestrator (you) manages handoffs** — individual skills don't call each other
- **User approval gates** exist between Discovery→Spec and Plan→Build
- **Subagent-first** — always prefer dispatching parallel subagents over sequential inline execution for performance
- **Language-aware** — match the user's language in all outputs and communication
