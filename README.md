# AI-DLC — Pipeline de Agentes para Desenvolvimento de Software

Uma implementação open-source e distribuível da metodologia **AI-DLC** (AI-Driven Development Life Cycle) — um pipeline estruturado de agentes de IA especializados que guiam o ciclo de vida completo de desenvolvimento de software.

## O que é

Um framework que você copia para o seu projeto e imediatamente tem acesso a 6 agentes especializados trabalhando em pipeline:

```
Solutions Architect → Tech Lead → [DevOps | Security | QA] → Senior SDE
```

Cada agente tem um domínio claro e o fluxo respeita a cadeia de responsabilidades — desde o design da solução até a implementação autônoma de código.

## Agentes

| Agente | Role |
|--------|------|
| **Solutions Architect** | Design de solução, diagramas, Well-Architected review, breakdown em features |
| **Tech Lead** | Decomposição de features em user stories implementáveis |
| **DevOps Specialist** | Review: observabilidade, resiliência, performance |
| **Security Specialist** | Review: autenticação, validação, proteção de dados, LGPD |
| **QA Specialist** | Review: pirâmide de testes, edge cases, cobertura |
| **Senior SDE** | Implementação autônoma: código, testes, infra, PR |

## Quick Start (Kiro CLI)

```bash
# Clone
git clone https://github.com/<org>/aidlc.git

# Copie para o seu projeto
cp -R aidlc/.kiro/ <seu-projeto>/.kiro/
cp -R aidlc/agents/ <seu-projeto>/agents/

# Use
cd <seu-projeto>
kiro chat  # abre com o Tech Lead por padrão
```

Para usar um agente específico:
```bash
kiro chat --agent solutions-architect
kiro chat --agent senior-sde
```

## Estrutura

```
.kiro/
├── agents/               # Definições JSON dos agentes (Kiro schema)
│   ├── solutions-architect.json
│   ├── tech-lead.json
│   ├── devops-specialist.json
│   ├── security-specialist.json
│   ├── qa-specialist.json
│   └── senior-sde.json
├── settings/
│   └── cli.json          # Default agent + model config
└── steering/
    └── project.md        # Contexto always-on do projeto

agents/                   # Prompts completos (source of truth)
├── solutions-architect.md
├── tech-lead.md
├── devops-specialist.md
├── security-specialist.md
├── qa-specialist.md
└── senior-sde.md
```

## Multi-Harness (Roadmap)

O framework é desenhado para ser harness-neutral. Hoje suporta:

- ✅ **Kiro CLI** — `.kiro/agents/` + agent JSONs
- 🔜 **Claude Code** — `.claude/` + CLAUDE.md
- 🔜 **Cursor** — `.cursor/rules/`
- 🔜 **Codex CLI** — `.codex/`
- 🔜 **opencode** — `.opencode/`
- 🔜 **GitHub Copilot** — `.github/copilot-instructions.md`

## Princípios

1. **Human-in-the-loop** — Cada transição requer aprovação humana
2. **Distribuível** — `cp -R` e funciona. Sem build steps para o consumidor
3. **Portável** — Paths relativos, zero hardcoding
4. **Self-contained** — Cada agente é JSON + MD, sem dependências externas
5. **Open-source** — Licença permissiva (MIT-0)

## Inspiração

Este projeto se inspira no [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) — a implementação completa do AI-DLC com 14 agentes e 32 estágios — mas com foco em simplicidade e distribuibilidade sem dependências de runtime.

## Licença

MIT-0
