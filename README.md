# Backend
Essa é uma API que foi construída para um desafio da Husky. Ela simula uma aplicação que gerencia
entregas de delivery, que se pode manipular os pedidos dos usuários.

## Projeto

Essa aplicação é feita com PHP puro e utiliza todas as normas de programação seguindo as PSRs (PHP Standards Recommendations), arquitetura e padrões de projeto como: MVC,  Active Record e Layer Supertype;

### Requistos para executar a API
+ Servidor Apache (wamp, xampp ou qualquer um de sua preferência)
+ MySQL
+ PHP versão  7.4
+ Composer

## Instalação do Backend

1° passo: 

Dentro do diretório raiz do seu servidor apache (htdocs ou WWW), insira a pasta desse projeto com nome monorepo_husky ou através do git: git clone https://github.com/netocastro/monorepo_husky.git. Caso não seja possível colocar no diretório raiz, será necessário mudar as rotas das requisições ajax no jquery, que será explicado mais adiante.

2° passo:  

Abra o terminal dentro da pasta backend e execute o comando: "composer update" (sem as aspas),
para se certificar da existência de todas as dependências.

3° passo:

Dentro do diretório backend, na pasta database execute o arquivo backend_huskye.sql em seu SGBD  para criar as tabelas e popular o banco de dados. De preferência crie sua database com o mesmo nome do arquivo, arquivo "backend_husky", editando dentro do Config.php.

Será necessário editar a constante DATA_LAYER_CONFIG com as informações do seu banco de dados, caso crie um banco de dados com nome diferente.Se seguiu o passo 1, não precisa alterar nada, a não ser usuário e senha do banco de dados se não forem padrões do MySQL.

Exemplo DATA_LAYER_CONFIG:
   
    define('DATA_LAYER_CONFIG', [
        'driver' => 'mysql',  
        'host' => 'localhost',          // nome do seu host
        'port' => '3306',
        'dbname' => 'backend_husky',    // nome da database
        'username' => 'root',           // usuário
        'passwd' => '',				    // senha
        'options' => [
            PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES utf8',
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_OBJ,
            PDO::ATTR_CASE => PDO::CASE_NATURAL,
        ],
    ]);

4° passo:

Dentro da pasta src/Core se encontra o arquivo Config.php, nele você precisa editar a
constante BASE_PATH para o diretório no qual você colocou a pasta do projeto. Esse passo só é necessário se as recomendações do passo 1 não forem implementadas. 

Exemplo BASE_PATH:

    Se o aquivo foi extraído para a raiz da sua pasta pública, matenha a constante assim:

	define("BASE_PATH", "http{$s}://{$_SERVER['HTTP_HOST']}/monorepo_husky/backend");  

    Caso não, edite para o diretório onde vc colocou a pasta, assim:

    define("BASE_PATH", "http{$s}://{$_SERVER['HTTP_HOST']}/<minha_pasta>/monorepo_husky/backend");  

5° passo:

Caso o passo 1 não tenha sido seguido, é preciso ir na pasta documentation, no arquivo index.php e editar
a url da constante ui para o diretório atual da pasta backend.

    const ui = SwaggerUIBundle({
        url: "https://localhost/<meu_diretorio>/monorepo_husky/backend/doc/Api.php",
        dom_id: '#swagger-ui',
        deepLinking: true,
        presets: [
          SwaggerUIBundle.presets.apis,
          SwaggerUIStandalonePreset
        ],
        plugins: [
          SwaggerUIBundle.plugins.DownloadUrl
        ],
        layout: "StandaloneLayout"
    });

    Então a rota para documentação ficará:
    http://localhost/<meu_diretorio>/monorepo_husky/backend/doc


Depois disso é só acessar o projeto através do navegador, cliente de API REST (insomnia ou postman) ou através da documentação dessa API, apartir da do endpoint acima pra poder fazer as requisições com swagger.

    
O BASE_PATH reconhece automaticamente se o servidor é HTTP ou HTTPS. Se você estiver utilizando um certificado SSL, retire os comentários das linhas 10, 11 e 12 no arquivo .htaccess que se encontra na raiz do projeto, removendo o "#". 


Depois disso é só acessar o projeto através do navegador, cliente de API REST (insomnia ou postman) ou através da documentação dessa API, apartir da rota /doc pra poder fazer as requisições com swagger.


### Explicando o Backend

O backend possui o CRUD de todas as tabelas do banco de dados e poderão ser acessadas através do swagger.

## Métodos
As requisições para a API devem seguir os padrões:

### Users
| Método | URI | Descrição |
|---|---|---|
| `GET` | /user | Retorna todos os registros de usuários do banco de dados.|
| `GET` | /user/{id} | Retorna as informações de um usuário específico através do id no banco de dados. |
| `POST` | /user | Insere um usuário no banco de dados. |
| `PUT` | /user/{id} | Atualiza as informações de um usuário específico através do id no banco de dados.|
| `DELETE` | /user/{id} | Deleta um usuário específico através do id no banco de dados. |

### Motoboys
| Método | URI | Descrição |
|---|---|---|
| `GET` | /motoboy | Retorna todos os registros de motoboys do banco de dados.|
| `GET` | /motoboy/{id} | Retorna as informações de um motoboy específico através do id no banco de dados. |
| `POST` | /motoboy | Insere um motoboy no banco de dados. |
| `PUT` | /motoboy/{id} | Atualiza as informações de um motoboy específico através do id no banco de dados.|
| `DELETE` | /motoboy/{id} | Deleta um motoboy específico através do id no banco de dados. |

### Status
| Método | URI | Descrição |
|---|---|---|
| `GET` | /status | Retorna todos os registros de status do banco de dados.|
| `GET` | /status/{id} | Retorna as informações de um status específico através do id no banco de dados. |
| `POST` | /status | Insere um status no banco de dados. |
| `PUT` | /status/{id} | Atualiza as informações de um status específico através do id no banco de dados.|
| `DELETE` | /status/{id} | Deleta um status específico através do id no banco de dados. |


### Delivery
| Método | URI | Descrição |
|---|---|---|
| `GET` | /delivery | Retorna todos os registros de entregas do banco de dados.|
| `GET` | /delivery/{id} | Retorna as informações de uma entrega específica através do id no banco de dados. |
| `POST` | /delivery | Insere uma entrega no banco de dados. |
| `PUT` | /delivery/{id} | Atualiza as informações de uma entrega específica através do id no banco de dados.|
| `DELETE` | /delivery/{id} | Deleta uma entrega específica através do id no banco de dados. |

### Web
Também foi criada uma rota Web, que serve para requisicões específicas do site, para não haver a necessidade
de alterar as rotas padrões. Nela se pode fazer requisições, como: alterar apenas o status em uma tabela, alterar apenas o motoboy sem precisar as rotas padrões do CRUD. Essas rotas poderiam estar, se necessário, dentro das rotas de suas respectivas tabelas, mas como são exclusivas para a função do site, optei por colocar em rotas diferentes.

| Método | URI | Descrição |
|---|---|---|
| `POST` | /filterDeliveries | Retorna o filtro da pesquisa de pedidos entre motoboys e/ou status no banco de dados.|
| `POST` | /changeStatusDelivery | Atualiza apenas o status de um determinado pedido. |
| `POST` | /changeMotoboyDelivery | Atualiza apenas um motoboy de um determinado pedido. |

# Frontend

### Recursos disponíveis 

* Acesso a todos os pedidos
* Alteração de motoboys nos pedidos
* Alteração de status do pedido
* Atualização de pedidos
* Criação de pedidos
* Filtro de Pedidos por motoboys
* Filtro de Pedidos por status
* Filtro de Pedidos entre Motoboys e status

### Como usar o front End

O frontend do site foi criado de forma simples e intuitiva atendendo os requisitos do desafio. Na página inicial há apenas um texto explicando sobre do que se trata o site. Se as sugestões feitas na sessão de backend foram seguidas, basta acessar : https://localhost/monorepo_husky/frontend/index.html

Na barra de navegação você pode escolher entre visualizar ou cadastrar entregas. Ao clicar em visualizar você terá acesso a todos os pedidos com informações dos usuários, motoboys, status, endereços de coleta e endereços de destino podendo filtrá-los por motoboy, por status ou pelos dois.

 Para fazer uma pesquisa mais específica como por exemplo, saber os status das entregas de um motoboy,
 ao lado das informações do pedido existe um botão com o ícone de um olho, nele você irá até a tela do pedido onde poderá alterar o motoboy que fará a entrega e o status do pedido. Se clicar em editar poderá modificar todas as informações do pedido. 

Por estarem em projetos diferentes, as URLs das requisições AJAX no frontend estão estáticas e são baseadas na URL do seu servidor. É necessário modificá-las com o caminho igual a da sua variável BASE_PATH no backend.

Se seu servidor possuir SSL adicionar o s em "http://" pra se tornar "https://". 

Exemplo:

    Se decidir extrair todos os arquivos da pasta do projeto dentro da raiz da sua pasta pública do servidor, como descritas no passo 1, não será necessário fazer essa alteração, logo o JQuery estará assim:

	$.ajax({
        url: 'http://localhost/monorepo_husky/backend/status',
        type: 'GET',
        dataType: "JSON",
        success: (data) => {
            changeStatus(data);
        },
        error: (error) => {
            console.log(error.responseText);
        }
    });

    Se estiver salvo em um diretório diferente, será necessário alterar todos os 4 arquivos javascripts com o endereço no qual a pasta do servidor foi adicionada. 

    $.ajax({
        url: 'http://localhost/<meu_diretorio>/monorepo_husky/backend/status',
        type: 'GET',
        dataType: "JSON",
        success: (data) => {
            changeStatus(data);
        },
        error: (error) => {
            console.log(error.responseText);
        }
    });
    

## Observações

+ O BANCO DE DADOS FOI CRIADO DE FORMA SIMPLES E OBJETIVA PARA ESSE PROJETO VISANDO EXECUTAR O DESAFIO DE FORMA SUCINTA.

+ NÃO FORAM FEITAS TODAS AS VALIDAÇÕES POSSÍVEIS NO FRONTEND.

+ O TAMANHO DA PASTA DO PROJETO ESTÁ GRANDE POR CAUSA DA PASTA DO SWAGGER.

+ AS CHAVES ESTRANGEIRAS DA TABELA DELIVERY DEVEM SER ADICIONADAS DEPENDENDO DO REQUERIMENTO DO PROJETO.
SENDO ASSIM, FORAM COLOCADAS TODAS AS CHAVES ESTRANGEIRAS COM UPDATE E DELETE CASCADE, DESSA FORMA OS AVALIADORES PODEM EXECUTAR O CRUD SEM SE PREOCUPAR COM AS RESTRIÇÕES.
