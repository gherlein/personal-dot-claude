# DEPRECATED AND ARCHIVED - HISTORICAL VALUE ONLY
# dot-agents

A portable AI coding agent configuration: coding standards, 38 workflow skill files, and a security rule library — installable into any supported agent in one command.

Claude Code is the primary target. Pi coding agent is supported. The structure is designed to minimize lock-in to any single agent.

## What's in the Box

```
dot-agents/
├── CLAUDE.md              Agent instructions: preferences, code quality, security, git, workflow
├── INDEX.md               Skill and security-rule index: maps task types to the right files
├── safe-install           Installer: backs up existing config, deploys to ~/.claude/
├── skills/                38 workflow skill directories (one SKILL.md per skill, plus supporting files)
└── security-rules/        85+ security rule files organized by language, framework, and domain
```

See [DESIGN.md](DESIGN.md) for the full architectural description of how everything fits together.

## Skills

Skills are workflow guides — step-by-step protocols an agent follows for specific types of work. Each skill is a directory containing a `SKILL.md` file and optional supporting prompt files, checklists, and scripts.

### Process & Workflow

| Skill | When to use |
|-------|-------------|
| `brainstorming` | Before any implementation — explore intent, get design approved |
| `build-autonomous` | Full hands-off cycle: brainstorm → design → implement → review → deliver |
| `dispatching-parallel-agents` | 2+ independent tasks that can run concurrently |
| `executing-plans` | Execute a pre-written plan in a fresh session |
| `finishing-a-development-branch` | Complete and merge a development branch |
| `orchestrate` | Decompose complex multi-step projects into atomic tasks |
| `plan` | Lightweight planning for non-trivial features |
| `plan-todo` | Multi-step task checklist with user confirmation at each step |
| `subagent-driven-development` | Execute a plan with per-task review gates |
| `writing-plans` | Write a rigorous multi-step implementation plan |

### Code Quality

| Skill | When to use |
|-------|-------------|
| `code-review` | Review code before merging |
| `clean-comments` | Remove bad or redundant comments |
| `edge-case-discovery` | Find missing edge cases after implementation |
| `refactoring` | Safe incremental restructuring |
| `refine` | Iterative improvement of algorithms or design |
| `requesting-code-review` | Request a structured code review |
| `receiving-code-review` | Process incoming code review feedback |
| `test-as-guardrails` | Test strategy and edge case coverage |
| `test-driven-development` | Enforce test-first implementation order |
| `verification-before-completion` | Final gate before claiming work is done |

### Debugging & Quality Gates

| Skill | When to use |
|-------|-------------|
| `evidence-based-debugging` | Closed-loop debugging with domain tools (Go, K8s, web) |
| `systematic-debugging` | Debugging with Iron Law and root cause mandate |

### Language & Domain

| Skill | When to use |
|-------|-------------|
| `go-performance` | Go GC tuning, allocation reduction, pprof profiling |
| `go-usb` | USB/HID/serial device development in Go |
| `postgresql` | Schema design, indexing, queries, migrations |
| `rest-api-design` | RESTful API conventions and OpenAPI documentation |
| `web-frontend` | React/TypeScript/Tailwind/shadcn frontend work |

### Documentation & Learning

| Skill | When to use |
|-------|-------------|
| `documentation` | READMEs, API docs, design documents, Mermaid diagrams |
| `learn` | Record a tricky solution for future reference |
| `onboard` | Ramp up on an unfamiliar codebase |
| `reverse-engineer` | Understand an existing system's architecture |
| `spec-driven` | Treat specs as the authoritative source; regenerate code from them |
| `three-experts` | Complex architecture decisions needing multiple perspectives |

### Git & Infrastructure

| Skill | When to use |
|-------|-------------|
| `git-ops` | Cherry-pick, rebase, complex git operations |
| `using-git-worktrees` | Isolate work in a git worktree |

### Meta

| Skill | When to use |
|-------|-------------|
| `using-superpowers` | Select which skill to use |
| `writing-skills` | Create a new skill |

### Miscellaneous

`emoji` — add contextual emoji to headers and lists

## Security Rules

`security-rules/` contains 85+ policy files organized by domain. Agents read the relevant rules before writing code for any task involving source files.

```
security-rules/
├── _core/         OWASP Top 10, agent security, AI/ML security, MCP, RAG
├── languages/     Go, TypeScript, JavaScript, Python, C/C++, Rust, Java, C#, SQL, Ruby, Julia, R
├── frontend/      React, Next.js, Angular, Vue, Svelte
├── backend/       FastAPI, Django, Flask, Express, NestJS, LangChain, AutoGen, and more
├── containers/    Docker, Kubernetes, general container security
├── iac/           Terraform, Pulumi, general IaC security
├── cicd/          GitHub Actions, GitLab CI, general CI/CD security
└── rag/           Document processing, embeddings, vector stores, retrieval, graph databases
```

`INDEX.md` maps task types to the right rule files so agents load only what is relevant.

## Installation

`safe-install` deploys `CLAUDE.md`, all skills, `INDEX.md`, and `security-rules/` to `~/.claude/`. It creates timestamped backups of any existing configuration and does not touch operational data (credentials, history, settings, cache).

### Global install (default)

```bash
./safe-install
```

Installs to `~/.claude/` so every Claude Code session inherits the configuration.

### Project-level install

```bash
./safe-install /path/to/project
```

Installs `CLAUDE.md` and skills into the project directory. `INDEX.md` and `security-rules/` are always installed globally.

## Multi-Agent Support

Skills are written to work with both Claude Code and Pi coding agent. Where a skill uses subagent dispatch (Claude Code's Agent tool), it includes a sequential self-review fallback for single-session agents like Pi. See [DESIGN.md](DESIGN.md) for the full compatibility story.

## Design Principles

**Minimal baseline context.** `CLAUDE.md` is ~150 lines. Domain knowledge lives in skill files loaded on demand. Every session starts lean.

**Specs over code.** Requirements and design documents are the primary artifacts. Code is ephemeral; specs are authoritative. When they disagree, fix the code.

**Evidence over intuition.** Debugging requires a reproducible reproduction before any fix is accepted. No fix is complete without a test that would have caught the failure.

**Fresh context for quality.** Review tasks are performed with clean context — either via subagents (Claude Code) or explicit self-review passes using structured checklists (Pi and other single-session agents).

**Safe by default.** No secrets in commits, parameterized queries, input validation at boundaries, HTTPS for external calls. Tests are never skipped or stubbed — only a human may do that.
