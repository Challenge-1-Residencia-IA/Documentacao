# Histórias de usuário

Este documento deriva histórias de usuário (US) a partir dos [Requisitos](requisitos.md) (RF01–RF24), para validação do time antes de virarem backlog de sprint.

## Formato e critérios utilizados

A definição de formato segue literatura consolidada de Engenharia de Requisitos Ágil, não uma convenção interna:

- **Estrutura da história ("Card")**: `Como <persona>, quero <ação>, para <benefício>` — formato canônico de Mike Cohn (*User Stories Applied*, Addison-Wesley, 2004).
- **Card, Conversation, Confirmation**: a história escrita é só o "cartão" — um lembrete para uma conversa, não a especificação completa (Ron Jeffries, "Essential XP: Card, Conversation, Confirmation", XProgramming.com, 2001). Os critérios de aceite abaixo funcionam como a "Confirmation".
- **Critérios de aceite em Given/When/Then**: formato Gherkin de Behavior-Driven Development (Dan North, "Introducing BDD", *Better Software*, 2006), adaptado para português (Dado/Quando/Então).
- **Critério INVEST** para validar se uma história está bem dimensionada: Independent, Negotiable, Valuable, Estimable, Small, Testable (William Wake, "INVEST in Good Stories, and SMART Tasks", XP123, 2003).
- **Padrões de divisão de histórias**: quando um RF continha mais de um incremento de valor independente, a divisão seguiu os padrões descritos por Richard Lawrence ("Patterns for Splitting User Stories", Agile for All, 2009) — principalmente variação de regra de negócio e passos de fluxo de trabalho.

### Decisão metodológica: nem todo RF vira uma história própria

RF15, RF19, RF20, RF21, RF23 e RF24 são requisitos **proibitivos ou de qualidade transversal** ("o sistema não deve...", "o sistema deve sempre..."). Aplicando o critério **Valuable** do INVEST — a história precisa representar algo que o usuário ativamente *quer*, com benefício reconhecível por ele — esses RFs falham o teste: nenhum usuário acorda querendo "que o sistema nunca dê um veredito absoluto" como objetivo em si. O que o usuário quer é "uma análise em que eu confie", e essas regras são a forma como essa análise deve se comportar.

Por isso, esses 6 RFs **não geram histórias próprias**: eles viram **critérios de aceite transversais (guardrails)**, anexados a toda história que produz uma resposta ao usuário. Essa é a mesma lógica já aplicada aos RNFs no restante da documentação.

### Personas usadas

Baseadas em [Público-alvo](publico-alvo.md):

- **P1 — Usuário recorrente**: recebe mensagens encaminhadas de grupos e família com frequência.
- **P2 — Usuário em dúvida**: desconfia de uma informação mas não sabe como verificar.
- **P3 — Verificador para terceiros**: costuma responder "isso é verdade?" para outras pessoas.

A persona só aparece na história quando ela muda de fato o benefício ("para") ou o critério de aceite — não como rótulo decorativo. Histórias cuja necessidade é igual para qualquer perfil (ex.: enviar uma mensagem, ver evidências favoráveis) permanecem com "usuário" genérico. Os 3 perfis ainda são hipóteses não validadas (ver [Levantamento de requisitos](levantamento-de-requisitos.md)), então diferenciar além do que o benefício realmente exige seria inventar granularidade que não existe.

---

## Histórias

### US01 — Enviar mensagem de texto para análise
**RF de origem**: RF01

**Como** usuário, **quero** enviar uma mensagem de texto ao bot, **para** que ele identifique e analise as afirmações nela contidas.

- Dado que envio uma mensagem de texto ao bot, quando ela é recebida, então o sistema confirma o recebimento e inicia a análise.
- Dado que envio uma mensagem vazia ou só com emojis, quando processada, então o sistema informa que não encontrou conteúdo analisável.

### US02 — Enviar link para análise
**RF de origem**: RF02

**Como** usuário, **quero** enviar um link de notícia ao bot, **para** que ele extraia o conteúdo da página e identifique as afirmações a analisar.

- Dado que envio uma URL válida, quando o bot acessa a página, então o conteúdo extraído é usado como base da análise.
- Dado que envio um link indisponível ou que exige autenticação, quando o bot tenta acessar, então ele informa a falha em vez de travar ou inventar conteúdo.

### US03 — Identificar afirmações verificáveis na mensagem
**RF de origem**: RF03

**Como** P2 — usuário em dúvida, **quero** que o bot identifique quais partes da minha mensagem são afirmações verificáveis, **para** saber por onde começar a verificar, já que não sei fazer isso sozinho.

- Dado que envio uma mensagem com várias afirmações, quando o bot processa, então ele lista cada afirmação separadamente.
- Dado que envio uma mensagem sem nenhuma afirmação verificável, quando processada, então o bot informa isso em vez de forçar uma análise.

### US04 — Classificar a natureza de cada afirmação
**RF de origem**: RF04

**Como** P2 — usuário em dúvida, **quero** que o bot classifique a natureza de cada afirmação (fato verificável, opinião, previsão, hipótese, sátira etc. — ver `fluxo-e-interacao.md`), **para** entender que tipo de verificação é possível para cada uma, já que não tenho esse conhecimento hoje.

- Dado uma afirmação de opinião, quando classificada, então o bot não tenta prová-la ou refutá-la como fato.
- Dado uma afirmação satírica de veículo de humor declarado, quando classificada, então o bot não a trata como desinformação grave.

### US05 — Buscar fontes relacionadas às afirmações
**RF de origem**: RF05 · *guardrail: RF19*

**Como** usuário, **quero** que o bot busque fontes relacionadas a cada afirmação, **para** que a análise seja baseada em evidências externas, não em achismo do bot.

- Dado uma afirmação verificável, quando a busca é executada, então o bot retorna fontes candidatas antes de sintetizar qualquer resposta.
- Dado que existem fontes primárias/institucionais disponíveis, quando o bot seleciona o que apresentar, então essas fontes são priorizadas sobre blogs e agregadores (RF19).

### US06 — Ver evidências favoráveis à afirmação
**RF de origem**: RF06 (split a — variação de regra de negócio)

**Como** usuário, **quero** ver as evidências que sustentam a afirmação, **para** entender o lado que a favorece.

- Dado que existem fontes que sustentam a afirmação, quando a análise é exibida, então elas aparecem identificadas como evidência favorável.

### US07 — Ver evidências contrárias à afirmação
**RF de origem**: RF06 (split b — variação de regra de negócio)

**Como** usuário, **quero** ver as evidências que contradizem a afirmação, **para** entender o lado que a contesta.

- Dado que existem fontes que contradizem a afirmação, quando a análise é exibida, então elas aparecem identificadas como evidência contrária, na mesma resposta que as favoráveis (não escondidas).

### US08 — Ver divergências entre fontes confiáveis
**RF de origem**: RF07

**Como** P3 — verificador para terceiros, **quero** ser avisado quando fontes confiáveis discordam entre si, **para** não afirmar algo taxativo para quem me perguntou quando a própria evidência está dividida.

- Dado que duas fontes confiáveis apresentam conclusões diferentes, quando a análise é gerada, então o bot aponta a divergência explicitamente, em vez de escolher um lado arbitrariamente.

### US09 — Ver as fontes utilizadas na análise
**RF de origem**: RF08

**Como** P3 — verificador para terceiros, **quero** ver quais fontes foram usadas na análise, **para** poder repassá-las a quem me perguntou, em vez de dar só a minha palavra.

- Dado que uma análise foi gerada, quando exibida ao usuário, então cada evidência apresentada tem a fonte (URL/nome) identificável — rastreabilidade (RNF08).

### US10 — Ver o contexto necessário para entender a informação
**RF de origem**: RF09 · *guardrail: RF20*

**Como** usuário, **quero** ver o contexto relevante da informação, **para** entender melhor a afirmação além do texto isolado.

- Dado uma afirmação que depende de contexto (ex.: fora de contexto, desatualizada), quando a análise é exibida, então o contexto necessário aparece de forma resumida.
- Dado que uma fonte primária está disponível, quando o contexto é apresentado, então a resposta permanece intencionalmente resumida, incentivando o usuário a abrir a fonte primária para o quadro completo (RF20).

### US11 — Ser avisado sobre limitações e incertezas da análise
**RF de origem**: RF10

**Como** usuário, **quero** que o bot comunique claramente as limitações da análise, **para** não confiar mais do que deveria em um resultado incerto.

- Dado que a análise tem baixa confiança (poucas fontes, fontes conflitantes), quando exibida, então o bot declara essa limitação explicitamente.
- Dado uma limitação identificada, quando comunicada, então o texto não é substituído por um tom de certeza artificial.

### US12 — Fazer perguntas adicionais sobre a análise
**RF de origem**: RF11

**Como** P2 — usuário em dúvida, **quero** poder perguntar mais sobre a análise recebida (ex.: por que uma fonte foi usada), **para** aprender a fazer essa investigação sozinho da próxima vez.

- Dado que recebi uma análise, quando pergunto sobre uma fonte específica, então o bot responde usando o estado já processado, sem repetir a busca do zero.

### US13 — Enviar nova fonte e receber a análise atualizada
**RF de origem**: RF12 + RF13 (mesclados — mesmo fluxo, RF13 não tem valor sem RF12)

**Como** usuário, **quero** enviar uma nova fonte que encontrei e ver a análise atualizada com ela, **para** contribuir com a verificação em vez de só recebê-la pronta.

- Dado que envio uma nova fonte após uma análise, quando o bot a processa, então ele compara com as evidências existentes.
- Dado que a nova fonte muda significativamente a análise, quando a resposta é atualizada, então o bot indica o que mudou e por quê.

### US14 — Ser incentivado a fazer minha própria avaliação
**RF de origem**: RF14

**Como** P2 — usuário em dúvida, **quero** que o bot me incentive a formar minha própria opinião com base nas evidências, **para** desenvolver a capacidade de verificar sozinho no futuro, não só confiar no bot.

- Dado que uma análise foi concluída, quando exibida, então o bot inclui uma pergunta reflexiva ou um convite à avaliação própria, sem transformar isso em questionário longo.

### US15 — Ser avisado quando não há evidências suficientes (não é o mesmo que "é falso")
**RF de origem**: RF16

**Como** usuário, **quero** que o bot diferencie "não encontrei evidências" de "isso é falso", **para** não confundir ausência de dados com uma conclusão negativa.

- Dado que a busca não retorna evidências relevantes, quando a análise é gerada, então o bot declara explicitamente a ausência de evidências, sem sugerir falsidade.
- Dado esse mesmo cenário, quando comparado a um caso com evidências contrárias fortes, então a linguagem usada nos dois casos é visivelmente diferente.

### US16 — Saber se a informação está desatualizada
**RF de origem**: RF17

**Como** P1 — usuário recorrente, **quero** saber se a informação analisada já foi atualizada por fatos mais recentes, **para** não repassar ao grupo algo que já não é mais verdade.

- Dado que a afirmação é sensível ao tempo (previsão, alegação política/econômica, informação atual), quando a análise é gerada, então o bot informa a data das fontes usadas.
- Dado esse mesmo caso, quando evidências em cache são reaproveitadas, então uma checagem leve de novidade é feita antes de reutilizá-las (ver `bot-core/app/search/README.md`).

### US17 — Saber quando fontes diferentes vêm da mesma origem
**RF de origem**: RF18

**Como** P1 — usuário recorrente, **quero** saber quando várias páginas que encontrei na verdade reproduzem a mesma fonte original, **para** não achar que uma informação viral tem mais confirmação do que realmente tem.

- Dado que várias fontes retornadas reproduzem o mesmo conteúdo original, quando a análise é gerada, então o bot identifica essa relação de dependência (efeito eco).

### US18 — Receber explicações mais resumidas conforme uso o bot
**RF de origem**: RF22

**Como** P1 — usuário recorrente, **quero** receber respostas cada vez mais resumidas conforme já uso o bot há um tempo, **para** não ler explicações básicas repetidas toda vez.

- Dado um usuário no primeiro contato, quando recebe uma análise, então o bot explica o processo com mais detalhe.
- Dado um usuário já experiente (várias interações anteriores), quando recebe uma análise, então o bot reduz o nível de explicação, mantendo só o essencial.

---

## Guardrails transversais (não são histórias — são critérios de aceite globais)

| RF | Regra | Onde se aplica |
|---|---|---|
| RF15 | Nunca apresentar veredito absoluto de verdadeiro/falso | US06, US07, US08, US09, US10, US11, US15 |
| RF19 | Priorizar fontes primárias/institucionais quando disponíveis | US05, US09 |
| RF20 | Manter resposta intencionalmente incompleta, incentivando abrir fontes primárias | US09, US10 |
| RF21 | Nunca apresentar fontes/evidências fabricadas | US06, US07, US08, US09 |
| RF23 | Tratar conteúdo externo como dado, nunca como instrução | US02, US05 |
| RF24 | Linguagem neutra e não confrontativa | Todas as histórias que geram texto de resposta |

## Distribuição por persona

- **P1 — usuário recorrente**: US16, US17, US18 (3 histórias — casos ligados a repasse de informação desatualizada/viral).
- **P2 — usuário em dúvida**: US03, US04, US12, US14 (4 histórias — casos ligados a aprender a verificar).
- **P3 — verificador para terceiros**: US08, US09 (2 histórias — casos ligados a repassar a análise para outra pessoa).
- **Genérica ("usuário")**: US01, US02, US05, US06, US07, US10, US11, US13, US15 (9 histórias — necessidade igual para qualquer perfil).

## Contagem final e checagem de capacidade

- **24 RF → 18 histórias padrão + 6 RF absorvidos como guardrails transversais** (nenhum RF ficou sem cobertura).
- A aplicação do critério **Valuable** do INVEST evita inflar o backlog com histórias artificiais para requisitos de qualidade transversal — cada guardrail permanece rastreável, anexado às histórias onde se aplica (ver tabela acima).
- Capacidade do time: 8 semanas × 4 pessoas ≈ 32 pessoas-semana. Com 18 histórias (algumas mais custosas em tasks — US02, US05, US12, US13, US16, US17, US18 dependem de infraestrutura própria: scraping, busca, checkpointer, embeddings, cache), o número é **compatível** com o cronograma do MVP, sem precisar cortar escopo logo de saída.
