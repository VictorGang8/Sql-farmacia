# 💊 Sistema de Farmácia — SQL e Segurança de Banco de Dados

![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Database%20Management-orange?style=for-the-badge&logo=databricks&logoColor=white)
![Status](https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge)

## 📌 Sobre o projeto

Este projeto consiste na criação e configuração de um banco de dados para gerenciamento de informações de uma farmácia, utilizando **MySQL e SQL**.

Além da estruturação das tabelas e manipulação de registros, o projeto tem como foco principal a implementação de mecanismos de segurança, controle de acesso e gerenciamento de usuários.

A proposta é demonstrar como diferentes funcionários podem acessar o banco de dados de acordo com suas responsabilidades, aplicando o princípio do **menor privilégio**, no qual cada usuário recebe somente as permissões necessárias para executar suas atividades.

## 🎯 Objetivos

- Criar um banco de dados relacional utilizando MySQL.
- Estruturar tabelas para armazenamento de produtos e funcionários.
- Inserir e manipular registros utilizando comandos SQL.
- Criar usuários com diferentes níveis de acesso.
- Implementar permissões específicas para cada perfil.
- Conceder e revogar privilégios utilizando `GRANT` e `REVOKE`.
- Realizar testes de autenticação e autorização.
- Demonstrar o gerenciamento de contas, incluindo alteração de senhas, bloqueio, desbloqueio e exclusão de usuários.

## 🛠️ Tecnologias e ferramentas

| Tecnologia | Aplicação |
|---|---|
| MySQL | Sistema gerenciador de banco de dados |
| SQL | Linguagem utilizada para criação e manipulação dos dados |
| DDL | Definição da estrutura do banco de dados |
| DML | Inserção, consulta, alteração e exclusão de registros |
| DCL | Controle de privilégios e permissões |
| MySQL Workbench | Ambiente para execução e gerenciamento de comandos SQL |

## 🗄️ Estrutura do banco de dados

O banco de dados utilizado no projeto possui o nome:

`farmacia_seg`

### 1. Tabela: produto

Responsável por armazenar as informações dos produtos disponíveis na farmácia.

| Campo | Tipo de dado | Descrição |
|---|---|---|
| id | INT | Identificador único do produto |
| nome | VARCHAR(50) | Nome do produto |
| preco | DECIMAL(7,2) | Preço do produto |
| estoque | INT | Quantidade disponível em estoque |

### 2. Tabela: funcionario

Responsável por armazenar informações dos funcionários da farmácia.

| Campo | Tipo de dado | Descrição |
|---|---|---|
| id | INT | Identificador único do funcionário |
| nome | VARCHAR(50) | Nome do funcionário |
| cargo | VARCHAR(30) | Cargo ocupado |
| salario | DECIMAL(8,2) | Salário do funcionário |

## 🔐 Segurança e controle de acesso

Um dos principais objetivos do projeto é demonstrar como o controle de permissões pode proteger as informações armazenadas no banco de dados.

Foram definidos dois perfis de acesso:

### 👤 Usuário: atendente

O atendente possui permissão somente para consultar os produtos cadastrados.

**Permissões:**
- `SELECT` — consultar informações da tabela de produtos.

**Restrições:**
- Não pode inserir novos produtos.
- Não pode alterar registros.
- Não pode excluir informações.
- Não possui acesso autorizado às demais operações do banco.

### 👨‍💼 Usuário: gerente

O gerente possui permissões mais amplas para administrar informações do banco de dados.

**Permissões inicialmente concedidas:**
- `SELECT` — consultar registros.
- `INSERT` — cadastrar novos registros.
- `UPDATE` — atualizar informações existentes.

**Restrições:**
- Não possui permissão para executar `DELETE`.
- O privilégio `UPDATE` é posteriormente revogado por meio do comando `REVOKE`.

### 📊 Comparação de permissões

| Operação | Atendente | Gerente |
|---|:---:|:---:|
| Consultar produtos | ✅ | ✅ |
| Inserir registros | ❌ | ✅ |
| Atualizar registros | ❌ | ✅ Inicialmente |
| Excluir registros | ❌ | ❌ |
| Consultar funcionários | ❌ | ✅ |

*Observação: a tabela representa as permissões inicialmente concedidas. Após a execução do `REVOKE UPDATE`, o gerente também perde a permissão de atualização.*

## ⚙️ Principais comandos SQL utilizados

O projeto trabalha com diferentes categorias da linguagem SQL.

### DDL — Data Definition Language

Utilizada para definir a estrutura do banco de dados.

Exemplos:

```sql
CREATE DATABASE farmacia_seg;

USE farmacia_seg;

CREATE TABLE produto (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    preco DECIMAL(7,2) NOT NULL,
    estoque INT NOT NULL
);
```

### DML — Data Manipulation Language

Utilizada para manipular os registros armazenados nas tabelas.

| Comando | Função |
|---|---|
| INSERT | Inserir novos registros |
| SELECT | Consultar informações |
| UPDATE | Atualizar registros |
| DELETE | Excluir registros |

Exemplo:

```sql
-- Consultar todos os produtos
SELECT * FROM produto;

-- Inserir um novo produto
INSERT INTO produto (nome, preco, estoque)
VALUES ('Creme hidratante', 29.90, 20);

-- Atualizar o preço de um produto
UPDATE produto
SET preco = 13.50
WHERE id = 1;
```

### DCL — Data Control Language

Utilizada para controlar os privilégios dos usuários.

| Comando | Função |
|---|---|
| GRANT | Conceder permissões |
| REVOKE | Revogar permissões |

Exemplo:

```sql
-- Conceder permissão de consulta ao atendente
GRANT SELECT ON farmacia_seg.produto
TO 'atendente'@'localhost';

-- Conceder permissões ao gerente
GRANT SELECT, INSERT, UPDATE ON farmacia_seg.*
TO 'gerente'@'localhost';

-- Revogar a permissão de atualização do gerente
REVOKE UPDATE ON farmacia_seg.*
FROM 'gerente'@'localhost';
```

## 🔑 Gerenciamento de usuários

O projeto também demonstra operações administrativas para gerenciamento das contas do MySQL.

| Comando | Finalidade |
|---|---|
| CREATE USER | Criar usuários |
| ALTER USER | Alterar configurações de usuários |
| SHOW GRANTS | Consultar permissões concedidas |
| ACCOUNT LOCK | Bloquear uma conta |
| ACCOUNT UNLOCK | Desbloquear uma conta |
| DROP USER | Excluir usuários |

Exemplo de consulta de permissões:

```sql
SHOW GRANTS FOR 'atendente'@'localhost';

SHOW GRANTS FOR 'gerente'@'localhost';
```

## 🧪 Testes de segurança

Foram utilizados testes para verificar se cada usuário consegue executar somente as operações autorizadas.

**Teste do atendente:**

```sql
SELECT * FROM farmacia_seg.produto;
```

A consulta deve ser permitida, enquanto operações sem privilégios concedidos devem ser bloqueadas.

**Teste do gerente:**

```sql
INSERT INTO farmacia_seg.produto (nome, preco, estoque)
VALUES ('Protetor solar', 45.90, 15);
```

A inserção deve ser permitida enquanto o privilégio `INSERT` estiver ativo.

**Teste de revogação:**

Após executar o comando `REVOKE UPDATE`, uma tentativa de atualização pelo gerente deverá ser negada, caso não exista outra concessão de privilégio que permita essa operação.

> Importante: para validar corretamente as restrições, os testes devem ser executados utilizando as respectivas contas de usuário, e não uma conta administrativa como `root`.

## 🛡️ Conceitos de segurança aplicados

O projeto aborda conceitos importantes de segurança em bancos de dados:

- **Controle de acesso:** definição de quem pode acessar determinadas informações.
- **Autenticação:** identificação dos usuários que tentam acessar o banco.
- **Autorização:** definição das operações permitidas para cada usuário.
- **Princípio do menor privilégio:** concessão apenas das permissões necessárias.
- **Revogação de privilégios:** remoção de permissões anteriormente concedidas.
- **Gerenciamento de contas:** criação, alteração, bloqueio e exclusão de usuários.

## 📚 Aprendizados e conhecimentos desenvolvidos

Com o desenvolvimento deste projeto, foram praticados conhecimentos relacionados a:

- Modelagem e estruturação de bancos de dados relacionais.
- Criação de bancos de dados e tabelas.
- Definição de chaves primárias e tipos de dados.
- Manipulação de registros utilizando SQL.
- Administração básica do MySQL.
- Gerenciamento de usuários e permissões.
- Implementação de controles de segurança.
- Aplicação do princípio do menor privilégio.
- Utilização de comandos DDL, DML e DCL.

## 🚀 Possíveis melhorias futuras

O projeto pode ser expandido com novas funcionalidades:

- Implementação de uma tabela de fornecedores.
- Cadastro de categorias de medicamentos.
- Controle de entradas e saídas de estoque.
- Registro de movimentações financeiras.
- Histórico de alterações realizadas pelos usuários.
- Criação de perfis adicionais, como estoquista e administrador.
- Implementação de permissões específicas por coluna.
- Desenvolvimento de uma aplicação integrada ao banco de dados.

## 👨‍💻 Autor

**Victor Gabriel Tavares Oliveira**

Estudante de Engenharia de Software, com interesse em desenvolvimento de software, banco de dados, SQL, automação e tecnologias Microsoft Power Platform.

- GitHub: [@VictorGang8](https://github.com/VictorGang8)
- Repositório: [Sql-farmacia](https://github.com/VictorGang8/Sql-farmacia)

---

<p align="center">
  Desenvolvido para fins acadêmicos e de aprendizado em SQL, MySQL e segurança de bancos de dados.
</p>
