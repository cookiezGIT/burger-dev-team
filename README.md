# Burger Team

Agent-based development team plugin for Claude Code. Orchestrates 10 specialized roles through a complete development lifecycle, powered by [superpowers](https://github.com/anthropics/superpowers) skills.

## Installation

### Local testing
```bash
claude --plugin-dir /path/to/burger-team
```

### From marketplace (after publishing)
```bash
claude plugin install burger-team@your-marketplace
```

## Skills

| Skill | Role | Command |
|-------|------|---------|
| **burger-team** | Orchestrator — runs the full pipeline | `/burger-team:burger-team` |
| **burger-init** | Discovery — auto-detects stack, gaps, produces Project Brief | `/burger-team:burger-init` |
| **burger-spec** | Spec Writer — transforms requirements into detailed specs | `/burger-team:burger-spec` |
| **burger-plan** | Planner — creates task-by-task implementation plans | `/burger-team:burger-plan` |
| **burger-architect** | Architect — designs data models, APIs, system boundaries | `/burger-team:burger-architect` |
| **burger-build** | Build Dispatcher — dispatches data/backend/frontend experts | `/burger-team:burger-build` |
| **burger-review** | Code Reviewer — spec & architecture compliance + code quality | `/burger-team:burger-review` |
| **burger-test** | Tester — coverage gaps, integration & e2e tests | `/burger-team:burger-test` |
| **burger-security** | Security Expert — OWASP Top 10 audit, secret scan | `/burger-team:burger-security` |
| **burger-deploy** | DevOps — CI/CD, Docker, monitoring, infrastructure | `/burger-team:burger-deploy` |

## Usage

### Full Pipeline (new or existing project)
```
/burger-team:burger-team
```
This runs all 9 phases with approval gates between them.

### Individual Phases
Use any skill standalone:
```
/burger-team:burger-init      # Discover and analyze a project
/burger-team:burger-spec      # Write feature specifications
/burger-team:burger-security  # Run a security audit
```

## How It Works

### Pipeline
```
Discovery → Specification → Planning → Architecture → Build → Review → Test → Security → Deploy
```

### Superpowers Integration
Each burger- role delegates execution to superpowers skills:

- **Spec & Architecture** → `superpowers:brainstorming`
- **Planning** → `superpowers:writing-plans`
- **Building** → `superpowers:executing-plans` / `superpowers:subagent-driven-development` / `superpowers:dispatching-parallel-agents`
- **All implementation** → `superpowers:test-driven-development`
- **Review** → `superpowers:requesting-code-review`
- **Verification** → `superpowers:verification-before-completion`
- **Debugging** → `superpowers:systematic-debugging`
- **Workspace** → `superpowers:using-git-worktrees`
- **Completion** → `superpowers:finishing-a-development-branch`

### Artifacts
Each phase produces documents that feed the next:

```
docs/burger-team/project-brief.md    ← burger-init
docs/superpowers/specs/*.md          ← burger-spec (via brainstorming)
docs/superpowers/plans/*.md          ← burger-plan (via writing-plans)
docs/burger-team/architecture.md     ← burger-architect
```

## Requirements

- Claude Code with superpowers plugin installed
- Git repository (for worktree isolation during build phase)

## Project Types

Auto-detects and adapts to any project type:
- SaaS web apps (Next.js, Django, Rails, etc.)
- API services (REST, GraphQL)
- CLI tools
- Libraries/packages
- Monorepos
- Scrapers/bots
- Static sites

## License

MIT
