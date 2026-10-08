📄 README — Integração com Banco de Dados Vetorial na Nuvem (Neon)

Data: 08/10/2026
Responsável: Larissa Giffoni (Pessoa 3)
Sprint: Sprint 2 — Núcleo do Sistema
Status: ✅ Concluído

🎯 Objetivo

Configurar um banco de dados vetorial na nuvem, gratuito, que funcione mesmo com o bloqueio de rede da faculdade, para armazenar os padrões de fake news e o histórico de análises do projeto.

🧐 O Problema Encontrado

O Bloqueio da Rede da Faculdade

A rede da faculdade bloqueia conexões de saída para portas de banco de dados (como a porta 5432 do PostgreSQL). Isso significa que o computador da faculdade não consegue se conectar a um banco de dados na nuvem usando o protocolo padrão (TCP).

O erro que aparecia era:

text
connection to server at "...", port 5432 failed: timeout expired
connection to server at "...", port 443 failed: timeout expired
Ou seja, nem a porta padrão (5432) nem a porta HTTPS (443) funcionavam com o driver tradicional (psycopg2).

Por Que Isso Acontece

Redes de faculdade (e de empresas) costumam bloquear conexões de saída para portas de banco de dados e para serviços de nuvem por segurança. Eles fazem isso para evitar que alunos rodem servidores ou acessem dados externos.

🛠️ A Solução Implementada

A Descoberta: Driver neon-serverless

O Neon oferece um driver Python chamado neon-serverless que faz as conexões via HTTPS (porta 443) em vez de TCP. Como a porta 443 é a mesma usada para navegar na internet, o firewall da faculdade não bloqueia.

A diferença é que, em vez de abrir uma conexão TCP persistente, o driver envia requisições HTTP POST para o servidor do Neon. Isso "disfarça" o tráfego de banco de dados como tráfego web normal.

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
2. Instalação do Driver neon-serverless

O driver tradicional (psycopg2) não funciona na rede da faculdade, porque usa TCP na porta 5432. Por isso, instalamos o neon-serverless:

bash
pip3 install neon-serverless
3. Teste da Conexão via HTTPS

O código de teste ficou assim:

python
from neon_serverless import neon

DATABASE_URL = "postgresql://<usuario>:<senha>@<host-removido>/<db>?sslmode=require"

print("1. Tentando conectar ao Neon via HTTPS...")

sql = neon(DATABASE_URL)

rows = sql("SELECT version();")
print(f"2. Conectado com sucesso!")
print(f"Versão do Postgres: {rows[0]['version'][:50]}...")

vector_check = sql("SELECT extversion FROM pg_extension WHERE extname = 'vector';")
print(f"pgvector ativo, versão: {vector_check[0]['extversion']}")

print("\n[OK] Tudo funcionando!")
4. Resultado do Teste

text
1. Tentando conectar ao Neon via HTTPS...
2. Conectado com sucesso!
Versão do Postgres: PostgreSQL 18.6
pgvector ativo, versão: 0.8.6

[OK] Tudo funcionando!
Isso provou que a conexão via HTTPS funciona mesmo com o firewall da faculdade.

5. Criação da Tabela de Análises

No SQL Editor do Neon, rodamos:

sql
CREATE TABLE IF NOT EXISTS analises (
    id SERIAL PRIMARY KEY,
    data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    mensagem TEXT,
    evidencias JSONB,
    analise TEXT,
    modelo VARCHAR(50)
);
6. Integração no servidor.py

O servidor.py foi alterado para usar o driver neon-serverless e salvar cada análise no banco:

python
from neon_serverless import neon
import json

DATABASE_URL = "postgresql://<usuario>:<senha>@<host-removido>/<db>?sslmode=require"
sql = neon(DATABASE_URL)

# ... dentro da função analisar() ...

print("[INFO] Salvando no banco de dados...")
try:
    sql(
        """
        INSERT INTO analises (mensagem, evidencias, analise, modelo)
        VALUES ($1, $2, $3, $4)
        """,
        [
            mensagem.texto,
            json.dumps(evidencias),
            analise,
            "qwen2.5:7b"
        ]
    )
    print("[INFO] Salvo com sucesso!")
except Exception as e:
    print(f"[ERRO] Falha ao salvar no banco: {e}")
7. Resultado Final do Teste

text
[INFO] Recebida mensagem: A Terra é plana.
[INFO] Buscando evidencias na web...
[INFO] Encontradas 3 evidencias
[INFO] Analisando com IA...
[INFO] Salvando no banco de dados...
[INFO] Salvo com sucesso!
INFO:     127.0.0.1:60490 - "POST /analisar HTTP/1.1" 200 OK
✅ O Que Já Está Pronto

Item	Status
Banco de dados PostgreSQL na nuvem (Neon)	✅ Criado
Extensão pgvector ativada	✅ Versão 0.8.6
Firewall da faculdade contornado	✅ Via HTTPS (neon-serverless)
Conexão do Python com o Neon	✅ Funcionando
Tabela analises criada	✅
API salvando no banco	✅
⚠️ Limitações e Cuidados

Cold Start (Primeira Conexão)

O Neon desliga o banco após 5 minutos de inatividade. A primeira consulta depois disso demora cerca de 300 a 500 milissegundos para "acordar". Isso é imperceptível para o usuário final.

Limite do Plano Gratuito

O plano gratuito dá 100 CU-Hours por mês, o que equivale a cerca de 400 horas de uso contínuo. Para uma feira de 8 horas, o consumo é de apenas 2 CU-Hours (2% do limite).

📌 Próximos Passos

Popular o banco com o dataset Fake.br para reconhecimento de padrões.
Criar a tabela de padrões de fake news com coluna vetorial.
Integrar a busca vetorial no fluxo de análise.
📎 Links Úteis

Neon: https://neon.tech
Documentação do neon-serverless: https://pypi.org/project/neon-serverless/
pgvector: https://github.com/pgvector/pgvector
Documentação criada por: Larissa Giffoni
Revisada por: (a preencher pela equipe)

