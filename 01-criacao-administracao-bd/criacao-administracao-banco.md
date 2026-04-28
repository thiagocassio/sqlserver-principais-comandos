# 1. Criação e Administração de Banco de Dados

## 1.1 CREATE DATABASE

### O que é: 
Comando utilizado para criar um novo banco de dados no SQL Server, definindo arquivos de dados, log e configurações iniciais.

**Quando usar:**
Utilizado na criação de novos ambientes, implantação de sistemas, homologação, testes e novos projetos em produção.

```sql
CREATE DATABASE DBA_LAB
ON
PRIMARY
(
    NAME = DBA_LAB_DATA,
    FILENAME = 'C:\SQLData\DBA_LAB_DATA.mdf',
    SIZE = 500MB,
    MAXSIZE = 5GB,
    FILEGROWTH = 100MB
)
LOG ON
(
    NAME = DBA_LAB_LOG,
    FILENAME = 'C:\SQLLog\DBA_LAB_LOG.ldf',
    SIZE = 200MB,
    MAXSIZE = 2GB,
    FILEGROWTH = 50MB
);
GO
```

---

## 1.2 ALTER DATABASE

### O que é:
Comando responsável por alterar configurações de um banco de dados já existente, como recovery model, opções de acesso e comportamento operacional.

### Quando usar:
Utilizado quando é necessário ajustar parâmetros do banco após sua criação ou adequar o ambiente a novas necessidades operacionais.

```sql
ALTER DATABASE DBA_LAB
SET RECOVERY FULL;
GO

ALTER DATABASE DBA_LAB
SET AUTO_CLOSE OFF;
GO
```

---

## 1.3 DROP DATABASE

### O que é:
Comando utilizado para remover permanentemente um banco de dados do servidor SQL Server.

### Quando usar:
Utilizado em ambientes de testes, homologação ou quando um banco precisa ser descontinuado de forma controlada.

```sql
ALTER DATABASE DBA_LAB
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO

DROP DATABASE DBA_LAB;
GO
```

---

## 1.4 Filegroups

### O que é:
Estrutura lógica usada para organizar arquivos físicos de dados dentro de um banco de dados.

### Quando usar:
Utilizado para distribuir grandes volumes de dados, melhorar administração de storage e facilitar estratégias de backup e restore.

```sql
ALTER DATABASE DBA_LAB
ADD FILEGROUP FG_2025;
GO
```

---

## 1.5 Datafiles / Logfiles

### O que é:
Arquivos físicos responsáveis pelo armazenamento dos dados (.mdf/.ndf) e do log de transações (.ldf) do banco.

### Quando usar:
Utilizado na criação e expansão de bancos, separação de cargas e melhor gerenciamento de desempenho e armazenamento.

```sql
ALTER DATABASE DBA_LAB
ADD FILE
(
    NAME = DBA_LAB_DATA_2025,
    FILENAME = 'C:\SQLData\DBA_LAB_DATA_2025.ndf',
    SIZE = 500MB,
    FILEGROWTH = 100MB
)
TO FILEGROUP FG_2025;
GO
```

---

## 1.6 Recovery Model

### O que é:
Configuração que define como o SQL Server gerencia o log de transações e a capacidade de recuperação de dados.

### Quando usar:
Utilizado para definir estratégias de backup e restore conforme a necessidade de recuperação do ambiente, como FULL, SIMPLE ou BULK_LOGGED.

```sql
ALTER DATABASE DBA_LAB
SET RECOVERY SIMPLE;
GO

ALTER DATABASE DBA_LAB
SET RECOVERY FULL;
GO
```
