# 5. Performance e Tuning

## 5.1 Execution Plan

```sql
SET STATISTICS IO ON;
SET STATISTICS TIME ON;
GO

SELECT
    ID_CLIENTE,
    NM_CLIENTE,
    DT_CADASTRO
FROM adm.TB_CLIENTE
WHERE NM_CLIENTE = 'CLIENTE TESTE';
GO

SET STATISTICS IO OFF;
SET STATISTICS TIME OFF;
GO
```

---

## 5.2 DMVs principais

```sql
SELECT
    DB_NAME(database_id) AS NM_DATABASE,
    file_id,
    num_of_reads,
    num_of_writes,
    io_stall_read_ms,
    io_stall_write_ms
FROM sys.dm_io_virtual_file_stats(NULL, NULL);
GO
```

---

## 5.3 Query Store

```sql
ALTER DATABASE DBA_LAB
SET QUERY_STORE = ON;
GO

ALTER DATABASE DBA_LAB
SET QUERY_STORE
(
    OPERATION_MODE = READ_WRITE
);
GO
```

---

## 5.4 Statistics

```sql
SELECT
    name AS NM_ESTATISTICA,
    auto_created,
    user_created,
    no_recompute
FROM sys.stats
WHERE object_id = OBJECT_ID('adm.TB_CLIENTE');
GO
```

---

## 5.5 Missing Index

```sql
SELECT
    migs.avg_total_user_cost,
    migs.avg_user_impact,
    mid.statement,
    mid.equality_columns,
    mid.inequality_columns,
    mid.included_columns
FROM sys.dm_db_missing_index_group_stats migs
JOIN sys.dm_db_missing_index_groups mig
    ON migs.group_handle = mig.index_group_handle
JOIN sys.dm_db_missing_index_details mid
    ON mig.index_handle = mid.index_handle
ORDER BY migs.avg_user_impact DESC;
GO
```

---

## 5.6 Wait Stats

```sql
SELECT TOP 20
    wait_type,
    waiting_tasks_count,
    wait_time_ms,
    max_wait_time_ms
FROM sys.dm_os_wait_stats
ORDER BY wait_time_ms DESC;
GO
```

---

## 5.7 TempDB análise

```sql
SELECT
    session_id,
    user_objects_alloc_page_count,
    internal_objects_alloc_page_count
FROM sys.dm_db_session_space_usage
ORDER BY user_objects_alloc_page_count DESC;
GO
```
