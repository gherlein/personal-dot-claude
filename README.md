# personal-dot-claude

A portable Claude Code configuration: one `CLAUDE.md` file, 23 skill files, and an installer. Drop it into `~/.claude/` and every Claude Code session inherits a consistent set of coding standards, workflows, and domain-specific guidance.

## What's in the Box

```
personal-dot-claude/
├── CLAUDE.md                     Global config: preferences, code quality, security, git, workflow
├── safe-install                  Installer script (backs up existing config, deploys to ~/.claude/)
└── .claude/skills/
    ├── three-experts-method.md   Multi-perspective analysis (3 domain experts)
    ├── iterative-refinement.md   Progressive improvement through 3 rounds of critique
    ├── evidence-based-debugging.md  Closed-loop debugging with 5 Whys root cause
    ├── test-as-guardrails.md     Three-context testing: write, test, triage in separate sessions
    ├── spec-driven-development.md   Specs as primary artifact; code generated from specs
    ├── code-review.md            Four-category review (Architecture, Quality, Maintainability, Safety)
    ├── implementation-planning.md   Four-phase planning with Go/web/embedded/cross-tier templates
    ├── task-orchestration.md     Maestro pattern for multi-step projects and sub-agent delegation
    ├── refactoring.md            Gardener pattern: safe incremental improvement, never break behavior
    ├── project-onboarding.md     Bootstrap CLAUDE.md and hierarchical context for new projects
    ├── edge-case-discovery.md    Systematic gap analysis with domain-specific checklists
    ├── clean-comments.md         Strip redundant comments, preserve TODOs and suppression directives
    ├── emoji.md                  Add contextual emoji to headers and lists
    ├── documentation.md          README/DESIGN.md conventions, API docs, Mermaid diagrams
    ├── git-operations.md         Cherry-pick by inspection, rebase conflict resolution
    ├── go-performance.md         GC tuning, allocation reduction, profiling with pprof
    ├── go-usb.md                 USB device libraries, udev rules, endpoint handling in Go
    ├── postgresql.md             Schema design, indexing, query optimization, migrations
    ├── rest-api-design.md        Resource URLs, status codes, pagination, JSON envelope structure
    ├── reverse-engineering.md    10-phase codebase analysis framework
    ├── learn.md                  Record tricky solutions in CLAUDE.local.md for future sessions
    ├── plan-todo.md              Create and execute task lists in .llm/todo.md
    └── web-frontend.md           React/TypeScript, Tailwind, shadcn/ui, testing-library, Playwright
```

## Design Principles

### Minimal context loading

`CLAUDE.md` is ~110 lines. It contains only global preferences, code quality rules, and workflow guidance. Detailed domain knowledge lives in individual skill files that Claude Code loads on demand through the `.claude/skills/` directory. This keeps every session's baseline context small and avoids wasting tokens on irrelevant guidance.

### Layered architecture

The configuration follows Claude Code's hierarchical context model:

- **Global** (`~/.claude/CLAUDE.md`) -- applies to all projects
- **Project** (project root `CLAUDE.md`) -- project-specific build commands, architecture, conventions
- **Module** (subdirectory `CLAUDE.md`) -- overrides for specific subsystems

This repository provides the global layer. Each project then adds its own overrides without duplicating the shared standards.

### Specs over code

Requirements and design documents are the most valuable artifacts. Code is ephemeral and can be regenerated from specs. When specs and code disagree, fix the code. Constraint IDs trace from spec sections to code comments, maintaining traceability across the spec-to-implementation boundary.

### Fresh context for quality

Critical thinking tasks -- code review, test writing, triage -- are performed in separate contexts from implementation. This prevents the author's assumptions from leaking into verification, a problem the test-as-guardrails skill calls "specification gaming."

### Evidence over intuition

Debugging requires reproducible proof before a fix is accepted. The evidence-based-debugging workflow enforces a closed loop: reproduce the failure, investigate with domain-specific tools (pprof, devtools, UART logs, kubectl), verify the fix eliminates the root cause. No fix is complete without a test that would have caught it.

### Domain-adapted, not generic

Every skill is tailored to a specific tech stack: Go, TypeScript, embedded (RP2040), SBCs (Raspberry Pi/Orange Pi), PostgreSQL, Kubernetes. Recommendations account for the constraints of each tier -- memory budgets on embedded, goroutine safety in Go, bundle size on the web, partition tolerance in distributed systems.

### Actionable over theoretical

Each skill file contains executable workflows, concrete templates, and checklists. No abstract principles without a corresponding procedure. Patterns like the three-experts method and iterative-refinement define exact steps, roles, and output formats.

### Safe by default

Security rules are non-negotiable: no secrets in commits, parameterized queries, input validation at boundaries, HTTPS for external calls. The refactoring skill follows a "Hippocratic oath" -- never change external behavior. Git operations forbid `--no-verify`. Tests are never skipped or stubbed.

## Skill Categories

**Methodology** -- how to approach work:
three-experts-method, iterative-refinement, evidence-based-debugging, test-as-guardrails, spec-driven-development, code-review, implementation-planning, task-orchestration, refactoring

**Domain** -- technology-specific guidance:
project-onboarding, edge-case-discovery, go-performance, go-usb, postgresql, rest-api-design, reverse-engineering, web-frontend

**Utility** -- helper operations:
clean-comments, emoji, documentation, git-operations, learn, plan-todo

## Installation

The `safe-install` script deploys `CLAUDE.md` and all skill files to `~/.claude/`. It creates timestamped backups of any existing configuration and removes stale skill files from previous installations. It does not touch operational data (credentials, history, settings, cache).

```bash
./safe-install
```

For project-level use without global installation:

```bash
cp CLAUDE.md /path/to/project/
mkdir -p /path/to/project/.claude/skills
cp .claude/skills/*.md /path/to/project/.claude/skills/
```

Then add a project-specific `CLAUDE.md` in the project root with build commands, architecture notes, and any overrides.

## Origins

This configuration was synthesized from four source repositories on agentic coding practices:

- **agentic-coding-handbook** -- workflows (TDD, debug, spec-first, memory bank), prompt engineering, project setup
- **agentic-prompts** -- role-based personas (Principal Engineer, Lead Implementer, Test Engineer, Maestro), subtask decomposition, context compression
- **agentic-system-prompts** -- Claude Code system prompt analysis, tool usage patterns, agent orchestration
- **agenticoding.github.io** -- four-phase methodology, grounding, tests-as-guardrails, spec-driven development, agent-friendly code patterns

The content was deduplicated, adapted to a specific engineer's tech stack and preferences, and restructured to follow the minimal-context-loading principle.
