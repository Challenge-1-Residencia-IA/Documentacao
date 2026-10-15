# Relatório técnico do modelo de IA

Documentação do processo de preparação dos dados, construção, avaliação e das decisões técnicas do modelo de IA do projeto. O foco é o modelo, sua execução e sua avaliação; a aplicação (bot, API, interfaces) está fora do escopo deste relatório.

## Resumo

O modelo é uma **tutora baseada em LLM com recuperação de evidências (RAG)**: recebe uma mensagem, alegação ou link, busca checagens e fontes na internet e numa base local, e apresenta trechos literais dessas fontes, sem declarar a informação verdadeira ou falsa. Envolvendo a tutora, guardrails de entrada (com NeMo Guardrails) e de saída aplicam as regras do produto.

Não há treino de pesos. O LLM (Qwen 2.5 7B) é usado como está, localmente, pelo Ollama. O que o time construiu, versiona e entrega como "modelo" é o sistema em volta dele: prompt, formato de resposta, base de evidências, índice vetorial e guardrails, empacotados como um modelo **MLflow pyfunc**. A avaliação usa um **golden set** de 65 boatos reais anotados pelo time e um conjunto de casos adversariais para os guardrails.

| Artefato | Onde está |
|---|---|
| Notebook de carga, execução e avaliação | `experimentos/notebooks/04-validacao-modelo.ipynb` |
| Modelo (MLflow pyfunc, inclui `python_model.pkl`) | gerado por `python -m scripts.registrar_modelo_rag_mlflow --salvar-em models/tutora-rag-v1` |
| Dados e golden set (versionados com DVC no DagsHub) | `experimentos/data/` (`dvc pull`, `dvc repro`) |
| Este relatório | esta página e o PDF correspondente |

## 1. O modelo

| Componente | Escolha |
|---|---|
| LLM | Qwen 2.5 7B Instruct, quantizado, servido localmente pelo Ollama |
| Recuperação | Busca na internet (DuckDuckGo) e base local; embeddings `intfloat/multilingual-e5-small`; índice ChromaDB |
| Base de evidências | 1.073 checagens e notícias (3.788 trechos) do FakeTrue.Br, mais 2 materiais educativos |
| Geração | O LLM só seleciona trechos literais das fontes, num JSON com formato fixo; o texto ao usuário é montado por regras a partir dos trechos validados |
| Guardrails | Entrada: verificações por padrão e NeMo Guardrails (classificação pelo Qwen). Blindagem das fontes antes do prompt. Saída: remoção de veredito e de julgamento do usuário, aviso em temas de risco |
| Empacotamento | MLflow pyfunc: código, prompt, guardrails, corpus, índice e manifesto com os hashes; os pesos do Qwen são referenciados pelo digest |

Ao carregar, o modelo confere se o Qwen instalado tem o mesmo digest do empacotado e recusa executar se não tiver. Trocar o prompt, a base ou o índice gera hashes novos e, portanto, uma versão nova do modelo.

## 2. Preparação dos dados e construção do modelo

### 2.1 Datasets e análise exploratória

Foram analisados dois datasets públicos em português (notebooks 01 a 03 do repositório `experimentos`):

| Dataset | Conteúdo | Principais achados |
|---|---|---|
| **Fake.br-Corpus** | 3.600 notícias falsas e 3.600 verdadeiras, pareadas por tema | Só o comprimento do texto separa as classes com 94,2% de acerto; 92,7% das falsas vêm de um único site. O que distingue as classes é o registro de redação (blog × agência), não a veracidade |
| **FakeTrue.Br** | 1.791 pares boato × texto verdadeiro | O lado falso é o boato coletado pelo Boatos.org; o verdadeiro é, em geral, uma checagem do G1. Pré-processamento desigual entre as classes (acentos, maiúsculas) cria atalhos. O pareamento é ruidoso: muitas vezes o texto verdadeiro trata de outro assunto |

A comparação entre os dois mostrou que um classificador acerta 88–90% dentro de cada dataset, mas só 57–62% ao ser treinado num e testado no outro, e que a origem do texto é previsível com 88% de acerto. Os datasets medem coisas diferentes e não servem como referência de "detectar notícia falsa".

### 2.2 Pipeline de preparação

O pacote `dados/` e o pipeline do DVC (`dvc.yaml`) transformam os dados brutos em duas camadas:

- **Camada genérica** (`data/interim/textos.parquet`): os dois datasets num esquema único, sem descartar nada, com sinalizações (texto sem acento, duplicata, quase-duplicata entre datasets, id inconsistente) e o texto limpo de boilerplate e rodapés.
- **Visões por finalidade** (`data/processed/`): base de checagens para o RAG, pares boato → checagem para avaliar a busca, candidatos ao golden set e candidatos a exemplos de prompt.

Regras de separação garantidas pelo pipeline: textos de desinformação nunca entram na base de evidências (RD-01), e golden set e exemplos de prompt não compartilham itens nem quase-duplicatas (RD-05). Verificou-se que nenhum item do golden set está na base indexada, nem como texto igual nem como quase-cópia (similaridade máxima 0,64).

Os datasets brutos, a planilha de anotação e os arquivos derivados são versionados com DVC, com armazenamento no DagsHub; o pipeline foi reexecutado em ambientes diferentes e reproduziu os mesmos arquivos.

### 2.3 Construção da base de evidências

A base local é formada pelas 1.073 checagens e notícias verdadeiras do FakeTrue.Br, divididas em 3.788 trechos e indexadas com embeddings `multilingual-e5-small` no ChromaDB. Na consulta, a tutora combina essa base com páginas recuperadas na internet no momento da pergunta.

### 2.4 Golden set

O golden set v1 tem 65 boatos reais do FakeTrue.Br, anotados pelo time conforme um guia de anotação: afirmações verificáveis e sua natureza (fato verificável, opinião, previsão, sátira, fora de contexto), tema, técnicas de manipulação, sensibilidade ao tempo, necessidade de contexto e tema de risco. A fonte esperada de cada item é a página de checagem do Boatos.org sobre o próprio boato. A concordância entre dois anotadores foi medida pelo kappa de Cohen numa amostra de itens anotados em dupla (seção 4.3).

### 2.5 Por que não há treino de pesos

A primeira linha do projeto foi ajustar o modelo: o desempenho medido só com prompt e exemplos não foi satisfatório, e o time treinou adaptadores LoRA sobre o Qwen. Essa linha foi encerrada:

- o ciclo de preparar dados, treinar e validar checkpoints consumiu muito tempo, o teste inicial não produziu respostas utilizáveis e o treino mais longo falhou numericamente;
- os dados disponíveis não eram compatíveis com a tarefa: a análise exploratória mostrou que os datasets ensinariam atalhos (comprimento, veículo, época) e não veracidade;
- o ajuste de pesos não resolve requisitos centrais do produto: informação atualizada, fontes rastreáveis e atualização da base sem retreino.

O projeto passou a usar o modelo-base com prompt, recuperação de evidências e guardrails. O "treinamento" do modelo entregue é, portanto, a construção e o versionamento da base de evidências e do índice.

## 3. Critérios de seleção da abordagem

| Decisão | Critério | Alternativas consideradas |
|---|---|---|
| Não classificar verdadeiro/falso | Requisito do produto: a decisão é do usuário; um veredito errado causa mais dano que nenhum. A análise exploratória mostrou que classificadores aprendem atalhos | Classificador supervisionado (descartado: 57–62% em generalização cruzada) |
| RAG em vez de fine-tuning | Informação atualizada, fontes citáveis, base atualizável sem retreino; o fine-tuning não convergiu | Adaptadores LoRA (encerrados) |
| LLM local e leve (Qwen 2.5 7B) | Privacidade, custo zero por chamada e hardware disponível no time; bom desempenho em português entre modelos abertos de 7B | Modelos maiores por API (custo e dependência externa) |
| Citações literais validadas | Evitar fonte inventada (GR-02): toda evidência precisa ser um trecho copiado de uma fonte recuperada | Resumo livre pelo LLM (risco de alucinação) |
| NeMo Guardrails nos rails de entrada | Padrão de mercado para guardrails de LLM, com rails declarativos; combinado com verificações por padrão, que decidem os casos claros sem chamar o LLM | Guardrails AI, LLM Guard (mesma restrição de versão do Python); só padrões (não cobrem pedidos fora do escopo escritos de forma incomum) |
| Avaliação por golden set e corretores por requisito | Medir os comportamentos definidos nos requisitos de IA, comparar versões e registrar no MLflow | Avaliação manual caso a caso (não comparável entre versões) |

## 4. Métricas e resultados

Os resultados abaixo vêm da execução do notebook `04-validacao-modelo.ipynb` sobre o modelo `tutora-rag-v1`. As definições das métricas estão no glossário do repositório `experimentos` (`references/glossario-metricas.md`).

### 4.1 Comportamento no golden set

Cada uma das 65 mensagens foi enviada ao modelo carregado do MLflow, e a resposta foi comparada com o gabarito por corretores, um por requisito. A taxa considera só os itens em que o corretor se aplica.

| Requisito | Métrica | Itens | Acertos | Taxa |
|---|---|---|---|---|
| GR-01 | Resposta sem veredito de verdadeiro/falso | 65 | 65 | 100% |
| RC-04 | Fontes apresentadas com veículo, data e link | 61 | 49 | 80% |
| IA-03 | Busca encontrou a checagem esperada | 65 | 7 | 11% |
| IA-03 | Checagem esperada apresentada como relevante | 65 | 0 | 0% |
| IA-03 | Checagem esperada usada como evidência | 65 | 0 | 0% |
| GR-06 | Aviso e fontes oficiais em tema de risco | 39 | 25 | 64% |

**Não medidos**: natureza das afirmações (IA-01) e técnicas de manipulação (IA-02). Estão anotados no golden set, mas a versão atual da tutora não os produz.

**Operação:** 65 mensagens, nenhum erro de execução; latência mediana de 7,6 s e p95 de 21,7 s por mensagem (RNF-05), dominada pela busca na internet. Todas as respostas foram classificadas como "evidências insuficientes".

**Análise:**

- **Sem veredito (GR-01).** Respeitado em todas as respostas. O resultado decorre principalmente do desenho: o LLM só seleciona trechos e o texto ao usuário é montado por regras, o que também elimina fontes inventadas (GR-02).
- **Busca de evidências (IA-03).** É a principal limitação. A checagem esperada não está na base local, então depende da busca na internet, que a encontrou em 7 dos 65 itens. Nesses 7 casos, o filtro de relevância a descartou, que decide pela correspondência entre a mensagem e o título da página; a hipótese é que boatos longos tenham pouca sobreposição com o título da checagem, o que precisa ser confirmado. Sem fonte direta, o LLM não é acionado e a resposta fica como "evidências insuficientes". O comportamento é seguro (nenhuma conclusão sem evidência), mas pouco útil.
- **Variação entre execuções.** Numa avaliação anterior com a mesma versão da tutora, a busca encontrou a checagem esperada em 13 dos 65 itens (20%). A diferença vem da busca na internet, que retorna resultados diferentes a cada execução; comparações entre versões precisam de mais de uma execução ou de uma base local que contenha as checagens.
- **Aviso em tema de risco (GR-06).** O aviso é acrescentado pelo guardrail de saída quando a mensagem tem termos de saúde, eleição, golpe ou segurança. Ficou ausente em 14 dos 39 itens anotados como tema de risco, em que os termos não aparecem de forma explícita.

### 4.2 Guardrails

Casos escritos pelo time, separados em **calibração** (usados para ajustar o prompt do classificador do NeMo) e **teste** (nunca vistos no ajuste). Os 65 itens do golden set entram como mensagens legítimas. Para mensagens legítimas, acerto significa não recusar.

| Conjunto | Categoria | Mensagens | Acerto | Decididas pelo LLM |
|---|---|---|---|---|
| Calibração | Fora do escopo | 13 | 100% | 10 |
| Calibração | Injeção | 4 | 100% | 1 |
| Calibração | Legítima | 4 | 100% | 0 |
| **Teste** | **Fora do escopo** | **12** | **67%** | 6 |
| **Teste** | **Injeção** | **4** | **100%** | 2 |
| **Teste** | **Saudação** | **4** | **100%** | 0 |
| **Teste** | **Legítima** | **6** | **100%** | 0 |
| Golden set | Legítima | 65 | 100% | 0 |

**Análise:**

- **Nenhuma mensagem legítima foi recusada** (75 de 75), que era o critério principal: recusar uma informação a verificar anularia a função do produto.
- **Todas as tentativas de prompt injection foram recusadas**, a maioria pelas verificações por padrão, em menos de 1 ms.
- **Pedidos fora do escopo**: 100% na calibração e 67% no teste. A diferença indica sobreajuste do prompt aos exemplos usados no ajuste. Passaram perguntas de conhecimento geral ("quem ganhou a copa de 2002?", "como funciona a fotossíntese"), que o modelo trata como possíveis alegações. O custo desse erro é baixo: a tutora busca fontes e informa que não encontrou a alegação.
- O classificador do LLM custa cerca de 0,8 s e só é usado em mensagens curtas e sem link. Na calibração, sem esse limite e sem exemplos no prompt, o Qwen recusou 16 de 35 boatos legítimos.

### 4.3 Confiabilidade do gabarito

A concordância entre dois anotadores foi medida em 10 itens anotados de forma independente (kappa de Cohen):

| Campo | Concordância bruta | Kappa | Leitura (Landis e Koch) |
|---|---|---|---|
| Tema | 80% | 0,71 | substancial |
| Pedido de compartilhamento | 100% | 1,00 | quase perfeita |
| Natureza da afirmação principal | 90% | 0,00 | leve (paradoxo do kappa: quase todos os itens são "fato verificável") |
| Urgência | 70% | 0,40 | razoável |
| Tema de risco | 70% | 0,35 | razoável |
| Apelo emocional | 60% | 0,31 | razoável |
| Autoridade anônima, precisa de contexto | 50% | 0,00 | leve |
| Sensível ao tempo | 40% | −0,20 | pior que o acaso |

Os campos objetivos (tema, pedido de compartilhamento) têm boa concordância; os mais subjetivos (técnicas de manipulação, contexto, sensibilidade ao tempo) não atingiram o mínimo de 0,6, e as métricas que dependem deles devem ser lidas com essa ressalva. A amostra dupla é pequena e foi anotada depois de uma discussão entre os anotadores, o que tende a elevar a concordância.

### 4.4 Síntese

| Aspecto | Situação |
|---|---|
| Segurança (sem veredito, sem fonte inventada, injeção, falso positivo dos guardrails) | Atende |
| Escopo (recusa de pedidos não relacionados) | Atende parcialmente (67% no teste) |
| Utilidade (encontrar e apresentar a checagem certa) | Não atende: a fonte certa é descartada pelo filtro de relevância |
| Funções de tutor (natureza das afirmações, técnicas de manipulação) | Ainda não implementadas; o golden set já permite medi-las |

Os próximos passos com maior impacto são indexar as páginas de checagem (Boatos.org e agências) na base local, trocar o filtro de relevância por título por um critério semântico sobre o conteúdo e incluir a extração de afirmações e de técnicas de manipulação na resposta.

## 5. Principais desafios

- **Dados incompatíveis com a tarefa.** Os datasets disponíveis permitem acertos altos por atalhos (comprimento, veículo, época, pré-processamento desigual) e não representam a tarefa do produto. Foi preciso uma análise exploratória extensa para perceber isso antes de treinar ou avaliar qualquer coisa sobre eles.
- **Pareamento ruidoso do FakeTrue.Br.** O texto "verdadeiro" pareado com cada boato muitas vezes trata de outro assunto. A solução foi usar a página do Boatos.org, de onde o boato foi extraído, como fonte esperada.
- **Fine-tuning sem retorno.** O custo de treinar e validar adaptadores localmente foi alto e o resultado não foi utilizável, o que levou à mudança de abordagem no meio do desafio.
- **Hardware limitado.** O Qwen 7B e o modelo de embeddings dividem uma GPU de 4 GB; carregar o modelo duas vezes no mesmo processo esgotou a memória.
- **Anotação subjetiva.** Campos como técnicas de manipulação e necessidade de contexto tiveram concordância baixa entre anotadores mesmo depois de um guia de anotação, o que limita a confiabilidade das métricas que dependem deles.
- **Falha silenciosa do NeMo em código assíncrono.** A verificação síncrona do NeMo não roda quando já existe um event loop (caso do Jupyter e de servidores como FastAPI). Como os guardrails voltam às verificações por padrão quando o NeMo falha, o sistema continuou seguro, mas a classificação pelo LLM deixou de ser aplicada sem nenhum erro aparente. A falha só apareceu nos sinais registrados em cada resposta; a correção executa o NeMo numa thread própria e ganhou um teste.
- **Ferramentas e ambiente.** As bibliotecas de guardrails ainda não suportam o Python 3.14, o que exigiu fixar a versão do ambiente; o pandas 3 com pyarrow mudou o comportamento de expressões regulares com acentos sem gerar erro; o DVC indicou dados como enviados ao remoto quando não estavam, o que só foi percebido conferindo cada arquivo no armazenamento.

## 6. Aprendizados

- **Explorar os dados antes de modelar.** Os achados da análise exploratória mudaram o papel dos datasets (de dados de treino para base de evidências e golden set) e evitaram uma avaliação enganosa.
- **Avaliar o comportamento, não o veredito.** Para um produto que não classifica, as métricas certas são as dos requisitos (fonte encontrada, sem veredito, aviso em tema de risco), medidas por corretores sobre um golden set.
- **Separar calibração e teste também em prompts.** O classificador de escopo acertou tudo nos casos usados para ajustá-lo e menos nos casos novos; ajustar prompt com exemplos sofre de sobreajuste como um modelo treinado.
- **O erro mais caro define o projeto do guardrail.** Recusar uma mensagem legítima é pior do que deixar passar um pedido fora do escopo; por isso o classificador do LLM só é usado em mensagens curtas e sem link, e a calibração priorizou zero falso positivo.
- **Versionar tudo o que define o comportamento.** Prompt, base, índice, golden set e guardrails mudam o resultado tanto quanto o LLM; empacotá-los com hashes permite saber exatamente o que foi avaliado.
- **Fallback seguro precisa ser monitorado.** Um mecanismo que degrada sem erro protege o usuário, mas também esconde falhas; os sinais que cada resposta registra (como `nemo_indisponivel`) precisam ser acompanhados em produção.
- **Conferir em vez de confiar.** Mensagens de sucesso de ferramentas (envio de dados, execução de notebook) precisaram ser verificadas no resultado final.

## 7. Reprodutibilidade

No repositório `experimentos` (Python 3.11 a 3.13, Ollama com o Qwen 2.5 7B):

```bash
pip install -r requirements.txt -r requirements-rag.txt
dvc pull && dvc repro                                    # dados, visões e golden set
python -m scripts.coletar_rag --config sources.json      # materiais educativos
python -m scripts.converter_dataset_colega_rag           # base de evidências
python -m scripts.indexar_rag                            # índice vetorial
python -m scripts.registrar_modelo_rag_mlflow --salvar-em models/tutora-rag-v1
jupyter nbconvert --to notebook --execute notebooks/04-validacao-modelo.ipynb
```
