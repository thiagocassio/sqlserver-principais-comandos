# 5. Performance e Tuning

## 5.1 Execution Plan

**O que é:**

Recurso utilizado para mostrar como o SQL Server executa uma consulta, exibindo operadores, custos e possíveis gargalos.

**Quando usar:**

Utilizado na análise de consultas lentas, identificação de scans desnecessários e oportunidades de otimização.

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

**O que é:**

Dynamic Management Views são visões internas que mostram informações de desempenho, uso de recursos e comportamento do servidor.

**Quando usar:**

Utilizado para monitoramento contínuo, troubleshooting e análise de consumo de CPU, memória, I/O e sessões.

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

**O que é:**

Recurso que armazena histórico de execução de consultas, planos e desempenho ao longo do tempo.

**Quando usar:**

Utilizado para investigar degradação de performance, regressão de planos e análise histórica de queries.

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

**O que é:**

Informações utilizadas pelo otimizador de consultas para estimar volume de dados e escolher o melhor plano de execução.

**Quando usar:**

Utilizado na análise de performance quando há planos ruins causados por estimativas incorretas.

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

**O que é:**

Recurso que indica possíveis índices que podem melhorar o desempenho de consultas executadas no ambiente.

**Quando usar:**

Utilizado durante tuning de performance para identificar oportunidades de criação de novos índices.

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

**O que é:**

Métrica que mostra onde o SQL Server está aguardando recursos, ajudando a identificar gargalos de desempenho.

**Quando usar:**

Utilizado em investigações de lentidão geral, travamentos e análise de comportamento do servidor.

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

**O que é:**

Avaliação do uso do banco TempDB, responsável por objetos temporários, ordenações e operações internas do SQL Server.

**Quando usar:**

Utilizado quando há lentidão, contenção ou alto consumo de disco relacionado ao TempDB.

```sql
SELECT
    session_id,
    user_objects_alloc_page_count,
    internal_objects_alloc_page_count
FROM sys.dm_db_session_space_usage
ORDER BY user_objects_alloc_page_count DESC;
GO
```
