# Guia completo: Django + Git
![Django](https://img.shields.io/badge/Django-4.2-green)
![Python](https://img.shields.io/badge/Python-3.8+-blue)
![Git](https://img.shields.io/badge/Git-Latest-orange)

Um tutorial prático e completo para começar com Django do zero, incluindo boas práticas de Git e estrutura de projetos.

📋Índice

* Como Funciona o Django
* 1° Passo: Ambiente Virtual
* 2° Passo: Instalar Dependências
* 3° Passo: Configurar o Banco de Dados
* 4° Passo: Criar Projeto Django
* 5° Passo: Configurar Projeto
* 6° Passo: Criar e Configurar App Django
* 7° Passo: Branches
* 8° Passo: Primeiro Commit Local
* 9° Passo: Comandos Adicionais
* 10° Passo: Criar Arquivos HTML

## ✒️ Como Funciona o Django 

Primeiramente o Django é um framework web de código aberto escrito em Python. Ele é usado por grandes sites por motivos de ser um framework full-stack, robusto, com sistemas integrados de:

* Autenticação
* ORM (Object-Relational Mapping
* Painel Administrativo
* Segurança Integrada

>  Ideal para apps complexos e escaláveis

### 1° Passo: Ambiente Virtual 
O ambiente virtual (venv) serve para isolar o seu projeto Python dos outros projetos e do sistema operacional, é como se fosse uma gaveta específica que você utiliza para trabalhar apenas com as ferramentas daquele projeto. No caso, aqui vamos utilizar Django, então vamos instalar todas as suas dependências e elas vão ficar armazenadas nesse ambiente virtual.

Criar um ambiente virtual:
`python -m venv venv` 

Ativar ambiente virtual:

 **Windows**

`venv\Scripts\activate`

 **Linux/MacOS**

`source venv/bin/activate`

> Com esses comandos você irá notar que criou uma pasta escrito venv, e depois de ativar o ambiente virtual vai ter um (venv) no seu terminal, no início do seu prompt.

### 2° Passo: Instalar Dependências

`pip install django` 
> instalar o Django puro

`pip install djangorestframework` 
> instalar o Django REST Framework, utilizado para APIs REST

`pip install psycopg2-binary`
> Instalar Banco de Dados PostgreSQL

`pip install django-filter` 
> Serve para filtros avançados nas APIs

`pip install python-decouple`
> Serve para gerenciar variáveis de ambiente

`pip install pillow` 
> Instalar caso deseje fazer upload de imagens

Depois de instalar todas as dependências que você for usar, salve elas utilizando o requirements.txt

`pip freeze > requirements.txt`

## 3° Passo: Configurar o Banco de Dados

O banco de dados mais recomendado para Django é o PostgreSQL, então você pode instalar ele direto pelo site da PostgreSQL, mas caso esteja usando a máquina virtual (codespace) pode instalar ele pelo terminal: 

`sudo apt update`
`sudo apt install postgresql postgresql-contrib`
`sudo service postgresql start`
`sudo -u postgres psql`
`ALTER USER postgres PASSWORD 'senha da sua preferência';
CREATE DATABASE meu_projeto_db;`
`\q`
`psql -U postgres -h localhost`

Caso escolha instalar PostgreSQL no Windows:
https://www.postgresql.org/download/windows/

Escolha a versão: PostgreSQL 16x Windows x86-64

Agora a parte que pra quem não é bilíngue é um desafio, a confirmação de instalação, o que marcar e o que não marcar. Eu vou te ajudar nisso:

Tela 1- Welcome: next> 

Tela 2- Installation Directory: 
  padrão: C:\Program Files\PostgreSQL\16: next> 
  
Tela 3- Select Components:
  Deixe tudo marcado: next>
  
Tela 4- Data Directory:
  padrão: C:\Program Files\PostgreSQL\16\data: next>
  
Tela 5- Password: 
  Essa é sua senha, ela é importante e você irá precisar dela no Django: next>
  
Tela 6- Port: 
  padrão: 5432: next>
  
Tela 7- Advanced Options:
  Locale: Escolha Portuguese, Brazil: next> 
  
Tela 8- Pre Installation Summary: 
  Revise as Configurações: next> 
  
Tela 9- Ready to Install: next>

Tela 10- Completing Installation: Finish!

Verifique se o PostgreSQL realmente foi instalado: 
Vá em Configurações > Apps > Procure por "PostgreSQL" na lista > Deve aparecer: PostgreSQL16, pgAgent_PG16, psqlOBDC. Se acaso não aparecer sugiro que repita o processo de instalação como administrador seguindo next em tudo e instalando novamente. 
Agora para verificar se está tudo certo, você precisa procurar nas configurações do seu computador a opção: "Variáveis de Ambiente", em seguida configurar ela, vai em "Path" e clique em "Editar", em "Novo" e digite: C:\Program Files\PostgreSQL\16\bin salve apertando em "OK" 
Se estiver funcionando corretamente você pode acessar o CMD como administrador e digitar: `psql --version` precisa aparecer psql (PostgreSQL) 16.x
> Para você acessar o app do PostgreSQL você pode apertar em Windows e digitar "pgAdmin", vai pedir uma senha master, porque o pgAdmin pede pra criar uma nova senha, a senha é diferente da do PostgreSQL.

> [!NOTE]
> A senha do PostgreSQL pode ser mais simples, porém se estiver fazendo em produção, hospedando o seu site Django, ela precisa ser complexa e segura. Agora a senha do pgAdmin é apenas para proteger o app, já que se alguém acessar o seu computador e abrir o pgAdmin, terá acesso a todos os seus bancos de dados, já que o pgAdmin só pede a senha na primeira vez que você abre. 

### 4° Passo: Criar o Projeto Django

## Definir o nome do seu projeto

`django-admin startproject` (qualquer nome que você colocar no seu projeto) .

> [!TIP]
> Exemplo: django-admin startproject meu-projeto .

> [!NOTE]
> O ponto (.) serve para não criar uma pasta extra sem necessidade.

### 5° Passo: Configurar o seu Projeto:
Agora que você acabou de criar a pasta principal do seu projeto eu vou te explicar o que são essas pastas que foram criadas automaticamente:

    -meu_projeto (ou o nome que você acabou de dar pro seu projeto)
      -init__.py (esse arquivo fica vazio, ele serve apenas para mostrar que essa pasta é um pacote python, não precisa colocar nada nele)
      -asgi.py: É um servidor assincrônico, é utilizado quando você deseja um chat em tempo real no e-commerce, ou ter notificações push, ou ter atualizações em tempo real do estoque, aqui são as aplicações que precisam de alta performance.
      -settings.py: é o centro de controle do seu projeto Django. É aqui onde você define todas as configurações importantes (abaixo eu explico o que você deve arrumar nesse arquivo)
      -urls.py: Esse arquivo é o roteador principal do seu projeto, é como se fosse um GPS você dá a rota pra ele e ele direciona para o caminho certo. Quando você fizer novas funcionalidades no seu projeto precisa adicionar as URLs aqui. 
      -wsgi.py: É padrão para servir aplicações Python na web, ele será uma interface entre seu projeto Django e o servidor web, é utilizado na produção, ou seja, quando acaba o desenvolvimento e você coloca o seu projeto em um local de hospedagem.

    -manage.py: Esse é o arquivo mais importante para o desenvolvimento, ele é como um "assistente virtual" que executa os comandos do Django. 
    (comandos mais usados: 
          python manage.py runserver -- inicia o servidor 
          python manage.py startapp nome_do_app -- cria o seu app 
          python manage.py makemigrations --  cria mirações do banco de dados)
> O manage.py é gerado automaticamente pelo Django e você não precisa editar ele. 
    
**Ajustar o settings.py**

O arquivo `settings.py` é o centro de controle do seu projeto Django, vou te explicar quais são as informações que você precisa adicionar/alterar:

1. Criar um arquivo .env
O arquivo .env serve para esconder dados importantes, ou senhas, para que elas não fiquem expostas para ninguém que não tenha acesso.

No terminal digite esses comandos:
     `cd /workspaces/raiz_do_seu_projeto` (primeiro arquivo disponível)
     `touch .env` -- criar o arquivo .env 
     `code .env` -- abrir o arquivo para editar ele

   Agora precisamos adicionar o conteúdo dentro deste arquivo .env. Para isso você irá precisar ir até settings.py do seu projeto e em seguida procurar essa função:
     SECRET_KEY = "django-insecure-(senha longa com vários caracteres e símbolos)"
   Vamos esconder essa senha no nosso arquivo .env, então na primeira linha do arquivo coloque isso:
      SECRET_KEY=django-insecure-(senha longa com vários caracteres e símbolos

Agora volte no arquivo settings e altere a senha longa e comprida para SECRET_KEY, veja só: 
      SECRET_KEY = config('SECRET_KEY')

   Vamos adicionar mais uma linha no arquivo .env:
     DEBUG = True 
   No settings mude essa linha para:
     DEBUG = config('DEBUG', default=False, cast=bool)

  Agora vamos adicionar os dados do banco de dados diretamente no arquivo .env, e depois vamos alterar os dados no settings:
     
  DATABASE_NAME=meu_projeto_db -- nome do seu banco de dados
  DATABASE_USER=postgres
  DATABASE_PASSWORD=senha do seu banco de dados
  DATABASE_HOST=localhost -- normalmente isso é padrão
  DATABASE_PORT=porta do seu banco de dados (informado no PostgreSQL)

Alterando os dados do banco no settings:
Se encontra assim:
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}

Vamos alterar conforme o nome que determinamos no .env:
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql', 
        'NAME': config('DATABASE_NAME'),
        'USER': config('DATABASE_USER'),
        'PASSWORD': config('DATABASE_PASSWORD'),
        'HOST': config ('DATABASE_HOST'),
        'PORT': config ('DATABASE_PORT'),
    }
}

> Percebeu que o nome que colocamos no arquivo .env virou o segredo aqui no settings, exemplo:
DATABASE_NAME=meu_projeto_db 🤝 'NAME': config('DATABASE_NAME'),

> [!NOTE]
> Após adicionar todos os dados no aquivo .env, você deve criar um arquivo chamado .gitignore e incluir o .env nele.

Vamos criar o arquivo .gitignore: 
  `touch .gitignore`

Conteúdo dentro do .gitignore:
  .env
  venv/
  __pycache__/
  *.pyc
  db.sqlite3
  
2. Adicionar as Importações:
Agora que já escondemos os segredos vamos arrumar os settings desde o começo para não faltar nenhuma configuração 
    from pathlib import Path
    from decouple import config
    import os  

3. Altere o HOSTS:
ALLOWED_HOSTS = []  /Altere para: ALLOWED_HOSTS = ['localhost', '127.0.0.1']

4. INSTALLED_APPS:
Nessa opção você irá adicionar o seu app, ou seja, colocar o nome do seu app como primeiro da lista dessa função. Eu vou ensinar como criar um app no passo 5, que fica abaixo desse, mas você já pode colocar o nome que você quer dar pro seu app aqui, e depois criar o app de fato. As configurações desta função vão ter mais essas opções:

  "nome_do_seu_app",
  
  'rest_framework',
  'corsheaders', 
  'django_filters',

5. MIDDLEWARE:
Adicione apenas uma linha no topo dessa configuração:
  'corsheaders.middleware.CorsMiddleware', -- somente para desenvolvimento, em produção pode ocorrer grandes riscos

6. Configurações Básicas de Localização:
Você precisa ajustar o seu idioma e o fuso horário, no settings vai ter esse campo:
   LANGUAGE_CODE = "en-us" /Altere para: LANGUAGE_CODE = "pt-br"

   TIME_ZONE = "UTC"  /Altere para: TIME_ZONE = "America/Sao_Paulo"

   USE_I18N = True /Pode deixar assim;

   USE_TZ = True /Pode deixar assim;

7. Altere e adicione as configurações dos arquivos estáticos:
   STATIC_URL = "static/"
   
   Altera para:
   STATIC_URL = "static/"
   STATICFILES_DIRS = [
       os.path.join(BASE_DIR, 'static'),
   ]
   STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

   MEDIA_URL = '/media/'
   MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
   > Serve para arquivos de mídia (upload)
   
8. No final do arquivo do settings adicione as configurações do REST Framework:
  REST_FRAMEWORK = {
     'DEFAULT_AUTHENTICATION_CLASSES': [
         'rest_framework.authentication.TokenAuthentication',
     ],
     'DEFAULT_PERMISSION_CLASSES': [
         'rest_framework.permissions.IsAuthenticated',
     ],
     'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
     'PAGE_SIZE': 20
     ]

10. Configurações do CORS:
   CORS_ALLOW_ALL_ORIGINS = True -- somente para desenvolvimento

   
### 6° Passo: Criar e Configurar um app Django:

`python manage.py startapp Ecomerce`

> [!IMPORTANT]
> Aqui é o seu app, pode colocar o nome que você quiser, ele terá pastas diferentes das do seu projeto django. Vou te mostrar todas as pastas do seu app, algumas você terá que adicionar manualmente, em adicionar novo arquivo (dentro do seu app) e terminar todos eles com py (para mostrar que são arquivos em python) vou te explicar pra que cada uma delas serve:

* init: Esse arquivo você nem precisa mexer ou adicionar qualquer coisa nele, só caso seja necessário, ele serve para informar que esta pasta do django é um arquivo em Python;

* admin: Aqui serão as configurações do painel do admin que o django cria automaticamente, conforme você vai configurando os outros arquivos como o models, views e etc. Esse aqui é utilizado para gerenciar os dados que foram adicionados, dependendo da maneira conforme você configura o seu app, ele criar um CRUD (create, read, update, e delete) do seu app, assim você poderá adicionar, ler, atualizar e deletar os seus produtos/itens/usuários de uma forma muita mais prática e eficiente;

* apps: Define configurações específicas do app, conf. cache, ou verificando as dependências; 

* models: Aqui é onde ficará todos os seus modelos de dados, estrutura das tabelas do banco de dados, alias o banco de dado mais recomendado para Django é o PostgreSQL, algumas pessoas também utilizam o MySQL Workbench, e existe o banco de dados para testes ou sistemas pequenos, que seria o SQLite que é um banco de dados virtual. No models é onde você irá determinar os valores de cada atributo das entidades, ou seja, vamos supor que temos um app chamado Vida e dentro dele tenha o arquivo chamado models, nele terão as entidades: orgaos, partes_do_corpo, e sentimentos, dentro de orgaos vai ter os atributos como coração que vai receber um valor próprio como VARCHAR ou coisa assim, e vamos determinar o que esse atributo vai fazer, seguindo a lógica irá bombear o sangue para o corpo, e assim vamos criar essa lógica utilizando também o views para auxiliar. Esse arquivo serve para trazer a estrutura do banco de dados sem precisar criar manualmente as tabelas, já que o django faz isso automáticamente;

* tests: Aqui como o nome mesmo já diz, onde ficam os testes do app, onde é feito testes automatizados para verificação se está tudo ocorrendo da maneira como deveria, podendo fazer testes de login adicionando os dados de login fake e armazenando em um banco de dados fake que é idêntico ao seu verdadeiro banco de dados, para poder fazer uma vista grossa do que está funcionando e o que não está;

* views: Serve para informar a lógica do app, aqui deve ser informado o que acontece quando o usuário acessa uma url, por exemplo: o usuário entra na página inicial e já aperta no campo login, então deverá informar esse caminho na views com um campo de return para a outra página que é a página de login, e a partir disso vai seguir a lógica da função de login;

* urls: A url padrão do projeto é diferente da urls do seu app, a url do seu projeto ela informa e distribui o tráfego do seu projeto inteiro, já a do seu app que você terá que adicionar de forma manual, será onde define as rotas específicas do seu app, como todas as rotas que seu produto pode fazer, as rotas do carrinho e as rotas dos pedidos;  
Porém tem uma informação adicional, à views e a urls dentro do seu app vão sempre estar ligadas uma na outra, ou seja, todos os dados que você adicionar na views você irá precisar configurar eles na urls do seu app, então segue uma dica, todas as vezes que você adicionar um modelo novo no models.py, já adicione a lógica dele na views e configure as urls que esse modelo pode fazer;

> [!WARNING]
> Para entender bem essa parte, vou usar um exemplo:
Você criou um app de ecommerce e adicionou um produto com o id 123

1 - O usuário se interessou por esse produto e apertou nele

2 - O Django vai ver que teve uma solicitação para acessar um arquivo, que no caso foi esse produto, então ele vai acessar: meu_projeto/urls.py (urls principal do seu projeto)

3 - Em seguida ele vai ser redirecionado para: path(' ', include ('meu_app.urls')) -- ou path(' ', include ('ecommerce.urls'))

4 - Com isso ele vai acessar essa urls e vai constar essa informação: meu_app/urls.py, ou seja, só agora ele está verificando as informações da urls do seu app

5 - Aqui ele vai tentar encontrar as urls do produto para saber onde direcionar o usuário, e vai encontrar a url: path('produto/<int:produto_id>/', views.detalhes_produto) -- com isso o Django vai acessar todos os produtos e todos os ids, então vamos supor que esteja assim em JSON: produto/111 produto/122 produto/123 e bingo ele achou o que estava procurando, e agora ele precisa saber qual é a lógica desse produto

6 - Ele vai acessar detalhes_produtos na views para descobrir qual é a lógica, e nisso poderá executar views.detalhes_produto(request, produto_id=123) e direcionar o cliente

* serializer: Serve para criar APIs REST com base nos arquivos do models.py, que são os arquivos do banco de dados, e com isso você irá conseguir criar APIs com rest framework, já que não é do Django puro, e sim do Django REST Framework (DRF);

* forms: É utilizada para organizar os formulários da sua aplicação, esses formulários também utilizam como base dados do models.py, serve para receber dados de usuários de forma segura;

* signals: Ele automatiza processos, mas deve ser utilizado com moderação, para eventualmente não travar a sua aplicação;

* utils: Você armazena dados úteis que são utilizados em diferentes partes do seu projeto, mas que não pertence a models, views e forms. Então com o utils você pode validar identidade de usuário, validar idade mínima, formatar telefone ou textos, pode também redimensionar imagens, calcular valor de frete, ou calcular descontos. Quando estiver adicionando a mesma informação em diferentes arquivos, mude para o arquivo utils e import ele nos arquivos que você estava utilizando, usando a função import;

* permissions: Controle de acesso, definem o que cada um pode fazer, especialmente em API REST, para proteger endpoints;

* filters: Utilizado para filtros e busca, ele é principalmente mais utilizado para APIs REST. Então esse arquivo cria parâmetros de URL automáticos;

* constants: Armazena valores fixos que são usados em vários lugares diferentes do sistema, como valores fixos, configurações gerais (padronizar 20 produtos por páginas), mensagens padrões, configurações de email, ou caminhos de uploads;

* managers: Cria consultas otimizadas para os seus models, adicionando um método especial no .object. Deixando apenas produtos ativos, produtos em promoção, mais vendidos, mais baratos, ou status do pedido (entregue, pendente, enviado). Ele é importado nos models e usados na view, sendo assim ele adiciona consultas personalizadas ou predefinidas;

### 7° Passo: Sobre as Branches:

Elas permitem criar linhas de desenvolvimento paralelas e independentes dentro de um mesmo repositório.

O que são Branches?

Um branch, ou ramo em português, é essencialmente um ponteiro móvel para um commit específico, ou seja, quando você cria um novo branch você está criando uma nova linha de desenvolvimento que diverge do ponto atual, isso permite trabalhar em diferentes funcionalidades ou testes sem afetar o código principal, que é armazenado na branch principal, a branch main/master, nenhum código com erros ou sem ter feitos os devidos testes pode ir para a branch principal, e é por isso que existem outras branches. 

Como utilizar as Branches?
* A branch principal: Geralmente é chamada de main ou master, contém o código estável, sem erros. Essa branch que mantém o site/app funcionando independente se está sendo desenvolvido outras funcionalidades, enquanto não encaminhar as alterações para a main, ela continuará igual;
* Branch de development: A branch de desenvolvimento é utilizada em casos de sites maiores ou que precisam de uma estabilidade maior, onde é feito a integração das novas funcionalidades, para ter certeza que o código está apto para ir para a branch main;
* Branch feature: Essa é branch das novas funcionalidades, por exemplo, quero criar um ecommerce vou começar pela página inicial, ai você cria uma branch de feature/pagina-inicial, se quiser adicionar campo de produtos dentro desse e-commerce, terá que criar uma nova feature/produtos, e assim por diante, todas as novas funcionalidades sejam elas quais forem, terão que ser registradas em uma branch feature;
* Branch bugfix: Essa branch é utilizada para correção de erros específicos, para ficar registrado o que você teve que mexer para arrumar esse erro, e também serve para identificar de onde veio esse erro;
* Branch hotfix: Aqui são registrados os problemas urgentes, que precisam ser corrigidos o quanto antes, poderia ter utilizado a branch acima, porém isso serve para saber quantas vezes tiveram problemas críticos e urgentes no projeto, para saber o que deve ser feito a respeito disso;
* Branch refactor: Essa branches servem para deixar registrado as melhorias, seja no estilo do botão, em algum documento, ou deixou o código mais limpo, mas sem de fato alterar o código, tudo isso é registrado na branch refactor;
  
> [!IMPORTANT]
>  As mudanças em uma branch em específico não altera nenhuma outra, assim podem ser feitos testes sem quebrar o código principal, e quando está trabalhando em equipe esse é um ótimo método, porque diferentes pessoas podem trabalhar em funcionalidades diferentes no projeto de forma simultânea. Além de ter uma branch para cada funcionalidade permite uma organização mais eficiente, e um entendimento do projeto mais claro para quem entrar nele depois do projeto já ter começado, simplesmente analisando os commits e o que foi feito.

Conventional Commits é uma especificação para padronizar mensagens de commit, facilitando a leitura do histórico e automatização de processos.
Exemplo: `git commit -m "feat: Criação da página inicial"`

* feat: nova funcionalidade para o usuário, serve para quando você adicionar algo novo que o usuário final possa usar;
* fix: correção de bugs, resolve os problemas existentes no código;
* hotfix: Correção urgente em produção, somente as correções emergenciais que não podem esperar a nova atualização do development;
refactor: Mudança no código que não adiciona funcionalidade nem corrige bugs, servem para melhorar a qualidade do código sem alterar de fato o código;
* perf: Aqui são as mudanças que melhoram a performance, mas focado em velocidade e eficiência;
* style: Mudança na formatação, como espaços, vírgulas, ou mudar o estilo do css, ou javascript, ou seja, alterações que não afetam o significado do código;
* docs: Mudanças apenas na documentação; 
* test: Adicionar testes ou modificar testes;
* chore: Pode parecer que aqui vai ser registrado o fim, mas não é (piada a parte), aqui são registrada as mudanças em ferramentas, configurações ou dependências;
* build: Mudanças no sistema de build ou dependências externas;
* ci: Mudanças nos arquivos de CI/CD (integração contínua/entrega contínua), esse tópico vai ser explicado futuramente, porque é muito útil para o Django, são os arquivos do actions os workflows;
revert: Quando você fizer um commit e por algum motivo não era isso que você queria fazer, pode usar o revert para reverter o commit anterior;

> [!NOTE]
> Pode usar parênteses para especificar ainda mais o que você acabou de fazer.
Exemplo: fix(api): corrigir validação de email da API REST

Por que utilizar esse método de commit e de branches? 

Isso vai facilitar o entendimento do histórico de commits, sabendo exatamente qual commit que acabou resultando em erro no projeto, o time todo segue o mesmo padrão e todo mundo vai se entender de uma maneira muito mais fácil. A ideia é que cada commit tenha um propósito claro e seja facilmente identificável, isso torna o histórico do projeto muito mais legível e permite automatizar processos. 


### 8° Passo: Fazer o primeiro commit local:

Os commits servem para organização do projeto pelo github, ou seja, é recomendado que todas as alterações que você faz no ambiente virtual deve ter um commit detalhando o que foi feito. 

(passo 3° )

* `cd` (o nome do seu projeto)
> esse comando serve para você mudar o caminho do seu terminal, ou seja, ele te coloca dentro do seu projeto e todos os comandos que você faz depois disso estão dentro do seu projeto;

* `git init`
> esse comando serve para inicializar o seu repositório do Git e organizar ele;

* `git branch`
> esse comando serve para você saber em qual branch você está;

* `git checkout -b development`(mais antigo)
  `git branch development`(mais comum)
  `git switch -c development`(mais moderno)
> estes comandos servem para criar uma branch de desenvolvimento, onde você irá integrar as funcionalidades, mas com esse comando você pode criar qualquer branch;

* `git checkout -b feature/pagina-inicial`
> [!IMPORTANT]
> a branch feature serve para criar novas funcionalidades, como adicionar uma cor de botão nova, ou criar a página inicial, a página de login. Tudo isso você vai documentar como uma nova funcionalidade. Na parte de pagina-inicial será o campo onde você irá mencionar o que essa funcionalidade irá fazer;

* `git add .`
> adicionar tudo que aconteceu até agora, por exemplo, esse é o seu primeiro commit então estará adicionando a estrutura que você acabou de criar no django;

* `git commit -m "feat: Começando aplicação django, e organizando a estrutura"`
> [!NOTE]
>  o feat serve para identificar que você está adicionando uma nova funcionalidade, e o commit serve para você deixar registrado no sistema o que você fez (isso é explicado na parte de commits);

* `git push origin feature/pagina-inicial`
> enviar os dados que você acabou de comentar para o GitHub, você sempre irá dar push nas branchs que você criou anteriormente, ou seja, se você tinha feito isso na branch feature/login precisa finalizar ela aqui;

* `git checkout development`
> voltar para a branch development;

* `git merge feature/pagina-inicial`
> [!IMPORTANT]
> o comando merge serve para combinar o conteúdo de uma branch para outra, com isso agora você sabe que as branchs não estão sincronizadas e precisam ser feito merge para adicionar as suas alterações para a branch desejada, por exemplo, como é seu primeiro commit a suas alterações são na realidade a estrutura do django que você acabou de criar, então pode jogar na branch principal (main), mas quando são coisas que precisam ser passadas por um teste ou uma verificação antes, precisa ser passado para a branch development antes, por motivos de segurança, para não quebrar o código na branch principal;

* `git push origin development`
> manda os dados que você acabou de receber da merge que acabou de fazer para o GitHub;

Também tem a opção de fazer uma merge via Pull Request no GitHub, é mais comum em projetos colaborativos.


### 9° Passo: Comandos Adicionais:

Comandos Git Básicos: 
* `git status`: para verificar o status dos arquivos e as suas mudanças, se possuir, normalmente é utilizado depois de entrar na branch que você deseja (git checkout), e antes do comando git add . 

* `git log --oneline`: para ver histórico resumido, serve para ver todos os commits, desde o primeiro;

* `python manage.py test`: realizar testes;

Comandos Django - Migrations: 
> [!WARNING]
> Antes de rodar os comandos abaixo, precisa salvar as alterações que você fez no GitHub, e realizar testes do banco de dados no arquivo de test.py do arquivo do app, e após isso você salva as alterações do banco de dados.

* `python manage.py makemigrations`: serve para detectar mudanças no models, aqui é onde informa as mudanças no arquivo migrations no seu app, onde tem as instruções SQL necessárias;

* `python manage.py migrate`: Aqui é onde de fato faz as alterações no banco de dados, ou seja, se você adiciona ou altera alguma tabela no models, será adicionado com esse comando, quando efetuado esse comando, no terminal irá constar as alterações com um OK verde do lado:
Exemplo: Running migrations:
  Applying myapp.0001_initial... OK

* `python manage.py showmigrations`: Ver as migrações pendentes;

* `python manage.py makemigrations nome_do_app`: Caso tenha mais de um app e tenha feito alteração somente em um, utilizar o nome do seu app no final do comando;

* `python manage.py migrate nome_do_app 0001`: se a caso você se arrepender de ter feito alguma alteração, poderá ver as alterações no migrations, e selecionar aquela na qual você deseja voltar:
Por exemplo: 0001 – eu quero essa;
0002 – quero excluir essa;

* `python manage.py createsuperuser`: para criar um usuário admin, com acesso ao painel de administrador que o Django cria automaticamente;

  
> Parece haver bastantes comandos agora, mas depois que isso entra na sua cabeça fica a coisa mais fácil de programar utilizando o django.😁

## 10° Passo: Criar arquivos HTML

Você irá adicionar uma nova pasta no seu app com o nome templates, e dentro da pasta templates adicionar outra pasta com o mesmo nome do seu app, assim:

    -Seu APP

      -migrations (pasta que já tem no seu app, que serve para informar as alterações que teve no banco de dados)
  
      -templates (pasta que você acabou de criar)
  
         -Seu APP (pasta que foi criada no templates, pasta acima)
    
         -base.html (esse arquivo terá que ser adicionado dentro da pasta acima, aqui você poderá adicionar os seus arquivos HTML)
         -index.html (página inicial)

         -produto (nova pasta onde terão todos os arquivos html de produtos, essas novas pastas devem ser organizadas por funcionalidades)

             -detalhes_produto.html (detalhes do produto)

        -carrinho (só para mostrar a lógica acima, você poderá criar quantas pastas forem necessárias para o seu app)

            -checkout 
         
Isso serve para que não tenha conflitos com outros apps que você queira adicionar no seu projeto, até mesmo se você não tiver a intenção de criar outro app é recomendado fazer dessa maneira por deixar a estrutura mais clara e por boas práticas.
