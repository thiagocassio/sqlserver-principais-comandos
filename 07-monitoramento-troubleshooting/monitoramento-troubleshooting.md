# 7. Monitoramento e Troubleshooting

## 7.1 Blocking

**O que é:**

Situação em que uma sessão impede outra de acessar ou modificar dados devido a locks ativos no banco de dados.

**Quando usar:**

Utilizado na investigação de lentidão, travamentos e filas de execução causadas por concorrência entre sessões.

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

**O que é:**

Condição em que duas ou mais sessões ficam bloqueadas entre si, aguardando recursos que nunca serão liberados.

**Quando usar:**

Utilizado na análise de falhas de transação e problemas de concorrência que geram abortos automáticos pelo SQL Server.

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

**O que é:**

Informações sobre conexões ativas no SQL Server, incluindo usuários, aplicações, consumo de recursos e status.

**Quando usar:**

Utilizado para monitorar usuários conectados, identificar consumo excessivo e acompanhar atividades em tempo real.

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

**O que é:**

Consultas ou comandos que estão sendo executados no momento dentro das sessões ativas do servidor.

**Quando usar:**

Utilizado para identificar queries lentas, processos em execução e operações que impactam desempenho.

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

**O que é:**

Análise dos principais recursos consumidos pelo SQL Server para identificar gargalos de processamento e armazenamento.

**Quando usar:**

Utilizado quando há lentidão geral, alto consumo de servidor ou necessidade de análise de capacidade do ambiente.

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

**O que é:**

Arquivo de log interno do SQL Server que registra eventos importantes, falhas, inicializações e mensagens críticas.

**Quando usar:**

Utilizado em troubleshooting de erros, falhas de serviço, problemas de login e eventos administrativos relevantes.

```sql
EXEC xp_readerrorlog;
GO
```

---

## 7.7 SQL Server Logs

**O que é:**

Conjunto de registros operacionais que ajudam a acompanhar comportamento do servidor e atividades administrativas.

**Quando usar:**

Utilizado para auditoria técnica, análise de incidentes e validação de eventos ocorridos no ambiente.

```sql
EXEC sp_readerrorlog;
GO
```
