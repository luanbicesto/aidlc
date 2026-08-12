---
inclusion: always
---

# AI-DLC Framework — Steering Document

## Visão

Este projeto é uma implementação **open-source e distribuível** da metodologia AI-DLC (AI-Driven Development Life Cycle) — um pipeline estruturado de agentes de IA especializados que guiam o ciclo de vida completo de desenvolvimento de software, desde a concepção arquitetural até a entrega de código produtivo.

O objetivo é entregar um framework que qualquer desenvolvedor possa copiar para o seu projeto e imediatamente ter acesso a um pipeline completo de engenharia de software com agentes especializados, gates de aprovação humana, e qualidade enterprise.

## Princípios Fundamentais

1. **Multi-harness** — O core é harness-neutral. A mesma metodologia roda em Kiro CLI, Kiro IDE, Claude Code, Codex CLI, Cursor, opencode, GitHub Copilot, ou qualquer outro harness capaz. Hoje começamos pelo Kiro.
2. **Distribuível** — Um `cp -R` ou instalação simples leva o framework para qualquer projeto. Sem dependências de runtime pesadas, sem build steps obrigatórios para o consumidor.
3. **Open-source** — Licença permissiva. Qualquer pessoa pode usar, adaptar, e contribuir.
4. **Human-in-the-loop** — Cada transição significativa requer aprovação humana. Os agentes propõem, o humano decide.
5. **Pipeline sequencial com especialistas** — Cada agente tem domínio claro. O fluxo respeita a cadeia: Solutions Architect → Tech Lead → [DevOps/Security/QA Specialists] → Senior SDE.
6. **Portável** — Paths relativos, sem hardcoding de diretórios, nomes de projetos, ou configurações específicas.

## Pipeline de Agentes

```
Solutions Architect → Tech Lead → [DevOps | Security | QA Specialists] → Senior SDE
```

| Agente | Responsabilidade |
|--------|-----------------|
| **Solutions Architect** | Design de solução AWS-native, diagramas Mermaid, Well-Architected review, breakdown em features |
| **Tech Lead** | Decomposição de features em user stories implementáveis com design técnico completo |
| **DevOps Specialist** | Review de user stories: observabilidade, resiliência, performance, operational safety |
| **Security Specialist** | Review de user stories: autenticação, validação, proteção de dados, LGPD, hardening |
| **QA Specialist** | Review de user stories: pirâmide de testes, edge cases, cobertura, boundary conditions |
| **Senior SDE** | Implementação autônoma end-to-end: código, testes, infra, PR |

## Arquitetura do Repositório

```
aidlc/
├── .kiro/
│   ├── steering/          # DEV-ONLY: contexto always-on para maintainers
│   │   └── project.md
│   ├── agents/            # DIST: JSONs declarativos dos agentes (portáveis)
│   │   └── *.json
│   └── settings/          # DIST: configuração do Kiro
│       └── cli.json
├── agents/                # DIST: prompts .md (source of truth das personas)
│   └── *.md
├── LICENSE
└── README.md
```

**DEV-ONLY** = usado por maintainers ao trabalhar no framework. Não vai para o projeto do consumidor.
**DIST** = distribuível. O consumidor copia para o projeto dele.

## Modelo de Distribuição

O consumidor copia apenas as pastas DIST:

```bash
cp -R aidlc/.kiro/agents/   <seu-projeto>/.kiro/agents/
cp -R aidlc/.kiro/settings/ <seu-projeto>/.kiro/settings/
cp -R aidlc/agents/         <seu-projeto>/agents/
```

Os JSONs usam paths relativos (`file://agents/tech-lead.md`) resolvidos a partir da raiz do projeto. O `allowedPaths` e `resources` são genéricos — o consumidor pode customizar para o seu contexto.

## Steering para Desenvolvimento

O `.kiro/steering/project.md` é lido automaticamente pelo Kiro quando se abre uma sessão **dentro do repo**. Ele NÃO é distribuído. Para que funcione no workspace pai (ex: `aidlc-framework/`), mantemos uma cópia em `aidlc-framework/.kiro/steering/project.md`.

## Relação com aidlc-workflows

O [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) é uma implementação completa e madura do AI-DLC com 14 agentes, 32 estágios, máquina de estados, e engine TypeScript. Usamos como **referência e inspiração** para:

- Formato dos agent JSONs (schema, tools, hooks)
- Padrões de distribuição multi-harness (manifest + dist)
- Conceitos de knowledge, sensors, e memory layers
- Modelo de escopo adaptativo

Porém, nosso projeto é uma **implementação independente** com foco em:
- Pipeline mais simples e direto (6 agentes vs 14)
- Sem dependência de bun/runtime para o consumidor
- Prompts standalone (cada .md é self-contained)
- Foco inicial no fluxo Solutions Architect → Tech Lead → Specialists → SDE

## Próximos Passos (Roadmap)

1. ✅ Estrutura base com 6 agentes para Kiro CLI
2. Adicionar agent conductor/orchestrator
3. Adicionar suporte ao harness Claude Code
4. Implementar knowledge base por agente
5. Adicionar hooks de quality gate (sensors)
6. Expandir para Cursor, Codex, opencode
7. Documentação de contribuição
8. CI/CD para validação de drift

## Convenções de Desenvolvimento

- Prompts em português e inglês (prompts de agente em inglês por padronização com LLMs)
- JSONs seguem o schema `agent-v1.json` do Kiro
- Paths sempre relativos à raiz do projeto consumidor
- Cada agente é auto-contido: JSON + MD = tudo que ele precisa
- Não commitamos outputs de projeto (user stories, designs) — só o framework
