# Burger Dev Team v2

An agent-based development team plugin for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Orchestrates 15 specialized AI agent roles through a complete development-to-launch lifecycle, powered by [superpowers](https://github.com/anthropics/superpowers) and 40+ specialized skills.

**What's new in v2** — Inspired by GSD's best innovations, now with:
- Discussion phase (lock decisions before planning)
- 8-dimension plan verification
- Wave-based parallel execution with fresh context per executor
- User acceptance testing (UAT) gate
- Error recovery (RETRY / DECOMPOSE / PRUNE)
- Atomic git history (one commit per task, enables `git bisect`)
- State persistence (`.planning/` directory survives context resets)
- Context handoff (HANDOFF.json for session recovery)
- Requirements traceability (Spec → Decisions → Plan → Code → Tests)
- Auto-advance (`/burger-team:burger-next` detects state, chains next step)

**Subagent-first architecture** — every phase dispatches parallel subagents. Independent phases (Review + Test + Security, SEO + Marketing) run simultaneously.

**Multilingual** — responds in the user's language. Full Spanish support built-in. / **Soporte completo en español.**

<p align="center">
  <img src="assets/demo.svg" alt="Burger Team demo" width="750"/>
</p>

## Installation

### Option 1: Install from GitHub (recommended)

```bash
claude plugin add github:cookiezGIT/burger-dev-team
```

### Option 2: Clone and install locally

```bash
git clone https://github.com/cookiezGIT/burger-dev-team.git
claude --plugin-dir ./burger-dev-team
```

### Option 3: Add as a project plugin

Add to your project's `.claude/plugins.json`:

```json
[
  {
    "source": "github:cookiezGIT/burger-dev-team"
  }
]
```

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- A git repository (needed for worktree isolation during the build phase)

### Required Plugin Dependencies

| Plugin | Used By | Install Command |
|--------|---------|-----------------|
| [Superpowers](https://github.com/anthropics/superpowers) | All agents (execution backbone) | `claude plugin add github:anthropics/superpowers` |
| [UI/UX Pro Max](https://github.com/anthropics/ui-ux-pro-max) | burger-build (Frontend Expert) | `claude plugin add github:anthropics/ui-ux-pro-max` |
| [SEO Suite](https://github.com/anthropics/seo) | burger-seo | `claude plugin add github:anthropics/seo` |
| [Marketing Skills](https://github.com/anthropics/marketing-skills) | burger-marketing | `claude plugin add github:anthropics/marketing-skills` |

> **Note**: Core development agents (phases 1-11) only require Superpowers. SEO and Marketing plugins are needed for phases 12-13.

#### Quick install all dependencies

```bash
claude plugin add github:anthropics/superpowers
claude plugin add github:anthropics/ui-ux-pro-max
claude plugin add github:anthropics/seo
claude plugin add github:anthropics/marketing-skills
```

## Quick Start

### Run the full pipeline

```
/burger-team:burger-team
```

Auto-detects whether you're starting fresh or onboarding an existing codebase, then runs all 13 phases with approval gates.

### Auto-advance (let it figure out what's next)

```
/burger-team:burger-next
```

### Or use individual agents

```
/burger-team:burger-init        # Phase 1:  Discover and analyze a project
/burger-team:burger-spec        # Phase 2:  Write feature specifications
/burger-team:burger-discuss     # Phase 3:  Lock implementation decisions
/burger-team:burger-plan        # Phase 4:  Create verified implementation plan
/burger-team:burger-architect   # Phase 5:  Design system architecture
/burger-team:burger-build       # Phase 6:  Wave-based implementation
/burger-team:burger-verify      # Phase 7:  User acceptance testing
/burger-team:burger-review      # Phase 8:  Code review
/burger-team:burger-test        # Phase 9:  Coverage analysis and test writing
/burger-team:burger-security    # Phase 10: OWASP Top 10 security audit
/burger-team:burger-deploy      # Phase 11: CI/CD and infrastructure
/burger-team:burger-seo         # Phase 12: SEO audit and optimization
/burger-team:burger-marketing   # Phase 13: Marketing strategy and growth
/burger-team:burger-next        # Auto-advance to next logical phase
```

## The Team

| Agent | Role | What It Does |
|-------|------|-------------|
| **burger-team** | Orchestrator | Runs the full 13-phase pipeline with state persistence and approval gates |
| **burger-init** | Discovery Agent | Auto-detects stack, architecture, gaps; produces Project Brief |
| **burger-spec** | Spec Writer | Transforms requirements into specs with acceptance criteria and requirement IDs |
| **burger-discuss** | Discussion Facilitator | Locks implementation decisions before planning (Interview or Assumptions mode) |
| **burger-plan** | Planner | Creates plans verified across 8 quality dimensions with requirements traceability |
| **burger-architect** | System Architect | Designs data models, API contracts, system boundaries with ADRs |
| **burger-build** | Build Dispatcher | Wave-based execution with fresh context per task, error recovery, atomic commits |
| **burger-verify** | Verification Agent | User acceptance testing with regression gates and failure diagnosis |
| **burger-review** | Code Reviewer | Spec + architecture + code quality review in parallel |
| **burger-test** | Tester | Coverage gap analysis, writes tests across 4 categories in parallel |
| **burger-security** | Security Expert | Full OWASP Top 10 audit with 3 parallel audit groups |
| **burger-deploy** | DevOps Engineer | CI/CD, Docker, monitoring with 3 parallel infra agents |
| **burger-seo** | SEO Expert | Technical SEO, schema markup, content quality, AI search optimization |
| **burger-marketing** | Marketing Strategist | Launch strategy, copywriting, CRO, ads, email, social, growth |
| **burger-next** | Auto-Advance | Detects current state and chains to the next logical phase |

## How It Works

### The Pipeline

```
Discovery → Spec → Discuss → Plan → Architecture → Build → Verify → ┬─ Review   ─┬→ Deploy → ┬─ SEO       ─┐
                                                                     ├─ Test      ─┤          └─ Marketing  ─┘
                                                                     └─ Security  ─┘
```

Phases 8-10 run in parallel. Phases 12-13 run in parallel. Each phase also dispatches internal subagents.

### Key Innovations (v2)

#### Decision Locking (Phase 3: Discussion)

Before planning, the Discussion phase captures and locks key implementation decisions:
- **Interview Mode**: Guided questioning about gray areas (libraries, patterns, visual standards)
- **Assumptions Mode**: Reads codebase, formulates assumptions with confidence ratings (Confident/Likely/Unclear)
- Produces `CONTEXT.md` with locked decisions that prevent planning drift

#### 8-Dimension Plan Verification (Phase 4: Plan)

Every plan is verified before execution:
1. Requirement Coverage — all acceptance criteria mapped
2. Task Atomicity — each task small enough for one session
3. Dependency Ordering — no forward references
4. Verification Completeness — concrete verification per task
5. Error Handling — failure scenarios covered
6. Rollback Safety — irreversible changes flagged
7. Context Window Fit — tasks won't overflow executors
8. Cross-Task Contracts — dependent tasks agree on interfaces

#### Wave-Based Execution (Phase 6: Build)

```
Wave 1 (parallel):  DB schema + Auth middleware + Frontend layout
Wave 2 (parallel):  API endpoints (needs schema) + Auth pages (needs middleware)
Wave 3 (sequential): Integration tests (needs everything)
```

Each executor gets a **fresh context window** — only loads the specific task, Project Brief, and relevant source files. This prevents quality degradation from accumulated context.

#### Error Recovery

When tasks fail: **RETRY** (fresh context) → **DECOMPOSE** (split into sub-tasks) → **PRUNE** (mark blocked, surface to user)

#### Atomic Git History

Each task = one commit: `feat(06-02): add user registration API endpoint`

Enables `git bisect` to find exactly which task broke something, and surgical reverts of individual tasks.

#### User Acceptance Testing (Phase 7: Verify)

Before code review, walk through every testable deliverable with the user:
- Extract deliverables from spec acceptance criteria
- PASS/FAIL each with evidence
- Auto-diagnose failures with `superpowers:systematic-debugging`
- Run regression tests against prior phases

#### State Persistence

All state in `.planning/` (git-tracked, human-readable):
```
.planning/
├── STATE.md               # Current phase, decisions, metrics
├── HANDOFF.json           # Context reset checkpoint
├── phases/completed.md    # Completed phases with summaries
└── recovery/continue-here.md  # Resume instructions
```

Survives context resets. Resume with `/burger-team:burger-next`.

### Phase Artifacts

| Phase | Artifact | Location |
|-------|----------|----------|
| Discovery | Project Brief | `docs/burger-team/project-brief.md` |
| Specification | Feature Specs | `docs/superpowers/specs/*.md` |
| Discussion | Decision Context | `docs/burger-team/decisions/*-CONTEXT.md` |
| Planning | Verified Plan | `docs/superpowers/plans/*.md` |
| Architecture | Architecture Doc + ADRs | `docs/burger-team/architecture.md` |
| Verification | UAT Report | `docs/burger-team/verification/*-UAT.md` |
| SEO | SEO Report | `docs/burger-team/seo-report.md` |
| Marketing | Marketing Report | `docs/burger-team/marketing-report.md` |

### Skills Integration

```
burger-init      → 4 parallel discovery subagents
burger-spec      → superpowers:brainstorming + 5 parallel lens subagents
burger-discuss   → superpowers:brainstorming + 4 parallel decision domain agents
burger-plan      → superpowers:writing-plans + 8-dimension verification
burger-architect → superpowers:brainstorming + 4 parallel domain subagents
burger-build     → superpowers:subagent-driven-development
                   wave execution + error recovery (RETRY/DECOMPOSE/PRUNE)
                   superpowers:using-git-worktrees
                   ui-ux-pro-max (Frontend Expert)
burger-verify    → superpowers:systematic-debugging + regression gates
burger-review    → superpowers:requesting-code-review + 3 parallel review layers
burger-test      → superpowers:test-driven-development + 4 parallel test writers
burger-security  → superpowers:systematic-debugging + 3 parallel OWASP auditors
burger-deploy    → 3 parallel infra subagents (env, CI/CD, monitoring)
burger-seo       → seo-technical, seo-schema, seo-content, seo-sitemap
                   seo-geo, seo-images, seo-hreflang, seo-plan
burger-marketing → product-marketing-context, copywriting, page-cro
                   + 25 more marketing skills
```

### Build Phase: Expert Subagents

- **Data Expert** — schema design, migrations, ORMs, query optimization
- **Backend Expert** — APIs, auth, middleware, services, error handling
- **Frontend Expert** — components, state management, routing, powered by `ui-ux-pro-max`

### SEO Phase: Multi-Dimensional Analysis

8+ specialized SEO skills: technical SEO, schema markup, content quality (E-E-A-T), sitemap, image optimization, AI search optimization (GEO), international SEO (hreflang).

### Marketing Phase: Full Growth Stack

25+ skills across messaging, conversion, acquisition, retention, monetization, infrastructure, and strategy.

## Supported Project Types

Auto-detects and adapts to: SaaS web apps, API services, CLI tools, libraries, monorepos, scrapers, static sites, and JAMstack.

## Plugin Structure

```
burger-dev-team/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   ├── burger-team/SKILL.md        # Orchestrator
│   ├── burger-init/SKILL.md        # Discovery
│   ├── burger-spec/SKILL.md        # Specification
│   ├── burger-discuss/SKILL.md     # Discussion (v2)
│   ├── burger-plan/SKILL.md        # Planning + Verification
│   ├── burger-architect/SKILL.md   # Architecture
│   ├── burger-build/SKILL.md       # Implementation + Waves
│   ├── burger-verify/SKILL.md      # UAT (v2)
│   ├── burger-review/SKILL.md      # Code Review
│   ├── burger-test/SKILL.md        # Testing
│   ├── burger-security/SKILL.md    # Security
│   ├── burger-deploy/SKILL.md      # Deployment
│   ├── burger-seo/SKILL.md         # SEO Expert
│   ├── burger-marketing/SKILL.md   # Marketing Strategist
│   └── burger-next/SKILL.md        # Auto-Advance (v2)
├── assets/
│   └── demo.svg
└── README.md
```

## License

MIT
