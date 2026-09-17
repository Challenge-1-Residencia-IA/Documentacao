# Riscos

## Riscos do projeto

### Risco 1: Alucinação

A IA pode inventar informações ou interpretar incorretamente uma fonte.

**Mitigação**: recuperação de fontes, citações, validação, comunicação de incerteza.

### Risco 2: Viés da IA

O sistema pode selecionar fontes que favoreçam uma determinada conclusão.

**Mitigação**: buscar deliberadamente evidências favoráveis e contrárias.

### Risco 3: Dependência

O usuário pode parar de pensar criticamente.

**Mitigação**: perguntas reflexivas e avaliações sem assistência.

### Risco 4: Falsa sensação de certeza

Um score pode ser interpretado como verdade absoluta.

**Mitigação**: priorizar evidências e contexto em vez de uma nota única.

### Risco 5: Fontes falsas

Uma página pode parecer confiável sem ser.

**Mitigação**: avaliação de origem, contexto, autoria e independência.

### Risco 6: Informação desatualizada

A IA pode encontrar fontes antigas.

**Mitigação**: consideração explícita da data.

### Risco 7: Amplificação

A resposta pode reproduzir desinformação.

**Mitigação**: resumir alegações e priorizar contexto e evidências.

### Risco 8: Privacidade

Mensagens podem conter informações pessoais.

**Mitigação**: minimização de dados e política clara de retenção.

### Risco 9: Prompt injection

Conteúdo externo pode tentar manipular o comportamento da IA.

**Mitigação**: separação entre dados recuperados e instruções do sistema, validação das fontes e políticas de segurança. Essa separação deve ser parte do design do pipeline desde as primeiras semanas, não apenas da etapa final de robustez.

## Riscos do cronograma de desenvolvimento

| Risco | Probabilidade | Mitigação |
|---|---|---|
| Atraso na integração com Telegram | Alta | Iniciar o webhook o quanto antes e manter uma alternativa caso a integração apresente problemas |
| Qualidade das evidências da Tavily | Média | Testar consultas reais no início do projeto e avaliar alternativas caso necessário |
| Limites dos LLMs gratuitos | Média | Manter alternativas de provedores disponíveis |
| Pipeline muito lento | Média | Reduzir a quantidade de fontes analisadas caso necessário |
| Alucinação na síntese | Alta | Utilizar a etapa de crítica e exigir evidências para as conclusões |
| Aumento do escopo | Alta | Manter as funcionalidades fora do MVP para uma etapa posterior |

!!! warning "Risco mais importante para o cronograma"
    O risco mais importante para o cronograma é o aumento do escopo. O objetivo do período de desenvolvimento é entregar o núcleo funcional do sistema, e não uma versão completa do produto.
