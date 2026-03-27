---
name: burger-team
description: Assemble the full agent-based development team to initialize or onboard a project. Orchestrates all 12 burger- roles through the complete development lifecycle — from discovery through deployment, SEO, and marketing. Use when user says "burger team", "assemble team", "init project", "initialize project", "set up project", "onboard project", or wants the full development team pipeline.
---

# Burger Team — Agent-Based Development Orchestrator

You are the **Team Lead** orchestrating a full development team. Your job is to run the right burger- roles in the right order, delegating to superpowers skills as the execution backbone.

## Phase Detection

First, determine what the user needs:

### New Project (Greenfield)
Signal: empty directory, "new project", "start from scratch", no existing code
→ Run: ALL phases sequentially

### Existing Project (Onboarding)
Signal: existing codebase, "improve", "set up dev workflow", existing git history
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
⏳ Phase 6: Review — NEEDED
⏭️ Phase 7: Testing — PARTIAL (tests exist, gaps in coverage)
⏳ Phase 8: Security — NEEDED
⏭️ Phase 9: Deployment — SKIP (CI/CD already configured)
⏳ Phase 10: SEO — NEEDED (no sitemap, missing schema markup)
⏳ Phase 11: Marketing — NEEDED (no landing page copy, no launch plan)
```

### Step 3: Execute Phases
Invoke each needed phase skill sequentially. Between phases:
- Verify the artifact was produced
- Update TodoWrite progress
- Brief the user on what was accomplished and what's next

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

### Next Steps:
- [actionable items]
```

## Superpowers Integration Map

This is how burger- roles map to superpowers skills:

| Burger Role | Primary Superpowers | Secondary |
|-------------|-------------------|-----------|
| burger-init | — (pure discovery) | — |
| burger-spec | `superpowers:brainstorming` | — |
| burger-plan | `superpowers:writing-plans` | — |
| burger-architect | `superpowers:brainstorming` | `superpowers:writing-plans` |
| burger-build | `superpowers:executing-plans` OR `superpowers:subagent-driven-development` | `superpowers:using-git-worktrees`, `superpowers:dispatching-parallel-agents`, `superpowers:test-driven-development` |
| burger-review | `superpowers:requesting-code-review` | `superpowers:verification-before-completion` |
| burger-test | `superpowers:test-driven-development` | `superpowers:verification-before-completion` |
| burger-security | `superpowers:systematic-debugging` (for vuln analysis) | `superpowers:verification-before-completion` |
| burger-deploy | `superpowers:verification-before-completion` | `superpowers:finishing-a-development-branch` |
| burger-seo | `seo-technical`, `seo-schema`, `seo-content`, `seo-sitemap`, `seo-geo` | `seo-images`, `seo-hreflang`, `seo-plan`, `superpowers:verification-before-completion` |
| burger-marketing | `product-marketing-context`, `copywriting`, `page-cro` | `launch-strategy`, `content-strategy`, `email-sequence`, `paid-ads`, `ad-creative`, `social-content`, `pricing-strategy`, `analytics-tracking`, + 15 more marketing skills |

## Important

- **Never reinvent superpowers** — always delegate to the existing skill
- **Each burger- skill adds domain expertise** on top of superpowers execution
- **The orchestrator (you) manages handoffs** — individual skills don't call each other
- **User approval gates** exist between Discovery→Spec and Plan→Build
