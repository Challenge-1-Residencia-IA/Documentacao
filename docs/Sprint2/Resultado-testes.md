📄 Documentação — Resultados dos Testes - Larissa

Data: 18/09/2026
Responsável: Larissa
Sprint: Sprint 1 — Fundação Técnica
Status: ✅ Concluído

🎯 Objetivo

Testar o script busca_web.py com diferentes tipos de mensagem para avaliar a qualidade da análise do modelo e identificar pontos de melhoria.

🧪 Teste 1: Informação Falsa

Mensagem testada:

"Cientistas descobriram que tomar café cura câncer."
Resposta do modelo:

text
Afirmações identificadas:
1. "Tomar café cura câncer" — Fato verificável

O que precisa ser verificado:
- Se existe evidência científica suficiente para afirmar que tomar café cura o câncer.

Elementos de manipulação:
- Nenhuma fonte oficial citada
- Uso excessivo de termos positivos e assertivos, como "curar", sem apresentar provas completas

Detalhes adicionais:
As evidências fornecidas indicam que o café pode ter benefícios em relação ao câncer, mas não mencionam que ele cure o câncer completamente.
O que funcionou:

✅ Identificou a afirmação principal
✅ Apontou a falta de fonte
✅ Identificou exagero na linguagem ("curar")
✅ Não deu veredito absoluto de "falso"
✅ Citou as evidências encontradas
O que pode melhorar:

⚠️ Classificou como "Fato verificável", mas poderia ter classificado como "Alegação médica" ou "Exagero"
🧪 Teste 2: Informação Fora de Contexto

Mensagem testada:

"Estudo mostra que 90% das pessoas que usam celular têm problemas de visão."
Resposta do modelo:
(não foi possível testar este caso hoje — fica para a próxima sessão)

🧪 Teste 3: Sátira

Mensagem testada:

"Governo anuncia que vai taxar o ar que respiramos. A medida entra em vigor amanhã e o valor será de R$ 10 por respiração."
Resposta do modelo (antes da melhoria do prompt):

text
Afirmações identificadas:
1. "Governo anuncia que vai taxar o ar que respiramos" — Fato verificável
2. "A medida entra em vigor amanhã" — Previsão
3. "O valor será de R$ 10 por respiração" — Fato verificável

O que precisa ser verificado:
- Se houve anúncio oficial do governo sobre a taxa.
- A data exata em que a medida entrará em vigor.
- O valor exato da tarifa proposta.

Elementos de manipulação:
- Uso de "URGENTE" para criar senso de urgência
- Apelo emocional
- Falta de fonte oficial citada
O que funcionou:

✅ Separou as afirmações
✅ Identificou elementos de manipulação
✅ Não deu veredito absoluto
O que não funcionou:

❌ Não identificou que a mensagem é uma sátira
❌ Tratou como afirmação séria ("Fato verificável")
Resposta do modelo (depois da melhoria do prompt):

text
Afirmações identificadas:
1. "Governo anuncia que vai taxar o ar que respiramos" — Sátira
2. "A medida entra em vigor amanhã e o valor será de R$ 10 por respiração" — Informação fora de contexto

Explicação para a classificação como sátira:
- A afirmação descreve algo extremamente absurdo, exagerado (taxar o ar que respiramos) e impossível na prática atual.
- As evidências fornecidas mostram que essa é uma piada satírica.

Explicação adicional para a classificação como informação fora de contexto:
- O valor proposto R$ 10 por respiração não foi confirmado em nenhuma fonte oficial e parece ser um elemento acrescentado sem base factual.
O que melhorou:

✅ Identificou a sátira corretamente
✅ Explicou por que é uma piada
✅ Classificou a segunda parte como "fora de contexto"
✅ Manteve o tom educativo
📊 Resumo dos Testes

Teste	Tipo	Identificou?	Classificou?	Não deu veredito?
1	Falsa	✅	⚠️ Parcial	✅
2	Fora de contexto	⏳ Não testado	—	—
3 (antes)	Sátira	❌	❌	✅
3 (depois)	Sátira	✅	✅	✅
📌 Conclusões

O modelo funciona bem para identificar afirmações e listar o que precisa ser verificado.
O modelo não dá veredito absoluto de "verdadeiro" ou "falso", como planejado.
A sátira era um ponto fraco, mas foi resolvida com a melhoria do prompt.
A classificação ainda pode melhorar em alguns casos (ex: "Fato verificável" para algo que é exagero).
O modelo cita as evidências encontradas na busca, o que é essencial para o projeto.
📎 Próximos Passos

Testar o caso 2 (informação fora de contexto) na próxima sessão
Testar uma opinião apresentada como fato
Testar uma informação com múltiplas afirmações
Continuar refinando o prompt conforme necessário
Documentação criada por: Larissa
Revisada por: Larissa

