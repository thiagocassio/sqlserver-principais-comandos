# 12. Scripts Essenciais de DBA

## 12.1 Espaço em disco

**O que é:**

Consulta utilizada para verificar o espaço livre disponível nos discos do servidor onde o SQL Server está operando.

**Quando usar:**

Utilizado em monitoramento preventivo para evitar indisponibilidade causada por falta de espaço em disco.

```sql
EXEC xp_fixeddrives;
GO
```

---

## 12.2 Crescimento de banco

**O que é:**

Análise do tamanho atual e crescimento dos arquivos de dados e log dos bancos de dados.

**Quando usar:**

Utilizado para planejamento de capacidade, controle de expansão e prevenção de problemas de storage.

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

**O que é:**

Verificação que identifica bancos de dados que não receberam backup dentro do período esperado.

**Quando usar:**

Utilizado em auditorias operacionais e validação de rotinas de proteção e disaster recovery.

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

**O que é:**

Consulta utilizada para localizar jobs do SQL Server Agent que falharam em execuções recentes.

**Quando usar:**

Utilizado no acompanhamento diário de rotinas automatizadas e resposta rápida a falhas operacionais.

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

**O que é:**

Análise que ajuda a identificar usuários e acessos que não possuem mais necessidade operacional no ambiente.

**Quando usar:**

Utilizado em revisões de segurança, auditoria de acessos e aplicação do princípio do menor privilégio.

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

**O que é:**

Conjunto de verificações básicas para avaliar rapidamente a saúde operacional do SQL Server e dos bancos hospedados.

**Quando usar:**

Utilizado em rotinas preventivas, diagnósticos iniciais e validação geral da estabilidade do ambiente.

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
