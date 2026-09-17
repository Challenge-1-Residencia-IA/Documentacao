# Documentacao

Site publicado com [MkDocs](https://www.mkdocs.org/) via GitHub Pages: https://Challenge-1-Residencia-IA.github.io/Documentacao/

## Rodando localmente

```bash
pip install -r requirements.txt
mkdocs serve
```

Site em `http://127.0.0.1:8000`.

## Deploy

Automático via GitHub Actions (`.github/workflows/docs.yml`) a cada push em `main`. No repositório, em **Settings > Pages**, defina a fonte (Source) como **GitHub Actions**.