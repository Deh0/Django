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

django-admin startproject meu_projeto .

Exemplo: django-admin startproject Raiz_do_meu_projeto . 

O ponto serve para não criar uma pasta extra sem necessidade.

# 4° Passo: Configurar o seu projeto:

**Ajustar o settings.py**
Ajustar o idioma, timezone, variáveis sensíveis (SECRET_KEY) via .env (use python-decouple ou dotenv)
Configurar arquivos estáticos (STATICS_URL, STATICFILES_DIRS) serve para localizar os seus arquivos statics, onde é feito a parte de "Front-end", os arquivos Css, JavaScript e as imagens do seu projeto deve estar na pasta statics. 
Criar um .gitignore para ignorar venv/ .pyc __pycache__/ .env ...

# 5° Passo: Criar um app Django:

python manage.py startapp Ecomerce 

Aqui é o seu app, pode colocar o nome que você quiser, só não esquece de configurar esse nome no settings da raiz do seu projeto em INSTALLED_APPS 

# 6° Passo: Fazer o primeiro commit local:

Os commits servem para organização do projeto pelo github, ou seja, é recomendado que todas as alterações que você faz no ambiente virtual deve ter um commit detalhando o que foi feito

git init 
git add . 
git commit -m "Inicializar projeto Django com app core"
git push origin main

# Criar arquivos HTML

No seu app que você criou onde tem models.py, apps.py, e views.py. Você irá adicionar uma nova pasta no seu app com o nome templates, e dentro da pasta templates adionar outra pasta com o mesmo nome do seu app, assim:


    -Seu APP

      -migrations (pasta que já tem no seu app)
  
      -templates (pasta que você acabou de criar)
  
         -Seu APP (pasta que foi criada no templates, pasta acima)
    
         -HTML (aqui você poderá adcionar os seus arquivos HTML)
    
(aproveitando essa estrutura, tem alguns arquivos de configurações que você precisa adicionar no seu app, ou seja, arquivos criados na pasta principal do seu app)

    -urls.py (aqui vai ficar todas as urls/caminhos que você configurar no views.py)

    -serializer.py (utilizado para criação de API)

    -forms.py (aqui será feito todos os campos que você deseja simplificar, por exemplo, você tem um modelo de cadastro no models.py, e para não ficar inserindo o modelo de cadastro de forma manual, é feito uma simplificação usando o forms)



