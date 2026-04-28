# 3. Backup e Restore

## 3.1 BACKUP DATABASE

```sql
BACKUP DATABASE DBA_LAB
TO DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    FORMAT,
    INIT,
    COMPRESSION,
    STATS = 10;
GO
```

---

## 3.2 BACKUP LOG

```sql
BACKUP LOG DBA_LAB
TO DISK = 'C:\SQLBackup\DBA_LAB_LOG.trn'
WITH
    COMPRESSION,
    STATS = 10;
GO
```

---

## 3.3 RESTORE DATABASE

```sql
RESTORE DATABASE DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    REPLACE,
    RECOVERY,
    STATS = 10;
GO
```

---

## 3.4 RESTORE LOG

```sql
RESTORE LOG DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_LOG.trn'
WITH
    RECOVERY,
    STATS = 10;
GO
```

---

## 3.5 VERIFYONLY

```sql
RESTORE VERIFYONLY
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak';
GO
```

---

## 3.6 FILEGROUP Restore

```sql
RESTORE DATABASE DBA_LAB
FILEGROUP = 'FG_2025'
FROM DISK = 'C:\SQLBackup\DBA_LAB_FG_2025.bak'
WITH
    RECOVERY,
    STATS = 10;
GO
```

---

## 3.7 Point-in-Time Restore

```sql
RESTORE DATABASE DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_FULL.bak'
WITH
    NORECOVERY;
GO

RESTORE LOG DBA_LAB
FROM DISK = 'C:\SQLBackup\DBA_LAB_LOG.trn'
WITH
    STOPAT = '2026-04-28 14:30:00',
    RECOVERY;
GO
```
