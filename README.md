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
A função Lambda Inserir é responsável por gravar registros no banco de dados(DynamoDB), onde executa para criar o documento com os dados que vou precisar
para enviar a mensagem, como a mensagem em si, uma senha, tempo de armazenamento no banco de dados, quantidade de visualizações e o id referente a essas
informações.
Adicionei a função de criptografar a mensagem para que mesmo se o link for visualizado por terceiros, os dados estarão criptografados no banco de dados,
assegurando o conteúdo da mensagem.

Acesse o serviço Lambda, Funções e Criar funções.
Defina um nome para a função.
Em "Tempo de execução", defina a linguagem que será desenvolvida sua função.
Clique em "Criar função".
Após a função ser criada, desenvolva o código na aba código






