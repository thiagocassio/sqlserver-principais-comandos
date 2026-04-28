# 11. Particionamento e Grandes Volumes

## 11.1 Partition Function

```sql
CREATE PARTITION FUNCTION PF_VENDA_ANO (DATE)
AS RANGE RIGHT
FOR VALUES
(
    '2024-01-01',
    '2025-01-01',
    '2026-01-01'
);
GO
```

---

## 11.2 Partition Scheme

```sql
CREATE PARTITION SCHEME PS_VENDA_ANO
AS PARTITION PF_VENDA_ANO
TO
(
    FG_2024,
    FG_2025,
    PRIMARY,
    PRIMARY
);
GO
```

---

## 11.3 Sliding Window

```sql
ALTER PARTITION FUNCTION PF_VENDA_ANO()
SPLIT RANGE ('2027-01-01');
GO
```

---

## 11.4 Filegroup por partição

```sql
ALTER DATABASE DBA_LAB
ADD FILEGROUP FG_2026;
GO

ALTER DATABASE DBA_LAB
ADD FILE
(
    NAME = DBA_LAB_DATA_2026,
    FILENAME = 'C:\SQLData\DBA_LAB_DATA_2026.ndf',
    SIZE = 500MB,
    FILEGROWTH = 100MB
)
TO FILEGROUP FG_2026;
GO
```

---

## 11.5 Archive Strategy

```sql
-- Exemplo simples de arquivamento

INSERT INTO adm.TB_CLIENTE_HISTORICO
SELECT *
FROM adm.TB_CLIENTE
WHERE DT_CADASTRO < '2024-01-01';
GO
```
