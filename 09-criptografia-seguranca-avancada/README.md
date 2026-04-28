# 9. Criptografia e Segurança Avançada

## 9.1 TDE

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

```sql
CREATE CERTIFICATE CERT_APP_ACCESS
WITH SUBJECT = 'Certificado Aplicacao';
GO
```

---

## 9.4 DMK / SMK

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

```sql
CREATE SYMMETRIC KEY SK_DADOS
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE CERT_APP_ACCESS;
GO
```

---

## 9.6 Backup Encryption

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
