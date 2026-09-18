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
