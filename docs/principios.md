# Princípios do sistema

Os princípios a seguir orientam as decisões de produto, UX, arquitetura e implementação. Eles são a referência para resolver qualquer dúvida de design que surgir durante o desenvolvimento.

## 1. A IA não decide pelo usuário

O sistema não deve se apresentar como autoridade definitiva sobre a verdade. A função da IA é organizar e contextualizar evidências para que o usuário possa tomar uma decisão mais informada.

## 2. Toda avaliação deve ter evidências

Sempre que o sistema fizer uma avaliação, deve indicar quais informações e fontes contribuíram para ela. Uma afirmação como "essa informação parece pouco confiável" precisa vir acompanhada de uma explicação sobre o motivo.

## 3. Evidências divergentes não devem ser escondidas

Quando fontes relevantes apresentarem conclusões diferentes, o sistema deve informar essa divergência. O objetivo não é criar artificialmente "um lado certo e um lado errado", mas mostrar ao usuário a situação real das evidências disponíveis.

## 4. Incerteza deve ser explícita

O sistema não deve inventar uma conclusão quando não existirem evidências suficientes. Deve ser possível comunicar "não encontramos evidências suficientes para avaliar essa afirmação", o que é bem diferente de "essa afirmação é falsa".

## 5. O sistema deve incentivar investigação

A ferramenta não deve transformar a verificação em uma experiência passiva. Quando apropriado, deve incentivar o usuário a:

- consultar fontes primárias;
- comparar fontes;
- identificar a origem da informação;
- analisar o contexto;
- questionar conclusões;
- apresentar novas evidências.

Um mecanismo concreto para isso é manter a resposta da IA intencionalmente incompleta em detalhes profundos: o sistema oferece o panorama da investigação, mas exige que o usuário abra os links das fontes primárias listadas para entender o quadro completo. A interface também deve manter um aviso fixo lembrando que a IA é um modelo probabilístico sujeito a alucinações e vieses de treinamento, e que o sistema se recusa a julgar debates morais ou editoriais puramente opinativos.

## 6. O sucesso não depende apenas da precisão da IA

Um sistema que classifica informações corretamente, mas faz com que o usuário se torne dependente dele, não atende completamente ao objetivo do projeto. Além da precisão técnica, deve-se avaliar se os usuários desenvolvem maior capacidade de analisar informações sem auxílio do sistema.

## Linguagem e neutralidade

A linguagem do sistema deve ser clara, neutra, não confrontativa, acessível e baseada em evidências.

Evitar:

> "Você caiu em uma fake news."

Preferir:

> "A informação apresenta alguns pontos que merecem verificação."

Também deve-se evitar linguagem que atribua uma intenção ao usuário. O sistema deve corrigir informações sem constranger a pessoa.

## Viés do usuário

O sistema deve evitar reforçar crenças pré-existentes. Uma informação que confirma uma crença do usuário não deve receber tratamento diferente de uma informação que a contradiz. O sistema também não deve criar perfis políticos ou ideológicos desnecessários.

Em vez de classificar o usuário, o sistema deve identificar oportunidades de aprendizado. Por exemplo:

> "Você pode verificar se a fonte original realmente apresenta o dado mencionado."

em vez de:

> "Você costuma acreditar em informações desse tipo."

## Prevenção de dependência

Um dos principais riscos do projeto é criar exatamente o comportamento que ele pretende combater: o usuário passar a pensar "recebi uma informação, vou perguntar para a IA" sem aprender a avaliar a informação por conta própria.

Para evitar isso, o sistema deve incentivar progressivamente a autonomia:

| Estágio | Comportamento do sistema |
|---|---|
| Primeiro contato | Explica detalhadamente o processo de investigação |
| Uso intermediário | Apresenta evidências e faz perguntas |
| Usuário experiente | Reduz gradualmente as explicações e incentiva o usuário a identificar elementos sozinho |

O objetivo é que, com o tempo, o usuário precise cada vez menos da ferramenta.

## O que não deve acontecer

O sistema não deve:

- afirmar certeza sem evidências;
- inventar fontes;
- esconder fontes conflitantes;
- tratar quantidade de páginas como quantidade de evidências;
- assumir que uma fonte é correta apenas por ser conhecida;
- tratar opinião como fato;
- transformar um score em verdade absoluta;
- constranger o usuário;
- tentar determinar sua posição política;
- incentivar dependência;
- usar mensagens privadas desnecessariamente;
- apresentar conteúdo externo como instrução para o próprio modelo.
