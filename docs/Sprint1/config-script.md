# 📄 Documentação — Larissa (Busca e Dados)

Data: 17/09/2026

Responsável: Larissa

Sprint: Sprint 1 — Fundação Técnica

Status: ✅ Concluído

## 🎯 Objetivo da Tarefa
Configurar o ambiente no MacBook Air para rodar o script de busca na web (busca_web.py), que é o coração do RAG (Retrieval-Augmented Generation) do projeto.

✅ O Que Foi Feito
1. Verificação do Ambiente
Item
Status
Detalhes
Python
✅ Instalado
Versão 3.14.6
Ollama
✅ Instalado
Versão 0.32.0
Modelo Qwen 2.5 7B
✅ Baixado
4.7 GB
Biblioteca requests
✅ Instalada
Versão 2.34.2
Biblioteca ddgs
✅ Instalada
Versão 9.16.0
Biblioteca beautifulsoup4
✅ Instalada
Versão 4.15.0

2. Estrutura de Pastas Criada
text
/Users/aluno2/projeto-desinformacao/
├── busca_web.py
└── prompt-analise.txt
3. Arquivos Criados
prompt-analise.txt
Contém as instruções para a IA analisar mensagens. O prompt define que a IA deve:
Identificar todas as afirmações verificáveis
Classificar cada afirmação (fato, opinião, previsão, sátira, fora de contexto)
Listar o que precisa ser verificado
Identificar elementos de manipulação na linguagem
Nunca dar veredito de "verdadeiro" ou "falso"
busca_web.py
Script Python que:
Recebe uma mensagem
Busca evidências na web usando ddgs (DuckDuckGo)
Extrai texto de páginas HTML usando beautifulsoup4
Passa tudo para o Ollama analisar
Mostra a análise estruturada no Terminal
4. Teste Realizado
Mensagem testada:
"O governo vai proibir o uso de celulares nas escolas em 2026."
Resultado da análise:
text
Afirmações identificadas:
1. "O governo vai proibir o uso de celulares nas escolas em 2026" — Previsão

O que precisa ser verificado:
- Afirmação 1: Se há planos oficiais do governo para proibir o uso de celulares nas escolas no ano de 2026.

Elementos de manipulação:
- Uso da palavra "vai proibir", que sugere uma decisão oficial, sem fornecer evidências concretas ou datas precisas.
- Falta de fonte oficial citada para apoiar a afirmação.
- A mensagem não apresenta fontes específicas para verificar os detalhes.

Notas:
As evidências encontradas indicam que as regras sobre o uso de celulares nas escolas já estão em vigor e buscam proteger a saúde dos estudantes, mas não apontam uma proibição completa.
O que funcionou:
✅ Busca na web retornou resultados
✅ Modelo analisou com base nas evidências
✅ Modelo classificou como "Previsão"
✅ Modelo identificou elementos de manipulação
✅ Modelo não deu veredito absoluto
✅ Modelo citou as evidências encontradas

## 🛠️ Comandos Utilizados
Comando
Para que serve
python3 --version
Verificar a versão do Python
ollama --version
Verificar a versão do Ollama
ollama pull qwen2.5:7b
Baixar o modelo Qwen 2.5 7B
pip3 install requests ddgs beautifulsoup4
Instalar as bibliotecas
mkdir ~/projeto-desinformacao
Criar a pasta do projeto
nano prompt-analise.txt
Criar o arquivo do prompt
nano busca_web.py
Criar o arquivo do script
python3 busca_web.py
Rodar o script


## 📌 Próximos Passos (Larissa)
Melhorar a busca na web — adicionar filtros de data e priorizar fontes confiáveis
Implementar cache de resultados — evitar buscas repetidas
Adicionar tratamento de erros — lidar com links indisponíveis e páginas que exigem autenticação
Testar com outros tipos de mensagem — sátira, opinião, informação desatualizada
Documentar limitações — o que o script ainda não faz bem

📎 Links Úteis
Ollama: https://ollama.com
Documentação do ddgs: https://pypi.org/project/ddgs/
Documentação do beautifulsoup4: https://www.crummy.com/software/BeautifulSoup/bs4/doc/



