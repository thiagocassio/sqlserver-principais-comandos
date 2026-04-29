# 2. Segurança e Controle de Acesso

## 2.1 CREATE LOGIN

**O que é:**

Comando utilizado para criar uma credencial de acesso no nível da instância do SQL Server, permitindo autenticação no servidor.

**Quando usar:**

Utilizado quando um novo usuário, aplicação ou serviço precisa acessar a instância do SQL Server.

```sql
CREATE LOGIN login_app
WITH PASSWORD = '<StrongPasswordHere>';
GO
```

---

## 2.2 CREATE USER

**O que é:**

Comando responsável por criar um usuário dentro de um banco de dados e vinculá-lo a um login existente.

**Quando usar:**

Utilizado quando um login já criado precisa receber acesso específico a um banco de dados.

```sql
USE DBA_LAB;
GO

CREATE USER usr_app
FOR LOGIN login_app;
GO
```

---

## 2.3 ALTER ROLE

**O que é:**

Comando usado para adicionar ou remover usuários de roles, facilitando o gerenciamento de permissões.

**Quando usar:**

Utilizado quando é necessário conceder ou ajustar permissões de forma padronizada e centralizada.

```sql
ALTER ROLE db_datareader
ADD MEMBER usr_app;
GO

ALTER ROLE db_datawriter
ADD MEMBER usr_app;
GO
```

---

## 2.4 GRANT / DENY / REVOKE

**O que é:**

Comandos utilizados para conceder, negar ou remover permissões sobre objetos do banco de dados.

**Quando usar:**

Utilizado no controle detalhado de acesso a tabelas, views, procedures e demais objetos do ambiente.

```sql
GRANT SELECT ON dbo.TB_CLIENTE TO usr_app;
GO

DENY DELETE ON dbo.TB_CLIENTE TO usr_app;
GO

REVOKE INSERT ON dbo.TB_CLIENTE FROM usr_app;
GO
```

---

## 2.5 ALTER AUTHORIZATION

**O que é:**

Comando responsável por alterar o proprietário de um objeto, schema ou banco de dados.

**Quando usar:**

Utilizado em ajustes de segurança, padronização de ownership ou correção de permissões administrativas.

```sql
ALTER AUTHORIZATION
ON SCHEMA::dbo
TO dbo;
GO
```

---

## 2.6 Schemas

**O que é:**

Estrutura lógica utilizada para organizar objetos do banco de dados, como tabelas, views e procedures.

**Quando usar:**

Utilizado para separar objetos por área, aplicação ou responsabilidade administrativa, melhorando organização e segurança.

```sql
CREATE SCHEMA adm;
GO

CREATE TABLE adm.TB_CLIENTE
(
    ID_CLIENTE INT IDENTITY(1,1),
    NM_CLIENTE VARCHAR(200),
    DT_CADASTRO DATETIME DEFAULT GETDATE()
);
GO
```
