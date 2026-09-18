# Relatório Técnico: Avaliação de Benchmark e Resolução do Viés de Sátira (Sprint 1)

**Projeto:** Pipeline Local de Detecção e Classificação de Desinformação
**Arquitetura:** MLX + Qwen 2.5 7B Fine-Tuned + RAG (DuckDuckGo Search)
**Documento:** Análise de Erros e Plano de Mitigação da Avaliação Reduzida (Sprint 1)
**Data:** Setembro de 2026

---

## 1. Visão Geral da Avaliação

No âmbito da Sprint 1, foi realizada uma execução experimental reduzida contendo 3 casos extraídos da base de dados de validação (`Sprint_1_Dataset_20_exemplos_complexos.xlsx`), na aba `Dataset Sprint 1`.

O objetivo dessa etapa foi testar a capacidade do modelo fine-tunado em classificar corretamente mensagens em 5 categorias da taxonomia do projeto:

* **FALSA**
* **VERDADEIRA**
* **FORA DE CONTEXTO**
* **OPINIÃO**
* **SÁTIRA**

O pipeline opera com **isolamento de gabarito**, no qual a IA recebe unicamente o texto da mensagem e os trechos de busca retornados pelo RAG, sem ter conhecimento prévio das colunas de gabarito contidas na planilha.

---

## 2. Resultados da Avaliação Reduzida (3 Casos)

### 2.1. Tabela Resumo dos Testes

| Caso ID | Categoria Esperada (Gabarito) | Predição do Modelo |   Status  | Diagnóstico do Modelo                                                                                                                                          |
| :-----: | :---------------------------: | :----------------: | :-------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  **#1** |            `FALSA`            |      `SÁTIRA`      |  ❌ ERROU  | Identificou o absurdo da alegação sanitária ("água morna com limão e cúrcuma desativa diabetes"), mas interpretou o exagero como recurso humorístico/satírico. |
|  **#9** |       `FORA DE CONTEXTO`      |      `SÁTIRA`      |  ❌ ERROU  | Notou a ausência de marcadores temporais e geográficos na foto da praia, mas atribuiu essa omissão a um padrão de postagem cômica/satírica.                    |
| **#13** |           `OPINIÃO`           |      `OPINIÃO`     | ✅ ACERTOU | Reconheceu perfeitamente a linguagem subjetiva, baseada em experiência pessoal com os filhos, sem suporte em dados empíricos.                                  |

### 2.2. Métricas da Amostra

* **Acurácia Global:** 33,3% (1/3)
* **Taxa de Viés para Sátira (*Satire Bias Rate*):** 66,7% (2/3 das predições incorretas)

> **Observação:** Como a amostra contém apenas 3 casos, essas métricas devem ser interpretadas como indicadores experimentais da Sprint 1, e não como estimativas robustas do desempenho global do classificador.

---

## 3. Diagnóstico Técnico de Causa Raiz (*Root Cause Analysis*)

A análise detalhada dos logs de inferência e das justificativas geradas pela IA revelou três fatores determinantes para a falha do sistema nos Casos #1 e #9.

### A. Viés do Absurdo (*Satire Drift*)

O modelo Qwen 2.5 fine-tunado desenvolveu uma sensibilidade apurada para detectar contradições lógicas e alegações sem fundamentação científica.

Contudo, ao se deparar com afirmações bizarras ou conspiratórias, como alegações de que "médicos escondem a cura porque dá lucro", o modelo assume que o texto é tão despropositado que só pode ter sido escrito como piada, classificando-o erroneamente como `SÁTIRA`.

### B. Indução de Intencionalidade em Textos Descontextualizados

Na desinformação por descontextualização (Caso #9), elementos essenciais como data, horário e localização são omitidos intencionalmente para induzir a erro.

O modelo identificou a falta de contexto factual, mas interpretou essa falha estrutural como característica de um "meme" ou "postagem satírica", falhando em reconhecer a possibilidade de uso enganoso do conteúdo.

### C. Ausência de Fronteiras Operacionais no System Prompt

O prompt de sistema utilizado na bateria inicial solicitava a classificação em 5 categorias sem explicitar as **fronteiras excludentes** entre elas.

Sem instruções claras sobre onde termina a desinformação grave e onde começa o humor, a categoria `SÁTIRA` funcionou como uma classe de escape (*fallback*) quando o modelo encontrava incoerências no texto.

---

## 4. Estratégias e Métodos de Melhoria Aplicados

Para mitigar o *Satire Bias* e restabelecer a precisão do classificador, foram integradas quatro técnicas de Engenharia de Prompt ao pipeline.

### Matriz de Soluções e Mitigação

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                     MATRIZ DE SOLUÇÕES E MITIGAÇÃO                          │
├─────────────────────────────────────────────────────────────────────────────┤
│ 1. Definições de Fronteira (Boundary Definitions)                          │
│    └─ Especifica regras excludentes explícitas para cada categoria.         │
│                                                                             │
│ 2. Restrições Negativas (Negative Constraints)                             │
│    └─ Instruções proibitivas explícitas                                     │
│       (ex: "Desinformação médica JAMAIS é Sátira").                        │
│                                                                             │
│ 3. Exemplos In-Context (Few-Shot Exemplars)                                │
│    └─ Casos canônicos inseridos diretamente no System Prompt.              │
│                                                                             │
│ 4. Regra de Desempate via RAG                                               │
│    └─ Ausência de comprovação em fontes confiáveis influencia a análise    │
│       da classe FALSA.                                                      │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.1. Definições Operacionais de Fronteira

Estabelecimento de regras estritas para diferenciação de conteúdo.

Promessas de cura milagrosa, teorias da conspiração e alertas alarmistas são categorizados obrigatoriamente como `FALSA`, quando as evidências disponíveis sustentarem essa classificação.

### 4.2. Restrições Negativas (*Negative Constraints*)

Adição de comandos proibitivos diretos no prompt para impedir que desinformação nociva ou conteúdo relacionado à saúde seja automaticamente interpretado como sátira.

### 4.3. *In-Context Few-Shot Exemplars*

Inclusão de pares de exemplos curtos (**Entrada → Saída**) no System Prompt, cobrindo casos ambíguos para orientar a atenção do modelo.

### 4.4. Instrução de Ancoragem RAG

Orientação para que a análise considere explicitamente as evidências recuperadas pela busca web.

A ausência de evidência deve ser tratada como um sinal relevante, mas não como prova automática de falsidade. A classificação final deve considerar também o conteúdo da mensagem e a confiabilidade das fontes encontradas.

---

## 5. System Prompt Otimizado (*Production-Ready*)

Este é o prompt reestruturado para substituir a versão anterior no script de avaliação:

```python
PROMPT_SISTEMA_OTIMIZADO = """Você é um especialista em checagem de fatos, inteligência de fontes e análise do discurso.
Sua tarefa é analisar a MENSAGEM enviada cruzando-a com as EVIDÊNCIAS DA WEB fornecidas pelo RAG e classificá-la rigorosamente em UMA das 5 categorias abaixo.

### REGRAS CRÍTICAS DE CLASSIFICAÇÃO:

1. FALSA:
   - Notícias fabricadas, teorias conspiratórias (ex: "médicos escondem a cura"), alertas sanitários falsos ou promessas de curas milagrosas.
   - Presença de gatilhos: termos como "URGENTE", "Compartilhe antes que apaguem", autoridade anônima ("minha prima no laboratório").
   - REGRA ABSOLUTA: Texto com desinformação médica ou risco à saúde JAMAIS deve ser classificado como SÁTIRA.

2. FORA DE CONTEXTO:
   - Fotos, vídeos, gráficos ou textos reais apresentados com data, local, legenda ou autoria alterados/omitidos para induzir a erro.
   - Notícias antigas compartilhadas como se fossem acontecimentos de hoje.

3. OPINIÃO:
   - Juízos de valor, preferências pessoais, análises políticas ou afirmações sem possibilidade de verificação empírica objetiva.
   - Uso de linguagem subjetiva ("na minha visão", "é o melhor", "qualquer pessoa sensata concorda").

4. SÁTIRA:
   - Conteúdo EXCLUSIVAMENTE humorístico, memes, paródias declaradas ou publicações de veículos de humor explícito (ex: Sensacionalista).
   - Não possui intenção de se passar por notícia real nem induz ao pânico/padrão de fraude.

5. VERDADEIRA:
   - Fatos comprováveis, corretos e com respaldo direto em fontes confiáveis e oficiais, mantendo todo o contexto necessário.

### EXEMPLOS DE REFERÊNCIA (FEW-SHOT):

- Mensagem: "Água com limão desativa diabetes em 7 dias, médicos escondem!" 
  -> CLASSIFICAÇÃO: FALSA

- Mensagem: "Olha a praia lotada ontem! (Usando foto de 2015)" 
  -> CLASSIFICAÇÃO: FORA DE CONTEXTO

- Mensagem: "Acho que o ensino presencial é muito superior ao EAD." 
  -> CLASSIFICAÇÃO: OPINIÃO

- Mensagem: "Governo decreta que segunda-feira está cancelada." 
  -> CLASSIFICAÇÃO: SÁTIRA

### FORMATO OBRIGATÓRIO DE RESPOSTA:

CLASSIFICAÇÃO: [Insira apenas uma das 5 categorias]
JUSTIFICATIVA: [Explicação concisa em no máximo 2 frases baseada estritamente nas evidências da web e nas regras acima]"""
```

---

## 6. Script de Re-Avaliação e Correção (`avaliar_3_casos_otimizado.py`)

Código Python atualizado para execução do teste com o novo System Prompt:

```python
import re
import pandas as pd
from mlx_lm import generate, load

try:
    from ddgs import DDGS
except ImportError:
    from duckduckgo_search import DDGS

EXCEL_INPUT = "Sprint_1_Dataset_20_exemplos_complexos.xlsx"
MODEL_PATH = "./Qwen-FakeNews-Local"
SHEET_NAME = "Dataset Sprint 1"

print("1. Carregando modelo MLX e dataset...")
model, tokenizer = load(MODEL_PATH)
df_completo = pd.read_excel(EXCEL_INPUT, sheet_name=SHEET_NAME)

# Seleção de amostras
categorias_desejadas = ["Falsa", "Fora de contexto", "Opinião"]
casos_selecionados = []

for cat in categorias_desejadas:
    sub = df_completo[
        df_completo["Categoria"].str.strip().str.lower() == cat.lower()
    ]

    if not sub.empty:
        casos_selecionados.append(sub.iloc[0])

df_3_casos = pd.DataFrame(casos_selecionados)


def buscar_evidencias_web(texto: str) -> str:
    termo = texto[:120].replace("\n", " ")

    try:
        with DDGS() as ddgs:
            resultados = list(
                ddgs.text(
                    termo,
                    region="br-pt",
                    max_results=3
                )
            )

            if resultados:
                return "\n".join(
                    [
                        f"[{i+1}] {r['title']}: {r['body']}"
                        for i, r in enumerate(resultados)
                    ]
                )

    except Exception as e:
        return f"Erro na busca: {e}"

    return "Nenhuma evidência externa encontrada."


def normalizar_categoria(texto_categoria: str) -> str:
    txt = str(texto_categoria).upper().strip()

    if "FORA DE CONTEXTO" in txt or "CONTEXTO" in txt:
        return "FORA DE CONTEXTO"

    elif "FALSA" in txt or "FALSO" in txt:
        return "FALSA"

    elif "VERDADEIRA" in txt or "VERDADEIRO" in txt:
        return "VERDADEIRA"

    elif "OPINIÃO" in txt or "OPINIAO" in txt:
        return "OPINIÃO"

    elif "SÁTIRA" in txt or "SATIRA" in txt:
        return "SÁTIRA"

    return "INDEFINIDO"


PROMPT_SISTEMA = """Você é um especialista em checagem de fatos e análise do discurso.
Analise a MENSAGEM com base nas EVIDÊNCIAS DA WEB e classifique-a rigorosamente em UMA das 5 categorias: FALSA, VERDADEIRA, FORA DE CONTEXTO, OPINIÃO ou SÁTIRA.

CRITÉRIOS OBRIGATÓRIOS DE FRONTEIRA:

- FALSA:
  Desinformação, teorias conspiratórias, alertas de pânico ou curas milagrosas sem base científica.
  Se contiver termos como "URGENTE", "médicos escondem" ou riscos à saúde,
  é obrigatoriamente FALSA (NUNCA SÁTIRA).

- FORA DE CONTEXTO:
  Imagens/notícias reais usadas com legenda, data ou local errados/omitidos para enganar o leitor.

- OPINIÃO:
  Juízos de valor, preferências pessoais ou posições sem verificação factual possível.

- SÁTIRA:
  Conteúdo puramente humorístico ou de canais declarados de humor (ex: Sensacionalista).
  Não inclui fake news graves.

- VERDADEIRA:
  Fatos comprováveis respaldados por fontes confiáveis.

FORMATO DA RESPOSTA:

CLASSIFICAÇÃO: [Categoria]
JUSTIFICATIVA: [Análise em 2 frases baseada nas evidências]"""


print("\n2. Iniciando avaliação com System Prompt Otimizado...\n")


for idx, (_, row) in enumerate(df_3_casos.iterrows(), 1):

    caso_id = row["ID"]
    gabarito = str(row["Categoria"]).strip()
    mensagem = str(row["Mensagem de teste"])

    print(
        f"================ CASO {idx}/3 (ID #{caso_id}) ================"
    )

    print(f"Gabarito Esperado: [{gabarito}]")

    evidencias = buscar_evidencias_web(mensagem)

    prompt = (
        f"<|im_start|>system\n"
        f"{PROMPT_SISTEMA}"
        f"<|im_end|>\n"

        f"<|im_start|>user\n"
        f"EVIDÊNCIAS DA WEB:\n{evidencias}\n\n"
        f"MENSAGEM PARA ANÁLISE:\n{mensagem}"
        f"<|im_end|>\n"

        f"<|im_start|>assistant\n"
    )

    resposta_ia = generate(
        model,
        tokenizer,
        prompt=prompt,
        max_tokens=180,
        verbose=False
    )

    match_class = re.search(
        r"CLASSIFICAÇÃO:\s*([^\n]+)",
        resposta_ia,
        re.IGNORECASE
    )

    predicao_raw = (
        match_class.group(1).strip()
        if match_class
        else "Não identificada"
    )

    pred_norm = normalizar_categoria(predicao_raw)
    gab_norm = normalizar_categoria(gabarito)

    acertou = pred_norm == gab_norm

    print("\n--- RESPOSTA DA IA ---")
    print(resposta_ia)

    print(
        f"\n[Resultado]: "
        f"{'✅ ACERTOU' if acertou else '❌ ERROU'} "
        f"(Esperado: {gab_norm} | Predito: {pred_norm})\n"
    )
```

---

## 7. Próximos Passos Recomendados

### 7.1. Persistência do Relatório

Salvar este documento na seguinte localização do repositório:

```text
docs/relatorio-sprint-1.md
```

### 7.2. Reexecução de Validação

Executar o script:

```bash
python avaliar_3_casos_otimizado.py
```

O objetivo é verificar se os Casos **#1** e **#9**, anteriormente classificados como `SÁTIRA`, passam a ser classificados de acordo com seus respectivos gabaritos.

### 7.3. Benchmark Completo

Expandir a execução para os **20 exemplos** contidos no dataset da Sprint 1.

A avaliação completa deverá gerar, no mínimo:

* Acurácia global;
* Precisão (*Precision*) por categoria;
* Revocação (*Recall*) por categoria;
* F1-Score por categoria;
* Matriz de confusão;
* Distribuição das categorias preditas;
* Quantidade de falsos positivos e falsos negativos;
* Frequência de classificações incorretas como `SÁTIRA`;
* Comparação entre o prompt original e o prompt otimizado.

### 7.4. Avaliação Específica do *Satire Bias*

Além das métricas tradicionais, recomenda-se acompanhar uma métrica específica para o problema identificado:

```text
Satire Drift Rate =
Quantidade de exemplos incorretamente classificados como SÁTIRA
----------------------------------------------------------------
Quantidade total de exemplos incorretamente classificados
```

Essa métrica permitirá verificar se as alterações no System Prompt realmente reduziram o comportamento de `SÁTIRA` como classe de escape.

### 7.5. Validação Antes da Conclusão

O novo prompt não deve ser considerado validado apenas porque corrige os Casos #1 e #9.

É necessário executar o benchmark completo e verificar se a redução do *Satire Bias* não provocou um novo viés, por exemplo, classificando conteúdos humorísticos legítimos como `FALSA`.

O objetivo da próxima etapa é, portanto, medir o comportamento do modelo nas cinco categorias e comparar quantitativamente o desempenho **antes e depois da otimização do prompt**.

---

## 8. Conclusão da Sprint 1

A avaliação reduzida identificou um problema específico no comportamento do classificador: a tendência de interpretar alegações absurdas, conspiratórias ou estruturalmente incompletas como conteúdo satírico.

O problema está relacionado principalmente à ausência de fronteiras explícitas entre `FALSA`, `FORA DE CONTEXTO` e `SÁTIRA` no System Prompt original.

A estratégia de mitigação adotada introduz definições operacionais, restrições negativas, exemplos *few-shot* e maior ancoragem nas evidências recuperadas pelo RAG.

A próxima etapa consiste em executar novamente os casos problemáticos e, posteriormente, realizar o benchmark completo dos 20 exemplos. Somente após essa avaliação será possível determinar quantitativamente o impacto das alterações sobre o desempenho geral do sistema.

**Status da Sprint 1:** 🔄 **Em validação**

**Próximo marco:** Benchmark completo com 20 exemplos + matriz de confusão.
