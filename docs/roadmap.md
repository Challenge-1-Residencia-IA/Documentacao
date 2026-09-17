# Roadmap e cronograma

## Roadmap de pesquisa

### Fase 1: Pesquisa

- levantamento bibliográfico;
- estudo sobre desinformação;
- estudo sobre alfabetização midiática;
- estudo sobre fact-checking;
- estudo sobre IA e RAG;
- levantamento de soluções existentes.

### Fase 2: Definição

- requisitos;
- princípios;
- casos de uso;
- arquitetura;
- critérios de avaliação.

### Fase 3: Protótipo

- fluxo conversacional;
- protótipo do Telegram;
- estrutura das respostas;
- testes de UX.

### Fase 4: MVP

- integração com Telegram;
- processamento de mensagens;
- busca;
- análise;
- geração de resposta;
- fontes.

### Fase 5: Validação

- testes técnicos;
- testes de usabilidade;
- testes de precisão;
- experimento de pensamento crítico.

### Fase 6: Análise

- resultados;
- limitações;
- erros;
- comportamento dos usuários;
- impacto sobre alfabetização midiática.

## Próximos passos técnicos

Após a definição do escopo do MVP e dos principais requisitos, a próxima etapa é a investigação e validação das tecnologias necessárias para a implementação. O objetivo inicial não é desenvolver todo o sistema, mas validar tecnicamente os componentes fundamentais e identificar limitações antes da implementação definitiva.

1. **Definição da arquitetura**: estabelecer como os principais componentes se comunicam (ver [Arquitetura](arquitetura.md)).
2. **Integração com o Telegram**: investigar a API oficial do Telegram (Telegram Bot API), considerando recebimento e envio de mensagens, uso de webhooks (registrados via `setWebhook`), envio de textos e links, limitações da API, custos e requisitos de utilização, identificação e gerenciamento das conversas.
3. **Pipeline de processamento da informação**: desenvolver um primeiro protótipo capaz de transformar uma mensagem em informações estruturadas (ver [Pipeline de IA](pipeline-ia.md)).
4. **Busca e avaliação de fontes**: investigar APIs de busca, extração de conteúdo das páginas, identificação da fonte original, data da informação, relevância e qualidade da fonte, e mecanismos para reduzir o risco de a IA gerar ou atribuir evidências que não existem.
5. **Modelo de evidências**: definir a estrutura de dados usada para representar cada evidência (ver [Pipeline de IA](pipeline-ia.md)).
6. **Prova de conceito**: validar o núcleo do sistema antes de integrar com o Telegram (ver [Pipeline de IA](pipeline-ia.md)).
7. **Dataset inicial para testes**: montar o conjunto inicial de mensagens de teste (ver [Testes](testes.md)).

Ao final dessa fase, a equipe deve ter: uma arquitetura técnica inicial, as tecnologias candidatas definidas, uma estratégia de integração com o Telegram, um pipeline inicial de análise, um modelo estruturado para evidências e fontes, um POC funcional do núcleo do sistema e um dataset inicial para testes.

## Plano de desenvolvimento do MVP

### Escopo

A proposta é desenvolver uma versão simplificada do pipeline de verificação de informações utilizado pelo Sift, adaptada às necessidades do projeto. O objetivo não é reproduzir toda a infraestrutura do Sift, mas implementar o núcleo necessário para que o sistema consiga identificar afirmações, classificá-las, buscar fontes, extrair conteúdo, comparar evidências, identificar divergências, gerar uma síntese educativa, apontar limitações e entregar o resultado no Telegram.

O prazo estimado para o MVP é de 8 semanas, organizado para que as diferentes partes do sistema sejam desenvolvidas em paralelo por 4 pessoas.

!!! warning "Ponto de atenção"
    A prioridade durante todo o desenvolvimento deve ser manter o escopo controlado. Funcionalidades que não sejam necessárias para o funcionamento do núcleo do sistema devem ficar para uma etapa posterior. Este cronograma de 8 semanas descreve o trabalho de desenvolvimento do MVP em si; ele deve ser conciliado com o calendário geral do desafio, que reserva uma janela mais curta para a fase de construção.

### Semanas 1 e 2: fundação e pipeline mínimo

**Objetivo**: ter o pipeline básico funcionando localmente, ainda sem integração com o Telegram.

| Papel | Atividades |
|---|---|
| Pessoa 1: IA e prompts | Desenvolver e testar o prompt de extração de afirmações com pelo menos 20 mensagens variadas (verdadeiras, falsas, múltiplas afirmações, opiniões); definir saída estruturada (afirmação, tipo, possibilidade de verificação); desenvolver o prompt de classificação. |
| Pessoa 2: Backend e busca | Configurar a Tavily API; criar função que recebe uma afirmação e retorna título, URL e snippet dos principais resultados; implementar extração de conteúdo das páginas; testar scraping em notícias, blogs e páginas governamentais. |
| Pessoa 3: Orquestração e embeddings | Configurar PostgreSQL + pgvector via Docker; configurar o modelo de embeddings; criar a função de indexação dos textos extraídos; implementar busca por similaridade; criar o primeiro esqueleto do LangGraph (extrair → buscar → raspar → indexar → rankear). |
| Pessoa 4: Produto e testes | Criar o primeiro dataset de teste (~30 mensagens com gabarito); definir o formato esperado da saída da análise; documentar os critérios de avaliação dos resultados. |

**Marco da etapa**: dado um texto de entrada, o sistema deve retornar as afirmações identificadas, seus tipos, as principais evidências encontradas e o ranking das evidências. A síntese final ainda não é necessária.

### Semanas 3 e 4: síntese, crítica e resposta estruturada

**Objetivo**: completar o pipeline de análise e gerar uma resposta educativa estruturada.

| Papel | Atividades |
|---|---|
| Pessoa 1: IA e prompts | Prompt de comparação de evidências (favoráveis, contrárias, inconclusivas); prompt de síntese educativa, definindo como apresentar evidências, contexto, pontos de atenção e limitações. |
| Pessoa 2: Backend | Implementar o modelo de evidências (fonte, data, trecho, posição, nível de confiança); implementar o componente de crítica da análise, identificando ausência de fonte primária, fontes conflitantes e evidências insuficientes. |
| Pessoa 3: Orquestração | Expandir o LangGraph (extração → busca → extração de conteúdo → ranking → comparação → síntese → crítica); implementar detecção de divergências; implementar tratamento de falhas, informando explicitamente quando não há evidências suficientes em vez de gerar uma conclusão sem base. |
| Pessoa 4: Produto e testes | Executar o dataset completo, comparar resultados com os gabaritos e categorizar erros (alucinação, evidência irrelevante, interpretação incorreta, ausência de evidência, conclusão excessivamente absoluta). |

**Marco da etapa**: o pipeline funciona de ponta a ponta localmente e gera uma análise estruturada em português, sem apresentar conclusão absoluta quando as evidências não permitem esse nível de certeza.

### Semanas 5 e 6: integração com o Telegram

**Objetivo**: integrar o pipeline ao Telegram e implementar a camada conversacional.

| Papel | Atividades |
|---|---|
| Pessoa 1: IA e conversação | Agente de identificação de intenção do usuário (nova análise, pedido de explicação, contestação, envio de nova fonte); prompt de adaptação da análise ao formato do Telegram, dividindo a resposta em mensagens menores e usando indicadores visuais para os níveis de evidência. |
| Pessoa 2: Backend e Telegram | Criar o webhook em FastAPI; implementar `POST /webhook` para recebimento de mensagens, com o endpoint registrado via chamada `setWebhook` da Telegram Bot API; configurar o Ngrok para testes; implementar o envio das respostas, suportando mensagens progressivas e links das fontes utilizadas. |
| Pessoa 3: Orquestração | Integrar o webhook ao LangGraph, fazendo o grafo receber diretamente as mensagens do Telegram; implementar o estado da conversa, mantendo contexto entre mensagens; implementar o fluxo de recebimento de novas fontes enviadas pelo usuário, atualizando a análise. |
| Pessoa 4: Produto e testes | Testar o fluxo completo diretamente pelo Telegram; avaliar clareza das respostas, tamanho das mensagens e tom utilizado; ajustar a estrutura das mensagens com base nos testes. |

**Marco da etapa**: um usuário consegue enviar uma mensagem pelo Telegram e receber uma análise completa em português, com evidências e limitações.

### Semanas 7 e 8: robustez, avaliação e ajustes finais

**Objetivo**: testar o sistema em situações mais próximas do uso real e finalizar a avaliação do MVP.

| Papel | Atividades |
|---|---|
| Pessoa 1: IA e segurança | Proteções contra prompt injection, tratando o conteúdo extraído das páginas como dado, não como instrução para o modelo; testes com páginas contendo instruções maliciosas direcionadas ao LLM; implementar timeouts e respostas de fallback para análises que excedam o tempo esperado. |
| Pessoa 2: Backend e privacidade | Minimização de dados, definindo quais informações precisam ser armazenadas; documentar política de retenção; implementar logs de rastreabilidade, registrando quais fontes foram usadas em cada análise. |
| Pessoa 3: Orquestração e UX | Perguntas reflexivas ao final da análise; níveis de evidência (🟢🟡🟠🔴⚪) com explicação textual para cada um; ajuste de tom da linguagem para neutro, não confrontativo e sem atribuir intenção ao usuário. |
| Pessoa 4: Avaliação | Executar o experimento de pensamento crítico (teste inicial sem o sistema, uso do sistema, teste final com novas informações); aplicar questionários de experiência (facilidade de uso, clareza, satisfação); consolidar resultados técnicos e de experiência. |

**Marco da etapa**: ao final da oitava semana, o MVP deve estar funcional, testado e acompanhado de um relatório de avaliação.

### Distribuição das atividades por período

| Período | P1: IA/Prompts | P2: Backend/Busca | P3: Orquestração | P4: Produto/Testes |
|---|---|---|---|---|
| Semanas 1-2 | Extração e classificação | Tavily e scraping | PostgreSQL, embeddings e LangGraph | Dataset e formato de saída |
| Semanas 3-4 | Comparação e síntese | Modelo de evidências e crítica | LangGraph completo | Dataset e documentação de falhas |
| Semanas 5-6 | Conversação e adaptação | Webhook e Telegram | Integração e estado | Testes e clareza |
| Semanas 7-8 | Segurança e robustez | Privacidade e logs | UX e níveis de evidência | Avaliação e relatório |

A divisão acima serve como ponto de partida. As tarefas podem ser redistribuídas conforme a experiência de cada integrante e as dificuldades encontradas durante o desenvolvimento.

Os riscos identificados para esse cronograma estão detalhados em [Riscos](riscos.md).

### Resultado esperado

Ao final do desenvolvimento, espera-se ter um sistema capaz de receber uma informação pelo Telegram, identificar quais partes podem ser verificadas, buscar evidências externas, analisar essas evidências e apresentar o resultado de forma educativa. O sistema deve priorizar a apresentação das evidências e das limitações da análise, evitando tratar a resposta como um simples veredito de "verdadeiro" ou "falso".

A construção própria do pipeline também permite que a equipe tenha controle sobre os prompts, sobre o formato das respostas e sobre a forma como a análise é apresentada em português. Além disso, a arquitetura proposta mantém os principais componentes desacoplados, permitindo substituir tecnologias ou serviços no futuro sem precisar reconstruir todo o sistema.

### Próximos passos

Antes de começar o desenvolvimento, a equipe deve definir:

- divisão inicial das responsabilidades;
- formato exato da resposta final;
- dataset inicial de testes;
- critérios utilizados para avaliar uma análise;
- API de busca que será utilizada;
- LLM principal (validado por métricas reais por subtarefa, não escolhido a priori);
- estrutura inicial do repositório;
- forma de comunicação e acompanhamento das tarefas.

**Primeira entrega**: deve ser pequena e funcional. Dado um texto, o sistema deve identificar suas afirmações e retornar evidências relevantes encontradas na web. A partir disso, as próximas etapas adicionam comparação, síntese, crítica e integração com o Telegram. O foco inicial deve ser fazer esse fluxo funcionar de forma confiável antes de adicionar novas funcionalidades.
