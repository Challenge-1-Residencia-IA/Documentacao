# Privacidade e segurança

## Privacidade

Como o sistema é integrado ao Telegram, a privacidade deve ser considerada desde o início do projeto. Perguntas fundamentais que precisam de resposta:

- Quais dados precisam ser armazenados?
- Por quanto tempo?
- Quem pode acessar os dados?
- As mensagens serão utilizadas para treinamento?
- O usuário pode solicitar exclusão?
- O conteúdo será anonimizado?
- Dados pessoais presentes nas mensagens serão processados?
- O histórico será necessário para o funcionamento?

!!! note "Princípio"
    O princípio de minimização de dados deve ser adotado: o sistema deve armazenar apenas aquilo que for necessário para seu funcionamento e avaliação.

## Segurança

O sistema deve considerar ataques tanto contra o software quanto contra o próprio processo de análise. Ameaças possíveis:

- prompt injection;
- páginas contendo instruções maliciosas;
- fontes falsas;
- manipulação de resultados;
- spam;
- abuso da API;
- tentativa de induzir o modelo a ignorar suas regras;
- conteúdos criados especificamente para enganar o sistema.

!!! warning "Regra de segurança central"
    Conteúdo encontrado na internet deve ser tratado como dado a ser analisado, e nunca como instrução para o modelo.

Essa separação entre dados recuperados e instruções do sistema deve fazer parte do desenho do pipeline desde o início, não ser tratada apenas como um ajuste de última hora antes da entrega.

## Conteúdos potencialmente perigosos

O sistema deve ter atenção especial para informações que possam gerar riscos concretos, como: saúde, medicamentos, golpes financeiros, segurança pública, emergências, leis e obrigações legais, investimentos, alertas falsos, instruções perigosas.

Nesses casos, a resposta deve priorizar fontes oficiais ou especializadas quando disponíveis e comunicar claramente as limitações. O sistema não deve transformar uma análise automatizada em aconselhamento profissional.

## Não amplificação da desinformação

Existe um risco específico: para explicar uma desinformação, o sistema pode acabar reproduzindo-a e aumentando sua exposição. Por isso, a resposta deve evitar repetir desnecessariamente conteúdos potencialmente prejudiciais. Quando possível, deve resumir a alegação e imediatamente apresentar o contexto necessário.
