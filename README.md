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

## Quick Start

### Pré-requisitos

- [Kiro CLI](https://kiro.dev) ≥ 2.6 instalado e autenticado (`kiro login`)

### Instale no seu projeto

```bash
# Clone este repositório
git clone https://github.com/luanbicesto/aidlc.git

# Copie para o seu projeto (apenas o que é distribuível)
cp -R aidlc/.kiro/agents/   <seu-projeto>/.kiro/agents/
cp -R aidlc/.kiro/settings/ <seu-projeto>/.kiro/settings/
cp -R aidlc/agents/         <seu-projeto>/agents/
```

> [!NOTE]
> Não copie `.kiro/steering/` — é dev-only para maintainers do framework.

### Use

```bash
cd <seu-projeto>

# Abre com o Tech Lead (default agent)
kiro chat

# Ou escolha um agente específico
kiro chat --agent solutions-architect
kiro chat --agent senior-sde
kiro chat --agent qa-specialist
kiro chat --agent devops-specialist
kiro chat --agent security-specialist
```

### Verifique a instalação

Após copiar, confirme que a estrutura está completa:

```
<seu-projeto>/
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

O framework é desenhado para ser harness-neutral. Hoje suporta:

| Harness | Status | Estrutura |
|---------|--------|-----------|
| **Kiro CLI** | ✅ Disponível | `.kiro/agents/` + `agents/` |
| **Kiro IDE** | ✅ Disponível | Mesma estrutura do CLI |
| **Claude Code** | 🔜 Planejado | `.claude/` + `CLAUDE.md` |
| **Cursor** | 🔜 Planejado | `.cursor/rules/` |
| **Codex CLI** | 🔜 Planejado | `.codex/` |
| **opencode** | 🔜 Planejado | `.opencode/` |
| **GitHub Copilot** | 🔜 Planejado | `.github/copilot-instructions.md` |

## Princípios

1. **Human-in-the-loop** — Cada transição requer aprovação humana
2. **Distribuível** — Copie e funciona. Sem build steps para o consumidor
3. **Portável** — Paths relativos, zero hardcoding
4. **Self-contained** — Cada agente é JSON + MD, sem dependências externas
5. **Open-source** — Licença permissiva (MIT-0)

## Estrutura do Repositório

```
aidlc/
├── .kiro/
│   ├── steering/              # DEV-ONLY: contexto para maintainers
│   │   └── project.md
│   ├── agents/                # DIST: definições JSON dos agentes
│   │   └── *.json
│   └── settings/              # DIST: configuração do Kiro
│       └── cli.json
├── agents/                    # DIST: prompts completos das personas
│   └── *.md
├── LICENSE
└── README.md
```

- **DIST** = distribuível, o consumidor copia para o projeto dele
- **DEV-ONLY** = usado apenas por quem desenvolve o framework

## Contributing

### Setup do ambiente de desenvolvimento

```bash
git clone git@github.com:luanbicesto/aidlc.git
cd aidlc
kiro chat   # o steering document carrega automaticamente o contexto do projeto
```

O `.kiro/steering/project.md` define a visão, princípios e arquitetura — ele é injetado automaticamente em toda sessão do Kiro quando você trabalha neste repositório.

### Workflow

1. Crie uma branch a partir de `main`
2. Faça suas alterações
3. Abra um PR para review
4. Use GitHub Issues para propor ideias ou reportar bugs

### O que contribuir

- **Novos harnesses** — adaptar os agents para Claude Code, Cursor, Codex, etc.
- **Melhorias nos prompts** — refinar as instruções dos agentes em `agents/*.md`
- **Knowledge base** — adicionar documentos de referência por agente
- **Hooks e sensors** — quality gates e validações automáticas
- **Doctor command** — validação de instalação (#2)
- **Documentação** — exemplos, guias, troubleshooting

### Convenções

- Prompts de agente em **inglês** (melhor performance com LLMs)
- Docs e comunicação em **português ou inglês**
- JSONs seguem o [schema agent-v1](https://raw.githubusercontent.com/aws/amazon-q-developer-cli/refs/heads/main/schemas/agent-v1.json) do Kiro
- Paths sempre **relativos** à raiz do projeto consumidor
- Nunca commitar outputs de projeto (user stories, designs) — apenas o framework

### Backlog

Veja a [Issue #3](https://github.com/luanbicesto/aidlc/issues/3) para o roadmap completo e ideias em aberto.

## Inspiração

Este projeto se inspira no [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) — a implementação completa do AI-DLC com 14 agentes e 32 estágios — mas com foco em simplicidade e distribuibilidade sem dependências de runtime.

Para saber mais sobre a metodologia AI-DLC, leia o [blog post da AWS](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/).

## Licença

MIT-0
