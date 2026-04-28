# 10. Alta Disponibilidade e DR

## 10.1 Log Shipping

```sql
-- Backup do Transaction Log (Servidor Primário)

BACKUP LOG DBA_LAB
TO DISK = 'C:\SQLBackup\DBA_LAB_LOGSHIP.trn'
WITH
    COMPRESSION,
    STATS = 10;
GO
```

---

## 10.2 Replication

```sql
-- Verificar se o banco está habilitado para publicação

EXEC sp_replicationdboption
    @dbname = 'DBA_LAB',
    @optname = 'publish',
    @value = 'true';
GO
```

---

## 10.3 Always On

```sql
-- Verificar status das réplicas

SELECT
    replica_server_name,
    availability_mode_desc,
    failover_mode_desc,
    secondary_role_allow_connections_desc
FROM sys.availability_replicas;
GO
```

---

## 10.4 Failover

```sql
-- Executar failover manual

ALTER AVAILABILITY GROUP AG_DBA_LAB
FAILOVER;
GO
```

---

## 10.5 Restore Strategy

```sql
-- Restore com NORECOVERY para estratégia de DR

RESTORE DATABASE DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    NORECOVERY,
    REPLACE,
    STATS = 10;
GO
```

---

## 10.6 RPO / RTO

```sql
-- Consulta simples para auditoria de último backup

SELECT
    database_name,
    MAX(backup_finish_date) AS ULTIMO_BACKUP,
    type AS TIPO_BACKUP
FROM msdb.dbo.backupset
WHERE database_name = 'DBA_LAB'
GROUP BY
    database_name,
    type;
```
