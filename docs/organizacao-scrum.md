# Organização e Scrum

## Como o time trabalhou

- **Sprints** curtas, com entregas registradas neste site (relatórios da [Sprint 1](Sprint1/relatorio-avaliacao-modelo.md) e da [Sprint 2](Sprint2/Comparacao-Modelos.md)).
- **Backlog no GitHub Project** ["Backlog do MVP"](https://github.com/orgs/Challenge-1-Residencia-IA/projects/1): as histórias de usuário (US01–US17) e as histórias habilitadoras (EN01–EN13) são issues no repositório `bot-core`, com os critérios de aceite como checklist.
- **Quadro Kanban** com as etapas Backlog → A fazer → Em andamento → Em revisão → Concluído.
- **Requisitos → histórias → issues:** os 38 requisitos de [Requisitos](requisitos.md) foram transformados nas [Histórias de usuário](historias-de-usuario.md), que viraram itens do backlog.

## Definição de pronto

Uma mudança no produto só está pronta quando:

1. entrou por **pull request** (a `main` do `bot-core` é protegida, inclusive para administradores);
2. o **CI está verde** (lint e testes automatizados com banco de dados);
3. passou pelo **portão de qualidade** do deploy (golden set) e pelo teste de fumaça em produção;
4. a documentação correspondente foi atualizada.

## Papéis

| Pessoa | Papel | Principais entregas |
|---|---|---|
| Felipe Rodrigues | Backend, infraestrutura e MLOps | Núcleo do `bot-core` (LangGraph, RAG híbrido, guardrails), base de checagens, deploy na VM, CI/CD com portão champion/challenger, EDA, golden set e avaliação |
| Filipe Lacerda | RAG, dados e rastreamento de experimentos | Protótipo da tutora com RAG, versionamento de dados (DVC/DagsHub), MLflow, coletor RSS de checagens |
| Larissa Giffoni | IA e prompts, documentação | Comparação de modelos (Qwen × Llama), prompts de análise, integração com banco vetorial, documentação das sprints |
| Ranni Heler | Conector e testes | Conector de WhatsApp (scanner, fila, workers, dispatcher), elicitação de requisitos, casos de teste |

## Repositórios

| Repositório | Uso |
|---|---|
| [bot-core](https://github.com/Challenge-1-Residencia-IA/bot-core) | Aplicação em produção e backlog (issues) |
| [experimentos](https://github.com/Challenge-1-Residencia-IA/experimentos) | Dados, EDA, tutora RAG e avaliação |
| [Documentacao](https://github.com/Challenge-1-Residencia-IA/Documentacao) | Este site |
| [bot-test](https://github.com/Challenge-1-Residencia-IA/bot-test) | Protótipo do conector de WhatsApp, incorporado ao `bot-core` |

O planejamento original de fases e semanas está em [Roadmap e cronograma](roadmap.md).
