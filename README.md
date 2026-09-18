# Documentacao

Site publicado com [MkDocs](https://www.mkdocs.org/) via GitHub Pages: https://Challenge-1-Residencia-IA.github.io/Documentacao/

## Ambiente virtual Python (macOS)

Crie o ambiente virtual uma única vez, na raiz do repositório:

```bash
python3 -m venv .venv
```

Ative o ambiente (necessário em cada novo terminal):

```bash
source .venv/bin/activate
```

Com o ambiente ativo, instale as dependências:

```bash
pip install -r requirements.txt
```

Para sair do ambiente virtual:

```bash
deactivate
```

## Rodando localmente

Com o ambiente virtual ativo:

```bash
mkdocs serve
```

Site em `http://127.0.0.1:8000`. O `mkdocs serve` recarrega automaticamente a cada alteração nos arquivos de `docs/` ou no `mkdocs.yml`.

## Deploy

Automático via GitHub Actions (`.github/workflows/docs.yml`) a cada push em `main`. No repositório, em **Settings > Pages**, a fonte (Source) precisa estar definida como **GitHub Actions**.

## Como adicionar uma nova seção e documento

Cada página do site é um arquivo Markdown dentro de `docs/`, e o menu (nav) do site é montado manualmente no `mkdocs.yml`.

1. Crie o arquivo Markdown dentro de `docs/`, por exemplo `docs/nova-pagina.md`:

   ```bash
   touch docs/nova-pagina.md
   ```

2. Escreva o conteúdo da página em Markdown normal, começando com um título:

   ```markdown
   # Título da nova página

   Conteúdo aqui.
   ```

3. Abra o `mkdocs.yml` e adicione uma entrada na lista `nav` no final do arquivo, apontando para o novo arquivo:

   ```yaml
   nav:
     - Início: index.md
     - Visão do Projeto:
         - Visão geral: visao-geral.md
     # ...
     - Nova seção:
         - Nova página: nova-pagina.md
   ```

   Para adicionar uma página dentro de uma seção já existente (por exemplo, em "Produto"), basta incluir a nova linha dentro da lista dessa seção:

   ```yaml
   - Produto:
       - Casos de uso: casos-de-uso.md
       - Nova página: nova-pagina.md
   ```

4. Rode `mkdocs serve` e confirme que a nova página aparece no menu lateral e que o conteúdo renderiza como esperado.
5. Faça commit dos arquivos novos/alterados (`docs/nova-pagina.md` e `mkdocs.yml`) e dê push em `main`: o deploy do site acontece automaticamente.

Um arquivo em `docs/` que não estiver listado no `nav` ainda é publicado (acessível pela URL direta), só não aparece no menu.