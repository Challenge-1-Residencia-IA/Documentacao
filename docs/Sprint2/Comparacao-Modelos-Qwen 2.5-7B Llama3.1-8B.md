Data: 29/09/2026
Responsável: Larissa Giffoni (Pessoa 3)
Sprint: Sprint 2 — Núcleo do Sistema
Status: ⏳ Aguardando avaliação da equipe

🎯 Objetivo

Comparar o desempenho dos modelos Qwen 2.5 7B e Llama 3.1 8B na tarefa de análise de afirmações, utilizando o mesmo prompt e a mesma mensagem de teste, para decidir qual modelo será usado como oficial no projeto.

🧪 Metodologia

Mensagem de teste: "A Terra é plana."
Prompt utilizado: Versão estrita com formulário (sem veredito, sem opinião).
Modelos comparados: qwen2.5:7b e llama3.1:8b
Evidências: As mesmas 5 evidências foram usadas para os dois modelos.
📊 Resultados Obtidos

Resposta do Qwen 2.5 7B

Afirmações identificadas:

A Terra é plana — [classificação: alegação científica ou informação fora de contexto]
A Terra é plana — [classificação]
O que precisa ser verificado:

Afirmação 1: Verificar se a afirmação de que "A Terra é plana" corresponde à compreensão científica atual.
Afirmação 2: Idem, verificando a mesma afirmação.
Elementos de manipulação:

Apelo emocional (implicação de ignorância ou desinformação em quem não aceita a esfericidade da Terra)
Urgência (não há urgência implícita na declaração)
Faltam fontes oficiais para suportar a afirmação
Nenhum elemento de manipulação identificado.

Resposta do Llama 3.1 8B

Afirmações identificadas:

A Terra é plana. — [classificação: opinião, alegação científica]
Muitas culturas antigas concordavam sobre a cosmografia plana da Terra, incluindo a Grécia Antiga (até o período clássico), as civilizações da Idade do Bronze e da Idade do Ferro do Oriente Médio (até o período helenístico), na Índia (até o período Gupta, primeiros séculos d.C.), e na China até o século XVII (17). — [classificação: fato]
O que precisa ser verificado:

Afirmação 1: se a Terra é plana ou não
Afirmação 2: se as culturas antigas concordavam sobre a cosmografia plana da Terra
Elementos de manipulação:

Nenhum elemento de manipulação identificado.
🔍 Análise Técnica (Observações da Pessoa 3)

Critério	Qwen 2.5 7B	Llama 3.1 8B
Entendeu a mensagem original?	✅ Sim	❌ Não (inventou uma afirmação)
Seguiu o formato?	✅ Sim (com um pequeno erro)	❌ Não (ignorou as regras)
Inventou coisas (alucinação)?	❌ Não	✅ Sim (alucinação grave)
Observação importante: O Llama 3.1 8B pegou um trecho das evidências (textos da Wikipédia) e tratou como se fosse uma afirmação do usuário. Isso é um erro grave, porque o modelo deve analisar apenas a mensagem original.

📝 Espaço para Avaliação da Equipe

Instruções: Preencham os campos abaixo com as observações de vocês. Não precisa ser nada técnico, apenas a opinião de cada um sobre qual resposta é mais útil para o projeto.
Avaliação do Qwen 2.5 7B

Critério	Nota (0 a 10)	Comentário
Clareza da resposta	[ ]	
Fidelidade à mensagem original	[ ]	
Facilidade de entender	[ ]	
Útil para o usuário final?	[ ]	
Comentários gerais sobre o Qwen:
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
[ ] Llama 3.1 8B
[ ] Outro: _______________

Justificativa da equipe:
