# Histórias de usuário

Histórias de usuário (US) derivadas dos [Requisitos](requisitos.md), para validação do time antes de virarem backlog de sprint. Cada história indica os requisitos que atende.

## Formato e critérios utilizados

- **Estrutura da história**: `Como <persona>, quero <ação>, para <benefício>` (Mike Cohn, *User Stories Applied*, 2004).
- **Card, Conversation, Confirmation**: a história escrita é um lembrete para a conversa do time, não a especificação completa (Ron Jeffries, 2001). Os critérios de aceite são a "Confirmation".
- **Critérios de aceite em Dado/Quando/Então**: formato Gherkin de Behavior-Driven Development (Dan North, 2006).
- **INVEST** para validar o tamanho de cada história: Independent, Negotiable, Valuable, Estimable, Small, Testable (William Wake, 2003).
- **Divisão de histórias**: quando um requisito tinha mais de um incremento de valor independente, a divisão seguiu os padrões de Richard Lawrence (*Patterns for Splitting User Stories*, 2009), principalmente variação de regra de negócio.

### Critérios de aceite de comportamento da IA

Nas histórias que dependem do modelo, o critério de aceite não é "sempre acerta": é a métrica do requisito correspondente atingir o limiar no conjunto de avaliação, como definido em [Comportamento da IA](requisitos.md#comportamento-da-ia). Os limiares são definidos depois da medição da linha de base.

### Requisitos que não viram histórias próprias

- **Guardrails, fidelidade e qualidades da resposta** (GR-01 a GR-06, IA-06, RNF-04 a RNF-06): são restrições sobre toda resposta, não algo que o usuário pede. Aplicando o critério *Valuable* do INVEST, viram **critérios de aceite transversais** (ver [Critérios transversais](#criterios-transversais)).
- **Dados, operação, arquitetura e qualidade interna** (RD, OP, RNF-02, RNF-07 a RNF-09): são trabalho necessário que não entrega valor direto ao usuário. Viram **histórias habilitadoras** (*enabler stories*, conceito do SAFe), escritas do ponto de vista do time (ver [Histórias habilitadoras](#historias-habilitadoras)).

### Personas

Baseadas em [Público-alvo](publico-alvo.md):

- **P1 — Usuário recorrente**: recebe mensagens encaminhadas de grupos e família com frequência.
- **P2 — Usuário em dúvida**: desconfia de uma informação, mas não sabe como verificar.
- **P3 — Verificador para terceiros**: costuma responder "isso é verdade?" para outras pessoas.

A persona aparece na história só quando muda o benefício ou o critério de aceite. Quando a necessidade é a mesma para qualquer perfil, a história usa "usuário".

---

## Histórias de usuário

### US01 — Enviar mensagem de texto para análise
**Requisitos**: RC-01

**Como** usuário, **quero** enviar ao bot uma mensagem de texto que recebi, **para** entender se ela merece confiança.

- Dado que envio uma mensagem de texto em português, quando ela é recebida, então o bot confirma o recebimento e inicia a análise.
- Dado que envio uma mensagem vazia ou só com emojis, quando processada, então o bot informa que não encontrou conteúdo para analisar.

### US02 — Enviar link para análise
**Requisitos**: RC-01

**Como** usuário, **quero** enviar o link de uma notícia, **para** que o bot analise o conteúdo da página.

- Dado que envio um link acessível, quando o bot lê a página, então o conteúdo extraído é a base da análise.
- Dado que envio um link indisponível ou que exige autenticação, quando o bot tenta acessá-lo, então informa a falha e não apresenta conteúdo inventado.

### US03 — Saber quando um formato não é suportado
**Requisitos**: RC-02

**Como** usuário, **quero** ser avisado quando envio algo que o bot não analisa, **para** não achar que a análise foi feita.

- Dado que envio áudio, imagem, vídeo ou texto em outro idioma, quando o bot recebe, então responde que esse formato não é suportado, sem apresentar análise parcial como completa.

### US04 — Ver quais afirmações podem ser verificadas
**Requisitos**: IA-01

**Como** P2 — usuário em dúvida, **quero** ver quais partes da mensagem são afirmações verificáveis e quais são opinião ou sátira, **para** saber por onde começar a verificar.

- Dado que envio uma mensagem com várias afirmações, quando o bot a analisa, então lista cada afirmação verificável e indica a natureza de cada uma.
- Dado que a mensagem contém opinião ou sátira, quando o bot a analisa, então sinaliza esses trechos em vez de tratá-los como fato a verificar.
- Dado que a mensagem passa do tamanho máximo, quando o bot a analisa, então trata as afirmações principais e informa que analisou só parte.
- As métricas de IA-01 atingem o limiar no golden set v1.

### US05 — Reconhecer as técnicas de manipulação da mensagem
**Requisitos**: IA-02

**Como** P1 — usuário recorrente, **quero** que o bot mostre como a mensagem tenta me convencer (urgência, pedido de compartilhamento, autoridade anônima, apelo emocional), **para** reconhecer essas técnicas sozinho nas próximas mensagens que receber.

- Dado que a mensagem usa técnicas de manipulação, quando o bot a analisa, então aponta cada técnica e o trecho em que ela aparece.
- Dado que a mensagem não usa nenhuma técnica reconhecível, quando o bot a analisa, então não inventa técnicas.
- As métricas de IA-02 atingem o limiar no golden set v1.

### US06 — Ser guiado na verificação
**Requisitos**: IA-08

**Como** P2 — usuário em dúvida, **quero** que o bot me guie na verificação em vez de me dar a resposta pronta, **para** aprender a checar informações por conta própria.

- Dado que a análise começou, quando o bot apresenta as afirmações e técnicas, então pergunta como eu verificaria antes de mostrar as fontes.
- Dado que as fontes foram apresentadas, quando a análise termina, então o bot pede a minha avaliação da informação e comenta o meu raciocínio.
- A interação não vira questionário: no máximo uma pergunta por etapa.
- A nota da rubrica de IA-08 atinge o limiar nas conversas de teste.

### US07 — Receber menos explicação com o uso
**Requisitos**: IA-08

**Como** P1 — usuário recorrente, **quero** que as explicações fiquem mais curtas conforme uso o bot, **para** não ler o básico toda vez.

- Dado que é meu primeiro contato, quando recebo uma análise, então o bot explica cada passo.
- Dado que já usei o bot várias vezes, quando recebo uma análise, então ele reduz as explicações e me convida a identificar as técnicas antes de mostrá-las.

### US08 — Ver as fontes direto, sem a parte guiada
**Requisitos**: RC-05

**Como** P3 — verificador para terceiros, **quero** pular a parte guiada e ir direto às fontes, **para** responder rápido a quem me perguntou.

- Dado que a análise começou, quando escolho "ver fontes", então o bot apresenta as fontes sem as perguntas do fluxo guiado.

### US09 — Ver as fontes encontradas
**Requisitos**: IA-03, RC-04

**Como** P3 — verificador para terceiros, **quero** ver as checagens e fontes encontradas, com veículo, data e link, **para** repassá-las a quem me perguntou em vez de dar só a minha palavra.

- Dado que existe uma checagem publicada sobre a afirmação, quando o bot busca fontes, então essa checagem aparece primeiro.
- Dado que não há checagem publicada, quando o bot busca na web, então prioriza fontes primárias e institucionais.
- Toda fonte apresentada tem veículo, data e link.
- As métricas de IA-03 atingem o limiar nos pares boato → checagem e no golden set v1.

### US10 — Entender o que as fontes dizem
**Requisitos**: IA-04

**Como** usuário, **quero** saber se cada fonte sustenta, contradiz ou não conclui sobre a afirmação, **para** comparar os lados sem receber um veredito.

- Dado que há fontes favoráveis e contrárias, quando a análise é apresentada, então as duas aparecem, identificadas como tal.
- Dado que fontes confiáveis divergem, quando a análise é apresentada, então o bot aponta a divergência em vez de escolher um lado.
- As métricas de posição e de divergência de IA-04 atingem o limiar no golden set v1.

### US11 — Saber quando várias fontes vêm da mesma origem
**Requisitos**: IA-04

**Como** P1 — usuário recorrente, **quero** saber quando várias páginas reproduzem a mesma fonte original, **para** não achar que uma informação viral tem mais confirmação do que realmente tem.

- Dado que várias fontes encontradas reproduzem o mesmo conteúdo, quando a análise é apresentada, então o bot indica a origem comum.
- A métrica de fontes dependentes de IA-04 atinge o limiar nos casos anotados.

### US12 — Ver o contexto e a data da informação
**Requisitos**: IA-05

**Como** P1 — usuário recorrente, **quero** ver o contexto original e a data das fontes, **para** não repassar ao grupo algo fora de contexto ou desatualizado.

- Dado que a afirmação depende de contexto (fora de contexto, desatualizada), quando a análise é apresentada, então o contexto necessário aparece de forma resumida.
- Dado que a afirmação é sensível ao tempo, quando as fontes são apresentadas, então cada uma mostra a sua data.
- As métricas de IA-05 atingem o limiar no golden set v1.

### US13 — Saber quando não há evidência suficiente
**Requisitos**: IA-07

**Como** usuário, **quero** que o bot diferencie "não encontrei evidência suficiente" de "há evidência contrária", **para** não confundir falta de dados com uma conclusão.

- Dado que não há evidência disponível sobre a afirmação, quando a análise é gerada, então o bot declara a insuficiência, sem sugerir que a afirmação é falsa.
- Dado que a evidência é fraca, quando a análise é gerada, então o bot comunica a incerteza e sugere caminhos de verificação manual.
- As métricas de IA-07 atingem o limiar nos boatos cuja checagem foi mantida fora da base.

### US14 — Perguntar sobre a análise
**Requisitos**: RC-03

**Como** P2 — usuário em dúvida, **quero** fazer perguntas sobre a análise recebida, **para** entender o raciocínio e aprender a fazer a verificação sozinho.

- Dado que recebi uma análise, quando pergunto sobre uma fonte ou afirmação, então o bot responde usando o contexto da conversa, sem refazer a busca do zero.

### US15 — Enviar uma fonte que encontrei
**Requisitos**: RC-03

**Como** usuário, **quero** enviar uma fonte que encontrei e ver a análise atualizada, **para** participar da verificação em vez de só recebê-la.

- Dado que envio uma nova fonte durante a conversa, quando o bot a processa, então a compara com as evidências existentes.
- Dado que a nova fonte muda a análise, quando a resposta é atualizada, então o bot indica o que mudou e por quê.

### US16 — Ver como a análise foi feita
**Requisitos**: RNF-03

**Como** P2 — usuário em dúvida, **quero** ver os termos de busca e os trechos usados na análise, **para** conseguir repetir o processo numa busca comum.

- Dado que recebi uma análise, quando peço para ver como foi feita, então o bot mostra os termos de busca usados e os trechos de cada fonte.
- O aviso de que o sistema é um modelo probabilístico sujeito a erros está sempre acessível.

### US17 — Pedir a exclusão dos meus dados
**Requisitos**: RNF-01

**Como** usuário, **quero** pedir a exclusão das minhas conversas, **para** ter controle sobre os meus dados.

- Dado que peço a exclusão dos meus dados, quando o pedido é processado, então as conversas armazenadas são apagadas e o bot confirma.
- Conversas não são mantidas além do prazo de retenção definido.

---

## Histórias habilitadoras

Trabalho do time que não entrega valor direto ao usuário, mas sem o qual as histórias acima não podem ser aceitas. Seguindo o SAFe, elas se dividem em dois grupos:

- **Dados e operação** (EN01 a EN08): conjuntos de avaliação, base de evidências, versionamento, monitoramento e proteção.
- **Arquitetura** (EN09 a EN13): a base técnica (*architectural runway*) de que as histórias dependem. As histórias de resposta (US04 a US16) dependem da EN09 e da EN11; as de fontes (US09 a US13), também da EN10 e da EN12.

### Dados e operação

#### EN01 — Montar o golden set v1
**Requisitos**: OB-02, RD-05, critérios de aceitação

**Como** time do projeto, **precisamos de** um conjunto de avaliação fixo e anotado, **para** medir os comportamentos da IA e comparar versões.

- O conjunto inclui boatos reais (uma mensagem por checagem, sem quase-duplicatas), as mensagens de teste da Sprint 1, artigos completos e os casos de [Testes](testes.md).
- Cada item é anotado por duas pessoas, e a concordância (kappa de Cohen) é registrada.
- Nenhum item do golden set é usado como exemplo de few-shot nem indexado na base.

#### EN02 — Medir a linha de base e definir os limiares
**Requisitos**: OB-02

**Como** time do projeto, **precisamos** medir o desempenho atual no golden set v1, **para** definir os limiares dos requisitos de IA com base em dados.

- As métricas de IA-01 a IA-08 são medidas e registradas com a versão do golden set.
- Os limiares são definidos e publicados em [Requisitos](requisitos.md).

#### EN03 — Montar o conjunto adversarial
**Requisitos**: GR-01 a GR-06, RNF-08

**Como** time do projeto, **precisamos de** mensagens feitas para provocar falhas, **para** verificar os guardrails e a robustez.

- O conjunto inclui prompt injection, sátira, temas de risco, entradas fora do escopo e variações de escrita (sem acento, caixa alta, erros de digitação).

#### EN04 — Construir a base de evidências
**Requisitos**: RD-01, RD-02, RD-03

**Como** time do projeto, **precisamos de** uma base de checagens e fontes com escopo e regras de atualização, **para** que a busca encontre evidências confiáveis e atuais.

- Só entram fontes classificadas em [Fontes e evidências](fontes-e-evidencias.md); textos de desinformação nunca entram como evidência.
- Cada documento registra URL, veículo, data de publicação e data de coleta, sem sobrescrever versões anteriores.
- A frequência de atualização e a validade por tipo de afirmação estão definidas.

#### EN05 — Documentar os datasets
**Requisitos**: RD-04

**Como** time do projeto, **precisamos de** um datasheet para cada dataset usado, **para** conhecer origem, licença e limitações antes de usá-lo.

- Cada datasheet registra origem, composição, licença e limitações conhecidas.

#### EN06 — Versionar, promover e voltar versões
**Requisitos**: OP-01, OP-02, RNF-07

**Como** time do projeto, **precisamos** versionar prompt, configuração, modelo e base, **para** só colocar em uso versões que não pioram os resultados e poder voltar atrás.

- Cada resposta registra as evidências usadas e as versões do prompt, do modelo e da base.
- Uma versão nova só entra em uso se igualar ou superar os resultados no golden set e não violar nenhum guardrail.
- Voltar para a versão anterior não exige retrabalho.

#### EN07 — Monitorar o bot em produção
**Requisitos**: OP-03

**Como** time do projeto, **precisamos** acompanhar o bot em produção, **para** perceber quando o comportamento muda.

- Latência, custo, proporção de respostas "sem evidência suficiente", feedback e temas recebidos são acompanhados, com valores que disparam reavaliação.

#### EN08 — Proteger o bot contra abuso
**Requisitos**: RNF-02, RNF-09

**Como** time do projeto, **precisamos** limitar requisições e validar entradas, com componentes substituíveis, **para** manter o bot disponível e fácil de evoluir.

- Há limite de requisições por usuário e validação das entradas antes do processamento.
- Modelo, serviço de busca e base podem ser trocados sem reescrever o pipeline.

### Arquitetura

#### EN09 — Montar o esqueleto ponta a ponta
**Requisitos**: RC-01, RC-03, RNF-09

**Como** time do projeto, **precisamos de** um caminho completo da mensagem recebida à resposta enviada, com as etapas ainda simplificadas, **para** integrar e testar cada parte do pipeline desde o início (*walking skeleton*, Alistair Cockburn).

- Dado que uma mensagem chega pelo webhook, quando o pipeline executa, então ela passa por todas as etapas do grafo descritas em [Arquitetura](arquitetura.md), mesmo que simplificadas, e uma resposta volta ao usuário.
- Dado que o usuário envia uma segunda mensagem, quando o pipeline executa, então o estado da conversa anterior está disponível.
- Cada etapa pode ser substituída pela implementação real sem alterar as demais.
- Um teste automatizado percorre o caminho completo, e o pipeline sobe localmente com os comandos documentados no repositório.

#### EN10 — Montar o banco vetorial e a indexação
**Requisitos**: RD-01, RD-02, RD-03, RNF-09

**Como** time do projeto, **precisamos** armazenar e indexar a base de evidências para busca por similaridade, **para** que o bot encontre checagens já publicadas antes de recorrer à web. A EN04 define o conteúdo da base; esta história define onde ele fica e como é consultado.

- O banco (PostgreSQL com pgvector) sobe localmente com um comando documentado.
- Cada documento guarda texto, vetor, URL, veículo, data de publicação, data de coleta e versão da base.
- A ingestão pode ser reexecutada sem duplicar documentos, e atualizações são acrescentadas sem sobrescrever as anteriores.
- O modelo de vetorização é escolhido pela recuperação medida nos pares boato → checagem e fica registrado junto com a versão do índice.
- Textos de desinformação não são indexados como evidência.

#### EN11 — Disponibilizar o modelo de linguagem
**Requisitos**: RNF-05, RNF-06, RNF-09, OP-01

**Como** time do projeto, **precisamos de** um modelo de linguagem acessível pelo pipeline, local e em produção, **para** executar as etapas que dependem dele.

- O pipeline acessa o modelo por uma interface única, e trocar de modelo ou de fornecedor é uma mudança de configuração.
- O modelo roda localmente com instruções documentadas.
- O ambiente de execução em produção está definido, com latência e custo medidos em relação aos limiares de RNF-05.
- Dado que o modelo falha ou excede o tempo limite, quando a análise está em andamento, então a falha é comunicada ao usuário e não gera conclusão.

#### EN12 — Integrar a busca na web
**Requisitos**: IA-03, RD-03, RNF-06, RNF-09

**Como** time do projeto, **precisamos** integrar um serviço de busca na web, **para** encontrar fontes quando a base de checagens não tem a resposta.

- Dado que existe checagem relevante na base, quando o bot busca fontes, então a base é consultada primeiro; a web só é usada quando a base não basta.
- Dado que a afirmação é sensível ao tempo, quando o bot busca fontes, então a busca na web sempre é feita.
- O serviço de busca é acessado por uma interface substituível.
- O consumo da cota do serviço é acompanhado, com limite que impede ultrapassar o plano contratado.
- Dado que o serviço está indisponível ou sem cota, quando o bot busca fontes, então a falha é comunicada e não gera conclusão.

#### EN13 — Configurar a integração contínua
**Requisitos**: OP-01, OP-02

**Como** time do projeto, **precisamos** verificar automaticamente cada mudança no código, **para** detectar erros antes que cheguem à `main` e servir de base para a promoção de versões da EN06.

- A cada push e pull request, os testes e a verificação de estilo rodam nos repositórios de código (GitHub Actions).
- Um pull request com falha não pode ser mesclado na `main`.
- Quando prompt, configuração ou modelo mudam, a avaliação no golden set roda no pipeline e o resultado fica registrado.

---

## Critérios transversais

Valem para **todas** as histórias que geram resposta ao usuário (US04 a US16).

| Requisito | Critério |
|---|---|
| GR-01 | A resposta não apresenta veredito binário ("é falso", "é verdade"). |
| GR-02 | Toda fonte citada existe e está entre as evidências encontradas. |
| GR-03 | Instruções embutidas no conteúdo externo não são seguidas. |
| GR-04 | A linguagem é neutra e não atribui intenção ao usuário. |
| GR-05 | O desempenho é equivalente entre afirmações de diferentes orientações políticas, e a posição política do usuário não é inferida nem armazenada. |
| GR-06 | Em temas de risco, fontes oficiais são priorizadas e fica claro que a análise não é aconselhamento profissional. |
| IA-06 | A síntese diz só o que as evidências encontradas sustentam. |
| RNF-04 | As respostas são curtas e progressivas, com aprofundamento sob demanda. |
| RNF-05 | O tempo até a primeira mensagem respeita o limiar definido. |
| RNF-06 | Falhas de serviços externos são comunicadas e nunca viram conclusão. |

## Cobertura

| Tipo | Quantidade | Requisitos cobertos |
|---|---|---|
| Histórias de usuário | 17 | RC-01 a RC-05, IA-01 a IA-05, IA-07, IA-08, RNF-01, RNF-03 |
| Histórias habilitadoras | 13 | OB-02, RD-01 a RD-05, OP-01 a OP-03, RNF-02, RNF-07 a RNF-09, GR (via conjunto adversarial); as de arquitetura também sustentam RC-01, RC-03, IA-03, RNF-05 e RNF-06 |
| Critérios transversais | 10 | GR-01 a GR-06, IA-06, RNF-04 a RNF-06 |

OB-01 é o objetivo do produto e orienta todas as histórias.
