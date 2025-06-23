# Como funciona o Django:

Primeiramente o Django é um framework web de código aberto escrito em Python. Ele é usado por grandes sites por motivos de ser um framework full-stack, robusto, com sistemas integrado de autenticação, ORM, painel administrativo e segurança. Ideal para apps complexos e escalávies. 

# 1° Passo: Criar e ativar um ambiente virtual: 
**Windows**
venv\Scripts\activate

**Linux/MacOS**
source venv/bin/activate

# 2° Passo: Isntalar dependências:
**Comandos**
pip install django (pode instalar outras dependências como djangorestframework)

pip install > requirements.txt (gerar o arquivo requeriments.txt com base nas dependências que foram instaladas)

# 3° Passo: Criar o projeto Django:

**Definir o nome do seu projeto**

django-admin startproject meu_projeto 

Exemplo: django-admin startproject Ecomerce

# 4° Passo: Configurar o seu projeto:

**Ajustar o settings.py**
Ajustar o idioma, timezone, variáveis sensíveis (SECRET_KEY) via .env (use python-decouple ou dotenv)
Configurar arquivos estáticos (STATICS_URL, STATICFILES_DIRS) serve para localizar os seus arquivos statics, onde é feito a parte de "Front-end", os arquivos Css, JavaScript e as imagens do seu projeto deve estar na pasta statics. 
Criar um .gitignore para ignorar venv/ .pyc __pycache__/ .env ...

# 5° Passo: Criar um app Django:

python manage.py startapp core

# 6° Passo: Fazer o primeiro commit local:

Os commits servem para organização do projeto pelo github, ou seja, é recomendado que todas as alterações que você faz no ambiente virtual deve ter um commit detalhando o que foi feito

git init 
git add . 
git commit -m "Inicializar projeto Django com app core"
git push origin main


