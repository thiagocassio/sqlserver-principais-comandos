# 8. SQL Server Agent e Automação

## 8.1 Jobs

```sql
USE msdb;
GO

EXEC sp_add_job
    @job_name = 'JOB_BACKUP_FULL_DAILY';
GO
```

---

## 8.2 Schedules

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

```sql
EXEC sp_add_alert
    @name = 'ALERT_SEVERITY_016',
    @message_id = 0,
    @severity = 16;
GO
```

---

## 8.4 Operators

```sql
EXEC sp_add_operator
    @name = 'DBA_OPERATOR',
    @enabled = 1,
    @email_address = 'dba@empresa.com';
GO
```

---

## 8.5 Maintenance Tasks

```sql
EXEC sp_update_job
    @job_name = 'JOB_BACKUP_FULL_DAILY',
    @enabled = 1;
GO
```
