# 3. Backup e Restore

## 3.1 BACKUP DATABASE

**O que é:**

Comando utilizado para realizar o backup completo ou parcial de um banco de dados, garantindo a proteção das informações.

**Quando usar:**

Utilizado em rotinas de proteção de dados, planos de contingência e estratégias de recuperação de desastres.

```sql
BACKUP DATABASE DBA_LAB
TO DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    FORMAT,
    INIT,
    COMPRESSION,
    STATS = 10;
GO
```

---

## 3.2 BACKUP LOG

**O que é:**

Comando responsável pelo backup do transaction log, permitindo recuperação mais detalhada entre backups completos.

**Quando usar:**

Utilizado em bancos com recovery model FULL ou BULK_LOGGED, quando há necessidade de recuperação ponto a ponto.

```sql
BACKUP LOG DBA_LAB
TO DISK = 'C:\SQLBackup\DBA_LAB_LOG.trn'
WITH
    COMPRESSION,
    STATS = 10;
GO
```

---

## 3.3 RESTORE DATABASE

**O que é:**

Comando utilizado para restaurar um backup de banco de dados completo ou diferencial.

**Quando usar:**

Utilizado em cenários de falha, migração, testes de restore ou recuperação de ambientes.

```sql
USE MASTER

RESTORE DATABASE DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    REPLACE,
    RECOVERY,
    STATS = 10;
GO
```

---

## 3.4 RESTORE LOG

**O que é:**

Comando responsável por restaurar backups de transaction log após um restore full ou diferencial.

**Quando usar:**

Utilizado quando é necessário recuperar transações específicas até um ponto mais recente possível.

```sql
USE MASTER

RESTORE LOG DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_LOG.trn'
WITH
    RECOVERY,
    STATS = 10;
GO
```

---

## 3.5 VERIFYONLY

**O que é:**

Comando utilizado para validar a integridade de um arquivo de backup sem realizar sua restauração.

**Quando usar:**

Utilizado após a geração de backups para confirmar que o arquivo está íntegro e utilizável em caso de restore.

```sql
RESTORE VERIFYONLY
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak';
GO
```

---

## 3.6 FILEGROUP Restore

**O que é:**

Comando utilizado para restaurar apenas um filegroup específico de um banco de dados.

**Quando usar:**

Utilizado em ambientes com grandes volumes de dados e particionamento, reduzindo tempo de recuperação.

```sql
RESTORE DATABASE DBA_LAB
FILEGROUP = 'FG_2025'
FROM DISK = 'C:\SQLBackup\DBA_LAB_FG_2025.bak'
WITH
    RECOVERY,
    STATS = 10;
GO
```

---

## 3.7 Point-in-Time Restore

**O que é:**

Técnica de restore que permite recuperar o banco de dados até um momento exato anterior a uma falha.

**Quando usar:**

Utilizado em casos de exclusão acidental, erro operacional ou corrupção ocorrida em horário conhecido.

```sql
RESTORE DATABASE DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    NORECOVERY;
GO

RESTORE LOG DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_LOG.trn'
WITH
    STOPAT = '2026-04-28 14:30:00',
    RECOVERY;
GO
```
