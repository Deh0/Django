# Guia completo: Django + Git
![Django](https://img.shields.io/badge/Django-4.2-green)
![Python](https://img.shields.io/badge/Python-3.8+-blue)
![Git](https://img.shields.io/badge/Git-Latest-orange)

Um tutorial prático e completo para começar com Django do zero, incluindo boas práticas de Git e estrutura de projetos.

📋Índice

* Como Funciona o Django
* 1° Passo: Ambiente Virutal
* 2° Passo: Instalar Dependências
* 3° Passo: Criar Projeto Django
* 4° Passo: Configurara Projeto
* 5° Passo: Criar e Configurar App Django
* 6° Passo: Branches
* 7° Passo: Primeiro Commit Local
* 8° Passo: Comandos Adicionais
* 9° Passo: Criar Arquivos HTML

# ✒️ Como Funciona o Django 

Primeiramente o Django é um framework web de código aberto escrito em Python. Ele é usado por grandes sites por motivos de ser um framework full-stack, robusto, com sistemas integrados de:

* Autenticação
* ORM (Object-Relational Mapping
* Painel Administrativo
* Segurança Integrada

Ideal para apps complexos e escaláveis

# 1° Passo: Ambiente Virtual 
 **Windows**

venv\Scripts\activate

 **Linux/MacOS**

source venv/bin/activate

# 2° Passo: Instalar Dependências

pip install django 

pip install djangorestframework -- utilizado para APIs REST 

pip freeze > requirements.txt (gerar o arquivo requirements.txt com base nas dependências que foram instaladas)

# 3° Passo: Criar o Projeto Django

**Definir o nome do seu projeto**

django-admin startproject (qualquer nome que você colocar no seu projeto) .

Exemplo: django-admin startproject meu-projeto . 

O ponto (.) serve para não criar uma pasta extra sem necessidade.

# 4° Passo: Configurar o seu projeto:

**Ajustar o settings.py**
Ajustar o idioma, timezone, variáveis sensíveis (SECRET_KEY) via .env (use python-decouple ou dotenv)
Configurar arquivos estáticos (STATICS_URL, STATICFILES_DIRS) serve para localizar os seus arquivos statics, onde é feito a parte de "Front-end", os arquivos Css, JavaScript e as imagens do seu projeto deve estar na pasta statics. 
Criar um .gitignore para ignorar venv/ .pyc __pycache__/ .env ...

# 5° Passo: Criar um app Django:

python manage.py startapp Ecomerce 

# 6° Passo: Configurar arquivos do seu APP:

Aqui é o seu app, pode colocar o nome que você quiser, ele terá pastas diferentes das do seu projeto django. Vou te mostrar todas as pastas do seu app, algumas você terá que adicionar manualmente, em adicionar novo arquivo (dentro do seu app) e terminar todos eles com py (para mostrar que são arquivos em python) vou te explicar pra que cada uma delas serve:

* init: Esse arquivo você nem precisa mexer ou adicionar qualquer coisa nele, só caso seja necessário, ele serve para informar que esta pasta do django é um arquivo em Python;

* admin: Aqui serão as configurações do painel do admin que o django cria automaticamente, conforme você vai configurando os outros arquivos como o models, views e etc. Esse aqui é utilizado para gerenciar os dados que foram adicionados, dependendo da maneira conforme você configura o seu app, ele criar um CRUD (create, read, update, e delete) do seu app, assim você poderá adicionar, ler, atualizar e deletar os seus produtos/itens/usuários de uma forma muita mais prática e eficiente;

* apps: Define configurações específicas do app, conf. cache, ou verificando as dependências; 

* models: Aqui é onde ficará todos os seus modelos de dados, estrutura das tabelas do banco de dados, alias o banco de dado mais recomendado para Django é o PostgreSQL, algumas pessoas também utilizam o MySQL Workbench, e existe o banco de dados para testes ou sistemas pequenos, que seria o SQLite que é um banco de dados virtual. No models é onde você irá determinar os valores de cada atributo das entidades, ou seja, vamos supor que temos um app chamado Vida e dentro dele tenha o arquivo chamado models, nele terão as entidades: orgaos, partes_do_corpo, e sentimentos, dentro de orgaos vai ter os atributos como coração que vai receber um valor próprio como VARCHAR ou coisa assim, e vamos determinar o que esse atributo vai fazer, seguindo a lógica irá bombear o sangue para o corpo, e assim vamos criar essa lógica utilizando também o views para auxiliar. Esse arquivo serve para trazer a estrutura do banco de dados sem precisar criar manualmente as tabelas, já que o django faz isso automáticamente;

* tests: Aqui como o nome mesmo já diz, onde ficam os testes do app, onde é feito testes automatizados para verificação se está tudo ocorrendo da maneira como deveria, podendo fazer testes de login adicionando os dados de login fake e armazenando em um banco de dados fake que é idêntico ao seu verdadeiro banco de dados, para poder fazer uma vista grossa do que está funcionando e o que não está;

* views: Serve para informar a lógica do app, aqui deve ser informado o que acontece quando o usuário acessa uma url, por exemplo: o usuário entra na página inicial e já aperta no campo login, então deverá informar esse caminho na views com um campo de return para a outra página que é a página de login, e a partir disso vai seguir a lógica da função de login;

* urls: A url padrão do projeto é diferente da urls do seu app, a url do seu projeto ela informa e distribui o tráfego do seu projeto inteiro, já a do seu app que você terá que adicionar de forma manual, será onde define as rotas específicas do seu app, como todas as rotas que seu produto pode fazer, as rotas do carrinho e as rotas dos pedidos;  
Porém tem uma informação adicional, à views e a urls dentro do seu app vão sempre estar ligadas uma na outra, ou seja, todos os dados que você adicionar na views você irá precisar configurar eles na urls do seu app, então segue uma dica, todas as vezes que você adicionar um modelo novo no models.py, já adicione a lógica dele na views e configure as urls que esse modelo pode fazer;

Para entender bem essa parte: 
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

* utils: Você armazena dados úteis que era utilizado em diferentes partes do seu projeto, mas que não pertence a models, views e forms. Então com o utils você pode validar identidade de usuário, validar idade mínima, formatar telefone ou textos, pode também redimensionar imagens, calcular valor de frete, ou calcular descontos. Quando estiver adicionando a mesma informação em diferentes arquivos, mude para o arquivo utils e import ele nos arquivos que você estava utilizando, usando a função import;

* permissions: Controle de acesso, definem o que cada um pode fazer, especialmente em API REST, para proteger endpoints;

* filters: Utilizado para filtros e busca, ele é principalmente mais utilizado para APIs REST. Então esse arquivo cria parâmetros de URL automáticos;

* constants: Armazena valores fixos que são usados em vários lugares diferentes do sistema, como valores fixos, configurações gerais (padronizar 20 produtos por páginas), mensagens padrões, configurações de email, ou caminhos de uploads;

* managers: Cria consultas otimizadas para os seus models, adicionando um método especial no .object. Deixando apenas produtos ativos, produtos em promoção, mais vendidos, mais baratos, ou status do pedido (entregue, pendente, enviado). Ele é importado nos models e usados na view, sendo assim ele adiciona consultas personalizadas ou predefinidas;

# 7° Passo: Sobre as Branches:

Elas permitem criar linhas de desenvolvimento paralelas e independentes dentro de um mesmo repositório.

O que são Branches?

Um branch, ou ramo em português, é essencialmente um ponteiro móvel para um commit específico, ou seja, quando você cria um novo branch você está criando uma nova linha de desenvolvimento que diverge do ponto atual, isso permite trabalhar em diferentes funcionalidades ou testes sem afetar o código principal, que é armazenado na branch principal, a branch main/master, nenhum código com erros ou sem ter feitos os devidos testes pode ir para a branch principal, e é por isso que existem outras branches. 

Como utilizar as Branches?
* A branch principal: Geralmente é chamada de main ou master, contém o código estável, sem erros. Essa branch que mantém o site/app funcionando independente se está sendo desenvolvido outras funcionalidades, enquanto não encaminhar as alterações para a main, ela continuará igual;
* Branch de development: A branch de desenvolvimento é utilizada em casos de sites maiores ou que precisam de uma estabilidade maior, onde é feito a integração das novas funcionalidades, para ter certeza que o código está apto para ir para a branch main;
* Branch feature: Essa é branch das novas funcionalidades, por exemplo, quero criar um ecommerce vou começar pela página inicial, ai você cria uma branch de feature/pagina-inicial, se quiser adicionar campo de produtos dentro desse e-commerce, terá que criar uma nova feature/produtos, e assim por diante, todas as novas funcionalidades sejam elas quais forem, terão que ser registradas em uma branch feature;
* Branch bugfix: Essa branch é utilizada para correção de erros específicos, para ficar registrado o que você teve que mexer para arrumar esse erro, e também serve para identificar de onde veio esse erro;
* Branch hotfix: Aqui são registrados os problemas urgentes, que precisam ser corrigidos o quanto antes, poderia ter utilizado a branch acima, porém isso serve para saber quantas vezes tiveram problemas críticos e urgentes no projeto, para saber o que deve ser feito a respeito disso;
* Branch refactor: Essa branches servem para deixar registrado as melhorias, seja no estilo do botão, em algum documento, ou deixou o código mais limpo, mas sem de fato alterar o código, tudo isso é registrado na branch improvement;

  As mudanças em uma branch em específico não altera nenhuma outra, assim podem ser feitos testes sem quebrar o código principal, e quando está trabalhando em equipe esse é um ótimo método, porque diferentes pessoas podem trabalhar em funcionalidades diferentes no projeto de forma simultânea. Além de ter uma branch para cada funcionalidade permite uma organização mais eficiente, e um entendimento do projeto mais claro para quem entrar nele depois do projeto já ter começado, simplesmente analisando os commits e o que foi feito.

Conventional Commits é uma especificação para padronizar mensagens de commit, facilitando a leitura do histórico e automatização de processos.
Exemplo: git commit -m "feat: Criação da página inicial"

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

Pode usar parênteses para especificar ainda mais o que você acabou de fazer.
Exemplo: fix(api): corrigir validação de email da API REST

Por que utilizar esse método de commit e de branches? 

Isso vai facilitar o entendimento do histórico de commits, sabendo exatamente qual commit que acabou resultando em erro no projeto, o time todo segue o mesmo padrão e todo mundo vai se entender de uma maneira muito mais fácil. A ideia é que cada commit tenha um propósito claro e seja facilmente identificável, isso torna o histórico do projeto muito mais legível e permite automatizar processos. 


# 8° Passo: Fazer o primeiro commit local:

Os commits servem para organização do projeto pelo github, ou seja, é recomendado que todas as alterações que você faz no ambiente virtual deve ter um commit detalhando o que foi feito

(passo 3° )

* cd (o nome do seu projeto) -- esse comando serve para você mudar o caminho do seu terminal, ou seja, ele te coloca dentro do seu projeto e todos os comandos que você faz depois disso estão dentro do seu projeto;

* git init -- esse comando serve para inicializar o seu repositório do Git e organizar ele;

* git branch -- esse comando serve para você saber em qual branch você está;

* git checkout -b development (mais antigo)
  git branch development (mais comum)
  git switch -c development (mais moderno)
-- estes comandos servem para criar uma branch de desenvolvimento, onde você irá integrar as funcionalidades, mas com esse comando você pode criar qualquer branch;

* git checkout -b feature/pagina-inicial -- a branch feature serve para criar novas funcionalidades, como adicionar uma cor de botão nova, ou criar a página inicial, a página de login. Tudo isso você vai documentar como uma nova funcionalidade. Na parte de pagina-inicial será o campo onde você irá mencionar o que essa funcionalidade irá fazer;

* git add . -- adicionar tudo que aconteceu até agora, por exemplo, esse é o seu primeiro commit então estará adicionando a estrutura que você acabou de criar no django;

* git commit -m "feat: Começando aplicação django, e organizando a estrutura" -- o feat serve para identificar que você está adicionando uma nova funcionalidade, e o commit serve para você deixar registrado no sistema o que você fez (isso é explicado na parte de commits);

* git push origin feature/pagina-inicial -- enviar os dados que você acabou de comentar para o GitHub, você sempre irá dar push nas branchs que você criou anteriormente, ou seja, se você tinha feito isso na branch feature/login precisa finalizar ela aqui;

* git checkout development -- voltar para a branch development;

* git merge feature/pagina-inicial -- o comando merge serve para combinar o conteúdo de uma branch para outra, com isso agora você sabe que as branchs não estão sincronizadas e precisam ser feito merge para adicionar as suas alterações para a branch desejada, por exemplo, como é seu primeiro commit a suas alterações são na realidade a estrutura do django que você acabou de criar, então pode jogar na branch principal (main), mas quando são coisas que precisam ser passadas por um teste ou uma verificação antes, precisa ser passado para a branch development antes, por motivos de segurança, para não quebrar o código na branch principal;

* git push origin development -- manda os dados que você acabou de receber da merge que acabou de fazer para o GitHub;

Também tem a opção de fazer uma merge via Pull Request no GitHub, é mais comum em projetos colaborativos.


# 9° Passo: Comandos Adicionais:

Comandos Git Básicos: 
* git status: para verificar o status dos arquivos e as suas mudanças, se possuir, normalmente é utilizado depois de entrar na branch que você deseja (git checkout), e antes do comando git add . 

* git log --oneline: para ver histórico resumido, serve para ver todos os commits, desde o primeiro;

* python manage.py test: realizar testes;

Comandos Django - Migrations: 
Antes de rodar os comandos abaixo, precisa salvar as alterações que você fez no GitHub, e realizar testes do banco de dados no arquivo de test.py do arquivo do app, e após isso você salva as alterações do banco de dados.

* python manage.py makemigrations: serve para detectar mudanças no models, aqui é onde informa as mudanças no arquivo migrations no seu app, onde tem as instruções SQL necessárias;

* python manage.py migrate: Aqui é onde de fato faz as alterações no banco de dados, ou seja, se você adiciona ou altera alguma tabela no models, será adicionado com esse comando, quando efetuado esse comando, no terminal irá constar as alterações com um OK verde do lado:
Exemplo: Running migrations:
  Applying myapp.0001_initial... OK

* python manage.py showmigrations: Ver as migrações pendentes;

* python manage.py makemigrations nome_do_app: Caso tenha mais de um app e tenha feito alteração somente em um, utilizar o nome do seu app no final do comando;

* python manage.py migrate nome_do_app 0001: se a caso você se arrepender de ter feito alguma alteração, poderá ver as alterações no migrations, e selecionar aquela na qual você deseja voltar:
Por exemplo: 0001 – eu quero essa;
0002 – quero excluir essa;

Parece haver bastantes comandos agora, mas depois que isso entra na sua cabeça fica a coisa mais fácil de programar utilizando o django.😁

# Criar arquivos HTML

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


