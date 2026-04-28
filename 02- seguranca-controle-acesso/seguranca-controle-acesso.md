# 2. Segurança e Controle de Acesso

## 2.1 CREATE LOGIN

```sql
CREATE LOGIN login_app
WITH PASSWORD = '<StrongPasswordHere>';
GO
```

---

## 2.2 CREATE USER

```sql
USE DBA_LAB;
GO

CREATE USER usr_app
FOR LOGIN login_app;
GO
```

---

## 2.3 ALTER ROLE

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

```sql
ALTER AUTHORIZATION
ON SCHEMA::dbo
TO dbo;
GO
```

---

## 2.6 Schemas

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
