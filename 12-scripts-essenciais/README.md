# 12. Scripts Essenciais de DBA

## 12.1 Espaço em disco

```sql
EXEC xp_fixeddrives;
GO
```

---

## 12.2 Crescimento de banco

```sql
SELECT
    name AS DATABASE_NAME,
    size * 8 / 1024 AS SIZE_MB
FROM sys.master_files
WHERE database_id = DB_ID('DBA_LAB');
GO
```

---

## 12.3 Backup sem execução

```sql
SELECT
    name AS DATABASE_NAME,
    recovery_model_desc,
    state_desc
FROM sys.databases
WHERE name NOT IN
(
    SELECT DISTINCT database_name
    FROM msdb.dbo.backupset
    WHERE backup_finish_date >= DATEADD(DAY, -1, GETDATE())
);
GO
```

---

## 12.4 Jobs com falha

```sql
SELECT
    j.name AS JOB_NAME,
    h.run_date,
    h.run_time,
    h.message
FROM msdb.dbo.sysjobhistory h
JOIN msdb.dbo.sysjobs j
    ON h.job_id = j.job_id
WHERE h.run_status = 0;
GO
```

---

## 12.5 Usuários sem uso

```sql
SELECT
    name,
    type_desc,
    create_date
FROM sys.database_principals
WHERE type IN ('S', 'U')
AND name NOT IN ('dbo', 'guest', 'INFORMATION_SCHEMA', 'sys');
GO
```

---

## 12.6 Health Check geral

```sql
SELECT
    @@SERVERNAME AS SERVIDOR,
    @@VERSION AS VERSAO_SQL,
    GETDATE() AS DATA_ATUAL;
GO

SELECT
    name,
    state_desc,
    recovery_model_desc
FROM sys.databases;
GO
```
