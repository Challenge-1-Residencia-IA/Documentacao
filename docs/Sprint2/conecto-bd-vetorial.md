📄 Documentação — Conexão com Banco de Dados Vetorial na Nuvem (Neon)

Data: 08/10/2026
Responsável: Larissa Giffoni (Pessoa 3)
Sprint: Sprint 2 — Núcleo do Sistema
Status: ✅ Concluído

🎯 Objetivo

Configurar um banco de dados vetorial na nuvem, gratuito, que funcione mesmo com o bloqueio de rede da faculdade, para armazenar os padrões de fake news e o histórico de análises do projeto.

🧐 O Problema Encontrado

A faculdade bloqueia conexões de saída para portas de banco de dados (como a porta 5432 do PostgreSQL). Isso impedia que o computador se conectasse a qualquer banco de dados na nuvem. O erro era:

text
connection to server at "...", port 5432 failed: timeout expired
Mesmo tentando pela porta 443 (HTTPS), o firewall continuava bloqueando.

🛠️ A Solução Implementada

Usamos o Neon (banco de dados PostgreSQL gratuito na nuvem) combinado com o driver neon-serverless, que faz as conexões via HTTPS em vez de TCP. Como a porta 443 (HTTPS) é a mesma usada para navegar na internet, o firewall da faculdade não bloqueia.

Ferramentas Utilizadas

Ferramenta	Para Que Serve
Neon	Banco de dados PostgreSQL na nuvem (gratuito)
pgvector	Extensão que adiciona capacidade vetorial ao PostgreSQL
neon-serverless	Driver Python que conecta via HTTPS (contorna o firewall)
📋 Passo a Passo do Que Foi Feito

1. Criação do Banco no Neon

Criamos uma conta em neon.tech.
Criamos o projeto copiloto-desinformacao.
Ativamos a extensão pgvector rodando no SQL Editor:

sql
CREATE EXTENSION IF NOT EXISTS vector;
2. Instalação do Driver

bash
pip3 install neon-serverless
3. Código de Teste

python
from neon_serverless import neon

DATABASE_URL = "postgresql://<usuario>:<senha>@<host-removido>/<db>?sslmode=require"

sql = neon(DATABASE_URL)

rows = sql("SELECT version();")
print(f"Versão do Postgres: {rows[0]['version'][:50]}...")

vector_check = sql("SELECT extversion FROM pg_extension WHERE extname = 'vector';")
print(f"pgvector ativo, versão: {vector_check[0]['extversion']}")
4. Resultado do Teste

text
1. Tentando conectar ao Neon via HTTPS...
2. Conectado com sucesso!
Versão do Postgres: PostgreSQL 18.6
pgvector ativo, versão: 0.8.6

[OK] Tudo funcionando!
✅ O Que Já Está Pronto

Item	Status
Banco de dados PostgreSQL na nuvem (Neon)	✅ Criado
Extensão pgvector ativada	✅ Versão 0.8.6
Conexão do Python com o Neon	✅ Funcionando
Firewall da faculdade contornado	✅ Via HTTPS (porta 443)
📌 Próximos Passos

Integrar o Neon no servidor.py para salvar o histórico de análises.
Criar as tabelas do banco (histórico de consultas, padrões de fake news).
Popular o banco com o dataset Fake.br para reconhecimento de padrões.
📎 Links Úteis

Neon: https://neon.tech
Documentação do neon-serverless: https://pypi.org/project/neon-serverless/
Documentação criada por: Larissa Giffoni
Revisada por: (a preencher pela equipe)

