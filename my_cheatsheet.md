## SQL DATABASE
```sql
CREATE DATABASE dbname;
CREATE DATABASE IF NOT EXISTS dbname;
```
```sql
SHOW DATABASES;
SHOW DATABASES LIKE '%test%'; --`%` represents one or more characters
SHOW DATABASES LIKE '_test%'; --`_` represents one or more characters
```
```sql
USE dbname;
```
```sql
DROP DATABASE dbname;
DROP DATABAES IF EXISTS dbname;
```

## Backup and Restore Databases
```bash
mysqldump -u root -p dbname > dbname_backup.sql 
mysql -u root -p dbname < dbname_backup.sql
```
```bash
mysqldump -u root -p --all-databases > all_db_backup.sql 
mysql -u root -p < all_db_backup.sql 
```

## Rename Database
```bash
mysqldump -u root -p dbname > dbanme_backup.sql;    # Dump old database
```
```sql
DROP DATABASE dbname;   -- Drop old database
```
```sql
CREATE DATABASE new_dbanme;     -- Create new database
```
```bash
mysql -u root -p new_dbanme < dbname_backup.sql     # Restore from old database
```

## SQL Table
```sql
CREATE TABLE hall_info (
	id INT NOT NULL,
	name VARCHAR (255) NOT NULL UNIQUE,
	seat INT NOT NULL DEFAULT 0
	
	PRIMARY KEY (id)
);

CREATE TABLE IF NOT EXISTS student (
	id INT NOT NULL,
	name VARCHAR (255) NOT NULL DEFAULT 'Not Available',
	email VARCHAR (255) NOT NULL UNIQUE,
	cgpa DECIMAL (3, 2) NOT NULL,
	age INT NOT NULL CHECK age >= 18,
	hall_id INT NOT NULL,
	
	PRIMARY KEY (id),
	FOREIGN KEY (hall_id) REFERENCES hall_info (id)
);
```
```sql
SHOW TABLES;
```
```sql
DESC tabname;   --describe the structure of `tabname` table
```
```sql
RENAME TABLE tabname TO new_tabname;
ALTER TABLE tabname RENAME TO new_tabname;
```
```sql
TRUNCATE TABLE tabname;
```
```sql
DROP TABLE tabname;
DROP TABLE IF EXISTS tabname; 
```
```sql
DELETE FROM tabname;
DELETE FROM tabname WHERE [condition(s)];
```

## ALTER TABLE 
```sql
ALTER TABLE tabname  ADD            colname  INT NOT NULL DEFAULT 0;
ALTER TABLE tabname  RENAME COLUMN  colname  TO new_colname;
ALTER TABLE tabname  MODIFY COLUMN  colname  VARCHAR (255) DEFAULT 'Not Available';
ALTER TABLE tabname  DROP COLUMN    colname;
```
```sql
ALTER TABLE tabname ADD CONSTRAINT pk_name PRIMARY KEY (col1, col2, ..., colN);
ALTER TABLE tabname ADD CONSTRAINT uk_name UNIQUE (col1, col2, ..., colN);
```
```sql
ALTER TABLE tabname DROP PRIMARY KEY;           
ALTER TABLE tabname DROP CONSTRAINT uk_name;  
```

## SQL Queries
```sql
INSERT INTO tabname (col1, col2, ..., colN) VALUES (val1, val2, ..., valN);
INSERT INTO tabname VALUES (val1, val2, ..., valN);
```
```sql
INSERT INTO tabname 
SELECT (col1, col2, ..., colN) FROM tabname2; -- insert value into a table from another table 
```
```sql
INSERT INTO tabname TABLE tabname2;
```
```sql
SELECT * FROM tabname;
SELECT col1, col2, col3, ..., colN FROM tabname;
SELECT 56*56;
SELECT column1 AS alas1, column2, column3 FROM tabname;
SELECT CONCAT (col1, ' ', col2) AS alias, col3, col4 FROM tabname;  -- concate columns as a unit
```
```sql
UPDATE tabname 
SET col1 = val1, col2 = val2, ..., colN = valN 
WHERE [condition(s)]; 
```
```sql
DELETE FROM tabname;    -- delete all rows
DELETE FROM tabname WHERE [condition(s)];   -- delete specific rows based on condition
```
```sql
DELETE customers, orders 
FROM customers INNER JOIN orders ON orders.customer_id = customers.id
WHERE customers.salary > 2000;  -- delete rows from multiple table 
```
```sql
SELECT * FROM tabname ORDER BY colname ASC;
SELECT * FROM tabname ORDER BY colname DESC;
SELECT * FROM tabname ORDER BY col1 DESC, col2 DESC, col32; -- if not specified `ASC` is by default
```
```sql
SELECT * FROM tabname ORDER BY 
    (CASE address
        WHEN 'Dhake'    THEN 1
        WHEN 'Rajshahi' THEN 2
        WHEN 'Khulna' 	THEN 3
        WHEN 'Barishal' THEN 4
        ELSE 100 
    END) ASC, address DESC; -- Sorted by preferred order
```

## SQL Operators and Clauses

### WHERE 
```sql
SELECT * FROM customers WHERE age NOT IN (25, 23, 22);
```
```sql
SELECT * FROM customers WHERE name LIKE 'K___%' LIMIT 2;
```
```sql
SELECT * FROM customers WHERE (age = 25 OR salary < 4500) AND (name = 'Rifat' OR name = 'Arefin');
```
```sql
SELECT * FROM customers WHERE NOT (salary > 4500 AND age < 26);
```
```sql
SELECT * FROM customers WHERE NAME NOT LIKE 'K%';
```

### DISTINCT
```sql
SELECT DISTINCT salary FROM customers ORDER BY salary;
```
```sql
SELECT DISTINCT age, salary FROM customers ORDER BY age;
```
```sql
SELECT COUNT(DISTINCT age) as unique_age  FROM customers;
```

### GROUP BY
```sql
SELECT address, AVG(salary) as avg_salary  FROM customers GROUP BY address;
```
```sql
SELECT address, age, SUM(salary) AS total_salary FROM customers GROUP BY address, age;	-- GROUP BY multiple columns
```
```sql
SELECT age, MIN(salary) AS min_salary FROM customers GROUP BY age ORDER BY min_salary DESC;
```
```sql
SELECT address, age, MIN(salary) AS min_sum FROM customers GROUP BY address, age HAVING age > 24;
```
### Note
| Order of Clauses | Execution order |
|------------------|-----------------|
| 	SELECT	 | FROM | 
|	FROM	 | WHERE |
|	WHERE	 | GROUP BY |
|	GROUP BY | HAVING |
|	HAVING	 | SELECT |
|	ORDER BY | ORDER BY |

```sql
SELECT address, age, SUM(salary) AS total_salary 
FROM customers
GROUP BY address, age 
HAVING total_salary >=5000 
ORDER BY total_salary DESC;
```

## Wildcards

| S.No | Statement | Description |
|------|-----------|-------------|
| 1 | WHERE SALARY LIKE '200%' | Values that start with 200. |
| 2 | WHERE SALARY LIKE '%200%' | Values that have 200 in any position. |
| 3 | WHERE SALARY LIKE '_00%' | Values that have 00 in the second and third positions. |
| 4 | WHERE SALARY LIKE '2_%_%' | Values that start with 2 and are at least 3 characters in length. |
| 5 | WHERE SALARY LIKE '%2' | Values that end with 2. |
| 6 | WHERE SALARY LIKE '_2%3' | Values that have a 2 in the second position and end with a 3. |
| 7 | WHERE SALARY LIKE '2___3' | Values in a five-digit number that start with 2 and end with 3. |