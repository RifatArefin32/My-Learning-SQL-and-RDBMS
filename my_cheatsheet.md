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
	id INTname NULL,
	name VARCHARname5) NOT NULL UNIQUE,
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
SELECT * FROM tabname ORDER BY col1 DESC, col2 DESC, col32; 
-- Default `ASC`
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
SELECT * FROM customers WHERE (age = 25 OR salary < 4500) AND (name = 'Rifat' OR name = 'Mahim');
```
```sql
SELECT * FROM customers WHERE NOT (salary > 4500 AND age < 26);
```
```sql
SELECT * FROM customers WHERE NAME NOT LIKE 'K%';
```
```sql
SELECT * FROM customers WHERE salary NOT BETWEEN 1500.00 AND 2500.00;
```
```sql
SELECT * FROM customers WHERE salary IS NOT 2500.00;
```
```sql
SELECT COUNT(*) FROM customers WHERE address IS NOT NULL;
```
```sql
DELETE FROM customers WHERE salary IS NULL;
```
```sql
SELECT * FROM customers WHERE salary BETWEEN 4000 AND 10000 AND address IN ('Dhaka', 'Khulna');
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
SELECT address, age, SUM(salary) AS total_salary FROM customers GROUP BY address, age;
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
### Wildcards
| S.No | Statement | Description |
|------|-----------|-------------|
| 1 | WHERE SALARY LIKE '200%' | Values that start with 200. |
| 2 | WHERE SALARY LIKE '%200%' | Values that have 200 in any position. |
| 3 | WHERE SALARY LIKE '_00%' | Values that have 00 in the second and third positions. |
| 4 | WHERE SALARY LIKE '2_%_%' | Values that start with 2 and are at least 3 characters in length. |
| 5 | WHERE SALARY LIKE '%2' | Values that end with 2. |
| 6 | WHERE SALARY LIKE '_2%3' | Values that have a 2 in the second position and end with a 3. |
| 7 | WHERE SALARY LIKE '2___3' | Values in a five-digit number that start with 2 and end with 3. |
### ANY or ALL
```sql
Column_name operator [ANY|ALL] (subquery);	-- syntax
```
```sql
SELECT DISTINCT age FROM customers WHERE salary < ANY (SELECT AVG(salary) FROM customers);
```
```sql
SELECT * FROM customers WHERE age = ANY (SELECT age FROM customers WHERE NAME LIKE 'K%');
```
```sql
SELECT * FROM customers WHERE salary <> ALL (SELECT salary FROM customers WHERE age = 25);
```
```sql
SELECT NAME, age, address, salary FROM customers GROUP BY age, salary 
HAVING salary < ALL (SELECT AVG(salary) FROM customers);
```
### EXISTS
```sql
SELECT * FROM customers WHERE EXISTS (
   SELECT price FROM cars WHERE cars.id = customers.id AND price > 2000000
);
```
```sql
UPDATE customers SET NAME = 'Kushal' WHERE EXISTS (
   SELECT NAME FROM carsWHERE customers.id = cars.id
);
```
```sql
DELETE FROM customers WHERE NOT EXISTS (
   SELECT * FROM cars WHERE cars.id = customers.id AND cars.price = 2250000
);
```
### CASE
```sql
-- syntax
CASE
   WHEN condition1 THEN statement1,
   WHEN condition2 THEN statement2,
   WHEN condition THEN statementN
   ELSE result
END;
```
```sql
SELECT name, age,
CASE 
	WHEN age > 30 THEN 'Gen X'
	WHEN age > 25 THEN 'Gen Y'
	WHEN age > 22 THEN 'Gen Z'
	ELSE 'Gen Alpha' 
END AS generation
FROM customers;
```
```sql
SELECT *, 
CASE 
	WHEN salary < 4500 THEN (salary + salary * 25/100) 
END AS increment 
FROM customers;
```
```sql
SELECT name, address, 
   	CASE 
    	WHEN age < 25 THEN 'Intern'
      	WHEN age >= 25 and age <= 27 THEN 'Associate Engineer'
      	ELSE 'Senior Developer'
   	END as designation
FROM customers
WHERE salary >= 2000;
```
```sql
SELECT * FROM customers
ORDER BY (
	CASE
    	WHEN name LIKE 'k%' THEN name
    	ELSE address
	END
);
```
```sql
SELECT 
	CASE 
    	WHEN salary <= 4000 THEN 'Lowest paid'
    	WHEN salary > 4000 AND salary <= 6500 THEN 'Average paid'
   		ELSE 'Highest paid' 
    END AS salary_status,
   	SUM(salary) AS Total
FROM customers
GROUP BY 
   	CASE 
    	WHEN salary <= 4000 THEN 'Lowest paid'
      	WHEN salary > 4000 AND salary <= 6500 THEN 'Average paid'
   		ELSE 'Highest paid'
	END;
```
```sql
UPDATE customers
SET salary= 
	CASE age
		WHEN 25 THEN 17000
		WHEN 32 THEN 25000
		ELSE 12000
	END;
```
```sql
INSERT INTO customers (ID, name, age, address, salary)
VALUES (10, 'Viren', 28, 'Varanasi', 
   CASE 
      WHEN age >= 25 THEN 23000
      ELSE 14000
   END
);
```
### UNION and UNION ALL
- `UNION` only returns distinct rows 
- `UNION ALL` returns all the rows present in the tables.
```sql	
SELECT column1 , column2 FROM table1 WHERE [condition(s)]	
[UNION|UNION ALL]	
SELECT column1 , column2 FROM table2 WHERE [condition(s)]; 
-- same number of columns with same data-type from both of the tables
```
```sql
SELECT salary FROM customers UNION SELECT amount FROM orders;
```
```sql
SELECT id, 'customer' AS type FROM customers
UNION
SELECT oid, 'order' AS type FROM orders;
```
```sql
SELECT id, salary FROM customers WHERE id > 5
UNION
SELECT customer_id, amount FROM orders WHERE customer_id > 2 
ORDER BY salary;
```
```sql
SELECT  id, name, amount FROM customers LEFT JOIN orders ON customers.id = orders.customer_id
UNION
SELECT  id, name, amount FROM customers RIGHT JOIN orders ON customers.id = orders.customer_id;
```
### INTERSECT
```sql
SELECT name, age, hobby FROM student_hobby
INTERSECT 
SELECT name, age, hobby FROM student;
```
```sql
SELECT name, age, hobby FROM student_hobby WHERE age BETWEEN 25 AND 30
INTERSECT
SELECT name, age, hobby FROM student WHERE age NOT BETWEEN 20 AND 30;
```
```sql
SELECT name, age, hobby FROM student_hobby WHERE hobby IN('Cricket')
INTERSECT
SELECT name, age, hobby FROM student WHERE hobby IN('Cricket');
```

## SQL JOIN Operation
### Types of JOIN
- INNER JOIN
- OUTER JOIN
	- LEFT JOIN 
	- RIGHT JOIN
	- FULL JOIN
Other Joins
 - SELF JOIN
 - CROSS Join

 ```sql
SELECT id, name, amount, date
FROM customers 
INNER JOIN orders ON customer.id = orders.customer_id;
INNER JOIN employees ON orders.id = employees.id
WHERE orders.amount >= 20000;
```
```sql
SELECT customers.id, customers.name, orders.date, employee.employee_name
FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id
LEFT JOIN employee ON orders.id = employee.id;
WHERE orders.amount >= 20000;
```
```sql
SELECT customers.id, customers.name, orders.date, employee.employee_name
FROM customers
RIGHT JOIN orders ON customers.id = orders.customer_id
RIGHT JOIN employee ON orders.id = employee.id;
WHERE orders.amount >= 20000;
```
```sql
SELECT customers.id, customers.name, orders.date, employee.employee_name
FROM customers
FULL JOIN orders ON customers.id = orders.customer_id
FULL JOIN employee ON orders.id = employee.id;
WHERE orders.amount >= 20000;
```
```sql
SELECT customers.id, customers.name, orders.date, employee.employee_name
FROM customers
CROSS JOIN orders ON customers.id = orders.customer_id
CROSS JOIN employee ON orders.id = employee.id;
WHERE orders.amount >= 20000;
```
```sql
SELECT  a.id, b.name as earns_higher, a.name as earns_less, a.salary as lower_salary
FROM customers a, customers b
WHERE a.salary < b.salary
ORDER BY a.salary;
```