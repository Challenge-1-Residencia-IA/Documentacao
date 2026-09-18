📄 Documentação — Melhoria do Prompt (Detecção de Sátira)

Data: 18/09/2026
Responsável: Larissa
Sprint: Sprint 1 — Fundação Técnica
Status: ✅ Concluído

🎯 Objetivo

Melhorar o prompt de análise para que o modelo consiga identificar corretamente quando uma mensagem é uma sátira, em vez de tratá-la como uma afirmação séria.

🔍 Problema Identificado

No teste anterior, com a mensagem:

"Governo anuncia que vai taxar o ar que respiramos. A medida entra em vigor amanhã e o valor será de R$ 10 por respiração."
O modelo classificou a mensagem como "Fato verificável", tratando-a como uma afirmação séria. Isso é um problema, porque a mensagem é claramente uma sátira (uma piada).

Por que isso importa:

Sátiras são um tipo comum de desinformação quando interpretadas literalmente
O projeto precisa diferenciar sátira de fato
O modelo não pode tratar piadas como afirmações verificáveis
🛠️ Solução Implementada

Foi adicionada uma seção específica no prompt para ensinar o modelo a identificar sátira:

text
COMO CLASSIFICAR SÁTIRA:
- Sátira é um texto que usa humor, exagero ou ironia para criticar algo.
- Se a mensagem descreve algo absurdo, exagerado ou impossível, pode ser sátira.
- Exemplos de sátira:
  * "Governo anuncia que vai taxar o ar que respiramos."
  * "Novo estudo diz que dormir 2 horas por noite faz bem para a saúde."
  * "Prefeito declara que buracos nas ruas são decoração urbana."
- Se você identificar sátira, classifique como "sátira" e explique por que parece ser uma piada.
- NUNCA trate uma sátira como fato verificável.
✅ Resultado Após a Melhoria

Mensagem testada:

"Governo anuncia que vai taxar o ar que respiramos. A medida entra em vigor amanhã e o valor será de R$ 10 por respiração."
Resposta do modelo:

text
Afirmações identificadas:
1. "Governo anuncia que vai taxar o ar que respiramos" — Sátira
2. "A medida entra em vigor amanhã e o valor será de R$ 10 por respiração" — Informação fora de contexto

Explicação para a classificação como sátira:
- A afirmação descreve algo extremamente absurdo, exagerado (taxar o ar que respiramos) e impossível na prática atual.
- As evidências fornecidas mostram que essa é uma piada satírica.
O que melhorou:

Antes	Depois
Classificou como "Fato verificável"	Classificou como "Sátira" ✅
Tratou como afirmação séria	Explicou por que é uma piada ✅
Não mencionou sátira	Justificou a classificação ✅
📌 Limitações Conhecidas

O modelo ainda pode se confundir se a sátira for muito sutil
O modelo pode classificar a mesma afirmação de formas diferentes na mesma resposta
Sátiras que imitam notícias reais podem ser mais difíceis de detectar
📎 Próximos Passos

Testar o prompt melhorado com outros tipos de sátira
Documentar outros casos que o modelo não conseguiu identificar
Refinar o prompt conforme necessário
Documentação criada por: Larissa
Revisada por: Larissa
