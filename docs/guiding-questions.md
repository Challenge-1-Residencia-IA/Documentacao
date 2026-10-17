# Guiding Questions

O projeto partiu de uma pergunta essencial e de 25 perguntas-guia definidas pelo time na primeira semana. As perguntas estão reproduzidas como foram escritas. Cada resposta resume o que a Sophia faz hoje e aponta a página que aprofunda o assunto. Quando algo ainda não foi feito, a resposta diz isso.

**Pergunta essencial:** Como um sistema de IA pode ajudar as pessoas a avaliar a confiabilidade de informações sem substituir seu pensamento crítico?

!!! info "Canal"
    As perguntas 18 a 22 foram escritas quando o canal previsto era o Telegram. O produto final usa o WhatsApp, onde a maior parte das correntes circula.

## Desinformação

**1. Quais são os principais tipos de desinformação presentes nas plataformas digitais?**
Boatos de saúde, golpes, conteúdo político e eleitoral, alarmes de pânico e conteúdo fora de contexto. São os temas que aparecem nos datasets e no golden set. → [Datasets e EDA](datasets-e-eda.md)

**2. Quais tipos de desinformação oferecem maior risco aos usuários e à sociedade?**
Saúde, eleições, golpes financeiros e segurança. Nesses temas, a Sophia acrescenta um aviso à resposta. → [Produto final](produto-final.md)

**3. Como diferenciar informação falsa, desatualizada, opinativa, satírica e fora de contexto?**
A etapa de extração classifica cada afirmação como fato verificável, opinião, previsão, sátira ou fora de contexto. Opiniões não são pesquisadas, e toda resposta mostra a data da checagem mais recente. → [Fluxo e interação](fluxo-e-interacao.md)

**4. Quais características indicam que uma informação merece ser investigada?**
Urgência, apelo emocional, pedido de compartilhamento e frases como "antes que apaguem". A Sophia aponta essas técnicas na resposta. → [Datasets e EDA](datasets-e-eda.md)

## IA e avaliação da informação

**5. Como a IA pode avaliar a confiabilidade de uma informação sem determinar se ela é "verdadeira" ou "falsa"?**
Mostrando checagens publicadas e trechos literais das fontes. Um guardrail de saída remove qualquer frase com veredito. → [Princípios do sistema](principios.md)

**6. Como o sistema pode identificar quais afirmações de uma mensagem precisam ser verificadas?**
Uma etapa com o LLM extrai da mensagem as afirmações verificáveis. Se não houver nenhuma, a Sophia não faz a busca e pede uma informação que possa ser checada. → [Arquitetura](arquitetura.md)

**7. Como apresentar evidências, fontes e perspectivas divergentes de forma compreensível?**
A resposta traz até 5 fontes, com as agências de checagem primeiro. Cada uma tem veículo, data, link e um trecho citado. → [Produto final](produto-final.md)

**8. Como diferenciar ausência de evidências de evidências de falsidade?**
Quando não encontra fontes, a Sophia diz que não encontrou nada nesta busca e orienta a verificação. Ela não conclui que a informação é falsa.

**9. Como lidar com informações que possuem diferentes interpretações entre fontes confiáveis?**
As fontes aparecem lado a lado, cada uma com o seu trecho. A detecção automática de divergências entre fontes ainda não foi implementada.

**10. Como verificar informações que dependem de contexto temporal ou eventos recentes?**
Quando a checagem mais recente da base tem mais de 6 meses, ou quando não há checagem, a Sophia também busca na internet e avisa a data. → [Arquitetura](arquitetura.md)

**11. Como analisar diferentes formatos, como textos, imagens e vídeos?**
Hoje a Sophia analisa só texto e links. Imagens e áudios recebem um aviso, e a análise de mídia ficou fora do MVP.

**12. Quais tecnologias e fontes de dados podem apoiar esse processo?**
Qwen 2.5 7B (via Ollama), LangGraph, embeddings E5, Postgres com pgvector (Neon) para a busca híbrida, RSS das agências de checagem, busca na web e NeMo Guardrails. Os dados vêm do FakeTrue.Br, do Fake.br-Corpus e das agências. → [Arquitetura](arquitetura.md), [Datasets e EDA](datasets-e-eda.md)

## Transparência e confiabilidade da IA

**13. Como a IA pode explicar os critérios e fontes utilizados em sua análise?**
Cada fonte vem com veículo, data, link e trecho literal, conferido contra a página original. → [Relatório técnico da solução de IA](relatorio-tecnico-modelo.md)

**14. Como comunicar incertezas, limitações, vieses e possíveis erros da própria IA?**
Com avisos na resposta: data da checagem, nenhuma fonte encontrada, dados pessoais ocultados e tema de risco. As limitações conhecidas estão documentadas. → [Produto final](produto-final.md), [Riscos](riscos.md)

**15. Como o sistema deve agir quando não houver evidências suficientes?**
Dizer que não encontrou e sugerir como verificar (leitura lateral), sem inventar fontes. Citações que não existem na página são descartadas.

**16. Como permitir que o usuário conteste a análise e apresente novas evidências?**
Ainda não implementado. A pessoa pode enviar um link ou uma fonte nova como outra mensagem, mas a Sophia ainda não guarda o histórico da conversa.

**17. Como evitar que o usuário trate a IA como autoridade absoluta?**
A Sophia não dá veredito, mostra de onde veio cada informação e devolve a decisão à pessoa. → [Princípios do sistema](principios.md)

## Bot e experiência do usuário

**18. Como projetar uma interação no Telegram que incentive a investigação em vez de apenas fornecer respostas?**
No WhatsApp, a resposta combina as técnicas de manipulação encontradas, as fontes para abrir e o convite para a pessoa tirar a própria conclusão. → [Fluxo e interação](fluxo-e-interacao.md)

**19. Como o bot pode incentivar o usuário a refletir antes de compartilhar uma informação?**
Apontando o pedido de compartilhamento e a urgência como técnicas de manipulação.

**20. Como lidar com mensagens encaminhadas e identificar sua possível origem?**
A base guarda boatos já conhecidos. Quando uma corrente bate com um deles, a checagem desse boato aparece primeiro. A origem exata da mensagem não é rastreada.

**21. Como identificar quando diferentes mensagens são apenas reproduções de uma mesma fonte?**
Em parte: versões diferentes de um mesmo boato encontram a mesma checagem pela busca por similaridade. A análise de independência entre fontes ainda não foi implementada.

**22. Como apresentar fontes primárias e diferentes perspectivas de forma acessível?**
Com trechos curtos e links diretos, priorizando as agências de checagem, que citam as fontes primárias. → [Fontes e evidências](fontes-e-evidencias.md)

## Pensamento crítico e avaliação

**23. Como evitar que o uso do bot gere dependência e reduza o pensamento crítico?**
A Sophia não entrega conclusões prontas e mostra o caminho da verificação. O efeito disso ainda não foi medido com usuários.

**24. Como avaliar se o uso do sistema realmente melhora a capacidade do usuário de identificar desinformação?**
Com um experimento antes e depois do uso, previsto no planejamento. Ainda não foi realizado. → [Métricas e avaliação](metricas-e-avaliacao.md)

**25. Como medir tanto a precisão da IA quanto a evolução do pensamento crítico dos usuários?**
A precisão é medida com um golden set de 65 boatos reais: a checagem certa aparece entre as 5 primeiras em 89% dos casos, e nenhuma resposta avaliada trouxe veredito. A evolução do pensamento crítico ainda não foi medida. → [CI/CD e LLMOps](mlops-llmops.md), [Relatório técnico da solução de IA](relatorio-tecnico-modelo.md)
