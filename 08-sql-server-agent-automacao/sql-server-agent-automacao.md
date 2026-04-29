# 8. SQL Server Agent e Automação

## 8.1 Jobs

**O que é:**

Rotinas automatizadas executadas pelo SQL Server Agent para tarefas administrativas, operacionais e de manutenção.

**Quando usar:**

Utilizado para automatizar backups, manutenção de índices, execução de procedures e tarefas recorrentes.

```sql
USE msdb;
GO

EXEC sp_add_job
    @job_name = 'JOB_BACKUP_FULL_DAILY';
GO
```

---

## 8.2 Schedules

**O que é:**

Configuração de agendamento que define quando e com qual frequência um job será executado.

**Quando usar:**

Utilizado quando é necessário programar execuções automáticas em horários específicos ou recorrentes.

```sql
EXEC sp_add_schedule
    @schedule_name = 'SCHEDULE_DAILY_2200',
    @freq_type = 4,
    @freq_interval = 1,
    @active_start_time = 220000;
GO
```

---

## 8.3 Alerts

**O que é:**

Mecanismo que monitora eventos e erros do SQL Server e dispara notificações automáticas quando uma condição ocorre.

**Quando usar:**

Utilizado para resposta rápida a falhas críticas, erros de severidade e problemas operacionais importantes.

```sql
EXEC sp_add_alert
    @name = 'ALERT_SEVERITY_016',
    @message_id = 0,
    @severity = 16;
GO
```

---

## 8.4 Operators

**O que é:**

Cadastro de responsáveis que recebem notificações enviadas pelo SQL Server Agent, como e-mails e alertas.

**Quando usar:**

Utilizado quando o ambiente precisa alertar DBAs ou equipes responsáveis sobre incidentes e falhas automáticas.

```sql
EXEC sp_add_operator
    @name = 'DBA_OPERATOR',
    @enabled = 1,
    @email_address = 'dba@empresa.com';
GO
```

---

## 8.5 Maintenance Tasks

**O que é:**

Conjunto de tarefas administrativas voltadas para integridade, desempenho e saúde operacional do ambiente.

**Quando usar:**

Utilizado em rotinas preventivas como backup, atualização de estatísticas, reorganização de índices e verificações de integridade.

```sql
EXEC sp_update_job
    @job_name = 'JOB_BACKUP_FULL_DAILY',
    @enabled = 1;
GO
```
