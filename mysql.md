# MySQL

## Usuarios y permisos

```sql
-- Ver Usuarios 
SELECT * FROM mysql.user

-- Crear usuario
CREATE USER 'nuevousuario'@'localhost' IDENTIFIED BY 'contraseña';

-- Eliminar usuario 
DROP USER 'usuario'@'localhost';

-- Después de conceder/revocar permisos recargar para que surjan efecto
FLUSH PRIVILEGES;

-- Ver los permisos de un usuario 
SHOW GRANTS FOR 'usuario'@'%'

-- Conceder permisos
GRANT ALL PRIVILEGES ON *.* TO 'nuevousuario'@'localhost';
GRANT [tipo de permiso] ON [nombre de la base de datos].[nombre de la tabla] TO '[nombredeusuario]'@'localhost';

-- Revocar permisos
REVOKE [tipo de permiso] ON [nombre de la base de datos].[nombre de la tabla] FROM '[nombredeusuario]'@'localhost';

/*
    [tipo de permiso]
    ALL PRIVILEGES
    CREATE
    DROP
    DELETE
    INSERT
    SELECT
    UPDATE
    GRANT OPTION
*/
```

## Bases de datos y tablas

## Consulta 
```sql
-- COUNT() Condicional
SELECT 
    COUNT( IF(status = 'Cancelled', 1, NULL) ) 'Cancelled',
    COUNT( IF(status = 'On Hold', 1, NULL) ) 'On Hold',
    COUNT( IF(status = 'Disputed', 1, NULL) ) 'Disputed'
FROM
    orders;
```
