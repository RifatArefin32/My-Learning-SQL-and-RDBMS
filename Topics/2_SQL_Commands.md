# Necessary SQL Syntax and Commands
Content :
- [SQL Database](#sql-database)
- [SQL Wildcards](#sql-wildcards)

# SQL Database

### Database Creation and Usage
To create database, we to first enter into MySQL UI or in our terminal. From terminal enter the following command : 
```bash
mysql -u root -p
```
Here `root` is our user name and after that we have to enter our password.

Now we will create a database named `testDB`.
```bash
CREATE DATABASE testDB;    # Returns error if it already exists
CREATE DATABASE IF NOT EXISTS testDB; # No error returns
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

### Backup Database
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

### Rename Database
There is a hack to change the database name :
- First take a backup of a particular database
- Drop this database from the server
- Create another database with new name
- Restore from the backup database to new database



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


