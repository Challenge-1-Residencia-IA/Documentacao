# Requisitos

## Requisitos funcionais

| ID | Descrição |
|---|---|
| RF01 | O sistema deve permitir o envio de mensagens textuais. |
| RF02 | O sistema deve permitir o envio de links. |
| RF03 | O sistema deve identificar afirmações verificáveis. |
| RF04 | O sistema deve classificar a natureza das afirmações. |
| RF05 | O sistema deve buscar fontes relacionadas às afirmações. |
| RF06 | O sistema deve apresentar evidências favoráveis e contrárias quando disponíveis. |
| RF07 | O sistema deve identificar divergências relevantes entre fontes. |
| RF08 | O sistema deve apresentar as fontes utilizadas na análise. |
| RF09 | O sistema deve apresentar contexto relevante. |
| RF10 | O sistema deve comunicar limitações e incertezas. |
| RF11 | O sistema deve permitir perguntas adicionais do usuário. |
| RF12 | O sistema deve permitir que o usuário apresente novas fontes. |
| RF13 | O sistema deve atualizar a análise quando novas evidências relevantes forem apresentadas. |
| RF14 | O sistema deve incentivar o usuário a realizar uma avaliação própria. |
| RF15 | O sistema não deve apresentar uma determinação absoluta de verdade ou falsidade como resultado principal. |
| RF16 | O sistema deve diferenciar ausência de evidências de evidências de falsidade. |
| RF17 | O sistema deve considerar a data das fontes quando o contexto temporal for relevante. |
| RF18 | O sistema deve identificar possíveis relações entre fontes quando existirem evidências suficientes. |

## Requisitos não funcionais

| ID | Nome | Descrição |
|---|---|---|
| RNF01 | Privacidade | O sistema deve minimizar a coleta e retenção de dados pessoais. |
| RNF02 | Segurança | O sistema deve ter mecanismos contra abuso e manipulação das entradas. |
| RNF03 | Transparência | As principais fontes utilizadas devem ser apresentadas ao usuário. |
| RNF04 | Usabilidade | A interação deve ser compreensível para usuários sem conhecimento técnico. |
| RNF05 | Acessibilidade | As respostas devem ser adequadas ao formato conversacional do Telegram. |
| RNF06 | Desempenho | O sistema deve apresentar respostas em tempo aceitável, considerando que a análise depende de serviços externos. |
| RNF07 | Confiabilidade | Falhas de serviços externos não devem resultar em respostas apresentadas como conclusões confiáveis. |
| RNF08 | Rastreabilidade | As evidências utilizadas na geração de uma resposta devem poder ser identificadas. |
| RNF09 | Atualização | A arquitetura deve permitir atualização das fontes e componentes utilizados para análise. |
