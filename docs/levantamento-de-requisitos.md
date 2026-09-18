# Levantamento de requisitos

Esta página documenta **como** os requisitos, princípios e casos de uso descritos nas demais páginas foram levantados — algo que, até esta versão da documentação, existia apenas de forma implícita, espalhado em notas soltas ao longo de outras páginas.

## Técnicas já utilizadas

### Revisão bibliográfica e análise de soluções existentes

A Fase 1 do roadmap (ver [Roadmap e cronograma](roadmap.md)) previu explicitamente um levantamento bibliográfico sobre desinformação, alfabetização midiática, fact-checking e IA/RAG, além do estudo de soluções já existentes no mercado (por exemplo, o Sift, citado como referência de pipeline em [Roadmap e cronograma](roadmap.md)). Os artigos e fontes consultados estão listados em [Referências](referencias.md).

**Uso principal**: fundamentar o problema (ver [Visão geral](visao-geral.md)) e validar que a abordagem de apresentar evidências em vez de um veredito binário tem respaldo em pesquisa (ver, por exemplo, o artigo sobre o impacto de LLMs em práticas de fact-checking dos usuários, em [Referências](referencias.md)).

### Brainstorming interno da equipe

Parte do desenho do sistema — incluindo a ideia inicial de consultar bases de fact-checking antes da busca geral (ver nota em [Pipeline de IA](pipeline-ia.md)) — veio de sessões de brainstorming interno da equipe, sem participação de usuários externos.

!!! warning "Limitação"
    Nenhuma das técnicas usadas até aqui envolveu diretamente usuários externos ao time. Os requisitos funcionais e não funcionais (ver [Requisitos](requisitos.md)) e os três perfis de público-alvo (ver [Público-alvo](publico-alvo.md)) são, portanto, hipóteses fundamentadas em literatura e no conhecimento da equipe — ainda não validadas com o público real do sistema.

## Técnicas recomendadas para complementar

Dado o prazo do MVP (8 semanas, ver [Roadmap e cronograma](roadmap.md)) e o risco de aumento de escopo já identificado (ver [Riscos](riscos.md)), a recomendação é complementar o que já foi feito com técnicas leves, em vez de pesquisas extensas:

| Técnica | Vantagem | Trade-off |
|---|---|---|
| Survey/questionário curto, direcionado aos 3 perfis de [Público-alvo](publico-alvo.md) | Rápido de aplicar (ex.: em grupos de Telegram/WhatsApp reais), quantifica hipóteses | Respostas superficiais, não captura o "porquê" |
| Poucas entrevistas semi-estruturadas (3-5 pessoas, uma por perfil) | Profundidade, descobre necessidades não previstas pela equipe | Baixa generalização, consome tempo por pessoa |
| Protótipo de baixa fidelidade do fluxo de conversa (mockup do Telegram, sem IA real por trás) | Barato, valida o fluxo conversacional antes de codificar o pipeline inteiro; já é compatível com a Fase 3 (Protótipo) do roadmap | Ainda exige construir algo, mesmo que simulado |
| Observação contextual de uso real | Dado comportamental real, não relatado de memória | Caro, demorado, levanta questão ética/privacidade — provavelmente inviável no prazo do MVP |

Técnicas caras (observação contextual, pesquisa extensa) não são recomendadas para esta fase — o objetivo é validar o mínimo necessário das hipóteses de público-alvo e requisitos sem comprometer o cronograma de 8 semanas.

## Próximos passos

- Aplicar o survey curto antes ou durante a Fase 3 (Protótipo) do roadmap, para confirmar ou revisar os três perfis de [Público-alvo](publico-alvo.md).
- Registrar, à medida que forem conduzidas, quais entrevistas/protótipos validaram (ou invalidaram) quais requisitos específicos de [Requisitos](requisitos.md).
