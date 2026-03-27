# Burger Dev Team

An agent-based development team plugin for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Orchestrates 10 specialized AI agent roles through a complete development lifecycle, powered by [superpowers](https://github.com/anthropics/superpowers) skills.

Instead of reinventing development workflows, Burger Dev Team wraps the battle-tested superpowers skill chain (brainstorming, planning, TDD, code review, etc.) and adds domain-specific expertise for each role — so you get a full dev team without the overhead.

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
- [Superpowers](https://github.com/anthropics/superpowers) plugin installed (the execution backbone)
- A git repository (needed for worktree isolation during the build phase)

## Quick Start

### Initialize any project with the full team

```
/burger-team:burger-team
```

This auto-detects whether you're starting fresh or onboarding an existing codebase, then runs all 9 phases with approval gates between them.

### Or use individual agents

```
/burger-team:burger-init        # Discover and analyze a project
/burger-team:burger-spec        # Write feature specifications
/burger-team:burger-plan        # Create implementation plan
/burger-team:burger-architect   # Design system architecture
/burger-team:burger-build       # Dispatch implementation agents
/burger-team:burger-review      # Run code review
/burger-team:burger-test        # Assess coverage and write tests
/burger-team:burger-security    # OWASP Top 10 security audit
/burger-team:burger-deploy      # Set up CI/CD and infrastructure
```

## The Team

| Agent | Role | What It Does |
|-------|------|-------------|
| **burger-team** | Orchestrator | Runs the full 9-phase pipeline with approval gates |
| **burger-init** | Discovery Agent | Auto-detects stack, architecture, gaps; produces a Project Brief |
| **burger-spec** | Spec Writer | Transforms requirements into detailed specs with acceptance criteria |
| **burger-plan** | Planner | Creates task-by-task plans with parallel groups, role tags, checkpoints |
| **burger-architect** | System Architect | Designs data models, API contracts, system boundaries with ADRs |
| **burger-build** | Build Dispatcher | Dispatches data/backend/frontend expert subagents in parallel |
| **burger-review** | Code Reviewer | Spec compliance + architecture compliance + code quality review |
| **burger-test** | Tester | Coverage gap analysis, writes integration & e2e tests |
| **burger-security** | Security Expert | Full OWASP Top 10 audit, dependency scan, secret detection |
| **burger-deploy** | DevOps Engineer | CI/CD pipelines, Docker configs, monitoring, health checks |

## How It Works

### The Pipeline

```
Discovery → Spec → Plan → Architecture → Build → Review → Test → Security → Deploy
```

Each phase produces artifacts that feed the next:

| Phase | Artifact | Location |
|-------|----------|----------|
| Discovery | Project Brief | `docs/burger-team/project-brief.md` |
| Specification | Feature Specs | `docs/superpowers/specs/*.md` |
| Planning | Implementation Plan | `docs/superpowers/plans/*.md` |
| Architecture | Architecture Doc + ADRs | `docs/burger-team/architecture.md` |

### Superpowers Integration

Every agent delegates execution to proven superpowers skills rather than reinventing workflows:

```
burger-spec      → superpowers:brainstorming
burger-plan      → superpowers:writing-plans
burger-architect → superpowers:brainstorming + writing-plans
burger-build     → superpowers:executing-plans / subagent-driven-development
                   superpowers:dispatching-parallel-agents
                   superpowers:test-driven-development
                   superpowers:using-git-worktrees
burger-review    → superpowers:requesting-code-review
burger-test      → superpowers:test-driven-development
                   superpowers:verification-before-completion
burger-security  → superpowers:systematic-debugging
                   superpowers:verification-before-completion
burger-deploy    → superpowers:verification-before-completion
                   superpowers:finishing-a-development-branch
```

### New vs Existing Projects

**New projects** run all 9 phases sequentially to scaffold from zero to deployed.

**Existing projects** get a discovery scan first, then the orchestrator recommends which phases to run and which to skip:

```
Burger Team Execution Plan:
  Phase 1: Discovery       — COMPLETE
  Phase 2: Specification   — NEEDED (no specs found)
  Phase 3: Planning        — NEEDED
  Phase 4: Architecture    — SKIP (solid architecture detected)
  Phase 5: Implementation  — NEEDED
  Phase 6: Review          — NEEDED
  Phase 7: Testing         — PARTIAL (tests exist, gaps in coverage)
  Phase 8: Security        — NEEDED
  Phase 9: Deployment      — SKIP (CI/CD already configured)
```

### Build Phase: Expert Subagents

During implementation, `burger-build` dispatches specialized subagents with injected expertise:

- **Data Expert** — schema design, migrations, ORMs, query optimization
- **Backend Expert** — APIs, auth, middleware, services, error handling
- **Frontend Expert** — components, state management, routing, accessibility

Independent tasks run in parallel. All subagents enforce TDD (red-green-refactor).

## Supported Project Types

Auto-detects and adapts to:

- SaaS web apps (Next.js, Django, Rails, Laravel, etc.)
- API services (REST, GraphQL, tRPC)
- CLI tools
- Libraries and packages
- Monorepos
- Scrapers and bots
- Static sites and JAMstack

## Plugin Structure

```
burger-dev-team/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── burger-team/SKILL.md
│   ├── burger-init/SKILL.md
│   ├── burger-spec/SKILL.md
│   ├── burger-plan/SKILL.md
│   ├── burger-architect/SKILL.md
│   ├── burger-build/SKILL.md
│   ├── burger-review/SKILL.md
│   ├── burger-test/SKILL.md
│   ├── burger-security/SKILL.md
│   └── burger-deploy/SKILL.md
└── README.md
```

## License

MIT
