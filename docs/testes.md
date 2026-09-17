# Testes

## Casos que devem ser testados

O conjunto de testes deve conter pelo menos:

- informação verdadeira;
- informação falsa;
- informação verdadeira fora de contexto;
- informação desatualizada;
- opinião apresentada como fato;
- sátira;
- informação sem fonte;
- informação com fonte falsa;
- informação com fontes divergentes;
- informação com múltiplas cópias da mesma fonte;
- informação recente;
- informação com evidências insuficientes;
- informação potencialmente perigosa;
- mensagem com várias afirmações;
- usuário apresentando uma nova fonte;
- usuário contestando a análise;
- fonte contendo instruções maliciosas;
- link indisponível;
- fonte que exige autenticação;
- informação cujo significado depende do contexto.

## Dataset inicial para testes

A equipe deve montar um conjunto inicial de informações para avaliar o comportamento do sistema, cobrindo diferentes situações: informações verdadeiras, falsas, fora de contexto, desatualizadas, opiniões apresentadas como fatos, informações com evidências insuficientes, informações com fontes conflitantes e mensagens com múltiplas afirmações.

Esse conjunto é usado para identificar falhas e comparar diferentes abordagens técnicas, incluindo a escolha de modelo por subtarefa (ver [Métricas e avaliação](metricas-e-avaliacao.md)).

No plano de desenvolvimento do MVP, esse dataset é formalizado como aproximadamente 30 mensagens, cada uma com um gabarito para comparação, definido já nas duas primeiras semanas de desenvolvimento.
