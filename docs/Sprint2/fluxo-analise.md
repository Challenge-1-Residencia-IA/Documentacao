📄 Documentação — Integração do Banco Vetorial no Fluxo de Análise

Data: 08/10/2026
Responsável: Larissa Giffoni (Pessoa 3)
Sprint: Sprint 2 — Núcleo do Sistema
Status: ✅ Concluído

🎯 Objetivo

Integrar o banco de dados vetorial (Neon + pgvector) ao fluxo de análise do bot, para que a IA use três fontes de informação:

A mensagem do usuário.
As evidências da web (RAG).
Os padrões similares do banco vetorial (dataset Fake.br).
🧐 O Que Foi Feito

1. Criação da Tabela padroes_fake_news no Neon

sql
CREATE TABLE IF NOT EXISTS padroes_fake_news (
    id SERIAL PRIMARY KEY,
    texto TEXT,
    label VARCHAR(20),
    embedding vector(384)
);
2. Download e População do Dataset Fake.br

Baixamos o dataset vzani/corpus-fake-br do Hugging Face.
Geramos embeddings de 5.219 textos usando o modelo all-MiniLM-L6-v2.
Inserimos tudo na tabela padroes_fake_news do Neon.
3. Criação da Função de Busca Vetorial no busca_web.py

python
def buscar_padroes_similares(mensagem, limite=3):
    """Busca no banco vetorial os textos mais similares à mensagem."""
    _inicializar_busca_vetorial()
    
    embedding = _modelo_embedding.encode(mensagem).tolist()
    
    resultados = _sql(
        """
        SELECT texto, label, embedding <=> $1::vector AS distancia
        FROM padroes_fake_news
        ORDER BY distancia
        LIMIT $2
        """,
        [json.dumps(embedding), limite]
    )
    
    return resultados
4. Integração no servidor.py

O servidor.py agora faz:

Busca evidências na web.
Busca padrões similares no banco vetorial.
Passa tudo para a IA analisar.
Salva o resultado no banco.
✅ Resultado do Teste

text
[INFO] Recebida mensagem: A Terra é plana
[INFO] Buscando evidencias na web...
[INFO] Encontradas 3 evidencias
[INFO] Buscando padroes similares no banco...
[INFO] Encontrados 3 padroes
[INFO] Analisando com IA...
[INFO] Salvando no banco de dados...
[INFO] Salvo com sucesso!
INFO:     127.0.0.1:62019 - "POST /analisar HTTP/1.1" 200 OK
📊 O Que o Sistema Faz Agora

text
Usuário manda mensagem
        ↓
API (FastAPI) recebe
        ↓
Busca na web (RAG) ──────────► Evidências da web
        ↓
Busca no banco vetorial ─────► Padrões similares (Fake.br)
        ↓
IA (Ollama + Qwen 2.5 7B) analisa TUDO
        ↓
Salva no banco (Neon) ───────► Histórico de análises
        ↓
Devolve resposta ao usuário
📌 Próximos Passos

Testar o sistema com mais mensagens (sátira, opinião, etc.).
Documentar os resultados no Trello.
Preparar o deploy (subir a API para a nuvem).
Documentação criada por: Larissa Giffoni
Revisada por: (a preencher pela equipe)

