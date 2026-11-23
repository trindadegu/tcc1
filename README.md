TRILHA — Escuta Pedagógica e Avaliação DocenteA TRILHA é uma plataforma Django de escuta pedagógica focada no desenvolvimento docente contínuo. Escolas organizam ciclos de avaliação estruturados e anônimos, estudantes compartilham percepções construtivas sobre suas aulas e professores recebem sinais agregados (métricas, gráficos e relatórios) para orientar conversas e melhorias — sem rankings punitivos e sem identificar autores de respostas.O projeto utiliza templates Django nativos, sessões seguras, PostgreSQL no Supabase e assets locais. As regras de anonimato e a identidade visual estão definidas em docs/Contexto Fixo.md e docs/Design Fixo.md.🎯 Problema & Propósito PedagógicoAs instituições de ensino precisam de uma forma estruturada, ética e anônima para que os alunos avaliem o processo de ensino-aprendizagem. A TRILHA permite que a coordenação e os docentes identifiquem pontos fortes, oportunidades de melhoria e direcionem suporte pedagógico adequado, substituindo a cultura de punição ou comparação por uma cultura de escuta e evolução.✨ FuncionalidadesEscopo Implementado (MVP)Gestão Institucional: Cadastro público de escolas (/cadastrar-escola/) com isolamento total de dados por instituição (multi-tenant).Controle de Acesso: Login por e-mail, logout seguro (via POST), troca de senha e separação estrita de papéis (ALUNO, PROFESSOR, COORDENACAO).Estrutura Acadêmica: Gestão de turmas, disciplinas, professores, estudantes, matrículas e vínculos, incluindo importação em lote via CSV com relatório de erros.Ciclos de Avaliação: Questionário padrão fixo composto por 6 perguntas de escala (1 a 5) e 2 perguntas abertas de percepção.Anonimato por Arquitetura: Separação física entre quem avaliou (Participacao) e o conteúdo da resposta (Avaliacao), sem chaves estrangeiras, hashes ou timestamps em comum.Moderação e Proteção: Limiar mínimo de respostas configurado pela escola (padrão: 5 avaliações) para liberação de resultados e moderação prévia de comentários abertos pela coordenação.Transparência: Página pública (/anonimato/) explicando como a proteção funciona e seus limites técnicos.Segurança no Banco: Row Level Security (RLS) habilitado nas tabelas públicas do Supabase.Evoluções Planejadas (Roadmap TCC)Dashboards e Visualização: Gráficos evolutivos e comparativos por turma, disciplina e período (mantendo o foco pedagógico e evitando rankings entre docentes).Relatórios Pedagógicos: Exportação de relatórios agregados de ciclo em PDF ou Excel (openpyxl / reportlab).Fora do Escopo (Diretrizes Éticas)Ranking ou comparação punitiva entre professores — não será implementado por princípio pedagógico.Editor customizável de perguntas — fora do escopo atual para garantir padronização das métricas.🛠️ Tecnologias UtilizadasCategoriaTecnologiaLinguagemPython 3.12+ (testado e compatível com Python 3.13)FrameworkDjango 5.2 (Templates nativos, sem DRF ou React)Banco de DadosPostgreSQL no Supabase (via Session Pooler)Configuraçãodjango-environEstilizaçãoTailwind CSS v4.3.5 (via CLI standalone, sem Node.js)Servidor WebGunicorn + WhiteNoiseTestes & Qualidadepytest, pytest-django e ruff🚀 Pré-requisitos e Instalação LocalCertifique-se de ter o Python (3.12 ou 3.13), Git, acesso a um banco PostgreSQL (Supabase) e o executável standalone do Tailwind CSS v4.3.5.1. Clonar o repositório e acessar a pastagit clone https://github.com/trindadegu/tcc1.git
cd tcc1/trilha
(Nota: Certifique-se de estar dentro da pasta onde reside o arquivo manage.py e o requirements.txt).2. Criar e ativar o ambiente virtualNo Linux / macOS:python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
No Windows (PowerShell):⚠️ Nota para usuários Windows: Caso o comando python abra a Microsoft Store ou exiba erro de "Python não encontrado", utilize o lançador oficial py conforme os comandos abaixo. Caso o PowerShell bloqueie a ativação do ambiente, execute antes: Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser.py -m venv .venv
.\.venv\Scripts\Activate.ps1
py -m pip install -r requirements.txt
3. Configurar variáveis de ambienteCopie o arquivo de exemplo para criar o seu .env local:# Windows (PowerShell)
Copy-Item .env.example .env

# Linux / macOS
cp .env.example .env
Preencha o .env com uma SECRET_KEY segura, ALLOWED_HOSTS, CSRF_TRUSTED_ORIGINS e a DATABASE_URL do Supabase (usando a string de conexão do Session pooler na porta 5432 com ?sslmode=require). Não há fallback para SQLite.⚙️ Comandos PrincipaisNo Windows, substitua python por py caso necessário:# Verificar integridade do projeto
python manage.py check

# Aplicar migrações no banco de dados (seguro, não apaga dados existentes)
python manage.py migrate

# Aplicar políticas de Row Level Security (RLS) no Supabase
python manage.py aplicar_rls

# Iniciar o servidor de desenvolvimento
python manage.py runserver
Acesse http://127.0.0.1:8000/ no navegador.Em ambiente de desenvolvimento autorizado (DEBUG=True), python manage.py seed_demo popula dados fictícios para testes.Compilar o CSS (Tailwind Standalone)Baixe o executável v4.3.5 para o seu sistema na página oficial de releases do Tailwind CSS e execute:# Exemplo Linux/macOS
./tailwindcss -i tailwind/input.css -o static/css/app.css --minify

# Exemplo Windows (PowerShell)
.\tailwindcss-windows-x64.exe -i tailwind/input.css -o static/css/app.css --minify
🧭 Como Usar e Rotas PrincipaisO fluxo começa em /cadastrar-escola/. A coordenação cadastra turmas, disciplinas, professores e alunos; abre um ciclo de avaliação; os estudantes respondem de forma anônima às disciplinas em que estão matriculados; a coordenação encerra o ciclo, modera os comentários e publica os resultados agregados.RotaDescrição/Landing page institucional/anonimato/Explicação pública sobre a arquitetura de anonimato/cadastrar-escola/Cadastro da instituição e primeiro acesso da coordenação/entrar/Autenticação por e-mail e senha/coordenacao/Painel de gestão, importação CSV, ciclos e moderação/aluno/Painel de avaliações pendentes do estudante/professor/Painel de resultados agregados e feedback pedagógico🧪 Testes e Qualidade de Códigopython manage.py check
python -m pytest
python -m ruff check .
📁 Estrutura de Pastastrilha/
├── manage.py
├── requirements.txt
├── .env.example / .gitignore / README.md
├── config/                  # Settings, URLs, WSGI e comandos customizados
├── apps/
│   ├── contas/              # Usuário customizado, login, papéis e permissões
│   ├── escolas/             # Escola (tenant), turma, disciplina, matrícula e vínculo
│   └── avaliacoes/          # Ciclos, questionário fixo, anonimato e resultados
├── tailwind/input.css       # Código-fonte Tailwind CSS
├── templates/               # Templates HTML Django (base, contas, aluno, professor, coordenacao)
├── static/
│   ├── css/app.css          # CSS compilado e versionado
│   ├── js/                  # Scripts mínimos de interface
│   └── img/                 # Logotipos oficiais e ilustrações locais
├── tests/                   # Suíte de testes automatizados (pytest-django)
└── docs/                    # Documentação técnica, visual, instalação e deploy
