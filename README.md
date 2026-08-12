# AI-DLC — Agent Pipeline for Software Development

An open-source, distributable implementation of the **AI-DLC** (AI-Driven Development Life Cycle) methodology — a structured pipeline of specialized AI agents that guide the complete software development lifecycle.

## What is it

A framework you copy into your project to immediately get access to 6 specialized agents working in pipeline:

```
Solutions Architect → Tech Lead → [DevOps | Security | QA] → Senior SDE
```

Each agent has a clear domain and the flow respects the chain of responsibilities — from solution design to autonomous code implementation.

## Agents

| Agent | Role |
|-------|------|
| **Solutions Architect** | Solution design, diagrams, Well-Architected review, feature breakdown |
| **Tech Lead** | Feature decomposition into implementable user stories |
| **DevOps Specialist** | Review: observability, resilience, performance |
| **Security Specialist** | Review: authentication, validation, data protection, LGPD |
| **QA Specialist** | Review: test pyramid, edge cases, coverage |
| **Senior SDE** | Autonomous implementation: code, tests, infrastructure, PR |

## Quick Start

### Prerequisites

- [Kiro CLI](https://kiro.dev) ≥ 2.6 installed and authenticated (`kiro login`)
- Recommended model: Claude Opus 4.6 or higher

### Install into your project

```bash
# Clone this repository
git clone https://github.com/luanbicesto/aidlc.git

# Copy only the distributable files to your project
cp -R aidlc/.kiro/agents/   <your-project>/.kiro/agents/
cp -R aidlc/.kiro/settings/ <your-project>/.kiro/settings/
cp -R aidlc/agents/         <your-project>/agents/
```

> [!NOTE]
> Do not copy `.kiro/steering/` — it is dev-only for framework maintainers.

### Usage

```bash
cd <your-project>

# Opens with Tech Lead (default agent)
kiro chat

# Or choose a specific agent
kiro chat --agent solutions-architect
kiro chat --agent senior-sde
kiro chat --agent qa-specialist
kiro chat --agent devops-specialist
kiro chat --agent security-specialist
```

### Verify installation

After copying, confirm the structure is complete:

```
<your-project>/
├── .kiro/
│   ├── agents/
│   │   ├── solutions-architect.json
│   │   ├── tech-lead.json
│   │   ├── devops-specialist.json
│   │   ├── security-specialist.json
│   │   ├── qa-specialist.json
│   │   └── senior-sde.json
│   └── settings/
│       └── cli.json
└── agents/
    ├── solutions-architect.md
    ├── tech-lead.md
    ├── devops-specialist.md
    ├── security-specialist.md
    ├── qa-specialist.md
    └── senior-sde.md
```

## Multi-Harness (Roadmap)

The framework is designed to be harness-neutral. Current support:

| Harness | Status | Structure |
|---------|--------|-----------|
| **Kiro CLI** | ✅ Available | `.kiro/agents/` + `agents/` |
| **Kiro IDE** | ✅ Available | Same structure as CLI |
| **Claude Code** | 🔜 Planned | `.claude/` + `CLAUDE.md` |
| **Cursor** | 🔜 Planned | `.cursor/rules/` |
| **Codex CLI** | 🔜 Planned | `.codex/` |
| **opencode** | 🔜 Planned | `.opencode/` |
| **GitHub Copilot** | 🔜 Planned | `.github/copilot-instructions.md` |

## Principles

1. **Human-in-the-loop** — Every transition requires human approval
2. **Distributable** — Copy and it works. No build steps for the consumer
3. **Portable** — Relative paths, zero hardcoding
4. **Self-contained** — Each agent is JSON + MD, no external dependencies
5. **Open-source** — Permissive license (MIT-0)

## Repository Structure

```
aidlc/
├── .kiro/
│   ├── steering/              # DEV-ONLY: context for maintainers
│   │   └── project.md
│   ├── agents/                # DIST: JSON agent definitions
│   │   └── *.json
│   └── settings/              # DIST: Kiro configuration
│       └── cli.json
├── agents/                    # DIST: full agent prompts
│   └── *.md
├── LICENSE
└── README.md
```

- **DIST** = distributable, the consumer copies to their project
- **DEV-ONLY** = used only by framework developers

## Contributing

### Development environment setup

```bash
git clone git@github.com:luanbicesto/aidlc.git
cd aidlc
kiro chat   # steering document automatically loads project context
```

The `.kiro/steering/project.md` defines the vision, principles, and architecture — it is automatically injected into every Kiro session when working in this repository.

### Workflow

1. Create a branch from `main`
2. Make your changes
3. Open a PR for review
4. Use GitHub Issues to propose ideas or report bugs

### What to contribute

- **New harnesses** — adapt agents for Claude Code, Cursor, Codex, etc.
- **Prompt improvements** — refine agent instructions in `agents/*.md`
- **Knowledge base** — add reference documents per agent
- **Hooks and sensors** — quality gates and automatic validations
- **Doctor command** — installation validation (#2)
- **Install script** — automated distribution (#4)
- **Documentation** — examples, guides, troubleshooting

### Conventions

- **English** is the standard language for all artifacts (code, docs, prompts, commits, issues)
- JSONs follow the Kiro [agent-v1 schema](https://raw.githubusercontent.com/aws/amazon-q-developer-cli/refs/heads/main/schemas/agent-v1.json)
- Paths are always **relative** to the consumer project root
- Each agent is self-contained: JSON + MD = everything it needs
- Never commit project outputs (user stories, designs) — only the framework

### Backlog

See [Issue #3](https://github.com/luanbicesto/aidlc/issues/3) for the full roadmap and open ideas.

## Inspiration

This project is inspired by [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) — the full AI-DLC implementation with 14 agents and 32 stages — but focused on simplicity and distributable without runtime dependencies.

To learn more about the AI-DLC methodology, read the [AWS blog post](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/).

## License

MIT-0
