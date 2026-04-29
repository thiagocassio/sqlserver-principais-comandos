# 10. Alta Disponibilidade e DR

## 10.1 Log Shipping

**O que é:**

Solução de alta disponibilidade baseada em backups automáticos de transaction log enviados e restaurados em outro servidor.

**Quando usar:**

Utilizado para contingência simples, disaster recovery e ambientes que precisam de servidor secundário com baixo custo operacional.

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

**O que é:**

Recurso que permite copiar e distribuir dados entre servidores SQL Server de forma contínua e controlada.

**Quando usar:**

Utilizado quando diferentes ambientes precisam compartilhar dados, como BI, relatórios ou integração entre sistemas.

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

**O que é:**

Recurso avançado de alta disponibilidade que mantém réplicas sincronizadas entre servidores com failover automático ou manual.

**Quando usar:**

Utilizado em ambientes críticos que exigem alta disponibilidade, baixa indisponibilidade e proteção contra falhas de servidor.

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

**O que é:**

Processo de transferência de operação de um servidor principal para um servidor secundário em caso de falha ou manutenção.

**Quando usar:**

Utilizado em cenários de indisponibilidade planejada ou falhas inesperadas para manter continuidade do serviço.

```sql
-- Executar failover manual

ALTER AVAILABILITY GROUP AG_DBA_LAB
FAILOVER;
GO
```

---

## 10.5 Restore Strategy

**O que é:**

Planejamento estruturado de como backups serão restaurados em situações de falha, perda de dados ou desastre.

**Quando usar:**

Utilizado para garantir recuperação rápida, segura e previsível em incidentes operacionais e contingências reais.

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

**O que é:**

Indicadores que definem quanto dado pode ser perdido (RPO) e quanto tempo o ambiente pode ficar indisponível (RTO).

**Quando usar:**

Utilizado no planejamento de backup, disaster recovery e definição de SLA com áreas de negócio e infraestrutura.

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
