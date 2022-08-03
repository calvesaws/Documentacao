# Documentacao
Documentação do projeto GetmySecret

°Contextualização do projeto

A ideia principal do projeto é o compartilhamento seguro de dados sensíveis entre usuários, compartilhando esses dados com segurança evitando vazamentos, e
garantindo que somente o usuário poderá ter acesso aos seus dados após o compartilhamento utilizando a criptografia (chave assimétrica), onde apenas usuários
terão acesso a esses dados sensíveis.
O emissor irá gerar um link através de uma plataforma web fornecendo uma mensagem representando um dado sensível, uma senha, um tempo de expiração da
mensagem e
a quantidade de vezes que o usuário poderá acessar essa mensagem, garantindo maior segurança no compartilhamento de seus dados. O link gerado nada mais é do
que
o link da página web que ele utilizou para desenvolver a mensagem e o id desta mensagem no banco de dados.

Foi utilizado os seguintes serviços da AWS: Funções Lambda, banco de dados DynamoDB, ApiGateway e EC2. Por serem serviços distintos, por padrão não há
relacionamentos entre eles, portanto se faz necessário adicionar permissões para esses serviços interagirem entre si.

1- Banco de dados(DynamoDB)
Acesse o serviço DynamoDB na console AWS.
Criar tabela
Defina um nome, uma chave de partição(que será a identificação do seu documento) e defina o tipo que será essa chave,
Após criar a tabela, defina o tempo de vida que o documento permanecerá ativo na tabela, selecione-a e clique em "configurações adicionais". Em "Tempo de
vida (TTL)" selecione a opção "Habilitar". Insira o nome do atributo que terá a informação de tempo que o documento poderá ficar ativo(esse atributo será
utilizado na função Lambda futuramente para controlar o tempo de vida do documento) e clique em habilitar TTL.

2- Role
Role são permissões que os serviços precisam para interagir entre si. 

Acesse o serviço IAM.
Funções.
Criar função.
Selecione "Serviço da AWS".
Em "Caso de uso" selecione "Lambda" e clique em "próximo".
Pesquise por "AmazonDynamoDBFullAccess" e "AWSLambdaBasicExecutionRole". Clique em "próximo".
Defina o nome da função e a Descrição.
Clique em "Criar função".


3- Funções Lambda.
As funções lambda servem para você desenvolver o seu código backend.
Forão criadas três funções Lambda para executar o código com as orientações a seguir:
Inserir, pegar e excluir.
Função Inserir - Responsável por gravar registros na tabela do banco de dados(DynamoDB).
Adicionei a função de criptografar a mensagem.
Função Pegar- Responsável por resgatar um registro na tabela através do id.
Função excluir - Exclui o registro na tabea.


Acesse o serviço Lambda, Funções e Criar funções.
Defina um nome para a função.
Em "Tempo de execução", defina a linguagem que será desenvolvida sua função.
Clique em "Criar função".
Faça esse processo para as três funções do código, inserir, pegar e excluir.

Após as funções serem criadas, faça o download dos arquivos:
Função inserir:
https://github.com/calvesaws/Documentacao/raw/main/GravarMensagem-20e7bc2d-19f2-4396-9cd8-bf363f1639fd.zip

Função pegar:
https://github.com/calvesaws/Documentacao/raw/main/GetRegistros-c7deebb1-0e40-433c-aeed-56d8fb733465.zip

Funçao excluir:
https://github.com/calvesaws/Documentacao/raw/main/DeleteRegistro-a12931f7-ad90-4f43-b0c4-9622561ed7ce.zip

Faça uploud em cada função com o link dos dowloads.
Clique na função que for desenvolver para inserir registros na tabela do banco de dados.
Clique em "Fazer upload de" e em seguida "Arquivo zip".
Selecione a opção "Fazer download" escolha o download referente a função inserir e clique em "Salvar".
Faça esse mesmo procedimento para todas as funções com os downloads respectivos.

Com as funções criadas, adicione as permições criadas previamente.
Dentro de uma função, clique em "Configuração".
Em "Papel de execução", clique em "Editar" e selecione o grupo de permissões que foi criado e clique em "Salvar".
Faça isso para todas as funções.


4- Api Gateway
A ApiGateway tem a função de integração com as funções lambda.

Acesse o serviço API Gateway.
Crie uma API.
Selecione a opção "API REST" (pública).
Clique em "Compilar".
Defina um nome para a API e clique em "Criar API".
Na opção "Ações" defina o Recurso e método.
Recurso: Insira o nome do recurso e o caminho desse recurso.
Selecione a opção "Ativar CORS do API Gateway" e "Criar recurso".
Método: Selecione a opção "Post"
*Obs. Apenas para get e delete selecione "Usar a integração de proxy do Lambda" 
Função Lambda: Insira o nome da função responsável pelo código de inserir registros na tabela do bano de dados. Clique em "Salvar".


5- EC2
A plataforma EC2 é uma máquina virtual para hospedar o código front, que será utilizado para melhorar a experiência dos usuários.

Acesse o serviço EC2.
Clique em "Instâncias".
Executar instâncias.
Dê um nome a instância.
Em "Imagens de aplicação e de sistema operacional" escolha "Amazon Linux".
Em "Par de chaves (login)" é preciso criar um par de chaves para se conectar com segurança à sua instância. Clique em "Criar novo par de chaves".
Defina um nome para o par de chaves.
Clique em "Criar par de chaves".
Em "Network settings" habilite as seguintes regras:"Permitir tráfego HTTPs da Internet" e "Permitir tráfego HTTP da Internet".
Clique em "Executar intância".
Clique em "Ações", "Conectar" e "Conectar". Irá abrir um console em outra aba.
Precisará habilitar e instalar servidor web(nginx).
Digite os seguintes comandos no console: "sudo amazon-linux-extras enable nginx1", "sudo yum install nginx", "y".
Nessa etapa precisará configurar o arquivo de configuração.
Digite "sudo systemclt enable ngnix" e "sudo systemclt start ngnix"
Gere a chave e certificado auto-assinado utilizado pelo nginx para conexão https:
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/nginx/server.key -out /etc/pki/nginx/server.crt
Common Name (eg, your name or your server's hostname) []:*.compute-1.amazonaws.com
Adicione o seguinte trecho na seção server responsavel pela porta 80:
location / {
        try_files $uri $uri/ /index.html;
        }
        
Descomente toda seção serve responsável pela 443 server e realize as seguintes alterações:
Comente ou exclua esse trecho:
#   ssl_ciphers PROFILE=SYSTEM;
     #   ssl_prefer_server_ciphers on;

Adicione esse trecho:
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        location / {
        try_files $uri $uri/ /index.html;
        }


6- Código Front
O código front foi desenvolvido em linguagem Vue Js.
O zip do front é https://github.com/calvesaws/Documentacao/raw/main/dist.zip
Execute esses três comandos no EC2:
Para baixar o arquivo:
wget https://github.com/calvesaws/Documentacao/raw/main/dist.zip
Extrai o arquivo:
unzip dist.zip
A
Copia os arquivos:
sudo cp -a dist/* /usr/share/nginx/html/























