🐾 Pet Mania - API de Cadastro de Animais

Projeto acadêmico de Back-end em Java desenvolvido com Spring Boot e MySQL, com o objetivo de gerenciar o cadastro de animais para adoção.

A API recebe e devolve os dados dos pets em formato JSON e é consumida por um front-end em HTML, que exibe os animais em cards e permite cadastrar novos pets por um formulário.

📌 Contextualização

O Pet Mania é um desafio proposto pela professora. O projeto original foi entregue com problemas inseridos propositalmente, e a missão era investigar, identificar e corrigir o que impedia a API de funcionar.

Sem a correção, o sistema apresentava problemas como:

A tabela pet não era criada no banco de dados.
O cadastro de animais retornava erro.
Os dados enviados pelo front-end não eram aceitos pela API.
🎯 Proposta

Fazer a API funcionar de ponta a ponta:

Corrigir as classes e configurações do projeto.
Conectar a aplicação ao MySQL e criar a tabela automaticamente.
Validar os endpoints pelo Postman.
Validar o fluxo completo pelo front-end (cadastrar e visualizar os animais).
⚙️ Tecnologias utilizadas
Java 26
Spring Boot 4.1.2 (Spring Web MVC)
Spring Data JPA com Hibernate 7.4
MySQL 8.0
Maven
HTML, CSS e JavaScript (front-end em static/index.html)
Postman (testes da API)
IntelliJ IDEA (IDE)
📁 Estrutura do projeto
pet-mania-api-challenge-main/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/com/dt2/petmania/
│   │   │   ├── PetManiaApplication.java
│   │   │   ├── controllers/
│   │   │   │   └── PetController.java
│   │   │   ├── entities/
│   │   │   │   ├── Pet.java
│   │   │   │   └── enums/
│   │   │   │       ├── Porte.java
│   │   │   │       └── Sexo.java
│   │   │   └── repositories/
│   │   │       └── PetRepository.java
│   │   └── resources/
│   │       ├── application.properties
│   │       └── static/
│   │           └── index.html
│   └── test/
└── docs/
    └── images/
🗄️ Modelagem do Banco de Dados

O banco se chama petmania e a tabela pet é criada automaticamente pelo Hibernate a partir da entidade Pet.

Campo	Tipo Java	Descrição
id	int	Chave primária, gerada automaticamente (auto incremento)
nome	String	Nome do pet
especie	String	Espécie (Cachorro, Gato...)
raca	String	Raça do animal
idade	int	Idade em anos
sexo	Sexo (enum)	Macho ou Femea
porte	Porte (enum)	Porte do animal (Pequeno, Grande...)
personalidade	String	Descrição do temperamento
foto	String	URL da foto
disponivel	boolean	Se está disponível para adoção

Os enums Sexo e Porte são guardados no banco como número (tinyint), pela posição do valor no enum. Por isso, não se deve mudar a ordem dos valores depois que existem dados.

🔧 Problemas encontrados e corrigidos
Tabela pet não era criada: o campo nome era a chave primária e estava com @GeneratedValue(strategy = GenerationType.IDENTITY). O MySQL não aceita auto_increment em coluna varchar (erro Incorrect column specifier for column 'nome'). A solução foi criar um campo id numérico como chave primária e deixar nome como campo comum.
Entidade sem construtor vazio: o JPA e o Jackson precisam do construtor sem argumentos (public Pet() {}) para criar o objeto e converter o JSON.
Erro ao receber o JSON do sexo: o enum Sexo tinha valores misturados (femea e Macho), e o Jackson recusava o valor enviado pelo front-end. Os valores foram padronizados (Macho e Femea), mantendo a ordem para não alterar os dados já salvos.
Required request body is missing: o POST precisa receber um corpo JSON com o header Content-Type: application/json.
🚀 Como executar o projeto
Pré-requisitos
JDK instalado (o projeto foi executado com o Java 26)
MySQL 8.0 ou superior rodando em localhost:3306
IntelliJ IDEA ou outra IDE compatível com Maven
Passo a passo
Crie o banco de dados:
sql
CREATE DATABASE IF NOT EXISTS petmania;
Configure o acesso ao banco no arquivo src/main/resources/application.properties, informando a URL (jdbc:mysql://localhost:3306/petmania), o usuário e a senha do seu MySQL.
Execute a aplicação pela classe PetManiaApplication (botão Run da IDE).
Confira no log se o Hibernate executou o create table pet sem nenhum erro e se o Tomcat iniciou na porta 8080.
Acesse o front-end: http://localhost:8080
📡 Endpoints da API

URL base: http://localhost:8080/api/pets

Método	Endpoint	Descrição
GET	/api/pets	Lista todos os pets cadastrados
POST	/api/pets	Cadastra um novo pet (corpo em JSON)

Exemplo de corpo para o POST:

json
{
    "disponivel": true,
    "especie": "Gato",
    "foto": "https://exemplo.com/foto.jpg",
    "idade": 50,
    "nome": "Alexandre",
    "personalidade": "Brincalhão, vagabundo",
    "porte": "Pequeno",
    "raca": "golden",
    "sexo": "Fêmea"
}

O endpoint de exclusão (DELETE) não faz parte desta entrega, conforme orientação da professora.

🧪 Testes no Postman

Requisição GET em localhost:8080/api/pets, que retornou 200 OK com a lista dos pets cadastrados:

Postman - GET retornando a lista de pets

Corpo (JSON) utilizado nos testes:

Postman - corpo da requisição em JSON

🖥️ Testes no Front-end

Formulário de cadastro de um novo pet:

Formulário de cadastro - dados do pet

Continuação do formulário, com porte, personalidade, URL da foto e disponibilidade:

Formulário de cadastro - campos finais e botão Salvar

Pets cadastrados exibidos em cards:

Cards dos pets cadastrados

Tela inicial do Pet Mania, com a lista "Nossos amiguinhos":

Tela inicial do Pet Mania

👨‍💻 Autor

Projeto desenvolvido como atividade acadêmica de Back-end em Java.

Autor: LucasShift344

Desafio proposto por Brenda (repositório original: FiamaBrenda/pet-mania-api-challenge).

📚 Objetivo Educacional

Este projeto tem como finalidade colocar em prática:

Criação de uma API REST com Spring Boot.
Mapeamento de entidades com JPA/Hibernate e conexão com o MySQL.
Identificação e correção de erros a partir da leitura do log da aplicação.
Teste de endpoints com o Postman e integração com um front-end.

Projeto desenvolvido para fins educacionais. Os dados utilizados são fictícios e destinados exclusivamente a testes e demonstrações.
