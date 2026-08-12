---
inclusion: always
---

# AI-DLC Framework — Steering Document

## Language Convention

**English is the standard language for all development artifacts in this project** — including code, prompts, documentation, commit messages, PR descriptions, issues, and this steering document. Use English consistently across all contributions.

## Vision

This project is an **open-source, distributable** implementation of the AI-DLC (AI-Driven Development Life Cycle) methodology — a structured pipeline of specialized AI agents that guide the complete software development lifecycle, from architectural conception to production-ready code delivery.

The goal is to deliver a framework that any developer can copy into their project and immediately have access to a complete software engineering pipeline with specialized agents, human approval gates, and enterprise quality.

## Core Principles

1. **Multi-harness** — The core is harness-neutral. The same methodology runs on Kiro CLI, Kiro IDE, Claude Code, Codex CLI, Cursor, opencode, GitHub Copilot, or any other capable harness. We start with Kiro.
2. **Distributable** — A simple `cp -R` or install command brings the framework into any project. No heavy runtime dependencies, no mandatory build steps for the consumer.
3. **Open-source** — Permissive license. Anyone can use, adapt, and contribute.
4. **Human-in-the-loop** — Every significant transition requires human approval. Agents propose, humans decide.
5. **Sequential pipeline with specialists** — Each agent has a clear domain. The flow respects the chain: Solutions Architect → Tech Lead → [DevOps/Security/QA Specialists] → Senior SDE.
6. **Portable** — Relative paths, no hardcoding of directories, project names, or environment-specific configurations.

## Agent Pipeline

```
Solutions Architect → Tech Lead → [DevOps | Security | QA Specialists] → Senior SDE
```

| Agent | Responsibility |
|-------|---------------|
| **Solutions Architect** | AWS-native solution design, Mermaid diagrams, Well-Architected review, feature breakdown |
| **Tech Lead** | Feature decomposition into implementable user stories with full technical design |
| **DevOps Specialist** | User story review: observability, resilience, performance, operational safety |
| **Security Specialist** | User story review: authentication, validation, data protection, LGPD, hardening |
| **QA Specialist** | User story review: test pyramid, edge cases, coverage, boundary conditions |
| **Senior SDE** | Autonomous end-to-end implementation: code, tests, infrastructure, PR |

## Repository Architecture

```
aidlc/
├── .kiro/
│   ├── steering/          # DEV-ONLY: always-on context for maintainers
│   │   └── project.md
│   ├── agents/            # DIST: portable JSON agent definitions
│   │   └── *.json
│   └── settings/          # DIST: Kiro configuration
│       └── cli.json
├── agents/                # DIST: agent prompt files (source of truth)
│   └── *.md
├── LICENSE
└── README.md
```

**DEV-ONLY** = used by maintainers when working on the framework. Not shipped to consumers.
**DIST** = distributable. The consumer copies this to their project.

## Distribution Model

The consumer copies only the DIST directories:

```bash
cp -R aidlc/.kiro/agents/   <your-project>/.kiro/agents/
cp -R aidlc/.kiro/settings/ <your-project>/.kiro/settings/
cp -R aidlc/agents/         <your-project>/agents/
```

JSONs use relative paths (`file://agents/tech-lead.md`) resolved from the project root. The `allowedPaths` and `resources` are generic — consumers can customize for their context.

## Steering for Development

The `.kiro/steering/project.md` is automatically read by Kiro when opening a session **within the repo**. It is NOT distributed. For it to work in a parent workspace (e.g., `aidlc-framework/`), we maintain a copy at `aidlc-framework/.kiro/steering/project.md`.

## Relationship with aidlc-workflows

[awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) is a complete and mature AI-DLC implementation with 14 agents, 32 stages, a state machine, and a TypeScript engine. We use it as **reference and inspiration** for:

- Agent JSON format (schema, tools, hooks)
- Multi-harness distribution patterns (manifest + dist)
- Knowledge, sensors, and memory layer concepts
- Adaptive scope model

However, our project is an **independent implementation** focused on:
- Simpler and more direct pipeline (6 agents vs 14)
- No bun/runtime dependency for the consumer
- Standalone prompts (each .md is self-contained)
- Initial focus on the Solutions Architect → Tech Lead → Specialists → SDE flow

## Next Steps (Roadmap)

1. ✅ Base structure with 6 agents for Kiro CLI
2. Install script/command (#4)
3. Doctor command (#2)
4. Agent conductor/orchestrator
5. Claude Code harness support
6. Per-agent knowledge base
7. Quality gate hooks (sensors)
8. Expand to Cursor, Codex, opencode
9. Contributing documentation
10. CI/CD for drift validation

## Development Conventions

- **English** is the standard language for all artifacts (code, docs, prompts, commits, issues)
- JSONs follow the Kiro [agent-v1 schema](https://raw.githubusercontent.com/aws/amazon-q-developer-cli/refs/heads/main/schemas/agent-v1.json)
- Paths are always relative to the consumer project root
- Each agent is self-contained: JSON + MD = everything it needs
- Project outputs (user stories, designs) are never committed — only the framework itself
