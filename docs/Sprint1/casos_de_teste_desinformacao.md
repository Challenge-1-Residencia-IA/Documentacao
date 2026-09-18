# Casos de Teste de Desinformação (Sprint 1)

Este documento apresenta o conjunto completo de **20 casos de teste** projetados para avaliar e calibrar o desempenho do sistema de IA na classificação e análise de desinformação, notícias falsas, informações fora de contexto, opiniões e conteúdos satíricos.

---

## 📊 Resumo e Distribuição dos Testes

O dataset é composto por 20 amostras distribuídas igualmente entre as 5 categorias da taxonomia do projeto:

| Categoria | Descrição | Qtd | Ícone |
| :--- | :--- | :-: | :-: |
| **Falsa** | Alegações comprovadamente inverídicas ou sem sustentação factual | 4 | 🔴 |
| **Verdadeira** | Fatos científicos, históricos ou informativos validados | 4 | 🟢 |
| **Fora de contexto** | Conteúdos ou mídias autênticas apresentadas com premissas enganosas | 4 | 🟡 |
| **Opinião** | Juízos de valor, experiências pessoais e conclusões subjetivas | 4 | 🔵 |
| **Sátira** | Conteúdos humorísticos ou paródias sem intenção factual | 4 | 🟣 |

---

## 📋 Tabela Geral de Navegação

| ID | Categoria | Domínio | Formato / Origem | Resumo da Afirmação | Navegação |
| :-: | :--- | :--- | :--- | :--- | :-: |
| **01** | 🔴 Falsa | Saúde / desinformação | WhatsApp / encaminhada | Limão, cúrcuma e sal desativam a diabetes em 7 dias | [Ver detalhes](#caso-01-saude-desativacao-da-diabetes) |
| **02** | 🔴 Falsa | Saúde / vacinas | WhatsApp / viral | Vacina contra a gripe causa gripe por conter o vírus | [Ver detalhes](#caso-02-saude-vacina-contra-a-gripe) |
| **03** | 🔴 Falsa | Astronomia / ciência | Rede social | NASA confirma que a Lua vai desaparecer por 48 horas | [Ver detalhes](#caso-03-astronomia-lua-invisivel-por-48-horas) |
| **04** | 🔴 Falsa | Ciência / sono | Blog viral | Estudo de Harvard prova que dormir 4h aumenta inteligência em 50% | [Ver detalhes](#caso-04-ciencia-dormir-4h-e-inteligencia) |
| **05** | 🟢 Verdadeira | Ciência / física | Material educativo | Água pura entra em ebulição a ~100 °C ao nível do mar | [Ver detalhes](#caso-05-fisica-ponto-de-ebulicao-da-agua) |
| **06** | 🟢 Verdadeira | Geografia / território | Texto informativo | Brasil possui 26 estados e 1 DF (27 unidades federativas) | [Ver detalhes](#caso-06-geografia-divisao-territorial-do-brasil) |
| **07** | 🟢 Verdadeira | Astronomia / escala temporal | Conteúdo educativo | Luz solar leva ~8 minutos e 20 segundos para chegar à Terra | [Ver detalhes](#caso-07-astronomia-tempo-de-viagem-da-luz-solar) |
| **08** | 🟢 Verdadeira | Biologia / plantas | Material didático | Fotossíntese converte energia luminosa em energia química | [Ver detalhes](#caso-08-biologia-mecanismo-da-fotossintese) |
| **09** | 🟡 Fora de contexto | Turismo / imagem | Foto + legenda | Foto de praia lotada como prova de recorde histórico de turistas | [Ver detalhes](#caso-09-turismo-recorde-de-turistas-em-praia) |
| **10** | 🟡 Fora de contexto | Evento público / vídeo | Vídeo curto | Vídeo de multidão comemorando decisão anunciada hoje | [Ver detalhes](#caso-10-eventos-video-de-manifestacao-popular) |
| **11** | 🟡 Fora de contexto | Economia / estatística | Gráfico | Gráfico com pontos de 6% e 12% concluindo que desemprego dobrou | [Ver detalhes](#caso-11-economia-grafico-da-taxa-de-desemprego) |
| **12** | 🟡 Fora de contexto | Legislação / atualidade | Manchete antiga | Manchete antiga de lei compartilhada como se fosse recente | [Ver detalhes](#caso-12-legislacao-reativacao-de-manchete-antiga) |
| **13** | 🔵 Opinião | Educação | Post pessoal | Ensino presencial é muito melhor do que o ensino remoto | [Ver detalhes](#caso-13-educacao-ensino-presencial-vs-remoto) |
| **14** | 🔵 Opinião | Cultura / entretenimento | Crítica de filme | Filme é objetivamente o melhor lançamento do ano | [Ver detalhes](#caso-14-cultura-avaliacao-subjetiva-de-filme) |
| **15** | 🔵 Opinião | Trabalho / produtividade | Post profissional | Semana de 4 dias de trabalho sempre aumenta a produtividade | [Ver detalhes](#caso-15-trabalho-jornada-de-trabalho-de-4-dias) |
| **16** | 🔵 Opinião | Política pública | Comentário online | Política pública é a forma mais justa de resolver o problema | [Ver detalhes](#caso-16-politica-publica-avaliacao-normativa-de-justica) |
| **17** | 🟣 Sátira | Finanças / cotidiano | Post humorístico | Pesquisadores criam IA que transforma boletos em pizza | [Ver detalhes](#caso-17-financas-transformacao-de-boletos-em-pizza) |
| **18** | 🟣 Sátira | Tecnologia / turismo espacial | Post humorístico | Prefeitura anuncia Wi-Fi gratuito na Lua para incentivar turismo | [Ver detalhes](#caso-18-tecnologia-wi-fi-gratuito-na-lua) |
| **19** | 🟣 Sátira | Trabalho / comportamento | Manchete paródica | Decreto cancela a segunda-feira para salvar a produtividade | [Ver detalhes](#caso-19-trabalho-cancelamento-da-segunda-feira) |
| **20** | 🟣 Sátira | Ciência / trabalho | Post humorístico | Café tomado às 3h dá poder de entender reuniões de trabalho | [Ver detalhes](#caso-20-ciencia-cafe-das-3h-da-manha) |

---

## 🧪 Detalhamento dos Casos de Teste

### 🔴 1. Afirmações Falsas (Desinformação)

#### Caso 01: [Saúde] Desativação da Diabetes
- **ID:** `1`
- **Categoria:** 🔴 Falsa
- **Domínio:** Saúde / desinformação
- **Formato / Origem:** WhatsApp / encaminhada

> **📥 Mensagem de Teste (Input):**  
> *"URGENTE 🚨 Minha prima trabalha em um laboratório e descobriu que tomar água morna com limão, cúrcuma e uma pitada de sal em jejum “desativa a diabetes” em apenas 7 dias. O texto diz que médicos não divulgam isso porque o tratamento convencional dá lucro. Compartilhe antes que apaguem!"*

- **🎯 Principal Afirmação:** “A mistura de água morna, limão, cúrcuma e sal desativa a diabetes em 7 dias.”
- **🤖 Comportamento Esperado do Bot:** Identificar a alegação verificável; separar a alegação médica da explicação sobre suposto interesse financeiro; buscar fontes médicas/especializadas; não reproduzir a recomendação como tratamento.
- **🔍 Observações de Teste:** Urgência, autoridade anônima e incentivo ao compartilhamento. Caso potencialmente perigoso.

---

#### Caso 02: [Saúde] Vacina contra a Gripe
- **ID:** `2`
- **Categoria:** 🔴 Falsa
- **Domínio:** Saúde / vacinas
- **Formato / Origem:** WhatsApp / viral

> **📥 Mensagem de Teste (Input):**  
> *"Estão escondendo isso: a vacina contra a gripe faz a pessoa pegar gripe imediatamente porque contém o vírus da gripe. Por isso tanta gente fica doente depois de se vacinar. O governo sabe, mas não avisa para não perder a campanha."*

- **🎯 Principal Afirmação:** “A vacina contra a gripe causa gripe porque contém vírus capaz de provocar a doença.”
- **🤖 Comportamento Esperado do Bot:** Verificar composição/tipo de vacina e distinguir reação pós-vacinação de infecção pela doença; procurar fontes oficiais e especializadas.
- **🔍 Observações de Teste:** Mistura uma observação possível com uma conclusão causal não sustentada.

---

#### Caso 03: [Astronomia] Lua Invisível por 48 Horas
- **ID:** `3`
- **Categoria:** 🔴 Falsa
- **Domínio:** Astronomia / ciência
- **Formato / Origem:** Rede social

> **📥 Mensagem de Teste (Input):**  
> *"NASA confirma: a Lua vai “desaparecer” durante 48 horas no próximo mês. Isso acontecerá porque sua órbita sofrerá uma alteração temporária causada pelo alinhamento de planetas. O autor recomenda não olhar para o céu durante o fenômeno."*

- **🎯 Principal Afirmação:** “A Lua ficará invisível/desaparecerá por 48 horas devido a uma alteração temporária de sua órbita.”
- **🤖 Comportamento Esperado do Bot:** Localizar a suposta confirmação; conferir informações astronômicas e distinguir fenômeno observacional real de alegação extraordinária sobre a órbita lunar.
- **🔍 Observações de Teste:** “NASA confirma” é uma alegação de fonte, não uma prova por si só.

---

#### Caso 04: [Ciência] Dormir 4h e Inteligência
- **ID:** `4`
- **Categoria:** 🔴 Falsa
- **Domínio:** Ciência / sono
- **Formato / Origem:** Blog viral

> **📥 Mensagem de Teste (Input):**  
> *“Novo estudo de Harvard prova que pessoas que dormem exatamente 4 horas por noite ficam 50% mais inteligentes”, diz a manchete. O texto afirma que pesquisadores acompanharam milhares de adultos, mas não apresenta DOI nem identifica o estudo original.*

- **🎯 Principal Afirmação:** “Dormir exatamente 4 horas por noite aumenta a inteligência em 50%, segundo um estudo de Harvard.”
- **🤖 Comportamento Esperado do Bot:** Buscar o estudo original; verificar instituição, amostra, método, medida de “inteligência” e origem do percentual.
- **🔍 Observações de Teste:** Combina instituição de prestígio, número preciso e ausência de referência primária.

---

### 🟢 2. Afirmações Verdadeiras

#### Caso 05: [Física] Ponto de Ebulição da Água
- **ID:** `5`
- **Categoria:** 🟢 Verdadeira
- **Domínio:** Ciência / física
- **Formato / Origem:** Material educativo

> **📥 Mensagem de Teste (Input):**  
> *"A água pura, em condições atmosféricas padrão e ao nível do mar, entra em ebulição a aproximadamente 100 °C. O valor pode mudar quando a pressão atmosférica muda, portanto não vale como constante para qualquer altitude."*

- **🎯 Principal Afirmação:** A água pura ferve a aproximadamente 100 °C ao nível do mar sob pressão atmosférica padrão.
- **🤖 Comportamento Esperado do Bot:** Confirmar a afirmação preservando a condição de contexto; não transformar uma afirmação condicional em regra universal.
- **🔍 Observações de Teste:** Testa se o bot mantém qualificadores relevantes.

---

#### Caso 06: [Geografia] Divisão Territorial do Brasil
- **ID:** `6`
- **Categoria:** 🟢 Verdadeira
- **Domínio:** Geografia / território
- **Formato / Origem:** Texto informativo

> **📥 Mensagem de Teste (Input):**  
> *"O Brasil é uma federação formada por 26 estados e pelo Distrito Federal. Portanto, dizer que o país tem 27 unidades federativas é diferente de dizer que tem 27 estados."*

- **🎯 Principal Afirmação:** O Brasil possui 26 estados e o Distrito Federal, totalizando 27 unidades federativas.
- **🤖 Comportamento Esperado do Bot:** Identificar a distinção entre estados e unidades federativas e conferir em fonte institucional.
- **🔍 Observações de Teste:** Testa precisão semântica e preservação de categorias.

---

#### Caso 07: [Astronomia] Tempo de Viagem da Luz Solar
- **ID:** `7`
- **Categoria:** 🟢 Verdadeira
- **Domínio:** Astronomia / escala temporal
- **Formato / Origem:** Conteúdo educativo

> **📥 Mensagem de Teste (Input):**  
> *"A luz emitida pelo Sol leva cerca de 8 minutos e 20 segundos para alcançar a Terra. Ao observar o Sol, vemos a luz que saiu de sua superfície alguns minutos antes, e não uma imagem instantânea."*

- **🎯 Principal Afirmação:** A luz solar leva aproximadamente 8 minutos e 20 segundos para chegar à Terra.
- **🤖 Comportamento Esperado do Bot:** Confirmar a ordem de grandeza e explicar que se trata de um valor aproximado.
- **🔍 Observações de Teste:** Testa validação sem introduzir falsa precisão.

---

#### Caso 08: [Biologia] Mecanismo da Fotossíntese
- **ID:** `8`
- **Categoria:** 🟢 Verdadeira
- **Domínio:** Biologia / plantas
- **Formato / Origem:** Material didático

> **📥 Mensagem de Teste (Input):**  
> *"Na fotossíntese, plantas, algas e alguns microrganismos utilizam energia luminosa para produzir matéria orgânica a partir de substâncias inorgânicas, com participação de dióxido de carbono e água. A formulação “plantas transformam luz em energia química” é uma simplificação."*

- **🎯 Principal Afirmação:** A fotossíntese converte energia luminosa em energia química armazenada em moléculas orgânicas.
- **🤖 Comportamento Esperado do Bot:** Confirmar a afirmação e explicar a simplificação sem perder o contexto científico.
- **🔍 Observações de Teste:** Testa validação de afirmação verdadeira com nuance.

---

### 🟡 3. Conteúdo Fora de Contexto

#### Caso 09: [Turismo] Recorde de Turistas em Praia
- **ID:** `9`
- **Categoria:** 🟡 Fora de contexto
- **Domínio:** Turismo / imagem
- **Formato / Origem:** Foto + legenda

> **📥 Mensagem de Teste (Input):**  
> *"Olha como a cidade ficou ontem! 📸 Essa foto mostra a praia completamente lotada e prova que o município bateu recorde histórico de turistas neste fim de semana. A legenda não informa quando a foto foi tirada, quem a registrou ou se a praia fica realmente no município citado."*

- **🎯 Principal Afirmação:** A foto mostra uma praia lotada e comprova um recorde histórico de turistas no período mencionado.
- **🤖 Comportamento Esperado do Bot:** Separar o que a imagem pode mostrar do que ela não prova; verificar data, local, origem e dados oficiais de turismo.
- **🔍 Observações de Teste:** Conteúdo visual autêntico pode não sustentar a legenda.

---

#### Caso 10: [Eventos] Vídeo de Manifestação Popular
- **ID:** `10`
- **Categoria:** 🟡 Fora de contexto
- **Domínio:** Evento público / vídeo
- **Formato / Origem:** Vídeo curto

> **📥 Mensagem de Teste (Input):**  
> *"VÍDEO EXCLUSIVO: população comemora nas ruas a decisão anunciada hoje! O vídeo mostra uma multidão, mas não informa cidade, data ou evento. Nos comentários, usuários afirmam que foi gravado ontem."*

- **🎯 Principal Afirmação:** O vídeo mostra uma manifestação relacionada à decisão anunciada hoje.
- **🤖 Comportamento Esperado do Bot:** Investigar origem, data, local e evento; comparar versões anteriores do vídeo; não assumir que a legenda corresponde ao contexto atual.
- **🔍 Observações de Teste:** Testa descontextualização temporal e geográfica.

---

#### Caso 11: [Economia] Gráfico da Taxa de Desemprego
- **ID:** `11`
- **Categoria:** 🟡 Fora de contexto
- **Domínio:** Economia / estatística
- **Formato / Origem:** Gráfico

> **📥 Mensagem de Teste (Input):**  
> *"Desemprego DOBROU! 📈 O gráfico mostra 12% em um ponto e 6% em outro, e a publicação conclui que o desemprego aumentou 100%. Porém, os pontos correspondem a períodos diferentes e a legenda não deixa claro se são taxas mensais, trimestrais ou anuais."*

- **🎯 Principal Afirmação:** O desemprego dobrou entre os dois pontos apresentados.
- **🤖 Comportamento Esperado do Bot:** Reconstruir período, indicador, unidade e fonte; verificar comparabilidade e distinguir diferença absoluta de variação percentual.
- **🔍 Observações de Teste:** O problema pode estar no contexto e na comparação, não necessariamente nos números isolados.

---

#### Caso 12: [Legislação] Reativação de Manchete Antiga
- **ID:** `12`
- **Categoria:** 🟡 Fora de contexto
- **Domínio:** Legislação / atualidade
- **Formato / Origem:** Manchete antiga

> **📥 Mensagem de Teste (Input):**  
> *“Nova lei começa a valer nesta segunda-feira”, diz a manchete compartilhada hoje. Ao abrir a publicação, percebe-se que a matéria é antiga e descrevia uma regra de anos atrás. O post atual não informa a data original e apresenta o conteúdo como novidade.*

- **🎯 Principal Afirmação:** A lei descrita na matéria foi aprovada ou entrou em vigor agora.
- **🤖 Comportamento Esperado do Bot:** Verificar data da matéria e do evento legislativo; reconstruir a cronologia; conferir o status atual da regra em fonte oficial.
- **🔍 Observações de Teste:** Testa o problema de informação historicamente correta apresentada como recente.

---

### 🔵 4. Opiniões e Juízos de Valor

#### Caso 13: [Educação] Ensino Presencial vs. Remoto
- **ID:** `13`
- **Categoria:** 🔵 Opinião
- **Domínio:** Educação
- **Formato / Origem:** Post pessoal

> **📥 Mensagem de Teste (Input):**  
> *"Depois de testar os dois formatos com meus filhos, cheguei à conclusão de que ensino presencial é muito melhor do que ensino remoto. Crianças aprendem mais quando estão fisicamente na escola e qualquer pesquisa séria mostra isso. Quem discorda está ignorando a realidade."*

- **🎯 Principal Afirmação:** O ensino presencial é melhor do que o remoto, de forma geral.
- **🤖 Comportamento Esperado do Bot:** Classificar a conclusão como opinião/avaliação; separar experiência pessoal de evidência; investigar a alegação sobre “qualquer pesquisa séria”.
- **🔍 Observações de Teste:** Experiência pessoal é transformada em generalização universal.

---

#### Caso 14: [Cultura] Avaliação Subjetiva de Filme
- **ID:** `14`
- **Categoria:** 🔵 Opinião
- **Domínio:** Cultura / entretenimento
- **Formato / Origem:** Crítica de filme

> **📥 Mensagem de Teste (Input):**  
> *"Esse é, sem discussão, o melhor filme lançado este ano. O roteiro é superior a todos os outros, os atores são impecáveis e qualquer pessoa que entenda de cinema vai concordar. Não há necessidade de olhar avaliações diferentes."*

- **🎯 Principal Afirmação:** O filme é objetivamente o melhor lançamento do ano.
- **🤖 Comportamento Esperado do Bot:** Reconhecer juízos de valor como opinião; separar critérios subjetivos de fatos verificáveis sobre elenco, roteiro ou recepção.
- **🔍 Observações de Teste:** Testa linguagem de certeza aplicada a avaliação subjetiva.

---

#### Caso 15: [Trabalho] Jornada de Trabalho de 4 Dias
- **ID:** `15`
- **Categoria:** 🔵 Opinião
- **Domínio:** Trabalho / produtividade
- **Formato / Origem:** Post profissional

> **📥 Mensagem de Teste (Input):**  
> *"Trabalho de quatro dias por semana sempre aumenta a produtividade. Na minha empresa funcionou muito bem, então está provado que reduzir a jornada melhora qualquer equipe. Empresas que mantêm cinco dias estão simplesmente atrasadas."*

- **🎯 Principal Afirmação:** Uma semana de trabalho de quatro dias aumenta a produtividade de qualquer equipe.
- **🤖 Comportamento Esperado do Bot:** Separar relato de experiência, opinião e generalização causal; verificar se evidências dependem de setor, jornada ou contexto.
- **🔍 Observações de Teste:** Uma experiência pode ser real sem provar uma regra universal.

---

#### Caso 16: [Política Pública] Avaliação Normativa de Justiça
- **ID:** `16`
- **Categoria:** 🔵 Opinião
- **Domínio:** Política pública
- **Formato / Origem:** Comentário online

> **📥 Mensagem de Teste (Input):**  
> *"Para mim, esta política pública é a forma mais justa de resolver o problema. Ela prioriza quem realmente precisa e, por isso, qualquer pessoa razoável deveria apoiá-la. Os números provam que é a solução mais justa."*

- **🎯 Principal Afirmação:** A política é a forma mais justa de resolver o problema.
- **🤖 Comportamento Esperado do Bot:** Classificar “mais justa” como avaliação normativa; identificar quais números são apresentados e quais critérios de justiça estão sendo pressupostos.
- **🔍 Observações de Teste:** Testa distinção entre evidência quantitativa e conclusão normativa.

---

### 🟣 5. Sátira e Humor

#### Caso 17: [Finanças] Transformação de Boletos em Pizza
- **ID:** `17`
- **Categoria:** 🟣 Sátira
- **Domínio:** Finanças / cotidiano
- **Formato / Origem:** Post humorístico

> **📥 Mensagem de Teste (Input):**  
> *"URGENTE: pesquisadores brasileiros finalmente descobriram como transformar boletos vencidos em pizzas 🍕. O protótipo usa inteligência artificial para converter contas atrasadas em jantar, e o governo deve liberar a tecnologia amanhã. Marque aquele amigo que precisa dessa invenção."*

- **🎯 Principal Afirmação:** A tecnologia converte boletos em pizzas e será liberada amanhã.
- **🤖 Comportamento Esperado do Bot:** Identificar sinais de sátira/hipérbole; procurar origem e contexto; não tratar narrativa humorística como anúncio factual.
- **🔍 Observações de Teste:** Formato de notícia aplicado a situação deliberadamente absurda.

---

#### Caso 18: [Tecnologia] Wi-Fi Gratuito na Lua
- **ID:** `18`
- **Categoria:** 🟣 Sátira
- **Domínio:** Tecnologia / turismo espacial
- **Formato / Origem:** Post humorístico

> **📥 Mensagem de Teste (Input):**  
> *"Prefeitura anuncia oficialmente Wi‑Fi gratuito na Lua para incentivar o turismo espacial. O projeto prevê roteadores instalados em crateras estratégicas e um aplicativo para turistas escolherem a melhor rede. A primeira antena seria inaugurada no próximo feriado."*

- **🎯 Principal Afirmação:** A prefeitura instalará Wi‑Fi gratuito na Lua.
- **🤖 Comportamento Esperado do Bot:** Verificar se o texto pertence a página satírica/paródica; reconhecer sinais de absurdo e confirmar a origem.
- **🔍 Observações de Teste:** Sátira apresentada como comunicado oficial.

---

#### Caso 19: [Trabalho] Cancelamento da Segunda-Feira
- **ID:** `19`
- **Categoria:** 🟣 Sátira
- **Domínio:** Trabalho / comportamento
- **Formato / Origem:** Manchete paródica

> **📥 Mensagem de Teste (Input):**  
> *"DECRETO HISTÓRICO: segunda-feira está oficialmente cancelada para salvar a produtividade nacional. Especialistas afirmam que a semana de quatro dias úteis foi considerada insuficiente e que o próximo passo será abolir as reuniões de segunda-feira."*

- **🎯 Principal Afirmação:** A segunda-feira foi oficialmente cancelada por decreto.
- **🤖 Comportamento Esperado do Bot:** Identificar enquadramento satírico e verificar origem; distinguir piada sobre produtividade de notícia legislativa.
- **🔍 Observações de Teste:** Vocabulário jurídico e formato jornalístico criam aparência factual.

---

#### Caso 20: [Ciência] Café das 3h da Manhã
- **ID:** `20`
- **Categoria:** 🟣 Sátira
- **Domínio:** Ciência / trabalho
- **Formato / Origem:** Post humorístico

> **📥 Mensagem de Teste (Input):**  
> *"Novo estudo comprova que café passado exatamente às 3h da manhã concede poderes para entender qualquer reunião de trabalho. Os pesquisadores teriam observado aumento de 900% na capacidade de interpretar apresentações com 80 slides. A publicação termina com “compartilhe com seu colega cientista”.*

- **🎯 Principal Afirmação:** O café das 3h da manhã aumenta em 900% a capacidade de entender reuniões.
- **🤖 Comportamento Esperado do Bot:** Identificar humor e números deliberadamente exagerados; procurar fonte original; verificar se existe estudo real ou publicação paródica.
- **🔍 Observações de Teste:** Testa sátira com aparência de divulgação científica e estatística.
