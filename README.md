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


3-Funções Lambda.
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
Clique na função 






