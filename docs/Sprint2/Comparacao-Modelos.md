📄 Documentação — Comparação de Modelos: Qwen 7B vs Qwen 3B vs Llama 3.1 8B

Data: 06/10/2026
Responsável: Larissa Giffoni (Pessoa 3)
Sprint: Sprint 2 — Núcleo do Sistema
Status: ⏳ Aguardando avaliação da equipe

🎯 Objetivo

Comparar o desempenho de três modelos (Qwen 2.5 7B, Qwen 2.5 3B e Llama 3.1 8B) na tarefa de análise de afirmações, utilizando o mesmo prompt, as mesmas mensagens de teste e as mesmas evidências, para decidir qual modelo será usado como oficial no projeto.

🧪 Metodologia

Mensagens de teste: 3 (uma alegação científica, uma sátira e uma opinião)
Prompt utilizado: Versão estrita com formulário (sem veredito, sem opinião)
Modelos comparados: qwen2.5:7b, qwen2.5:3b, llama3.1:8b
Métricas: Similaridade com o gabarito (0 a 100%) e contagem de palavras comuns
Evidências: As mesmas evidências foram usadas para os 3 modelos
📊 Resumo Geral

Mensagem	Qwen 7B	Qwen 3B	Llama 3.1 8B
"A Terra é plana."	11,81%	34,38%	10,36%
"Governo vai taxar o ar..."	27,02%	61,91%	7,60%
"O prefeito é o pior..."	19,17%	41,05%	9,14%
Atenção: Similaridade alta não significa melhor resposta. Leiam as respostas completas abaixo para avaliar.

📝 Mensagem 1: "A Terra é plana."

Resposta Esperada (Gabarito)

text
Afirmações identificadas:
1. "A Terra é plana" — Alegação científica

O que precisa ser verificado:
- Se a afirmação contradiz o consenso científico sobre a forma da Terra.

Elementos de manipulação:
- Nenhum elemento de manipulação identificado.
Resposta do Qwen 2.5 7B

text
Afirmações identificadas:
1. A Terra é plana — [classificação: alegação científica ou informação fora de contexto]
2. A Terra é redonda — [classificação: fato]

O que precisa ser verificado:
- Afirmação 1: O formato atual da Terra, com base em evidências científicas.
- Afirmação 2: Não há necessidade de verificação, já que é um fato.

Elementos de manipulação:
- Apelo emocional: A afirmação busca provocar reações emocionais por meio do questionamento dos conhecimentos científicos atualmente aceitos.
- Falta de fonte: A afirmação "A Terra é plana" não fornece uma referência ou fonte confiável para a afirmação.

Nota: As evidências fornecidas são essencialmente utilizadas para refutar a ideia de que a Terra é plana, confirmando consistentemente sua esfericidade.
Resposta do Qwen 2.5 3B

text
Afirmações identificadas:
1. A Terra é plana — [classificação: opinião]
2. A Terra é plana — [classificação: alegação científica ou informação fora de contexto]

O que precisa ser verificado:
- Afirmação 1: [o que deve ser checado]
- Afirmação 2: [o que deve ser checado]

Elementos de manipulação:
- Urgência
- Apelo emocional
- Falsa equivalência entre opiniões e evidências científicas
Resposta do Llama 3.1 8B

text
Afirmações identificadas:
1. A Terra é plana. — [classificação: opinião, alegação científica ou informação fora de contexto]
2. A Terra não é nem perfeitamente plana nem uma esfera idealizada, mas um geoide – uma estrutura ligeiramente achatada nos polos devido à rotação. — [classificação: fato]

O que precisa ser verificado:
- Afirmação 1: A existência de evidências científicas que comprovem a esfericidade da Terra.
- Afirmação 2: A definição correta do termo "geoide" e sua relação com a forma da Terra.

Elementos de manipulação:
- Urgência (algumas fontes mencionam teorias conspiratórias e movimentos que ignoram leis físicas).
- Fato de que a mensagem é baseada em opiniões, alegações científicas ou informações fora de contexto.
📝 Mensagem 2: "Governo anuncia que vai taxar o ar que respiramos."

Resposta Esperada (Gabarito)

text
Afirmações identificadas:
1. "Governo anuncia que vai taxar o ar que respiramos" — Sátira

O que precisa ser verificado:
- Se houve anúncio oficial do governo sobre taxação do ar.

Elementos de manipulação:
- Uso de exagero para criar uma situação absurda.
Resposta do Qwen 2.5 7B

text
Afirmações identificadas:
1. Governo anuncia que vai taxar o ar que respiramos — opinião
2. Haddad decide taxar o ar que você respira — sátira

O que precisa ser verificado:
- Afirmação 1: Essa afirmação é uma generalização baseada em boatos e não é mencionada nos documentos oficiais.
- Afirmação 2: Esta afirmação também está presente nas evidências como um boato, portanto, seu status precisa ser verificado.

Elementos de manipulação:
- Apelo emocional
- Urgência
- Falsas fontes (não há nenhuma evidência oficial ou fonte confiável que apóie essas afirmações)

Nota: Ambas as afirmações são baseadas em boatos e não encontram suporte em evidências oficiais.
Resposta do Qwen 2.5 3B

text
Afirmações identificadas:
1. Governo anuncia que vai taxar o ar que respiramos. — [opinião]

O que precisa ser verificado:
- Afirmação 1: [o que deve ser checado]

Elementos de manipulação:
- Urgência
- Apelo emocional
- Falta de fonte
Resposta do Llama 3.1 8B

text
Afirmações identificadas:
1. Governo anuncia que vai taxar o ar que respiramos. — [classificação: fato, opinião, previsão, sátira, alegação científica ou informação fora de contexto]
2. Governo anuncia que vai taxar o ar que respiramos. — [classificação]

O que precisa ser verificado:
- Afirmação 1: Verificar se há alguma evidência de que um governo realmente anunciou que irá taxar o ar que as pessoas respiram.
- Afirmação 2: Identificar se a afirmativa é uma opinião, fato ou outra classificação.

Elementos de manipulação:
- Urgência
- Apelo emocional (por exemplo: "PARECE PIADA MAS NÃO É!")
- Falta de fonte para apoiar a afirmação
📝 Mensagem 3: "O prefeito é o pior da história da cidade."

Resposta Esperada (Gabarito)

text
Afirmações identificadas:
1. "O prefeito é o pior da história da cidade" — Opinião

O que precisa ser verificado:
- Se há dados objetivos que comprovem essa afirmação.

Elementos de manipulação:
- Generalização sem apresentar evidências.
Resposta do Qwen 2.5 7B

text
Afirmações identificadas:
1. O prefeito é o pior da história da cidade — [classificação: opinião]
2. Haddad foi o pior prefeito da história de São Paulo — [classificação]
3. Kalil foi pior prefeito da história em Belo Horizonte — [classificação]

O que precisa ser verificado:
- Afirmação 1: O que justifica a opinião do usuário sobre o prefeito.
- Afirmação 2: Evidências sobre as realizações e fracassos de Haddad como prefeito.
- Afirmação 3: Evidências sobre as realizações e fracassos de Kalil como prefeito.

Elementos de manipulação:
- Apelo emocional
- Falta de fonte

Nenhum elemento de manipulação identificado na Afirmação 1, pois a mensagem do usuário é clara e direta.
Resposta do Qwen 2.5 3B

text
Afirmações identificadas:
1. O prefeito é o pior da história da cidade — [classificação: opinião]
2. Simões chama Kalil de pior prefeito de BH e ex-prefeito rebate — [classificação: opinião]

O que precisa ser verificado:
- Afirmação 1: [o que deve ser checado a respeito da afirmação sobre o prefeito ser o pior da história da cidade]
- Afirmação 2: [o que deve ser checado a respeito da afirmação feita por Simões e pelo ex-prefeito]

Elementos de manipulação:
- urgência, falta de fonte
Resposta do Llama 3.1 8B

text
Afirmações identificadas:
1. O prefeito é o pior da história da cidade. — [classificação: opinião]
2. Haddad foi o pior prefeito da história de São Paulo, diz Nunes. — [classificação]

O que precisa ser verificado:
- Afirmação 1: A eficácia do governo atual e as críticas feitas a ele.
- Afirmação 2: As declarações de Ricardo Nunes sobre Fernando Haddad.

Elementos de manipulação:
- Urgência (a mensagem original faz referência ao prefeito como "o pior da história", o que pode criar uma sensação de urgência em quem está lendo);
- Alegações científicas (nenhum elemento específico foi identificado, mas é possível que a crítica feita ao governo atual seja baseada em estatísticas ou dados econômicos).
🔍 Análise Técnica (Observações da Pessoa 3)

Critério	Qwen 2.5 7B	Qwen 2.5 3B	Llama 3.1 8B
Entendeu a mensagem original?	✅ Sim	⚠️ Parcial	❌ Não
Seguiu o formato?	✅ Sim	⚠️ Só o formato	❌ Não
Preencheu os campos?	✅ Sim	❌ Não	⚠️ Parcial
Inventou coisas (alucinação)?	❌ Não	✅ Sim	✅ Sim
Observações importantes:

O Qwen 7B foi o único que preencheu todos os campos com conteúdo real.
O Qwen 3B copiou o formato do gabarito, mas deixou os campos em branco e inventou elementos de manipulação.
O Llama 3.1 8B não seguiu o formato e inventou afirmações que não estavam na mensagem original.
📝 Espaço para Avaliação da Equipe

Instruções: Preencham os campos abaixo com as observações de vocês. Não precisa ser nada técnico, apenas a opinião de cada um sobre qual resposta é mais útil para o projeto.
Avaliação do Qwen 2.5 7B

Critério	Nota (0 a 10)	Comentário
Clareza da resposta	[ ]	
Fidelidade à mensagem original	[ ]	
Facilidade de entender	[ ]	
Útil para o usuário final?	[ ]	
Comentários gerais sobre o Qwen 7B:
[Espaço em branco para preencher]

Avaliação do Qwen 2.5 3B

Critério	Nota (0 a 10)	Comentário
Clareza da resposta	[ ]	
Fidelidade à mensagem original	[ ]	
Facilidade de entender	[ ]	
Útil para o usuário final?	[ ]	
Comentários gerais sobre o Qwen 3B:
[Espaço em branco para preencher]

Avaliação do Llama 3.1 8B

Critério	Nota (0 a 10)	Comentário
Clareza da resposta	[ ]	
Fidelidade à mensagem original	[ ]	
Facilidade de entender	[ ]	
Útil para o usuário final?	[ ]	
Comentários gerais sobre o Llama:
[Espaço em branco para preencher]

Qual modelo vocês recomendam?

[ ] Qwen 2.5 7B
[ ] Qwen 2.5 3B
[ ] Llama 3.1 8B
[ ] Outro: _______________

Justificativa da equipe:
[Espaço em branco para preencher]

📌 Próximos Passos

Aguardar o preenchimento da avaliação pela equipe.
Com base no feedback, definir o modelo oficial do projeto.
Documentar a decisão final no Trello.
Documentação criada por: Larissa Giffoni
Revisada por: (a preencher pela equipe)

