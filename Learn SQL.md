# Standard Query Language (SQL)

SQL is the standard language for any Relational Database System. All the Relational Data Base Management Systems (RDBMS) like MySQL, MS Access, Oracle, Sybase, Informix, Postgres and SQL Server use SQL as their standard database language. SQL is used to store and retrieve the data from a database. 

### SQL Basic Commands 

| Type | Description | Commands |
|------|-------------|----------|
| Data Definition Language (DDL) | Used to create and modify the structure of database objects(tables, views, schemas, and indexes etc) | CREATE, ALTER, DROP, TRUNCATE |
| Data Manipulation Language (DML) | Used for adding, deleting, and modifying data in a database | SELECT, INSERT, UPDATE, DELETE |
| Data Control Language (DCL) | Used to control access to data stored in a database | GRANT, REVOKE |

### How SQL works?

In SQL architecture, when you execute an SQL command on any Relational Database Management System (RDBMS), several components work together to process the request efficiently. 

Here's a breakdown of the key components:

| Component | Description |
|-----------|-------------|
| Query Dispatcher | The Query Dispatcher routes the SQL query to the appropriate query processor or engine. It determines which part of the system is responsible for handling the specific query. |
| Optimization Engines | The Optimization Engine is responsible for improving the performance of the query by evaluating different execution plans and selecting the most efficient one. It tries to reduce the time and resources required to run the query. |
| Classic Query Engine | This engine is responsible for handling non-SQL queries, such as those used in legacy systems or file-based queries. It focuses on older query types that are not structured as SQL. |
| SQL Query Engine | The SQL Query Engine is the core part of the system that processes SQL-specific commands. It handles tasks such as SELECT, INSERT, UPDATE, and DELETE, and ensures the SQL query interacts with the database as expected. |

Overall Process Flow:
Parsing: The SQL query is first parsed to check its syntax and semantics.
Optimization: The Optimization Engine evaluates multiple ways to execute the query and chooses the most efficient one.
Execution: The chosen query plan is executed, and the requested data is retrieved, updated, or deleted.

A classic query engine handles all the non-SQL queries, but a SQL query engine won't handle logical files. Following is a simple diagram showing the SQL Architecture −

<img src="./images/sql-architecture.jpg" height="300" width="500">

To learn how SQL Request-Response cycle works, [click here](./Notes/how_sql_works.md).




## References
- [Tutorials point](https://www.tutorialspoint.com/sql/index.htm)