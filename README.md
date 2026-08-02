<p align="center">
<img src="https://github.com/user-attachments/assets/ec991ec1-2398-45da-ba43-a1e73930ef20" width="520"/>
</p>

<h1 align="center">Blog Pessoal: API RESTful com Spring Boot, Spring Security e JWT</h1>

<p align="center">
<img src="https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring&logoColor=white">
<img src="https://img.shields.io/badge/Spring_Security-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white">
<img src="https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white">
<img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
<img src="https://img.shields.io/badge/Java_17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white">
</p>

<p align="center">
Repositório referente à resolução do Projeto Blog Pessoal, proposto pela Generation Brasil: uma API RESTful para um sistema de blog pessoal, com autenticação e autorização via Spring Security e Token JWT.
</p>

---

## 🌸 Sobre o projeto

A aplicação disponibiliza um Blog Pessoal onde usuários autenticados podem publicar postagens associadas a temas. O modelo de dados contempla três entidades — **Usuário**, **Tema** e **Postagem** —, com as seguintes relações:

```
Usuário (1) ───── (N) Postagem (N) ───── (1) Tema
```

Cada Postagem pertence a um Usuário e a um Tema, o que caracteriza dois relacionamentos **Um para Muitos (1:N)**.

---

## 🌸 Habilidades trabalhadas

- Construção de API RESTful com **Spring Boot 3.5.4**
- Mapeamento Objeto-Relacional (ORM) com **Spring Data JPA** e **Hibernate**
- Persistência e manipulação de dados relacionais com **MySQL**
- Autenticação e autorização com **Spring Security**
- Geração e validação de **Token JWT** (jjwt 0.12.6), via filtro de autenticação (`JwtAuthFilter`) e serviço de token (`JwtService`)
- Criptografia de senha com **BCrypt**
- Validação de Dados de Entrada (**Bean Validation / Jakarta Validation**)
- Mapeamento de relacionamentos **1:N** e **N:1** entre entidades (`@OneToMany` e `@ManyToOne`)
- Testes automatizados com **JUnit 5** e banco de dados **H2** em memória
- Versionamento com **Git/GitHub**

---

## 🌸 Estrutura do projeto

```text
com.generation.blogpessoal
 ├── BlogpessoalApplication.java
 ├── controller
 │    ├── PostagemController.java
 │    ├── TemaController.java
 │    └── UsuarioController.java
 ├── model
 │    ├── Postagem.java
 │    ├── Tema.java
 │    ├── Usuario.java
 │    └── UsuarioLogin.java
 ├── repository
 │    ├── PostagemRepository.java
 │    ├── TemaRepository.java
 │    └── UsuarioRepository.java
 ├── security
 │    ├── JwtAuthFilter.java
 │    ├── JwtService.java
 │    ├── SecurityConfig.java
 │    ├── UserDetailsImpl.java
 │    └── UserDetailsServiceImpl.java
 └── service
      └── UsuarioService.java
```

---

## 🌸 Endpoints da API

### Usuários (`/usuarios`)

- `GET /usuarios/all` — Listar todos os usuários
- `GET /usuarios/{id}` — Buscar usuário por ID
- `POST /usuarios/cadastrar` — Cadastrar novo usuário (senha criptografada com BCrypt)
- `PUT /usuarios/atualizar` — Atualizar usuário existente
- `POST /usuarios/logar` — Autenticar usuário e receber o Token JWT

### Temas (`/temas`)

- `GET /temas` — Listar todos os temas
- `GET /temas/{id}` — Buscar tema por ID
- `GET /temas/descricao/{descricao}` — Buscar temas por descrição (Case Insensitive)
- `POST /temas` — Criar novo tema
- `PUT /temas` — Atualizar tema existente
- `DELETE /temas/{id}` — Deletar tema por ID

### Postagens (`/postagens`)

- `GET /postagens` — Listar todas as postagens
- `GET /postagens/{id}` — Buscar postagem por ID
- `GET /postagens/titulo/{titulo}` — Buscar postagens por título (Case Insensitive)
- `POST /postagens` — Criar nova postagem vinculada a um tema e a um usuário existentes
- `PUT /postagens` — Atualizar postagem existente
- `DELETE /postagens/{id}` — Deletar postagem por ID

Com exceção dos endpoints `/usuarios/logar`, `/usuarios/cadastrar` e da documentação Swagger, todas as rotas exigem um Token JWT válido no cabeçalho `Authorization`.

---

## 🌸 Requisitos do Projeto

- A entidade **Usuário** possui, no mínimo, os atributos `id`, `nome`, `usuario` (e-mail), `senha` e `foto`, além do relacionamento com a entidade **Postagem**.
- A entidade **Tema** possui, no mínimo, os atributos `id` e `descricao`, além do relacionamento com a entidade **Postagem**.
- A entidade **Postagem** possui, no mínimo, os atributos `id`, `titulo`, `texto` e `data`, além dos relacionamentos com as entidades **Usuário** e **Tema**.
- CRUD completo implementado para os três recursos, contemplando os métodos:
  1. Listar todos os registros persistidos
  2. Buscar um registro pelo identificador (ID)
  3. Listar registros com base em um atributo específico da entidade
  4. Cadastrar novos registros
  5. Atualizar registros existentes
  6. Remover registros do banco de dados
- Autenticação via Token JWT para acesso aos recursos protegidos.

---

## 🌸 Configuração do Banco de Dados

1. Certifique-se de ter o **MySQL** instalado e em execução na sua máquina.
2. Verifique o arquivo `src/main/resources/application.properties`:

```properties
spring.application.name=blogpessoal

spring.jpa.hibernate.ddl-auto=update

spring.datasource.url=jdbc:mysql://localhost/db_blogpessoal?createDatabaseIfNotExist=true&serverTimezone=America/Sao_Paulo&useSSl=false

spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.show-sql=true
spring.jpa.open-in-view=true
spring.jpa.properties.hibernate.jdbc.time_zone=America/Sao_Paulo

spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=America/Sao_Paulo
```

O Spring Data JPA criará a base de dados `db_blogpessoal` e as tabelas `tb_usuarios`, `tb_temas` e `tb_postagens` automaticamente ao iniciar a aplicação.

---

## 🌸 Como executar

1. **Clonar o repositório:**

```bash
git clone https://github.com/luizavpg-bit/backend_blogpessoal_spring.git
```

2. **Aceder à pasta do projeto:**

```bash
cd backend_blogpessoal_spring
```

3. **Configurar o banco de dados** seguindo os passos da seção [Configuração do Banco de Dados](#configuração-do-banco-de-dados) acima.

4. **Executar o projeto:**

```bash
./mvnw spring-boot:run
```

5. **Aceder e testar a API:** A aplicação estará em execução em `http://localhost:8080`. Cadastre um usuário em `/usuarios/cadastrar`, autentique-se em `/usuarios/logar` para obter o Token JWT e utilize-o no cabeçalho `Authorization` das demais requisições, via Insomnia ou Postman.

---

## 🌸 Testes Unitários

O projeto conta com testes automatizados utilizando **JUnit 5** e banco de dados **H2** (em memória, exclusivo para os testes), incluindo utilitários de apoio para geração de tokens JWT (`JwtHelper`) e construção de dados de teste (`TestBuilder`).

### Estrutura dos testes

```text
src/test/java/com/generation/blogpessoal
 ├── BlogpessoalApplicationTests.java
 ├── controller
 │    └── UsuarioControllerTest.java
 └── util
      ├── JwtHelper.java
      └── TestBuilder.java
```

### Como executar os testes

```bash
./mvnw test
```

---

## 🌸 Tecnologias utilizadas

- Java 17
- Spring Boot 3.5.4
- Spring Data JPA
- Spring Security
- JWT (jjwt 0.12.6)
- Jakarta Validation
- MySQL
- H2 (testes)
- Maven

---

## 🌸 Desenvolvido por:

**Luiza Valentina Paolinelli Guimarães**
