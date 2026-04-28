# 4. Integridade e Manutenção

## 4.1 DBCC CHECKDB

**O que é:**

Comando utilizado para verificar a integridade lógica e física de todo o banco de dados.

**Quando usar:**

Utilizado em rotinas preventivas de manutenção e após falhas suspeitas de corrupção de dados.

```sql
DBCC CHECKDB ('DBA_LAB')
WITH NO_INFOMSGS, ALL_ERRORMSGS;
GO
```

---

## 4.2 DBCC CHECKTABLE

**O que é:**

Comando responsável por validar a integridade de uma tabela específica e seus índices relacionados.

**Quando usar:**

Utilizado quando há suspeita de problema em uma tabela específica ou para validações direcionadas.

```sql
DBCC CHECKTABLE ('adm.TB_CLIENTE')
WITH NO_INFOMSGS, ALL_ERRORMSGS;
GO
```

---

## 4.3 DBCC CHECKFILEGROUP

**O que é:**

Comando utilizado para verificar a integridade de um filegroup específico dentro do banco de dados.

**Quando usar:**

Utilizado em ambientes com particionamento ou grandes volumes, permitindo manutenção segmentada.

```sql
DBCC CHECKFILEGROUP ('FG_2025')
WITH NO_INFOMSGS, ALL_ERRORMSGS;
GO
```

---

## 4.4 UPDATE STATISTICS

**O que é:**

Comando responsável por atualizar as estatísticas utilizadas pelo otimizador de consultas do SQL Server.

**Quando usar:**

Utilizado após grandes alterações de dados ou quando há degradação de performance em consultas.

```sql
USE DBA_LAB;
GO

UPDATE STATISTICS adm.TB_CLIENTE;
GO
```

---

## 4.5 ALTER INDEX REBUILD

**O que é:**

Comando utilizado para reconstruir completamente um índice, reduzindo fragmentação e reorganizando páginas.

**Quando usar:**

Utilizado quando a fragmentação do índice está alta e impactando desempenho de leitura e escrita.

```sql
ALTER INDEX ALL
ON adm.TB_CLIENTE
REBUILD
WITH (ONLINE = ON);
GO
```

---

## 4.6 ALTER INDEX REORGANIZE

**O que é:**

Comando responsável por reorganizar o índice de forma online e menos invasiva, corrigindo fragmentação moderada.

**Quando usar:**

Utilizado quando a fragmentação é intermediária e não há necessidade de rebuild completo.

```sql
ALTER INDEX ALL
ON adm.TB_CLIENTE
REORGANIZE;
GO
```
