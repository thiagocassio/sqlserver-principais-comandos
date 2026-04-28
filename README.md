# SQL Server DBA - Principais Scripts

## Visão Geral

Este repositório foi criado com o objetivo de centralizar os principais scripts utilizados na rotina de um DBA SQL Server, com foco em administração, segurança, backup, performance, troubleshooting, criptografia, alta disponibilidade e boas práticas operacionais.

A proposta é manter um material organizado, padronizado e reutilizável para estudos, consultas rápidas, documentação técnica e compartilhamento de conhecimento.

Todos os scripts seguem uma taxonomia padronizada de nomes para facilitar a leitura, manutenção e utilização em ambientes de laboratório ou produção controlada.

---

## Objetivos do Projeto

* Consolidar os principais comandos de administração SQL Server
* Criar uma base prática para estudos de DBA
* Disponibilizar scripts prontos para uso e adaptação
* Padronizar nomenclaturas de objetos
* Servir como portfólio técnico para GitHub e LinkedIn
* Apoiar rotinas reais de administração de banco de dados

---

## Estrutura do Projeto

### 1. Criação e Administração de Banco de Dados

* CREATE DATABASE
* ALTER DATABASE
* DROP DATABASE
* Filegroups
* Datafiles / Logfiles
* Recovery Model

### 2. Segurança e Controle de Acesso

* CREATE LOGIN
* CREATE USER
* ALTER ROLE
* GRANT / DENY / REVOKE
* ALTER AUTHORIZATION
* Schemas

### 3. Backup e Restore

* BACKUP DATABASE
* BACKUP LOG
* RESTORE DATABASE
* RESTORE LOG
* VERIFYONLY
* FILEGROUP Restore
* Point-in-Time Restore

### 4. Integridade e Manutenção

* DBCC CHECKDB
* DBCC CHECKTABLE
* DBCC CHECKFILEGROUP
* UPDATE STATISTICS
* ALTER INDEX REBUILD
* ALTER INDEX REORGANIZE

### 5. Performance e Tuning

* Execution Plan
* DMVs principais
* Query Store
* Statistics
* Missing Index
* Wait Stats
* TempDB análise

### 6. Índices e Estruturas

* CREATE INDEX
* DROP INDEX
* ALTER INDEX
* Clustered / Nonclustered
* Included Columns
* Filtered Index
* Partitioning

### 7. Monitoramento e Troubleshooting

* Blocking
* Deadlocks
* Sessions
* Requests
* CPU / Memória / I/O
* Error Log
* SQL Server Logs

### 8. SQL Server Agent e Automação

* Jobs
* Schedules
* Alerts
* Operators
* Maintenance Tasks

### 9. Criptografia e Segurança Avançada

* TDE
* Always Encrypted
* Certificates
* DMK / SMK
* Symmetric Keys
* Backup Encryption

### 10. Alta Disponibilidade e DR

* Log Shipping
* Replication
* Always On
* Failover
* Restore Strategy
* RPO / RTO

### 11. Particionamento e Grandes Volumes

* Partition Function
* Partition Scheme
* Sliding Window
* Filegroup por partição
* Archive Strategy

### 12. Scripts Essenciais de DBA

* Espaço em disco
* Crescimento de banco
* Backup sem execução
* Jobs com falha
* Usuários sem uso
* Health Check geral

---

## Padrão de Nomenclatura

### Banco de Dados

`DBA_LAB`

### Schemas

`adm`, `seg`, `bkp`, `mon`

### Tabelas

`TB_CLIENTE`
`TB_PEDIDO`
`TB_PRODUTO`

### Colunas

`ID_CLIENTE`
`NM_CLIENTE`
`DT_CADASTRO`
`VL_TOTAL`

### Logins / Users

`login_app`
`usr_app`

### Roles

`role_readonly`

### Certificados / Chaves

`CERT_TDE_DBA`
`DMK_DBA`
`SMK_DBA`

### Jobs

`JOB_BACKUP_FULL_DAILY`

### Filegroups

`FG_2024`
`FG_2025`

### Backups

`C:\SQLBackup\`

---

## Observações Importantes

* Sempre valide scripts em ambiente de homologação antes de produção
* Evite execução direta de comandos destrutivos sem validação prévia
* Mantenha backups testados e restore validado
* Documente alterações críticas no ambiente
* Segurança e auditoria devem fazer parte da rotina operacional

---

## Público-Alvo

Este material é útil para:

* DBAs SQL Server
* Administradores de infraestrutura
* Analistas de banco de dados
* Profissionais de segurança da informação
* Estudantes de administração de banco de dados
* Profissionais em preparação para certificações Microsoft

---

## Autor

Projeto desenvolvido com foco em práticas reais de administração SQL Server, organização técnica e construção de portfólio profissional.

---

## Licença

Este projeto pode ser utilizado para fins de estudo, consulta e adaptação profissional.
