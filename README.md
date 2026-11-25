# 🗄️ Backend ASTRAL


## 🔧 Tecnologias utilizadas

- Java;
- Bota de mola;
- JPA;
- AWS;

# Guia de Configuração - Backend ASTRAL

Este documento contém o passo a passo completo para configurar e executar o projeto em ambiente local.

## Índice

1. Requisitos
2. Instalação do Java
3. Instalação do Maven
4. Instalação do MySQL (XAMPP)
5. Configuração do Banco de Dados
6. Variáveis ​​de Ambiente
7. Executando o Projeto
8. Armazenamento de Imagens
9. Cadastros e Autenticação
10. URLs e pontos de extremidade
11. Solução de Problemas

## 1. Requisitos

| Requisito | Versão/Detalhe |
|---|---|
| Java | 17 ou superior (Java 21 ou 25 recomendado) |
| Maven | 3,6+ |
| MySQL | 8.x |
| Sistema | Windows, Linux ou macOS |

## 2. Instalação do Java

### 2.1. Baixar o Java

Oracle JDK: https://www.oracle.com/java/technologies/downloads/#java17

Eclipse Temurin (OpenJDK): https://adoptium.net/temurin/releases/

Escolha a versão 17 ou superior. Para Java 21 ou 25, comprove-se que o Lombok está na versão 1.18.40+ (o projeto já usa 1.18.42).

### 2.2. Configurar Variáveis de Ambiente (Java)

**Windows:**

- Pressione Win + R, digite sysdm.cpl e pressione Enter
- Aba Avançado → Variáveis de Ambiente
- Em Variáveis do sistema, clique em Novo:
  - Nome: JAVA_HOME
  - Valor: C:\Program Files\Java\jdk-17 (ajuste conforme o caminho de sua instalação)
- Edite a variável Path e adicione: %JAVA_HOME%\bin
- Feche e abra o terminal para aplicar as alterações

**Verificar instalação:**

```bash
java -version
javac -version
```

## 3. Instalação do Maven

### 3.1. Baixar o Maven

⚠️ Importante: Baixe o Apache Maven (ferramenta de build), não o Maven Daemon.

Site oficial: https://maven.apache.org/download.cgi

Baixe o arquivo zip binário (ex. apache-maven-3.9.6-bin.zip:)

### 3.2. Manual de Instalação

Extraia o arquivo para uma pasta (ex.: C:\Program Files\Apache\maven)

O caminho final deve ser algo como: C:\Program Files\Apache\maven\apache-maven-3.9.6

### 3.3. Configurar Variáveis de Ambiente (Maven)

**Windows:**

- Pressione Win + R, digite sysdm.cpl e pressione Enter
- Aba Avançado → Variáveis de Ambiente
- Crie uma variável MAVEN_HOME:
  - Nome: MAVEN_HOME
  - Valor: C:\Program Files\Apache\maven\apache-maven-3.9.6 (ajuste conforme o caminho)
- Edite a variável Path e adicione: %MAVEN_HOME%\bin
- Feche e abra o terminal para aplicar as alterações

**Verificar instalação:**

```bash
mvn -version
```

### 3.4. Alternativa: Instalação via Winget (Windows)

```bash
winget install Apache.Maven
```

## 4. Instalação do MySQL (XAMPP)

O XAMPP inclui o MySQL e o phpMyAdmin, facilitando o gerenciamento do banco.

### 4.1. Baixar e Instalar

Site: https://www.apachefriends.org/

Durante a instalação, marque MySQL e phpMyAdmin.

### 4.2. Iniciar o MySQL

- Abra o Painel de Controle XAMPP
- Clique em Iniciar no MySQL
- Aguarde até o status ficar verde

## 5. Configuração do Banco de Dados

### 5.1. Criar o Banco

Abra o phpMyAdmin: http://localhost/phpmyadmin

Ou clique em Admin ao lado do MySQL no XAMPP.

Na aba SQL, execute:

```sql
CREATE DATABASE astral;
```

### 5.2. Configuração Padrão

O projeto espera:

- Host: localhost
- Porta: 3306
- Banco: astral
- Usuário: root
- Senha: (vazia)

Se seu MySQL tiver senha definida, edite o arquivo src/main/resources/application.properties:

```properties
spring.datasource.password=sua_senha
```

### 5.3. Migrações

O projeto usa Flyway para as migrações. As tabelas são criadas automaticamente na primeira execução. Não é necessário executar SQL manualmente para criar tabelas.

## 6. Variáveis de Ambiente

### 6.1. Variáveis Necessárias (Produção)

| Variável | Descrição |
|---|---|
| MAIL_USERNAME | E-mail para envio (ex.: Gmail) |
| MAIL_SECRET | Senha de app do e-mail |
| JWT_TOKEN_SECRET | Chave secreta para JWT (mínimo 32 caracteres) |
| AWS_ACCESS_KEY | Chave de acesso AWS (se usar S3) |
| AWS_SECRET_KEY | Chave secreta AWS (se usar S3) |
| STORAGE_LOCAL | true para armazenamento local, false para AWS S3 |

### 6.2. Valores Padrão (Desenvolvimento)

O projeto possui valores padrão para desenvolvimento local. Você pode rodar sem configurar nenhuma variável:

- E-mail: dev@localhost / senha-dev
- JWT: chave-secreta-para-desenvolvimento-local-minimo-32-chars
- AWS: placeholder-key / placeholder-secret (local dos EUA)
- Armazenamento: local (padrão)

### 6.3. Configurar no PowerShell (Windows)

Antes de executar o JAR:

```powershell
$env:MAIL_USERNAME = "seu-email@gmail.com"
$env:MAIL_SECRET = "sua-senha-app"
$env:JWT_TOKEN_SECRET = "sua-chave-jwt-secreta"
$env:STORAGE_LOCAL = "false"
$env:AWS_ACCESS_KEY = "sua-chave-aws"
$env:AWS_SECRET_KEY = "sua-chave-secreta-aws"
```

## 7. Executando o Projeto

### 7.1. Compilador e Executor

⚠️ Importante: No Windows, use java -jar em vez de mvn spring-boot:run para evitar o erro de "comando muito longo" (linha de comando excede o limite do Windows).

**Navegar até a pasta do projeto:**

```bash
cd c:\repositorios\astral-backend
```

**Compilar e gerar o JAR:**

```bash
mvn clean package -DskipTests
```

**Executar:**

```bash
java -jar target/astral-backend-1.0.0.jar
```

### 7.2. Porta

A aplicação roda na porta 8080.

## 8. Armazenamento de Imagens

### 8.1. Modo Local (Padrão)

Quando STORAGE_LOCAL=true ou não configurado:

- As imagens são salvas na pasta uploads/ no diretório do projeto
- A pasta é criada automaticamente no primeiro upload
- Não é necessário criar a massa manualmente

### 8.2. URL das Imagens

Sem interface gráfica, use:

```text
http://localhost:8080${produto.imagem}
```

Exemplo:

```text
http://localhost:8080/uploads/produtor_2/Alface.jpg_1773698225633
```

As imagens /uploads/** são públicas (não é necessário autenticação).

### 8.3. Modo AWS S3

Para usar a AWS:

- Configurar STORAGE_LOCAL=false
- Configure AWS_ACCESS_KEY e AWS_SECRET_KEY

## 9. Cadastros e Autenticação

### 9.1. Criptografia de Senhas

O projeto usa BCrypt para hash de senhas.

### 9.2. Cadastrar Usuário Diretamente no Banco

Para criar um usuário manualmente (ex.: para testes):

**Gerar hash BCrypt da senha desejada:**

https://bcrypt-generator.com/

https://www.bcrypt.online/

**Inserir na tabela usuario:**

```sql
INSERT INTO usuario (email, senha)
VALUES ('produtor@email.com', '$2a$10$SEU_HASH_BCRYPT_AQUI');
```

**Para Produtor:**

```sql
INSERT INTO produtor (id, nome, telefone, disponivel, ativo)
VALUES (LAST_INSERT_ID(), 'Nome do Produtor', '11999999999', 1, 1);
```

**Para Administrador:**

```sql
INSERT INTO administrador (id)
VALUES (LAST_INSERT_ID());
```

## 10. URLs e Endpoints

| Recurso | URL |
|---|---|
| API Base | http://localhost:8080 |
| Interface do usuário Swagger | http://localhost:8080/swagger-ui.html |
| Imagens | http://localhost:8080/uploads/... |

## 11. Solução de Problemas

**Erro: "java não é reconhecido"**

Java não está instalado ou não está no PATH.

Verifique como java -version e configure as variáveis ​​de ambiente.

**Erro: "mvn não é reconhecido"**

Maven não está instalado ou não está no PATH.

Baixe o Apache Maven (não o Maven Daemon).

**Erro: "CreateProcess error=206 - O nome do arquivo ou a extensão é muito grande"**

Use java -jar target/astral-backend-1.0.0.jar em vez de mvn spring-boot:run.

**Erro: "Não foi possível resolver o marcador de posição 'AWS_ACCESS_KEY'"**

O projeto já possui valores padrão. Se ainda ocorrer, verifique se as variáveis ​​estão configuradas ou se os valores padrão foram desligados no application.yml e application.properties.

**Erro: "403 Forbidden" ao acessar imagens**

As rotas /uploads/** são definidas como públicas no SecurityConfigurations. Verifique se a alteração foi aplicada.

**XAMPP não instala Maven**

XAMPP inclui Apache, MySQL, PHP. O Maven é uma ferramenta separada e deve ser instalada independentemente.

**Lombok não gera getters/setters (Java 23+)**

Java 23+ requer configuração explícita do processador de anotação. O projeto já usa Lombok 1.18.42 e está configurado maven-compiler-plugin para isso.

## Resumo

```bash
# 1. Instalar: Java 17+, Maven, XAMPP (MySQL)

# 2. Criar banco:
CREATE DATABASE astral;

# 3. Iniciar MySQL no XAMPP

# 4. Executar:
cd c:\repositorios\astral-backend
mvn clean package -DskipTests
java -jar target/astral-backend-1.0.0.jar

# 5. Acessar:
http://localhost:8080/swagger-ui.html
```
