# loan-imobiliario

# Sistema de Empréstimo Imobiliário

Este é um sistema de **Empréstimo Imobiliário** desenvolvido com **Java 17** e **Spring Boot**, com foco na simulação de empréstimos, cálculo de parcelas, e gestão de crédito para clientes interessados em adquirir imóveis. A arquitetura é baseada em **microsserviços**, garantindo escalabilidade e manutenção facilitada.

## Funcionalidades

- **Simulação de Empréstimo**: O cliente pode simular o valor das parcelas e as condições de crédito para um determinado valor do imóvel.
- **Cadastro de Cliente**: Registro de clientes com dados pessoais e financeiros, utilizados para análise de crédito.
- **Aprovação de Crédito**: Sistema de análise de crédito baseado em dados do cliente, para aprovação ou negativa do empréstimo.
- **Gerenciamento de Contratos**: Após a aprovação do crédito, o cliente recebe um contrato com as condições de pagamento.
- **Cálculo de Juros e Parcelas**: O sistema realiza cálculos automáticos para determinar a taxa de juros e o valor das parcelas com base no perfil do cliente e no valor do empréstimo.

## Arquitetura

A arquitetura do sistema segue o padrão de **microsserviços**:

┌─────────────────┐
│ Config Server │ ← Spring Cloud Config
└─────────────────┘
│
▼
┌────────────────┐ ┌───────────────┐
│ Auth Service │◄──────►│ RabbitMQ │
└────────────────┘ └───────────────┘
│ JWT
▼
┌────────────────┐
│ Loan Service │ ← Cálculo de Empréstimo / Juros / Parcelas
└────────────────┘
│
┌────────────────┐
│ Customer Service│ ← Gerenciamento de Clientes
└────────────────┘
│
┌────────────────┐
│ Contract Service│ ← Geração de Contratos e Acompanhamento
└────────────────┘


## Tecnologias

- **Java 17**: Linguagem principal do projeto.
- **Spring Boot 3.x**: Framework principal para construção da aplicação.
- **Spring Web**: Para criação de APIs RESTful.
- **Spring Security (JWT)**: Autenticação e autorização via JWT.
- **Spring Data JPA + Oracle**: Persistência de dados utilizando JPA e Oracle.
- **Spring Batch + Quartz**: Para processamento assíncrono de dados e cálculos periódicos.
- **RabbitMQ**: Para mensageria entre microsserviços.
- **Flyway**: Para versionamento e migrações de banco de dados.
- **Swagger/OpenAPI**: Para documentação da API.
- **Docker**: Para containerização do projeto.
- **Jenkins**: Para automação de builds e deploys.
- **Kubernetes**: Para orquestração de microsserviços em produção.

## Instruções de Instalação

### 1. Clonando o Repositório

Clone este repositório para a sua máquina local:

bash
git clone https://github.com/seu-usuario/loan-imobiliario.git
cd loan-imobiliario

2. Configuração do Banco de Dados
Este projeto usa o Oracle XE como banco de dados. Você pode usar o Docker para rodar o banco de dados localmente:
bash
Copiar código
docker-compose up -d
Isso irá iniciar um container Docker com o Oracle XE.

3. Executando o Projeto
Após configurar o banco de dados, rode a aplicação utilizando o Maven:
bash
Copiar código
./mvnw spring-boot:run
Ou, se você já tiver o JDK 17 instalado:
bash
Copiar código
mvn spring-boot:run
A aplicação estará disponível em http://localhost:8080.

4. Endpoints da API
POST /api/v1/loans/simulate: Simula um empréstimo com base nos parâmetros fornecidos.
POST /api/v1/loans/apply: Solicita o empréstimo, iniciando o processo de análise de crédito.
GET /api/v1/loans/{id}/status: Verifica o status do empréstimo (aprovado, pendente, negado).

5. Dockerização
Se preferir, você pode também rodar a aplicação e o banco de dados com Docker. Para isso, crie a imagem e rode o container:
bash
Copiar código
docker build -t loan-imobiliario .
docker run -p 8080:8080 loan-imobiliario
O banco de dados Oracle será iniciado automaticamente com o Docker Compose.

Estrutura de Diretórios
bash
Copiar código
loan-imobiliario
├── loan-service
│   ├── src/main/java
│   │   └── br/com/empresa/loan
│   │       ├── config
│   │       │   ├── SecurityConfig.java
│   │       │   ├── SwaggerConfig.java
│   │       ├── controller
│   │       │   └── LoanController.java
│   │       ├── dto
│   │       │   └── LoanRequestDTO.java
│   │       ├── model
│   │       │   └── Loan.java
│   │       ├── repository
│   │       │   └── LoanRepository.java
│   │       ├── service
│   │       │   └── LoanService.java
│   │       └── util
│   │           └── LoanCalculator.java
│   └── src/main/resources
│       ├── application.yml
│       ├── db/migration
│       │   └── V1__create_loan_table.sql
│       └── bootstrap.yml
├── docker
│   ├── Dockerfile
│   └── docker-compose.yml
└── README.md


Como Contribuir
Se você deseja contribuir com o projeto, por favor siga as etapas abaixo:

Faça um fork deste repositório.

Crie uma branch com sua feature: git checkout -b minha-feature.

Commit suas mudanças: git commit -m 'Adicionando minha feature'.

Envie para o repositório remoto: git push origin minha-feature.

Crie um pull request.
