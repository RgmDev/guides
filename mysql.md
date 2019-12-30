# MySQL

> Entrar sin contraseñas a la BBDD
```sh
# Entrar en la maquina y detener el servicio mysql
/etc/init.d/mysqld stop
# Activar modo de entrada sin contraseña
mysqld_safe –skip-grant-tables &amp;
# Cambiar contraseñas
mysql -u root
use mysql;
update user set password=PASSWORD("nueva-contraseña") where user='root';
flush privileges;
quit
# Reiniciar el servicio de la base de datos
```

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

## Eventos y ejecuciones programadas
https://codigofacilito.com/articulos/eventos-mysql
```sql
SHOW VARIABLES;
SHOW GLOBAL VARIABLES;
SHOW SESSION VARIABLES;

SHOW GLOBAL VARIABLES WHERE variable_name = 'event_scheduler'
SET GLOBAL event_scheduler = 'ON' -- En AWS/RDS se modifica en Paremeter groups, editar el que haya o crear uno, buscar dentro del grupo el parametro event_scheduler y cambiar su valor a ON

CREATE DATABASE ourdb;
CREATE TABLE ourdb.ourtable (a TIME);
INSERT INTO ourdb.ourtable VALUES (NOW());
COMMIT; 

USE ourdb;
SELECT * FROM ourtable;
SELECT now() FROM DUAL;

-- Evento que se ejecuta UNA SOLA VEZ (One time)
CREATE EVENT testevent_one_time
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 MINUTE
ON COMPLETION PRESERVE -- Para seguir almacenado una vez ejecutado
DO UPDATE ourdb.ourtable SET a = NOW();

SHOW EVENTS FROM ourdb

-- Evento RECURRENTE (Recurring)
CREATE EVENT testevent_recurring
ON SCHEDULE EVERY 1 MINUTE STARTS '2019-12-20 14:13:00'
DO UPDATE ourdb.ourtable SET a = NOW();

-- Eventos de VARIAS SENTENCIAS
DELIMITER $$
CREATE EVENT testevent
ON SCHEDULE EVERY 1 MINUTE STARTS '2019-12-20 14:13:00'
DO BEGIN
	TRUNCATE TABLE ourdb.ourtable; 
	INSERT INTO ourdb.ourtable VALUES (NOW());
END$$
DELIMITER ;

-- HABILITAR / DESHABILITAR EVENTOS 
ALTER EVENT testevent DISABLE;
ALTER EVENT testevent ENABLE;

DROP EVENT testevent_recurring;
```



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
