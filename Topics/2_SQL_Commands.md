# Necessary SQL Syntax and Commands
### Contents
- [SQL Database](#sql-database)
    - [Create, show and use database](#database-creation)
    - [Delete database](#database-deletion)
    - [Backup database](#backup-database)
    - [Rename database](#rename-database)
- [SQL Table](#sql-tables)
    - [SQL constraints](#sql-constraints)
    - [Create database table](#table-creation)
    - [Rename table](#rename-table)
    - [Drop table](#drop-table)
    - [Truncate table](#truncate-table)
    - [Delete table](#delete-table)
    - [Drop vs Delete vs Truncate](#drop-vs-delete-vs-truncate)
- [SQL INSERT INTO Statement](#sql-insert-into-statement)
    - [Inserting data into a table](#inserting-data-into-a-table)
    - [Inserting data into a table using another table](#inserting-data-into-a-table-using-another-table)
- [SQL SELEECT statement](#sql-select-statement)
- [SQL UPDATE statement](#sql-update-statement)
- [SQL DELETE statement](#sql-delete-statement)


- [SQL Wildcards](#sql-wildcards)

# SQL Database

## Database Creation
To create database, we to first enter into MySQL UI or in our terminal. Enter the following command in terminal.  
```bash
mysql -u root -p
```
Here `root` is our user name and after that we have to enter our password.

Now we will create a database named `testDB`.
```sql
CREATE DATABASE testDB;                 --Returns error if it already exists
CREATE DATABASE IF NOT EXISTS testDB;   --No error returns
```
To see all existing databases in the server, 
```sql
SHOW DATABASES;
```
We can show database with a pattern e.g., a database that contains `'test'` in it's name. This is called `wildcards`.

Two types of wildcards are most commonly used :
- Percent (%) : Represents one or more characters
- Underscore (_) : Represents only one characters

Learn More about [wildcard](#sql-wildcards).

```sql
SHOW DATABASES LIKE '%test%';
```

To work further on `testDB` database, we must select it using the following command :
```sql
USE testDB;
```

## Database Deletion

```sql
DROP DATABASE testDB;               --Returns error if not exists
DROP DATABASE IF EXISTS testDB;     --No error returns
```

## Backup Database
We can backup a specific database and all the databases. Again we can restore them from the backups. To do so, we need to open our `terminal` and drop necessary commands.

```bash
mysqldump -u root -p testDB > testDB_backup.sql # Create backup
mysql -u root -p testDB < testDB_backup.sql     # Restore DB from backup
```

```bash
mysqldump -u root -p --all-databases > allDB_backup.sql # Create backup for all DBs
mysql -u root -p < allDB_backup.sql     # Restore all DBs from backup
```
**Note :** The backup files are stored where we open our terminal. Otherwise we have to specify the path along with the backup file name. For instance, `C:\backups\db_name_backup.sql` 

## Rename Database
There is a hack to change the database name :
- First take a backup of a particular database
- Drop this database from the server
- Create another database with new name
- Restore from the backup database to new database




# SQL Tables

## SQL Constraints
Constraints are the rules enforced on data columns of a table. Constraints can either be column level or table level. 
- Column level constraints are applied only to one column.
- Table level constraints are applied to the entire table.

Following are some of the most commonly used constraints available in SQL −

| Constatints | Description |
|-------------|-------------|
| NOT NULL | Ensures that a column cannot have a NULL value. |
| DEFAULT | Provides a default value for a column when none is specified. |
| CHECK | Ensures that all values in a column satisfy certain conditions. |
| UNIQUE Key | Ensures that all the values in a column are different. |
| PRIMARY Key | Uniquely identifies each row/record in a database table. |
| FOREIGN Key | Uniquely identifies a row/record in any another database table. Creates a relation with another table |
| INDEX | Used to create and retrieve data from the database very quickly. |

## Table Creation
Before create a table, we must ensure that our specific database has been selected where the table should be stored. 

```sql
USE testDB;
```

Now let's create a table `employee` with the possible constraints.

```sql
CREATE TABLE employee (
    id INT NOT NULL, --record id must have a value
    name VARCHAR (50) DEFAULT "Not available", --default value will be stored if `name` is missing while inserting a record
    employee_id VARCHAR (10) NOT NULL UNIQUE, --e.g. BS1511, BS1512 etc..
    age INT NOT NULL CHECK(age >= 18), --Employee's age must be greater than 18
    
    PRIMARY KEY (id)
);
```
Now we can see all the tables of the database and check our table has been created or not.
```sql
SHOW TABLES;
```
We can see the describe our `employee` table i.e. show the structure of the table.
```sql
DESC employee;
```
One important thing is, if we want to create a table which already exists, it'll generate an error. To avoid error we can use `IF NOT EXISTS` clause. Now we'll create another table `salary`

```sql
CREATE TABLE IF NOT EXISTS salary(
    id INT NOT NULL,
    employee_id INT NOT NULL,
    gross_salary DECIMAL (10, 2) NOT NULL,
    medical_allowance DECIMAL (10, 2) DEFAULT 0.00, 
    house_allowance DECIMAL (10, 2) DEFAULT 0.00,
    festival_bonus DECIMAL (10, 2) DEFAULT 0.00,
    total DECIMAL (10, 2) NOT NULL,

    PRIMARY KEY (id),
    FOREIGN KEY (employee_id) REFERENCES employee (id)  --`employee_id` creates a relationship with column `id` of `employee` table
); 
```

Let's see another example where multiple columns are used as `PRIMARY KEY`. (Also called `Composite Key`). 

*Consider a scenario with students enrolling in various courses across multiple semesters. Here, a student's enrollment in a specific course during a specific semester would be uniquely identified by a combination of three columns: `StudentID, CourseID, and Semester`.*

Here, 
- A student can enroll in multiple courses across different semesters (`enrollment` table).
- For each enrolled course, attendance is tracked (`course_attendance` table).

Create the Enrollment Table with a Composite Primary Key
```sql
CREATE TABLE enrollment (
    student_id INT,
    course_id INT,
    semester VARCHAR(10),
    enrollment_date DATE,
    grade CHAR(2),
    PRIMARY KEY (student_id, course_id, semester)
);
```
```sql
CREATE TABLE course_attendance (
    attendance_id INT PRIMARY KEY,
    student_id INT,
    course_id INT,
    semester VARCHAR(10),
    attendance_date DATE,
    status VARCHAR(10),
    FOREIGN KEY (student_id, course_id, semester) REFERENCES enrollment(student_id, course_id, semester)
);
```
### Example
### `enrollment` table
| student_id | course_id | semester   | enrollment_date | grade |
|------------|-----------|------------|-----------------|-------|
| 1001       | 501       | Fall2024   | 2024-08-15      | A     |
| 1001       | 502       | Fall2024   | 2024-08-16      | B+    |
| 1002       | 501       | Fall2024   | 2024-08-15      | B     |
| 1003       | 503       | Fall2024   | 2024-08-17      | A-    |
| 1002       | 504       | Spring2025 | 2025-01-12      |       |

### `course_attendance` table
| attendance_id | student_id | course_id | semester    | attendanceDate | status  |
|---------------|------------|-----------|-------------|----------------|---------|
| 1             | 1001       | 501       | Fall2024    | 2024-09-01     | Present |
| 2             | 1001       | 501       | Fall2024    | 2024-09-03     | Absent  |
| 3             | 1001       | 502       | Fall2024    | 2024-09-01     | Present |
| 4             | 1002       | 501       | Fall2024    | 2024-09-01     | Present |
| 5             | 1003       | 503       | Fall2024    | 2024-09-05     | Present |
| 6             | 1002       | 504       | Spring2025  | 2025-02-01     | Present |

## Rename Table
```sql
RENAME TABLE tabname TO new_tabname;    --using RENAME TABLE statement
ALTER TABLE tabname RENAME TO new_tabname;  --using ALTER TABLE statement
```

## Drop Table
- The `DROP TABLE` statement is a **Data Definition Language (DDL)** command that is used to remove a table's definition, and its data, indexes, triggers, constraints and permission specifications (if any). 
- SQL `DROP TABLE` command removes an existing table completely in a database. - Once SQL `DROP` command is issued then there is no way back to recover the table including its data.

```sql
DROP TABLE tabname;        --Returns error if `tabname` table doesn't exist
DROP TABLE IF EXISTS tabname; --Returns no error
```

## Truncate Table
- `TRUNCATE TABLE` command is used to empty the table completely instead of deleting table records one by one which will be very time consuming and cumbersome process. 
- This command is a sequence of `DROP TABLE` and `CREATE TABLE` statements and requires the `DROP privilege`.

```sql
TRUNCATE TABLE table_name;
```
**Note:** There is no `IF EXISTS` clause with `TRUNCATE`.

## Delete Table
- The `DELETE` is a command of **Data Manipulation Language (DML)**, so it does not delete or modify the table structure but it delete the existing records from the table. 
- Any constraints, indexes, or triggers defined in the table will still exist after you delete data from it.
- To delete only the specific number of rows from the table, we can use the `WHERE` clause with the `DELETE` statement. 
- Without `WHERE` clause, All rows in the table will be deleted. 

**Note:** The SQL `DELETE` statement operates on a single table at a time.

```sql
DELETE TABLE tabname; --Delelte all records of a table 
DELETE TABLE tabname WHERE [condition]; --Delete specific records of a table based on condition
```

## DROP vs DELETE vs TRUNCATE

| **Feature** | **DROP** | **DELETE** | **TRUNCATE** |
|---------|--------|----------|------------|
| **Operation Type** | DDL (Data Definition Language) | DML (Data Manipulation Language) | DDL (Data Definition Language) |
| **Purpose** | Removes the entire table, including its structure and data | Deletes specific rows from the table | Removes all rows from the table |
| **Impact on Table Structure** | Table structure and data are completely removed | Table structure remains intact; only selected data is affected | Table structure remains intact; only all data is removed |
| **Can Specify Conditions** | No | Yes, using `WHERE` clause | No |
| **Affects Table Constraints** | Yes, all constraints are removed | No | No |
| **Auto-increment Reset** | Yes | No | Yes, resets auto-increment counter |
| **Transaction Handling** | Causes an implicit commit; cannot be rolled back | Can be rolled back if inside a transaction | Causes an implicit commit; cannot be rolled back |
| **Space Reclamation** | Frees up space occupied by the table and its indexes | Does not reclaim space; leaves table structure intact | Reclaims the space of the data but keeps table structure |
| **Permissions Required** | `ALTER` on the table and `CONTROL` on the schema | `DELETE` permission on the table | `ALTER` permission on the table |




# SQL INSERT INTO Statement

## Inserting data into a table
The SQL `INSERT INTO` Statement is used to add new rows of data into a table in the database. Almost all the RDBMS provide this SQL query to add the records in database tables
```sql
INSERT INTO tabname (col1, col2, col3, ..., colN) VALUES (val1, val2, val3, ..., valN); --insert values of specified columns
INSERT INTO tabname VALUES (val1, val2, val3, ..., valN);   --insert values of all columns
```

## Inserting data into a table using another table
- Using `INSERT INTO ... SELECT` statement
- Using `INSERT INTO ... TABLE` statement

### INSERT INTO ... SELECT 
- Two tables must exist in database
- Two tables must have similar structures, similar data-type
- The `SELECT` statement first retrieves the data from an existing table and the `INSERT INTO` statement inserts the retrieved data into another table (if they have same table structures).

```sql
INSERT INTO first_tabname (col1, col2, col3, ..., colN) 
SELECT (col1, col2, col3, ..., colN) FROM second_tabname 
WHERE [condition(s)];   #condition is optional
```
```sql
-- Create the employees table
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    position VARCHAR(50),
    salary DECIMAL(10, 2),
    hire_date DATE
);

-- Insert some sample data into the employees table
INSERT INTO employees (name, position, salary, hire_date)
VALUES 
('John Doe', 'Software Engineer', 75000.00, '2020-01-15'),
('Jane Smith', 'Project Manager', 85000.00, '2018-03-10'),
('Alice Johnson', 'QA Engineer', 65000.00, '2019-05-22'),
('Bob Brown', 'UX Designer', 70000.00, '2021-07-30');

-- Create the archived_employees table
CREATE TABLE archived_employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    position VARCHAR(50),
    salary DECIMAL(10, 2),
    hire_date DATE
);

-- Use INSERT INTO ... SELECT to copy employees hired before 2020 to the archived_employees table
INSERT INTO archived_employees (id, name, position, salary, hire_date)
SELECT id, name, position, salary, hire_date
FROM employees
WHERE hire_date < '2020-01-01';

-- Check the data in the archived_employees table
SELECT * FROM archived_employees;
```

### INSERT INTO ... TABLE statement
If two tables structure are exactly same, then instead of selecting specific columns we can insert the contents of one table into another using the `INSERT INTO ... TABLE` statement.

```sql
INSERT INTO first_table_name TABLE second_table_name;
```

# SQL SELECT statement
The SQL `SELECT` Statement is used to fetch the data from a database table which returns this data in the **form of a table**. These tables are called result-sets.

```sql
SELECT column1, column2, columnN FROM table_name; --select data from specific columns
SELECT * FROM table_name; --select all data from all columns
```
### Aliasing a Column in SELECT Statement
```sql
SELECT col1 AS alias1, col2, col3, ...., colN 
FROM table_name;
```
In the example below, we are trying to retrieve `customer details` **NAME** and **AGE** in a single column of the resultant table using the `concat()` expression and aliasing the column as `DETAILS` along with the customer addresses from the CUSTOMERS table.
```sql
SELECT CONCAT(name,' ',age) AS details, address 
FROM customers;
```

We can also use `SELECT` statement to perform arithmatic expression. 
```sql
SELECT 56*56 AS total;
```



# SQL UPDATE statement
- The SQL `UPDATE` Statement is used to modify the existing records in a table. - This statement is a part of `Data Manipulation Language (DML)`, as it only modifies the data present in a table without affecting the table's structure.
- The `UPDATE` statement makes use of **locks on each row** while modifying them in a table, and once the row is modified, the lock is released. 
- It can either make changes to a single row or multiple rows with a single query.

```sql
UPDATE table_name
SET column1 = value1, column2 = value2,..., columnN = valueN
WHERE [condition];
```
```sql
UPDATE customers SET age = 50 WHERE id = 6; --update single row
UPDATE customers SET age = age+5, salary = salary+3000; --Update multiple rows and columns
```


# SQL DELETE statement
- The SQL `DELETE` Statement is used to delete the records from an existing table. 
- In order to filter the records to be deleted (or, delete particular records), we need to use the `WHERE` clause along with the `DELETE` statement.
- If you execute `DELETE` statement without a WHERE clause, it will delete all the records from the table.
- Using the `DELETE` statement, we can delete one or more rows of a single table and records across multiple tables.

```sql
DELETE FROM table_name WHERE [condition];
```
```sql
DELETE FROM customers WHERE id = 6;     --delete single row
DELETE FROM customers WHERE age > 25;   --delete multiple rows
DELETE FROM customers;                  --delete all rows
```








# SQL Wildcards

SQL Wildcards are special characters used as substitutes for one or more characters in a string. They are used with the `LIKE` operator in SQL, to search for specific patterns.

**Note :** The `LIKE` operator in SQL is case-sensitive, so it will only match strings that have the exact same case as the specified pattern.

Following are the most commonly used wildcards in SQL −

| Wildcard | Description |
|----------|-------------|
| Percent (%) | Matches one or more characters. <br>**Note :** MS Access uses the asterisk (*) wildcard character instead of the percent sign (%) wildcard character. |
| Underscore (_) | Matches only one character. <br>**Note :** MS Access uses a question mark (?) instead of the underscore (_) to match any one character. |

### Examples 

The following table demonstrates various ways of using wildcards in conjunction with the LIKE operator within a WHERE clause:

| Statement | Description | Examples |
|-----------|-------------|----------|
| WHERE SALARY LIKE '200%' | Finds any values that start with 200.                                     | 20000, 2000, 200450            |
| WHERE SALARY LIKE '%200%' | Finds any values that have 200 in any position.                           | 12000, 92002, 520000, 3200     |
| WHERE SALARY LIKE '_00%' | Finds any values that have 00 in the second and third positions. | 1000, 2000, 3000, 40045        |
| WHERE SALARY LIKE '2_%_%' | Finds any values that start with 2 and are at least 3 characters in length. | 200, 2500, 21000               |
| WHERE SALARY LIKE '%2' | Finds any values that end with 2.                                         | 12, 102, 432                   |
| WHERE SALARY LIKE '_2%3' | Finds any values that have a 2 in the second position and end with a 3.   | 123, 523, 2234                 |
| WHERE SALARY LIKE '2___3' | Finds any values in a five-digit number that start with 2 and end with 3. | 20003, 21203, 29883            |


