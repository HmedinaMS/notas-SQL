# Notas SQL
- _DBeaver, SQL Server Management Studio (SSMS), MySQL Workbench se llaman software de administración de bases de datos_
- _Las colecciones de bases de datos se llaman "Conexiones" o "Conexiones de base de datos". Estas conexiones representan los accesos a las bases de datos y servidores específicos_
- MySQL es un servidor que se ejecuta en un host. Por este motivo, primero conectate por ssh al servidor remoto, y luego ejecutas con tus credenciales (se pide usuario y password):
```bash
mysql -u my_username -p
```

## Tipos de sentencias
![Alt text](./assets/img/image.png)

## Sentencias SQL
SQL es un lenguaje de programación imperativo.

### Sentencias DDL
- Ver lista de bases de datos
```sql
SHOW DATABASES;
```
- Crear base de datos
```sql
CREATE DATABASE my_database;
CREATE DATABASE IF NOT EXISTS my_database;
```
- Borrar base de datos (dejando referencias en memoria)
```sql
DROP DATABASE my_database;
DROP DATABASE IF EXISTS my_database;
```
- Crear tabla
```sql
CREATE TABLE IF NOT EXISTS `my_database`.my_table(
    id INT UNSIGNED AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(70) NOT NULL,
    PRIMARY KEY (id)
);
-- También se puede usar la sintaxis
CREATE TABLE IF NOT EXISTS `my_database`.my_table(
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(70) UNIQUE NOT NULL,
    age INT DEFAULT 0,
);
```
- Agregar campos en la tabla
```sql
ALTER TABLE `my_database`.my_table ADD COLUMN phone VARCHAR(12) NOT NULL;
```
- Modificar tipo de dato en campos en la tabla
```sql
ALTER TABLE `my_database`.my_table MODIFY phone INT UNSIGNED AUTO_INCREMENT NOT NULL;
```
- Renombrar campos en la tabla
```sql
ALTER TABLE `my_database`.my_table RENAME COLUMN phone TO telefono;
```
- Eliminar campos en la tabla
```sql
ALTER TABLE `my_database`.my_table DROP COLUMN telefono;
```
- Eliminar tabla
```sql
DROP TABLE `my_database`.my_table;
```
- Especificar la base de datos que usaremos
```sql
-- Especificar la DB que usaremos
USE my_database;
-- Esta sentencia depende de la anterior
SHOW TABLES;
-- Mostrar caracteristicas de una tabla
DESCRIBE my_table;
```

### Sentencias DML
- Ver campos de registros
```sql
SELECT name, email, age FROM `my_database`;
SELECT name, email, age FROM `my_database` WHERE email = 'john@example.com' AND age = 18;
-- Sentencia WHERE con multiples valores
SELECT name, email, age FROM `my_database` WHERE email IN ('john@example.com', 'john@outlook.com');
SELECT name, email, age FROM `my_database` WHERE email = 'john@example.com'
-- Mostrar registros usando comodines
-- Empiezan con 'M' y luego cualesquiera caracteres
SELECT email, age FROM `my_database` WHERE email LIKE 'M%';
SELECT email, age FROM `my_database` WHERE email LIKE '%@gmail.com';
SELECT email, age FROM `my_database` WHERE email NOT LIKE '%@hotmail.com';
-- Mostrar cantidad de registros
SELECT COUNT(*) FROM `my_database`;
SELECT COUNT(*) AS cantidad FROM `my_database`;
```
- Crear registro en una tabla
```sql
-- Se puede usar esta sintaxis
INSERT INTO `my_database`.my_table SET name = "John", email = "jones@gmail.com", edad = 18;
-- Insertar un registro
INSERT INTO `my_database`.my_table (name, email, age) VALUES ("John", "jones@gmail.com", 18);
-- Insertar varios registros
INSERT INTO `my_database`.my_table (name, email, age) VALUES 
("John", "jones@gmail.com", 18),
("Mircha", "mircha@gmail.com", 38),
("Hararec", "hararecmedina@gmail.com", 29),
("Nat", "natmartinez@gmail.com", 23);
```

### Sentencias DCL
Los usuarios tienen la notación `username@host` (donde `host` puede ser un dominio o ip)

- Crear usuario
```sql
CREATE USER 'my_username'@'localhost' IDENTIFIED BY 'my_password';
```

- Dar todos los privilegios sobre una base de datos a un determinado usuario en un host
```sql
GRANT ALL PRIVILEGES ON my_database TO 'my_username'@'localhost';
```

- Actualizar lista de privilegios para los usuarios
```sql
FLUSH PRIVILEGES;
```
- Ver los privilegios que tiene un usuario
```sql
SHOW GRANTS FOR 'my_username'@'localhost';
```
- Quitar los privilegios a un usuario
```sql
REVOKE ALL, GRANT OPTION FROM 'my_username'@'localhost';
```
- Eliminar usuario
```sql
DROP USER 'my_username'@'localhost';
```
