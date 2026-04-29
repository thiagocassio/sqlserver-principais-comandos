# 9. Criptografia e Segurança Avançada

## 9.1 TDE

**O que é:**

Transparent Data Encryption é o recurso que criptografa arquivos físicos do banco de dados em repouso, protegendo MDF, NDF, LDF e backups.

**Quando usar:**

Utilizado quando há necessidade de proteção contra acesso indevido aos arquivos físicos e exigências de compliance e auditoria.

```sql
USE master;
GO

CREATE MASTER KEY
ENCRYPTION BY PASSWORD = '<StrongPasswordHere>';
GO

CREATE CERTIFICATE CERT_TDE_DBA
WITH SUBJECT = 'Certificado TDE DBA';
GO

USE DBA_LAB;
GO

CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256
ENCRYPTION BY SERVER CERTIFICATE CERT_TDE_DBA;
GO

ALTER DATABASE DBA_LAB
SET ENCRYPTION ON;
GO
```

---

## 9.2 Always Encrypted

**O que é:**

Recurso que protege dados sensíveis no nível da coluna, mantendo a informação criptografada inclusive para administradores do banco.

**Quando usar:**

Utilizado em colunas com dados críticos como CPF, cartão, salário e informações sensíveis com alto nível de segurança.

```sql
CREATE COLUMN MASTER KEY CMK_DBA
WITH
(
    KEY_STORE_PROVIDER_NAME = 'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = 'CurrentUser/My/XXXXXXXXXXXX'
);
GO
```

---

## 9.3 Certificates

**O que é:**

Objetos criptográficos utilizados para proteger chaves, assinar objetos e suportar recursos como TDE e Backup Encryption.

**Quando usar:**

Utilizado em cenários de criptografia, assinatura digital e proteção de dados sensíveis dentro do SQL Server.

```sql
CREATE CERTIFICATE CERT_APP_ACCESS
WITH SUBJECT = 'Certificado Aplicacao';
GO
```

---

## 9.4 DMK / SMK

**O que é:**

Database Master Key e Service Master Key são chaves raiz responsáveis pela proteção hierárquica de outras chaves criptográficas.

**Quando usar:**

Utilizado na implementação de TDE, Always Encrypted, certificados e qualquer estrutura avançada de criptografia.

```sql
CREATE MASTER KEY
ENCRYPTION BY PASSWORD = '<StrongPasswordHere>';
GO

OPEN MASTER KEY
DECRYPTION BY PASSWORD = '<StrongPasswordHere>';
GO
```

---

## 9.5 Symmetric Keys

**O que é:**

Chaves criptográficas utilizadas para criptografar e descriptografar dados com alta performance dentro do banco de dados.

**Quando usar:**

Utilizado quando é necessário proteger dados em nível de coluna com criptografia personalizada e controle interno.

```sql
CREATE SYMMETRIC KEY SK_DADOS
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE CERT_APP_ACCESS;
GO
```

---

## 9.6 Backup Encryption

**O que é:**

Recurso que permite gerar arquivos de backup já criptografados durante o processo de backup.

**Quando usar:**

Utilizado quando backups precisam ser protegidos contra acesso indevido durante armazenamento, transporte ou retenção externa.

```sql
BACKUP DATABASE DBA_LAB
TO DISK = 'C:\SQLBackup\DBA_LAB_ENCRYPTED.bak'
WITH
    COMPRESSION,
    ENCRYPTION
    (
        ALGORITHM = AES_256,
        SERVER CERTIFICATE = CERT_TDE_DBA
    ),
    STATS = 10;
GO
```
