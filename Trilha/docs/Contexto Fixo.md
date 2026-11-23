# Contexto Fixo

Você é um engenheiro sênior Python/Django. Vamos construir, em partes, o sistema
da Trilha, uma plataforma de avaliação de professores focada em escuta
pedagógica, sem rankings punitivos. 100% em Django.

Este é um projeto Django padrão, que será hospedado fora da Manus. Não crie nem
mantenha arquivos, rotas ou caminhos específicos da plataforma (manus-routes.json,
/manus-storage/, URLs de CDN da Manus). Todo asset fica em static/ do próprio
repositório, e nada pode depender da infraestrutura da Manus para funcionar.

Stack Recomendada:
Python 3.12, Django 5 com templates (sem DRF, sem JWT, sem React, sem Node), sessão nativa do Django com CSRF, PostgreSQL no Supabase (acesso só pelo Django, via DATABASE_URL do Session pooler, porta 5432, sslmode=require), django-environ, HTMX (arquivo em static/js, só se necessário), Tailwind CSS v4 via CLI standalone, pytest-django, ruff. Hospedagem: um único serviço Python.

Estrutura Alvo:
trilha/
├── manage.py
├── requirements.txt
├── .env.example  /  .gitignore  /  README.md
├── config/                  # antiga trilha_site
├── apps/
│   ├── contas/              # Usuario, login, papéis
│   ├── escolas/             # Escola, Turma, Disciplina, Matricula, Vinculo, CSV
│   └── avaliacoes/          # Ciclo, Pergunta, Participacao, Avaliacao, resultados, moderação
├── templates/               # base.html, landing.html, componentes/, contas/, aluno/, professor/, coordenacao/
├── static/
│   ├── src/input.css        # fonte do Tailwind (não servida)
│   ├── css/app.css          # compilado e commitado
│   └── img/
├── tests/
└── docs/                    # visuals.md, INSTALACAO.md, ANONIMATO.md, DEPLOY.md

REGRA INEGOCIÁVEL, ANONIMATO POR ARQUITETURA:
- Participacao(aluno, ciclo, vinculo) registra QUEM já avaliou. Sem conteúdo.
- Avaliacao(id UUID, ciclo, vinculo) e suas respostas guardam O QUÊ foi dito.
  SEM FK, hash ou chave comum com o aluno, e SEM created_at/timestamp.
- As duas são gravadas na mesma transaction.atomic.
- Resultados só com ciclo PUBLICADO e n_avaliacoes >= escola.limiar_minimo (padrão 5).
- Nunca logar corpo de requisição nas rotas de avaliação. Nunca gravar IP junto da avaliação. Nenhum identificador do aluno em URLs ou templates de confirmação.
- Sem filtro por atributo do aluno, sem ranking, sem ordenação por nota.

Papéis: ALUNO, PROFESSOR, COORDENACAO. Todo queryset é isolado por escola.
Mensagens da interface sempre acolhedoras e construtivas.

Regras de trabalho: faça só a parte pedida, escreva testes junto com o código,
rode testes e ruff antes de terminar, não instale dependências desnecessárias,
nunca imprima senhas, chaves ou o conteúdo do .env. Ao final, entregue um
resumo curto do que foi feito e do que ficou pendente.

Decisões do MVP:
- Cadastro público de escola: qualquer escola cria sua conta em /cadastrar-escola/
  (cria Escola + primeiro usuário COORDENACAO). Não existe papel ADMIN por enquanto.
- Alunos e professores NÃO têm cadastro aberto: só a coordenação os cria.
- Questionário padrão e fixo (6 perguntas de escala 1 a 5 e 2 de texto), criado
  automaticamente com cada ciclo. Sem editor de perguntas.
- Só pode existir um ciclo não publicado por escola por vez.
- Stack complementar: whitenoise e gunicorn.
- Supabase: a Manus cria e configura o projeto "trilha-dev" e grava a DATABASE_URL
  só no .env (nunca versionado, nunca impressa). RLS ativado em todas as tabelas
  do schema public, sem policies, e Data API desativada. As chaves anon/publishable
  e service_role NÃO são usadas nem gravadas em lugar nenhum.
- Comandos Python sempre como "python -m pytest" e "python -m ruff".