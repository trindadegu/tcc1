# TRILHA — landing page

Landing page em português brasileiro, servida por Django 5 com templates. O projeto não possui banco de dados de aplicação nem API; todas as imagens da página ficam no repositório em `static/img/`.

## Requisitos

- Python 3.12
- Dependência Python listada e fixada em `requirements.txt`

## Executar localmente

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python manage.py runserver
```

Abra `http://127.0.0.1:8000/` no navegador. A página inicial é servida pela view `config.views.landing_page`.

## Compilar o CSS

O arquivo compilado `static/css/app.css` é versionado e servido diretamente pelo Django. Para regenerá-lo, baixe o executável standalone Tailwind CSS v4.3.5 adequado ao seu sistema operacional da [página oficial de releases](https://github.com/tailwindlabs/tailwindcss/releases/tag/v4.3.5). O exemplo abaixo usa Linux x86_64 e não requer Node.js:

```bash
curl -fL https://github.com/tailwindlabs/tailwindcss/releases/download/v4.3.5/tailwindcss-linux-x64 -o /tmp/tailwindcss
chmod +x /tmp/tailwindcss
/tmp/tailwindcss -i static/src/input.css -o static/css/app.css --minify
```

A fonte do CSS está em `static/src/input.css`; ela inclui os tokens de tema e os estilos da landing. As fontes Fraunces e Manrope são carregadas pelo Google Fonts. O menu e as perguntas frequentes usam elementos HTML nativos, sem JavaScript de interação.

## Estrutura

```text
manage.py              comandos Django
config/                settings, URLs, view da landing e WSGI
templates/base.html    metadados, fontes, CSS e estrutura comum
templates/landing.html conteúdo da página
static/src/input.css  fonte Tailwind e estilos da página
static/css/app.css    CSS compilado e versionado
static/img/           logos e ilustração local
docs/visuals.md       identidade visual e diretrizes
requirements.txt      dependências Python fixadas
```
