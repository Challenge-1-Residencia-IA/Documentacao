📄 Documento de Pesquisa — Comparação de Modelos de Linguagem para o Copiloto Investigativo

Data: 22/09/2026

Autora: Larissa Giffoni (Pessoa 3)

Sprint: Sprint 2 — Núcleo do Sistema

🎯 Objetivo

Definir as métricas e o processo para comparar modelos de linguagem (Qwen 2.5 vs Llama 3.1) e verificar se um modelo com menos parâmetros (3B) mantém a qualidade necessária para o projeto.

📊 1. Métricas para Comparar Modelos

Para escolher o melhor modelo para o projeto, não basta olhar para benchmarks genéricos.

É preciso avaliar o que o sistema realmente faz: buscar evidências e gerar uma análise baseada nelas. Por isso, as métricas mais importantes são as do framework RAGAS (Retrieval-Augmented Generation Assessment) .

Métricas Principais

Métrica	O Que Mede	Por Que é Importante

Faithfulness (Fidelidade)	Mede se a análise gerada está baseada exclusivamente nas evidências recuperadas, sem inventar informações.

É a métrica mais crítica para o projeto. O sistema não pode alucinar.

Answer Relevance (Relevância da Resposta)	Mede se a resposta realmente responde à pergunta ou afirmação feita 	Garante que o modelo não fuja do tema.

Context Precision	Mede se os trechos de evidência recuperados são relevantes para a afirmação 	Avalia a qualidade da busca na web.

Context Recall	Mede se as evidências recuperadas cobrem tudo o que é necessário para responder 	Garante que a busca não deixou informação importante de fora.

Como Aplicar no Projeto

Para cada mensagem de teste:

A IA gera uma análise.

Um avaliador (humano ou o próprio LLM) dá uma nota de 0 a 10 para Fidelidade e Relevância.

A média das notas de cada modelo define o vencedor.

Dica: Vocês podem usar o framework RAGAS para automatizar essa avaliação .

🔍 2. Comparação Qwen 2.5 vs Llama 3.1

O Que a Pesquisa Mostra

Qwen 2.5 tende a superar o Llama 3.1 em várias tarefas de raciocínio e compreensão .

Em tarefas de resposta a perguntas, Qwen e ChatGPT geralmente superam os modelos Llama, com Qwen atingindo até 70% de precisão, enquanto Llama fica em torno de 60% .

No entanto, Llama 3.1 também é forte: em um benchmark de raciocínio lógico, o Llama 3.1 70B superou o GPT-4 .

Ambos oferecem um equilíbrio entre velocidade e precisão .

Conclusão

Qwen 2.5 é a melhor escolha para o projeto, principalmente por ser melhor em português e em seguir instruções complexas. Mas vale a pena testar o Llama para confirmar.

🤏 3. Modelos Menores (3B) Mantêm a Qualidade?

O Que a Pesquisa Mostra

Modelos de 3B com instrução (instruct) podem superar modelos de 7B em tarefas específicas .

Um estudo mostrou que o Qwen2.5-3B-Instruct superou todos os modelos de 7B avaliados, incluindo o Qwen2.5-7B .

Em tarefas de reconhecimento de entidades, o Qwen2.5-3B atingiu precisão de 89,64%, muito próximo do 7B (90,91%) .

Ganhos além de 3B diminuem: a melhoria de 3B para 7B é de apenas +0,064 em algumas tarefas .

Conclusão

Sim, vale a pena testar o modelo de 3B. Ele pode ser mais rápido, mais leve e ainda manter qualidade suficiente para o projeto. O teste com a frase da "Terra plana" é um bom ponto de partida.

📋 4. Plano de Teste Prático

Passo	O Que Fazer	Modelos

1	Baixar os modelos	qwen2.5:3b e llama3.1:8b

2	Testar com 5 mensagens diferentes	Falsa, fora de contexto, opinião, sátira, múltiplas afirmações

3	Avaliar Fidelidade e Relevância	Nota de 0 a 10 para cada resposta

4	Comparar os resultados	Planilha com as notas

5	Escolher o modelo vencedor	Qwen, Llama ou Qwen 3B

📌 Nome Sugerido para a Pesquisa

"Avaliação Comparativa de Modelos de Linguagem para RAG em Alfabetização Midiática"

Ou, mais curto: "Benchmark de LLMs para Copiloto Investigativo"

📎 Próximos Passos

Baixar os modelos (qwen2.5:3b e llama3.1:8b).

Criar um script de teste em lote.

Avaliar as respostas com as métricas RAGAS.

Documentar os resultados no Trello.

Documentação criada por: Larissa Giffoni

Revisada por: Larissa Giffoni
