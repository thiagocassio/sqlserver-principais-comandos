# 6. Índices e Estruturas

## 6.1 CREATE INDEX

**O que é:**

Comando utilizado para criar índices em tabelas e melhorar a velocidade de consultas e pesquisas de dados.

**Quando usar:**

Utilizado quando consultas frequentes precisam de melhor desempenho em filtros, joins e ordenações.

```sql
CREATE NONCLUSTERED INDEX IX_TB_CLIENTE_NM_CLIENTE
ON adm.TB_CLIENTE (NM_CLIENTE);
GO
```

---

## 6.2 DROP INDEX

**O que é:**

Comando responsável por remover um índice existente de uma tabela.

**Quando usar:**

Utilizado quando um índice não traz benefício, gera sobrecarga ou precisa ser substituído por outro mais eficiente.

```sql
DROP INDEX IX_TB_CLIENTE_NM_CLIENTE
ON adm.TB_CLIENTE;
GO
```

---

## 6.3 ALTER INDEX

**O que é:**

Comando utilizado para gerenciar índices existentes, permitindo rebuild, reorganize, disable e outras ações administrativas.

**Quando usar:**

Utilizado em rotinas de manutenção e ajustes de performance relacionados à estrutura dos índices.

```sql
ALTER INDEX IX_TB_CLIENTE_NM_CLIENTE
ON adm.TB_CLIENTE
DISABLE;
GO
```

---

## 6.4 Clustered / Nonclustered

**O que é:**

Tipos de índices utilizados para organizar fisicamente os dados (clustered) ou criar estruturas auxiliares de busca (nonclustered).

**Quando usar:**

Utilizado no desenho de performance de tabelas conforme volume, padrão de acesso e necessidade de consultas rápidas.

```sql
CREATE CLUSTERED INDEX IX_TB_CLIENTE_ID_CLIENTE
ON adm.TB_CLIENTE (ID_CLIENTE);
GO
```

---

## 6.5 Included Columns

**O que é:**

Recurso que permite adicionar colunas extras em índices nonclustered sem que façam parte da chave principal.

**Quando usar:**

Utilizado para evitar key lookups e melhorar performance de consultas específicas.

```sql
CREATE NONCLUSTERED INDEX IX_TB_CLIENTE_BUSCA
ON adm.TB_CLIENTE (NM_CLIENTE)
INCLUDE (DT_CADASTRO);
GO
```

---

## 6.6 Filtered Index

**O que é:**

Índice criado apenas sobre um subconjunto de dados que atende uma condição específica.

**Quando usar:**

Utilizado quando consultas acessam frequentemente apenas parte dos dados da tabela, reduzindo custo e espaço.

```sql
CREATE NONCLUSTERED INDEX IX_TB_CLIENTE_ATIVO
ON adm.TB_CLIENTE (NM_CLIENTE)
WHERE NM_CLIENTE IS NOT NULL;
GO
```

---

## 6.7 Partitioning

**O que é:**

Técnica que divide grandes tabelas em partições menores com base em uma regra, como data ou faixa de valores.

**Quando usar:**

Utilizado em tabelas com grande volume de dados para melhorar manutenção, performance e estratégias de backup.

```sql
CREATE PARTITION FUNCTION PF_ANO_PEDIDO (DATE)
AS RANGE RIGHT
FOR VALUES
(
    '2024-01-01',
    '2025-01-01',
    '2026-01-01'
);
GO

CREATE PARTITION SCHEME PS_ANO_PEDIDO
AS PARTITION PF_ANO_PEDIDO
TO
(
    [PRIMARY],
    [FG_2024],
    [FG_2025],
    [PRIMARY]
);
GO
```
