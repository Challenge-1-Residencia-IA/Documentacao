# Datasets e EDA

## Datasets

| Dataset | O que é | Papel no projeto |
|---|---|---|
| [Fake.br-Corpus](https://github.com/roneysco/Fake.br-Corpus) | 7.200 notícias em português, metade falsas e metade verdadeiras (2015–2018) | Estudo exploratório e exemplos de desinformação. **Não** entra na base de evidências |
| [FakeTrue.Br](https://github.com/jpchav98/FakeTrue.Br) | Pares boato × checagem do boato (Boatos.org, G1 Fato ou Fake e outros) | Origem da base de checagens, da avaliação da busca e do golden set |
| Golden set v1 | 65 mensagens reais de boato (151 afirmações) anotadas pelo time: natureza, tema, pedido de compartilhamento e fonte esperada | Prova fixa que mede a busca a cada deploy |
| Base de checagens | Páginas originais das checagens do FakeTrue.Br, mais checagens novas coletadas por RSS de Aos Fatos, Lupa, Boatos.org e E-farsas, até 14/10/2026 | Base de conhecimento do RAG em produção (Postgres + pgvector) |

**Versionamento:** datasets brutos, planilha de anotação e golden set são versionados com **DVC**, com armazenamento no **DagsHub**. O pipeline (`dvc repro`) refaz os dados processados a partir dos brutos. Os dados nunca entram no git.

**Cuidados contra vazamento:** os boatos do golden set não entram na tabela de boatos conhecidos da base, e parte das checagens é escondida durante a avaliação para medir se a busca "inventa" uma checagem que não existe.

## Principais achados da EDA

Notebooks completos, com as saídas, no repositório [experimentos](https://github.com/Challenge-1-Residencia-IA/experimentos):
[01 Fake.br-Corpus](https://github.com/Challenge-1-Residencia-IA/experimentos/blob/main/notebooks/01-eda-fakebr-corpus.ipynb) ·
[02 FakeTrue.Br](https://github.com/Challenge-1-Residencia-IA/experimentos/blob/main/notebooks/02-eda-faketrue-br.ipynb) ·
[03 Comparativo](https://github.com/Challenge-1-Residencia-IA/experimentos/blob/main/notebooks/03-eda-comparativo.ipynb)

**Fake.br-Corpus**

- Só o comprimento do texto já acerta 94,2% (mediana de 157 palavras nas falsas contra 918 nas verdadeiras).
- 92,7% das falsas vêm de um único site; as verdadeiras vêm de grandes portais.
- O que separa as classes é, em boa parte, o estilo de redação (blog × agência), não a veracidade. Por isso o corpus serve como exemplo de desinformação, não como benchmark.

**FakeTrue.Br**

- O par é **boato × checagem do boato**, não notícia falsa × notícia verdadeira.
- O pré-processamento é desigual entre as classes (parte dos boatos sem acento ou sem maiúsculas), o que cria atalhos para um classificador.
- Textos verdadeiros se repetem em vários pares: as divisões de treino e teste precisam ser agrupadas pela checagem.
- Numa amostra de 20 pares, só 4 eram a checagem do mesmo boato. O pareamento não serve como gabarito sem revisão manual, por isso o golden set foi anotado pelo time.
- O sinal útil é estrutural: mensagem viral ("urgente", "compartilhe") × discurso relatado de checagem ("a mensagem diz").

**Comparativo**

- Um classificador acerta 88–90% dentro de cada dataset, mas só **57–62%** treinando num e testando no outro: os datasets medem coisas diferentes.
- Conclusão que orientou o produto: **treinar um classificador de verdadeiro/falso não generaliza**. A Sophia usa os dados para buscar evidências (RAG) e para avaliar, não para dar veredito.
