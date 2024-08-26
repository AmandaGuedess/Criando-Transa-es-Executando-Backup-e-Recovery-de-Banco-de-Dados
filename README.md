# PARTE 1 – TRANSAÇÕES

## Objetivo: Trabalhar com transações no MySQL, desabilitando o autocommit e executando várias instruções SQL dentro de uma transação.

### Desabilitar o Autocommit:

```sql

SET autocommit = 0;
```

### Iniciar a Transação:

```sql

START TRANSACTION;
```

### Executar as Queries de Modificação:

```sql
UPDATE tabela SET coluna = valor WHERE condição;
DELETE FROM tabela WHERE condição;
INSERT INTO tabela (colunas) VALUES (valores);
```

### Confirmar ou Desfazer a Transação:

Para confirmar as mudanças:

```sql
COMMIT;
```
Para desfazer as mudanças, se algo der errado:
```sql
ROLLBACK;
```

# PARTE 2 – TRANSAÇÃO COM PROCEDURE
## Objetivo: Criar uma transação dentro de uma procedure, incluindo verificação de erro e uso de ROLLBACK ou SAVEPOINT.

### Criar a Procedure:

```sql
DELIMITER //

CREATE PROCEDURE minha_procedure()
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        -- Aqui você pode adicionar outras ações, como registrar o erro
    END;

    START TRANSACTION;

    SAVEPOINT antes_da_modificacao;

    -- Suas instruções SQL aqui
    UPDATE tabela SET coluna = valor WHERE condição;

    -- Se houver necessidade de desfazer até o SAVEPOINT:
    -- ROLLBACK TO antes_da_modificacao;

    COMMIT;
END //

DELIMITER ;
```

### Chamar a Procedure:

```sql
CALL minha_procedure();
```

# PARTE 3 – BACKUP E RECOVERY

## Objetivo: Executar o backup e recuperação de um banco de dados MySQL, incluindo procedures, eventos, e outros recursos.

### Realizar o Backup: Use o comando mysqldump para criar um backup:

```sql
mysqldump -u usuario -p --databases e-commerce > backup.sql
```
### Para incluir procedures e eventos, você pode adicionar flags:
```sql
mysqldump -u usuario -p --routines --events --databases e-commerce > backup_completo.sql
```
### Recuperar o Backup: Use o mysql para restaurar o backup:
```sql
mysql -u usuario -p < backup.sql
```
