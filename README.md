# SQL-My-Cheat-Sheet

This SQL cheat sheet comprises the required commands only. You must go through the contents to learn and understand details. Click the heading of the correspoding section to see details.

## [SQL Database]()

```sql
CREATE DATABASE db_name;
CREATE DATABASE IF NOT EXISTS db_name;
```

```sql
SHOW DATABASES;
SHOW DATABASES LIKE '%test%';
```

```sql    
USE db_name;
```

```sql
DROP DATABASE db_name;
DROP DATABASE IF EXISTS db_name;
```

### Backup and Restore (Linux)

```console
mysqldump -u root -p db_name > db_name_backup.sql
mysql -u root -p db_name < db_name_backup.sql
```

```console
mysqldump -u root -p --all-databases > all_databases_backup.sql
mysql -u root -p < all_databases_backup.sql
```
### Backup and Restore (Windows) 

```console
mysqldump -u root -p db_name mytable > C:\backups\db_name_backup.sql
mysql -u root -p db_name < C:\backups\db_name_backup.sql
```

```console
mysqldump -u root -p --all-databases > C:\backups\all_databases_backup.sql
mysql -u root -p < C:\backups\all_databases_backup.sql
```

## [SQL Table]()

```sql
CREATE TABLE tab_name (
    col1 INT NOT NULL,
    col2 VARCHAR (255) NOT NULL,
    col3 VARCHAR (10) NOT NULL,
    col4 DECIMAL (3, 2) NOT NULL,
    PRIMARY KEY (col1)
);

CREATE TABLE IF NOT EXISTS tab_name (
    col1 INT NOT NULL,
    col2 VARCHAR (255) NOT NULL,
    col3 VARCHAR (10) NOT NULL,
    col4 DECIMAL (3, 2) NOT NULL,
    PRIMARY KEY (col1)
);
```

```sql
DESC tab_name;
```

```sql
DROP TABLE tab_name;
DROP TABLE IF NOT EXISTS tab_name;
```

```sql
SHOW TABLES;
```

```sql
RENAME TABLE old_name TO new_name;
ALTER TABLE old_name RENAME TO new_name;
```

```sql
TRUNCATE TABLE tab_name;
```

## [SQL Table Constraints]()

```sql
CREATE TABLE student_info (
    id        INT NOT NULL,
    name      VARCHAR (255) NOT NULL DEFAULT 'Not Available',
    email     VARCHAR (255) NOT NULL UNIQUE,
    cgpa      DECIMAL (3,2) NOT NULL,
    age       INT NOT NULL CHECK (age >= 18),
    hall_id   INT NOT NULL,

    PRIMARY KEY (id),
    FOREIGN KEY (hall_id) REFERENCES hall_info (id)
);
```

## [CREATE TABLE ... AS SELECT]()

```sql
CREATE TABLE new_table AS
SELECT column1, column2, ..., columnN
FROM existing_table;
```

## [ALTER TABLE]()

```sql
ALTER TABLE tab_name
ADD col_name Data_Type [constraints];
```

```sql
ALTER TABLE tab_name
DROP COLUMN col_name;
```

```sql
ALTER TABLE tab_name
RENAME COLUMN old_name TO new_name;
```

```sql
ALTER TABLE tab_name
MODIFY COLUMN col_name New_data_type;
```

```sql
ALTER TABLE tab_name
ADD CONSTRAINT pk_name
PRIMARY KEY (col1, col2, ...);
```

```sql
ALTER TABLE tab_name
DROP PRIMARY KEY;
```

```sql
ALTER TABLE tab_name
ADD CONSTRAINT uk_name
UNIQUE (col_name);
```    

```sql
ALTER TABLE tab_name 
DROP CONSTRAINT uk_name;
```

## [DELETE]()

```sql
DELETE FROM tab_name;
```

```sql
DELETE FROM tab_name WHERE [condition];
```

## [INSERT INTO]()

```sql
INSERT INTO tab_name (col1, col2, ..., colN) VALUES (val1, val2, ..., valN);
```

```sql
INSERT INTO tab_name VALUES (val1, val2, ..., valN);
```

```sql
INSERT INTO tab_name VALUES (val1, val2, ..., valN), (val1, val2, ..., valN), ..., (val1, val2, ..., valN);
```

## [INSERT INTO ... SELECT]()

```sql
INSERT INTO first_tab_name [column1, column2, ..., columnN]
SELECT column1, column2, ..., columnN
FROM second_tab_name
WHERE [condition(s)];
```

```sql
INSERT INTO first_table
SELECT *
FROM second_table
WHERE [condition(s)]
```

## [INSERT INTO ... TABLE]()

```sql
INSERT INTO first_table_name
TABLE second_table_name;
```

## [SELECT]()

```sql
SELECT * FROM tab_name;
```

```sql
SELECT column1, column2, ..., columnN FROM tab_name;
```

```sql
SELECT 98*2-5;
```

```sql
SELECT column1 AS aliasName, column2, ..., columnN
FROM table_name;
```

## [SELECT ... INTO]()

```sql
SELECT *
INTO new_tab_name
FROM existing_tab_name;
```

```sql
SELECT column1, column2, ..., columnN
INTO new_tab_name
FROM existing_tab_name;
```

```sql
SELECT column1, column2, ..., columnN
INTO new_tab_name
FROM existing_tab_name;
WHERE [condition(s)]
```

```sql
SELECT customer.name, order.id
INTO new_table
FROM customer LEFT JOIN order ON customer.id = order.customer_id;
```

## [UPDATE]()

```sql
UPDATE table_name
SET column1 = value1, column2 = value2,..., columnN = valueN
WHERE [condition(s)];
```

```sql
UPDATE table_name
SET age = age + 10, salary = salary + 10000;
```

## [DELETE FROM]()

```sql
DELETE FROM tab_name
WHERE [condition(s)];
```
```sql
DELETE FROM tab_name;
```
```sql
DELETE customer, order
FROM customer INNER JOIN order ON order.customer_id = customer.id
WHERE customer.salary > 2000;
```

## [ORDER BY ... DESC]()

```sql
SELECT * FROM tab_name ORDER BY salary DESC;
```




