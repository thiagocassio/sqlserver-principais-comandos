# 7. Monitoramento e Troubleshooting

## 7.1 Blocking

```sql
SELECT
    session_id,
    blocking_session_id,
    wait_type,
    wait_time,
    status,
    command
FROM sys.dm_exec_requests
WHERE blocking_session_id <> 0;
GO
```

---

## 7.2 Deadlocks

```sql
SELECT
    XEventData.XEvent.value('(event/@name)[1]', 'VARCHAR(100)') AS EVENTO,
    XEventData.XEvent.value('(event/data/value)[1]', 'VARCHAR(MAX)') AS DEADLOCK_XML
FROM
(
    SELECT CAST(target_data AS XML) AS TargetData
    FROM sys.dm_xe_session_targets st
    JOIN sys.dm_xe_sessions s
        ON s.address = st.event_session_address
    WHERE s.name = 'system_health'
) AS Data
CROSS APPLY TargetData.nodes('//RingBufferTarget/event[@name="xml_deadlock_report"]') AS XEventData(XEvent);
GO
```

---

## 7.3 Sessions

```sql
SELECT
    session_id,
    login_name,
    host_name,
    program_name,
    status,
    cpu_time,
    memory_usage
FROM sys.dm_exec_sessions
WHERE is_user_process = 1;
GO
```

---

## 7.4 Requests

```sql
SELECT
    session_id,
    status,
    command,
    cpu_time,
    total_elapsed_time,
    reads,
    writes
FROM sys.dm_exec_requests
WHERE session_id > 50;
GO
```

---

## 7.5 CPU / Memória / I/O

```sql
SELECT TOP 20
    qs.total_worker_time / qs.execution_count AS MEDIA_CPU,
    qs.total_logical_reads / qs.execution_count AS MEDIA_LEITURA,
    qs.execution_count,
    st.text
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
ORDER BY MEDIA_CPU DESC;
GO
```

---

## 7.6 Error Log

```sql
EXEC xp_readerrorlog;
GO
```

---

## 7.7 SQL Server Logs

```sql
EXEC sp_readerrorlog;
GO
```
