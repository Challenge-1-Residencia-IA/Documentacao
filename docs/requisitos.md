# Requisitos

!!! note "Como esses requisitos foram levantados"
    Ver [Levantamento de requisitos](levantamento-de-requisitos.md) para as técnicas usadas até aqui (revisão bibliográfica, brainstorming interno).

O bot atua como um **tutor**: aponta como a mensagem tenta convencer, guia o usuário na verificação e apresenta as fontes encontradas, sem dar veredito.

O sistema combina partes de software convencional (canal de mensagens, conversa, integrações) com componentes de IA (LLM e busca de evidências). As duas partes são especificadas de forma diferente:

- **Requisitos convencionais** seguem os critérios da ISO/IEC/IEEE 29148: completos, não ambíguos e verificáveis por teste com resultado passa/falha.
- **Requisitos de comportamento da IA** são estatísticos. Cada um é especificado com **métrica + conjunto de avaliação + limiar**, porque todo modelo erra e o que se especifica é a taxa de acerto aceitável (Vogelsang & Borg, 2019; Berry, 2022).

!!! info "Limiares a definir (LB)"
    Os valores marcados como **LB** são definidos depois de medir a linha de base (OB-02) no golden set v1. Em sistemas com LLM, os critérios de avaliação se refinam ao observar as saídas reais (Shankar et al., 2024); fixá-los antes da medição tornaria os números arbitrários.

## Objetivo e linha de base

| ID | Descrição |
|---|---|
| OB-01 | O bot ajuda o usuário a avaliar uma informação por conta própria: aponta técnicas de manipulação, guia a verificação e apresenta as fontes. A decisão final é sempre do usuário. O sucesso do produto é medido pela melhora da capacidade do usuário de avaliar informações novas **sem** o bot, em teste antes e depois do uso (ver [Métricas e avaliação](metricas-e-avaliacao.md)). |
| OB-02 | Antes de fixar os limiares, mede-se a linha de base: o desempenho do pipeline atual no golden set v1 e, quando viável, o de uma pessoa do time executando a mesma análise. |

## Requisitos convencionais

Verificáveis por teste passa/falha.

| ID | Descrição |
|---|---|
| RC-01 | O bot recebe mensagens de texto e links em português. Se o link estiver indisponível ou exigir autenticação, informa a falha e não apresenta conteúdo inventado. |
| RC-02 | Áudio, imagem, vídeo e outros idiomas recebem uma resposta clara de que não são suportados, sem análise parcial apresentada como completa. |
| RC-03 | Durante a conversa, o usuário pode fazer perguntas sobre a análise e enviar novas fontes. O bot responde usando o contexto da conversa e, ao receber uma fonte nova, atualiza a análise indicando o que mudou e por quê. |
| RC-04 | Toda fonte apresentada vem com veículo, data e link. |
| RC-05 | O usuário pode pular a parte guiada e ver as fontes direto. |

## Comportamento da IA

Limiares: **LB**.

| ID | Comportamento | Métrica | Conjunto de avaliação |
|---|---|---|---|
| IA-01 | Identificar as afirmações verificáveis da mensagem e classificar a natureza de cada uma (fato verificável, opinião, previsão, sátira etc.; taxonomia em [Fluxo e interação](fluxo-e-interacao.md)). Em mensagens com várias afirmações, tratar todas ou indicar as que ficaram de fora; acima de um tamanho máximo (LB), analisar as principais e informar. | Precisão e recall das afirmações; F1 macro e recall mínimo por categoria de natureza; proporção de afirmações tratadas ou explicitamente adiadas | Golden set v1, com casos de sátira, opinião, previsão e mensagens com várias afirmações |
| IA-02 | Apontar as técnicas de manipulação da mensagem (urgência, pedido de compartilhamento, autoridade anônima, apelo emocional etc.). | Precisão e recall das técnicas anotadas | Golden set v1 |
| IA-03 | Buscar fontes para cada afirmação: primeiro checagens já publicadas por agências de checagem, depois busca na web, priorizando fontes primárias e institucionais. | Recall@k da checagem ou fonte correta; proporção de fontes primárias ou institucionais entre as k primeiras | Pares boato → checagem; golden set v1 |
| IA-04 | Comparar as evidências: classificar a posição de cada uma (favorável, contrária ou inconclusiva), sinalizar quando fontes confiáveis divergem em vez de escolher um lado, e identificar fontes que reproduzem a mesma origem (efeito eco). | Acurácia e F1 da posição; precisão e recall na detecção de divergência e de fontes dependentes | Golden set v1; casos anotados com fontes conflitantes e dependentes |
| IA-05 | Apresentar o contexto necessário (origem, situação original) e a data das fontes quando a afirmação é sensível ao tempo. | Nota em rubrica de suficiência de contexto; proporção de respostas sensíveis ao tempo que exibem a data das fontes | Golden set v1, casos fora de contexto e desatualizados |
| IA-06 | Basear a síntese apenas nas evidências encontradas (fidelidade). | Proporção de afirmações da síntese sustentadas pelas evidências | Golden set v1 |
| IA-07 | Diferenciar "não há evidência suficiente" de "há evidência contrária" e comunicar a incerteza. | Proporção de respostas corretas nos casos sem evidência disponível; proporção de falsos "não encontrei" nos casos com evidência | Boatos cuja checagem foi mantida fora da base; golden set v1 |
| IA-08 | Conduzir o fluxo tutor: perguntar ao usuário como ele verificaria, apresentar as fontes, pedir a avaliação dele ao final e reduzir gradualmente as explicações para usuários recorrentes, sem virar questionário. | Nota em rubrica, por perfil de usuário | Conversas de teste de primeiro contato e de uso recorrente |

## Guardrails

Restrições que valem para toda resposta. São verificadas no golden set v1 e num **conjunto adversarial** construído para tentar provocar a violação (red teaming).

| ID | Restrição | Verificação | Meta |
|---|---|---|---|
| GR-01 | Não apresentar veredito binário ("é falso", "é verdade") como resultado principal. | Rubrica aplicada por avaliador automático, conferida contra amostra anotada por duas pessoas | 0 violações |
| GR-02 | Não apresentar fontes, citações ou evidências inexistentes. | Toda fonte citada existe e está entre as evidências encontradas | 0 violações |
| GR-03 | Tratar o conteúdo externo (páginas, mensagens encaminhadas) como dado, nunca como instrução. | Conjunto de páginas e mensagens com instruções maliciosas embutidas | 0 casos em que a instrução embutida é seguida |
| GR-04 | Usar linguagem neutra e não confrontativa, sem atribuir intenção ao usuário. | Rubrica | 0 violações |
| GR-05 | Ser politicamente neutro: ter desempenho equivalente para afirmações de diferentes orientações políticas, e não inferir nem armazenar a posição política do usuário. | Diferença nas métricas de IA-01 a IA-07 entre subconjuntos do golden set balanceados por orientação política; revisão do que é registrado por conversa | Diferença máxima: LB; nenhum atributo político armazenado |
| GR-06 | Em temas de risco (saúde, medicamentos, finanças, segurança, emergências), priorizar fontes oficiais e deixar claro que a análise não é aconselhamento profissional. | Rubrica no subconjunto de temas de risco | 0 violações |

## Dados e base de conhecimento

| ID | Descrição |
|---|---|
| RD-01 | A base de evidências tem escopo definido (idioma, temas, período) e só aceita fontes classificadas em [Fontes e evidências](fontes-e-evidencias.md). Textos de desinformação nunca são usados como evidência; se forem indexados para reconhecer narrativas já circuladas, ficam separados e rotulados. Afirmações fora do escopo da base vão para a busca na web ou recebem a declaração da limitação. |
| RD-02 | Cada documento indexado registra URL, veículo, data de publicação e data de coleta. Novas verificações são acrescentadas com data, sem sobrescrever as anteriores. |
| RD-03 | O índice é atualizado em frequência definida, e evidências reaproveitadas obedecem a uma validade por tipo de afirmação: afirmações sensíveis ao tempo sempre passam por nova busca. Frequência e validade: LB. |
| RD-04 | Cada dataset usado (base, few-shot ou avaliação) tem um datasheet com origem, composição, licença e limitações conhecidas (Gebru et al., 2021). O uso de um dataset depende da verificação da sua licença. |
| RD-05 | Exemplos usados em few-shot ou indexados na base não fazem parte do conjunto que avalia o mesmo comportamento. Divisões entre conjuntos respeitam grupos de itens relacionados (pares de notícias, checagens repetidas). |

## Requisitos não funcionais

Baseados na ISO/IEC 25010 (qualidade de software) e na ISO/IEC 25059 (qualidade de sistemas de IA).

| ID | Atributo | Descrição |
|---|---|---|
| RNF-01 | Privacidade | O bot minimiza a coleta de dados pessoais, tem prazo de retenção definido e atende pedidos de exclusão. Mensagens de usuários só entram em conjuntos de avaliação anonimizadas e com consentimento. Prazo de retenção: a definir. |
| RNF-02 | Segurança | O bot limita o número de requisições por usuário e valida as entradas antes do processamento. |
| RNF-03 | Transparência | O usuário pode ver como a análise foi feita (termos de busca, trechos usados), e há um aviso permanente de que o sistema é um modelo probabilístico sujeito a erros. |
| RNF-04 | Usabilidade | Usuários sem conhecimento técnico entendem as respostas, que são curtas e progressivas, com aprofundamento sob demanda. Verificado em teste de usabilidade. Tamanho máximo da primeira mensagem: LB. |
| RNF-05 | Desempenho | Tempo até a primeira mensagem e percentil 95 do tempo da análise completa: LB. |
| RNF-06 | Confiabilidade | Falhas de serviços externos são comunicadas ao usuário e nunca resultam em conclusão apresentada como confiável. |
| RNF-07 | Rastreabilidade | Cada resposta registra as evidências usadas e as versões do prompt, do modelo e da base de conhecimento. |
| RNF-08 | Robustez | Variações de escrita da mesma mensagem (sem acento, caixa alta, erros de digitação) não alteram as métricas de IA-01 a IA-07 além de um limite (testes de invariância; Ribeiro et al., 2020). Limite: LB. |
| RNF-09 | Manutenibilidade | Modelo, serviço de busca e base de conhecimento podem ser substituídos sem reescrever o pipeline. |

## Operação

| ID | Descrição |
|---|---|
| OP-01 | Prompts, configuração do pipeline, modelo e base de conhecimento são versionados. Uma versão nova (inclusive troca de modelo ou de fornecedor) só substitui a atual se igualar ou superar os resultados de [Comportamento da IA](#comportamento-da-ia) e não violar nenhum [guardrail](#guardrails). |
| OP-02 | É possível voltar para a versão anterior sem retrabalho. |
| OP-03 | Em produção são monitorados: latência, custo, proporção de respostas "sem evidência suficiente", feedback dos usuários e mudança nos temas recebidos. Valores que disparam reavaliação: LB. |

## Custo dos erros

Todo modelo erra; o que se define é quais erros são mais graves e como o sistema se recupera. A severidade abaixo é uma proposta a validar pelo time.

| Erro | Severidade proposta | Por quê |
|---|---|---|
| Apresentar evidência contrária forte para uma afirmação verdadeira | Alta | O sistema passa a desinformar |
| Citar fonte inexistente | Alta | Destrói a confiança e viola GR-02 |
| Dar veredito binário | Alta | Contraria o objetivo do produto (OB-01) |
| Seguir instrução embutida em conteúdo externo | Alta | Abre o sistema a manipulação |
| Deixar de apresentar evidência contrária existente | Média | Reduz a utilidade e pode reforçar a desinformação |
| Classificar errado a natureza da afirmação (ex.: sátira como fato) | Média | Leva a verificar o que não precisa, ou a não verificar o que precisa |
| Responder "não encontrei evidência" quando ela existe | Baixa | A resposta é honesta, mas pouco útil |

Quando a evidência é fraca ou a confiança é baixa, o bot comunica a incerteza (IA-07) e sugere caminhos para a verificação manual, em vez de forçar uma conclusão.

## Critérios de aceitação

- **Golden set v1**: conjunto fixo e versionado, com a resposta esperada conferida por pessoas. Composição inicial:
    - mensagens reais de boatos (lado fake do FakeTrue.Br), sem quase-duplicatas e com uma mensagem por checagem;
    - as mensagens de teste da Sprint 1;
    - artigos completos, para o caso de envio de link;
    - os casos listados em [Testes](testes.md).
- **Anotação**: cada item é anotado por duas pessoas. A concordância entre elas é medida (kappa de Cohen); concordância baixa indica requisito ambíguo e leva a revisar a rubrica.
- **Resposta esperada**: como o produto não dá veredito, cada item define o que a análise deve conter (afirmações, natureza, técnicas de manipulação, fontes esperadas) e o que não pode conter (veredito, fonte inventada).
- **Conjunto adversarial**: prompt injection, sátira, temas de risco, entradas fora do escopo e variações de escrita.
- **Rubricas**: escritas para os itens avaliados por nota (IA-05, IA-08, GR-01, GR-04, GR-06).
- **Limiares**: registrados por versão do golden set, depois da linha de base (OB-02).

## Referências desta página

- Berry, D. M. (2022). *Requirements engineering for artificial intelligence: what is a requirements specification for an artificial intelligence?* REFSQ 2022.
- Gebru, T. et al. (2021). *Datasheets for datasets*. Communications of the ACM, 64(12).
- ISO/IEC/IEEE 29148:2018. *Systems and software engineering — Requirements engineering*.
- ISO/IEC 25010:2023. *Product quality model*.
- ISO/IEC 25059:2023. *Quality model for AI systems*.
- Ribeiro, M. T. et al. (2020). *Beyond accuracy: behavioral testing of NLP models with CheckList*. ACL 2020.
- Shankar, S. et al. (2024). *Who validates the validators? Aligning LLM-assisted evaluation of LLM outputs with human preferences*. ACM UIST 2024.
- Vogelsang, A.; Borg, M. (2019). *Requirements engineering for machine learning: perspectives from data scientists*. AIRE 2019.
