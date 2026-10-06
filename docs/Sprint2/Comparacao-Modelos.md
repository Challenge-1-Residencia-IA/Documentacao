📄 Documentação — Comparação de Modelos: Qwen 7B vs Qwen 3B vs Llama 3.1 8B

Data: 06/10/2026

Responsável: Larissa Giffoni (Pessoa 3)

Sprint: Sprint 2 — Núcleo do Sistema

Status: ⏳ Aguardando avaliação da equipe

🎯 Objetivo

Comparar o desempenho de três modelos (Qwen 2.5 7B, Qwen 2.5 3B e Llama 3.1 8B) na tarefa de análise de afirmações, utilizando o mesmo prompt, a mesma mensagem de teste e as mesmas evidências, para decidir qual modelo será usado como oficial no projeto.

🧪 Metodologia

Mensagens de teste: 3 (uma alegação científica, uma sátira e uma opinião)

Prompt utilizado: Versão estrita com formulário (sem veredito, sem opinião)

Modelos comparados: qwen2.5:7b, qwen2.5:3b, llama3.1:8b

Métricas: Similaridade com o gabarito (0 a 100%) e contagem de palavras comuns

Evidências: As mesmas evidências foram usadas para os 3 modelos

📊 Resultados Obtidos

Tabela Resumo

Mensagem	Qwen 7B	Qwen 3B	Llama 3.1 8B

"A Terra é plana."	11,81%	34,38%	10,36%

"Governo vai taxar o ar..."	27,02%	61,91%	7,60%

"O prefeito é o pior..."	19,17%	41,05%	9,14%

Observações Sobre os Números

Atenção: A similaridade alta do Qwen 3B não significa que ele é melhor. Olhando as respostas completas, o Qwen 3B:

Deixou campos em branco ([o que deve ser checado])

Inventou elementos de manipulação que não existiam na mensagem

Copiou o formato do gabarito sem preencher o conteúdo

Ou seja, o Qwen 3B está "trapaceando" nas métricas: ele copia a estrutura do gabarito, mas não faz o trabalho de análise de verdade.

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

