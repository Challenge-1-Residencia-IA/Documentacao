📄 Documentação — Teste Comparativo: Qwen 2.5 3B vs 7B

Data: 24/09/2026
Responsável: Larissa Giffoni (Pessoa 3)
Sprint: Sprint 2 — Núcleo do Sistema
Status: ✅ Concluído

🎯 Objetivo

Testar se um modelo com menos parâmetros (Qwen 2.5 3B) mantém a qualidade necessária para a análise de desinformação, visando maior velocidade e menor uso de memória.

🧪 Metodologia

Modelos testados: Qwen 2.5 3B e Qwen 2.5 7B
Mensagem de teste: "A Terra é plana."
Prompt utilizado: O mesmo prompt otimizado de análise de afirmações
Métrica de avaliação: Capacidade de classificar corretamente, usar as evidências fornecidas pela busca e seguir o formato de resposta.
📊 Resultados

Qwen 2.5 7B (Modelo Atual)

Critério	Resultado
Classificação	✅ "Alegação científica" (correto)
Uso das evidências	✅ Citou as evidências e indicou o que verificar
Formatação	✅ Seguiu o formato do prompt
Alucinação	✅ Não inventou informações
Tempo de resposta	Aceitável para o MVP
Qwen 2.5 3B (Modelo Testado)

Critério	Resultado
Classificação	❌ "Sátira" (incorreto — não é piada, é uma teoria da conspiração)
Uso das evidências	❌ Ignorou completamente as evidências encontradas (Wikipédia, Brasil Escola)
Formatação	❌ Repetiu a resposta duas vezes na mesma saída
Alucinação	❌ Inventou que o erro de digitação "ë" era um "elemento de manipulação"
Tempo de resposta	Mais rápido, mas inutilizável
🔍 Análise: Por Que o 3B Não Serve?

O modelo de 3B falhou por três motivos principais:

1. Capacidade de Raciocínio Limitada

Com menos parâmetros, o modelo tem menos "neurônios" para processar instruções complexas. Ele não consegue, ao mesmo tempo:

Ler o prompt de análise
Ler as evidências da busca
Classificar corretamente
Formatar a resposta
Quando muitas instruções são dadas ao mesmo tempo, o modelo de 3B "se perde" e escolhe o caminho mais fácil (como classificar como "sátira" porque soa absurdo).

2. Dificuldade em Seguir Instruções Longas

O prompt de análise tem várias etapas (classificar, listar, identificar manipulação). O modelo de 7B consegue seguir todas. O de 3B ignora metade das etapas e se concentra apenas na primeira coisa que lê.

3. Tendência à Alucinação

O modelo de 3B inventou que o erro de digitação "ë" era um elemento de manipulação. Isso é um sinal claro de alucinação: ele precisava dizer algo sobre manipulação e, como não encontrou nada real, inventou.

✅ Conclusão

O modelo Qwen 2.5 3B não é adequado para o projeto.

Embora seja mais rápido e use menos memória, ele:

Não classifica corretamente
Ignora as evidências da busca
Se perde no formato de resposta
Inventa informações (alucinação)
Para uma ferramenta de alfabetização midiática, a precisão é mais importante que a velocidade. O Qwen 2.5 7B é o mínimo necessário para garantir a qualidade da análise.

📌 Recomendação

Manter o Qwen 2.5 7B como modelo oficial do projeto.
Não usar modelos abaixo de 7B para tarefas de análise crítica.
Se no futuro for necessário um modelo mais leve, testar o Llama 3.1 8B ou Qwen 2.5 14B (se o hardware permitir).
📎 Próximos Passos

Reverter o código para o modelo de 7B.
Testar o Qwen 2.5 7B com outras mensagens (sátira, opinião, fora de contexto).
Documentar os resultados no Trello.
Documentação criada por: Larissa Giffoni
Revisada por: (a preencher)

