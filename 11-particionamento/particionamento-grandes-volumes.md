# 11. Particionamento e Grandes Volumes

## 11.1 Partition Function

**O que é:**

Objeto responsável por definir as regras de divisão dos dados em partições, normalmente com base em datas ou faixas de valores.

**Quando usar:**

Utilizado em tabelas grandes que precisam ser segmentadas para facilitar manutenção, consultas e estratégias de backup.

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

**O que é:**

Estrutura que associa a Partition Function aos filegroups físicos onde cada partição será armazenada.

**Quando usar:**

Utilizado quando é necessário distribuir dados entre diferentes discos ou filegroups para melhor gerenciamento e desempenho.

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

**O que é:**

Técnica utilizada para adicionar novas partições e remover antigas de forma controlada e eficiente.

**Quando usar:**

Utilizado em ambientes com grande volume histórico, como tabelas de vendas, logs e movimentações contínuas.

```sql
ALTER PARTITION FUNCTION PF_VENDA_ANO()
SPLIT RANGE ('2027-01-01');
GO
```

---

## 11.4 Filegroup por partição

**O que é:**

Estratégia que separa partições em filegroups distintos para melhor administração física dos dados.

**Quando usar:**

Utilizado quando há necessidade de backup segmentado, arquivamento ou melhor distribuição de armazenamento.

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

**O que é:**

Processo de mover dados antigos para estruturas de histórico ou armazenamento secundário, reduzindo carga operacional.

**Quando usar:**

Utilizado quando tabelas crescem excessivamente e dados antigos precisam ser preservados sem impactar performance do ambiente principal.

```sql
-- Exemplo simples de arquivamento

INSERT INTO adm.TB_CLIENTE_HISTORICO
SELECT *
FROM adm.TB_CLIENTE
WHERE DT_CADASTRO < '2024-01-01';
GO
```
