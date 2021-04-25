# Installation and Setup

1. `sudo apt update`
2. `sudo apt install mysql-server`
3. `sudo mysql_secure_installation`

## Creating a Dedicated MySQL User and Granting Privileges

`$ sudo mysql`

- Login Using Sudo `sudo mysql`
- Create User `CREATE USER 'nitesh'@'localhost' IDENTIFIED BY 'Nitesh@12345';`

**Grand Access**

```mysql
# Grant All Access
GRANT ALL PRIVILEGES ON *.* TO 'nitesh'@'localhost' WITH GRANT OPTION;

# This will free up any memory that the server cached as a result of the preceding CREATE USER and GRANT statements.
FLUSH PRIVILEGES;

EXIT;
```

## Login into database

```bash
systemctl status mysql.service

# Login into database
$ sudo mysql -u username -p
```

# Getting Start

**This will help to understand below terminology**

```mysql
CREATE TABLE Persons (
    PersonID INT NOT NULL UNIQUE,
    LastName VARCHAR(255),
    FirstName VARCHAR(255),
    Address VARCHAR(255),
    City VARCHAR(255),
    PRIMARY KEY(PersonID)
);
```



## MySQL Prompts

| Mysql> | This prompt indicates ready for a new query.                 |
| ------ | ------------------------------------------------------------ |
| ->     | Next line of a multi-line query                              |
| `>     | Continues to wait for identifier completion beginning with.` |
| ‘>     | Continues to wait to the end of the string that was begun with ‘ |
| “>     | Continues to wait to the end of the string that was begun with “ |
| /*>    | Waits for the completion of a comment begun with /*          |

## Data Type

### Numeric Data Types

| Type      | Storage (Bytes) | Min Value (Signed/ Unsigned) | Max Value (Signed/ Unsigned) |
| --------- | --------------- | ---------------------------- | ---------------------------- |
| TINYINT   | 1               | -128                         | 127                          |
|           |                 | 0                            | 255                          |
| SMALLINT  | 2               | -32768                       | 32767                        |
|           |                 | 0                            | 65535                        |
| MEDIUMINT | 3               | -8388608                     | 8388607                      |
|           |                 | 0                            | 16777215                     |
| INT       | 4               | -2147483648                  | 2147483647                   |
|           |                 | 0                            | 4294967295                   |

**Note: **You can use either `INT` or `INTEGER` in the *CREATE TABLE* statement above because they are interchangeable.

**Example:**

```mysql
-- Declare Int variable
CREATE TABLE items (
    item_id INT AUTO_INCREMENT PRIMARY KEY,
    item_text VARCHAR(255)
);

-- Unsigned Int when we try to insert -45 it will generate error 
-- Error Code: 1264. Out of range value for column 'total_member' at row 1
CREATE TABLE classes (
    class_id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    total_member INT UNSIGNED,
    PRIMARY KEY (class_id)
);
```

#### MySQL `INT` with width attribute

MySQL provides an extension that allows you to specify the display width along with the `INT` datatype. The display width is wrapped inside parentheses following the `INT` keyword e.g., `INT(5)` specifies an `INT` with the display width of five digits.

It is important to note that the display width attribute does not control the value ranges that the column can store. The display width attribute is typically used by the applications to format the integer values. MySQL includes the display width attribute as the metadata of the returned result set.

#### MySQL  `INT` with `ZEROFILL`  attribute

In addition to the display width attribute, MySQL provides a non-standard ZEROFILL attribute. In this case, MySQL replaces spaces with zero.

**Example**

```plaintext
CREATE TABLE zerofill_tests(
    id INT AUTO_INCREMENT PRIMARY KEY,
    v1 INT(2) ZEROFILL,
    v2 INT(3) ZEROFILL,
    v3 INT(5) ZEROFILL
);
INSERT INTO zerofill_tests(v1,v2,v3)VALUES(1,6,9);
SELECT * FROM zerofill_tests;

-- output:
+----+------+------+-------+
| id | v1   | v2   | v3    |
+----+------+------+-------+
|  1 |   01 |  006 | 00009 |
+----+------+------+-------+
1 row in set (0.00 sec)
```

### MySQL String Data Types

a string can hold anything from plain text to binary data such as images or files. Strings can be compared and searched based on pattern matching by using the `LIKE` operator, regular expression and full text search.

| String Type | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| CHAR        | A fixed-length nonbinary (character) string                  |
| VARCHAR     | A variable-length non-binary string                          |
| BINARY      | A fixed-length binary string                                 |
| VARBINARY   | A variable-length binary string                              |
| TINYBLOB    | A very small BLOB (binary large object)                      |
| BLOB        | A small BLOB                                                 |
| MEDIUMBLOB  | A medium-sized BLOB                                          |
| LONGBLOB    | A large BLOB                                                 |
| TINYTEXT    | A very small non-binary string                               |
| TEXT        | A small non-binary string                                    |
| MEDIUMTEXT  | A medium-sized non-binary string                             |
| LONGTEXT    | A large non-binary string                                    |
| ENUM        | An enumeration; each column value may be assigned one enumeration member |
| SET         | A set; each column value may be assigned zero or more `SET` members |

#### `CHAR`

The `CHAR` data type is a fixed-length character type in MySQL. You often declare the `CHAR` type with a length that specifies the maximum number of characters that you want to store. For example, `CHAR(20)` can hold up to 20 characters.

If the data that you want to store is a fixed size, then you should use the `CHAR` data type. You’ll get a better performance in comparison with `VARCHAR` in this case.

The length of the CHAR data type can be any value from 0 to 255. When you store a `CHAR` value, MySQL pads its value with spaces to the length that you declared.

When you query the `CHAR` value, MySQL removes the trailing spaces.

## Constraints

Constraints are the rules that we can apply on the type of data in a  table. That is, we can specify the limit on the type of data that can be stored in a particular column in a table using constraints.

The available constraints in SQL are:

- **NOT NULL**: This constraint tells that we cannot store a null value in a column.  That is, if a column is specified as NOT NULL then we will not be able  to store null in this particular column any more.
- **UNIQUE**: This constraint when specified with a column, tells that all the values in the column must be unique. That is, the values in any row of a  column must not be repeated.
- **PRIMARY KEY**: A  primary key is a field which can uniquely identify each row in a table.  And this constraint is used to specify a field in a table as primary  key.
- **FOREIGN KEY**: A Foreign key is a field  which can uniquely identify each row in a another table. And this  constraint is used to specify a field as Foreign key.
- **CHECK**: This constraint helps to validate the values of a column to meet a  particular condition. That is, it helps to ensure that the value stored  in a column meets a specific condition.
- **DEFAULT**: This constraint specifies a default value for the column when no value is specified by the user.
- **AUTO INCREMENT**: Auto-increment allows a unique number to be generated automatically when a new record is inserted into a table.

**How to specify constraints?**
We can specify  constraints at the time of creating the table using CREATE TABLE  statement. We can also specify the constraints after creating a table  using ALTER TABLE statement.

**Syntax**

```mysql
CREATE TABLE sample_table
(
column1 data_type(size) constraint_name,
column2 data_type(size) constraint_name,
column3 data_type(size) constraint_name,
....
);
```

**Example**

```mysql
CREATE TABLE Persons (
    PersonID INT NOT NULL AUTO_INCREMENT,
    Name VARCHAR(255),
    Address VARCHAR(255),
    Country VARCHAR(255) DEFAULT "India",
    Phone VARCHAR(12) UNIQUE,
    Age INT(5) CHECK (Age >= 18)
    PRIMARY KEY(PersonID)
);
```

## Basic Operations (Insert, Read, Update, Delete)

```mysql
-- To Create Database
CREATE DATABASE database_name

-- To Use Database
USE database_name

-- Create Table
> CREATE TABLE Persons (
    PersonID INT NOT NULL AUTO_INCREMENT,
    Name VARCHAR(255),
    Address VARCHAR(255),
    Country VARCHAR(255) DEFAULT "India",
    Phone VARCHAR(12) UNIQUE,
    Age INT(5) CHECK (Age >= 18),
    PRIMARY KEY(PersonID)
);
Query OK, 0 rows affected, 1 warning (0.12 sec)


-- See Table structure
> DESC Persons;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| PersonID | int          | NO   | PRI | NULL    | auto_increment |
| Name     | varchar(255) | YES  |     | NULL    |                |
| Address  | varchar(255) | YES  |     | NULL    |                |
| Country  | varchar(255) | YES  |     | India   |                |
| Phone    | varchar(12)  | YES  | UNI | NULL    |                |
| Age      | int          | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
6 rows in set (0.02 sec)
```

### Insert

**Syntax**

```mysql
-- Specify both the column names and the values to be inserted
INSERT INTO table_name (column1, column2, column3, ...) VALUES (value1, value2, value3, ...); 


-- If you are adding values for all the columns of the table, you do not need to specify the column names in the SQL query. However, make sure the order of the values is in the same order as the columns in the table.
INSERT INTO table_name VALUES (value1, value2, value3, ...); 
```

**Example**

```mysql
INSERT INTO Persons (Name, Address, Country, Phone, Age) VALUES ("Rajesh", "Jalna", "India", "2361436111", 50);

INSERT INTO Persons (Name, Address, Country, Phone, Age) VALUES ("Sharad", "Baramati", "India", "9658253625", 80),
("Ram", "Nagpur", "India", "8596586958", 22),
("Satish", "Pune", "India", "125142536", 25),
("Udhav", "Mumbai", "India", "8858585847", 48),
("Raj", "Mumbai", "India", "6536929841", 42);
```

### Read

```mysql
SELECT * FROM Persons
+----------+--------+----------+---------+------------+------+
| PersonID | Name   | Address  | Country | Phone      | Age  |
+----------+--------+----------+---------+------------+------+
|        1 | Nitesh | Pune     | India   | 9130601739 |   25 |
|        2 | Sharad | Baramati | India   | 9658253625 |   80 |
|        3 | Ram    | Nagpur   | India   | 8596586958 |   22 |
|        4 | Satish | Pune     | India   | 125142536  |   25 |
|        5 | Udhav  | Mumbai   | India   | 8858585847 |   48 |
|        6 | Raj    | Mumbai   | India   | 6536929841 |   42 |
|        7 | Rajesh | Jalna    | India   | 2361436111 |   50 |
+----------+--------+----------+---------+------------+------+
7 rows in set (0.00 sec)
```

### Update

```mysql
>UPDATE Persons SET Name = "Akshay" WHERE PersonID = 1;
	Query OK, 1 row affected (0.01 sec)
	Rows matched: 1  Changed: 1  Warnings: 0

select * from Persons;
+----------+---------+----------+---------+------------+------+
| PersonID | Name    | Address  | Country | Phone      | Age  |
+----------+---------+----------+---------+------------+------+
|        1 | Akashay | Pune     | India   | 9130601739 |   25 |
|        2 | Sharad  | Baramati | India   | 9658253625 |   80 |
|        3 | Ram     | Nagpur   | India   | 8596586958 |   22 |
|        4 | Satish  | Pune     | India   | 125142536  |   25 |
|        5 | Udhav   | Mumbai   | India   | 8858585847 |   48 |
|        6 | Raj     | Mumbai   | India   | 6536929841 |   42 |
|        7 | Rajesh  | Jalna    | India   | 2361436111 |   50 |
+----------+---------+----------+---------+------------+------+
7 rows in set (0.00 sec)
```

### Delete

```mysql
> DELETE FROM Persons WHERE Age = 22;
Query OK, 1 row affected (0.01 sec)

mysql> select * from Persons;
+----------+---------+----------+---------+------------+------+
| PersonID | Name    | Address  | Country | Phone      | Age  |
+----------+---------+----------+---------+------------+------+
|        1 | Akashay | Pune     | India   | 9130601739 |   25 |
|        2 | Sharad  | Baramati | India   | 9658253625 |   80 |
|        4 | Satish  | Pune     | India   | 125142536  |   25 |
|        5 | Udhav   | Mumbai   | India   | 8858585847 |   48 |
|        6 | Raj     | Mumbai   | India   | 6536929841 |   42 |
|        7 | Rajesh  | Jalna    | India   | 2361436111 |   50 |
+----------+---------+----------+---------+------------+------+
6 rows in set (0.00 sec)
```

# Database Operations

```mysql
-- Create Databse
> CREATE DATABASE database_name;

-- Get list of databases
> SHOW DATABASES;

-- To Use Database
> USE database_name

-- To delete Database 
> DROP DATABASE database_name

```

**Note:** We can not rename database names.

# Table Operations

## Create

```mysql
-- Create Table
> CREATE TABLE Persons (
    PersonID INT NOT NULL AUTO_INCREMENT,
    Name VARCHAR(255),
    Address VARCHAR(255),
    Country VARCHAR(255) DEFAULT "India",
    Phone VARCHAR(12) UNIQUE,
    Age INT(5) CHECK (Age >= 18),
    PRIMARY KEY(PersonID)
);
Query OK, 0 rows affected, 1 warning (0.12 sec)
```

## Table List from database

```MYSQL
> SHOW TABLES;
+------------------+
| Tables_in_aadhar |
+------------------+
| Persons          |
+------------------+
1 row in set (0.00 sec)
```

## Alter Table

```mysql
CREATE DATABASE stackoverflow;
USE stackoverflow;

CREATE TABLE stack(
	id_user int NOT NULL,
    username varchar(30) NOT NULL,
    password varchar(30) NOT NULL,
)

ALTER TABLE stack ADD COLUMN submit date NOT NULL ; -- add new column
ALTER TABLE stack DROP COLUMN submit; -- drop column
ALTER TABLE stack MODIFY submit DATETIME NOT NULL; -- modify type column
ALTER TABLE stack CHANGE submit submit_date DATETIME NOT NULL; -- -- change type and name of column
ALTER TABLE stack ADD COLUMN mod_id INT NULL AFTER id_user; -- add add new column after existing column


-- Change Auto-increment value
ALTER TABLE your_table_name AUTO_INCREMENT = 101;

-- Renaming a MySQL Table
RENAME TABLE 'old_table_name' TO 'new_table_name';
ALTER TABLE 'old_table_name' RENAME TO 'new_table_name'

-- Changing the type of a primary key column
ALTER TABLE db_name.table_name DROP PRIMARY KEY;
ALTER TABLE db_name.table_name MODIFY COLUMN col_name DECIMAL(20,0) NOT NULL PRIMARY KEY;
-- An attempt to modify the type of this column without first dropping the primary key would result in an error.
```

## Drop Table

**Syntax**

```mysql
DROP TABLE table_name;

-- Drop table from database 
DROP TABLE db_name.table_name;
```

> **Please Note: ** Dropping table will completely delete the table from the database and all its information, and it will not be recovered.

## Describe Table

```mysql
> DESC table_name;
> DESC Persons;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| PersonID | int          | NO   | PRI | NULL    | auto_increment |
| Name     | varchar(255) | YES  |     | NULL    |                |
| Address  | varchar(255) | YES  |     | NULL    |                |
| Country  | varchar(255) | YES  |     | India   |                |
| Phone    | varchar(12)  | YES  | UNI | NULL    |                |
| Age      | int          | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
6 rows in set (0.03 sec)
```

## Table Structure

```mysql
> SHOW CREATE TABLE Persons \G
*************************** 1. row ***************************
       Table: Persons
Create Table: CREATE TABLE `Persons` (
  `PersonID` int NOT NULL AUTO_INCREMENT,
  `Name` varchar(255) DEFAULT NULL,
  `Address` varchar(255) DEFAULT NULL,
  `Country` varchar(255) DEFAULT 'India',
  `Phone` varchar(12) DEFAULT NULL,
  `Age` int DEFAULT NULL,
  PRIMARY KEY (`PersonID`),
  UNIQUE KEY `Phone` (`Phone`),
  CONSTRAINT `Persons_chk_1` CHECK ((`Age` >= 18))
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.01 sec)
```

## Cloning

```mysql
-- Cloning an existing table
CREATE TABLE ClonedPersons LIKE Persons;

-- 2nd method cloning 
CREATE TABLE ModifiedPersons SELECT PersonID, FirstName + LastName AS FullName from Persons WHERE Coutry = "USA"

CREATE TABLE stack (
id_user INT,
username VARCHAR(30),
password VARCHAR(30)
);

-- create a table from another table in the same database with all attributes
CREATE TABLE stack2 AS SELECT * FROM stack;

-- create a table from another table in the same database with some attributes
CREATE TABLE stack3 AS SELECT username, password FROM stack;

-- create a table from another table from another database with all attributes
CREATE TABLE stack2 AS SELECT * FROM second_db.stack;

-- create a table from another table from another database with some attributes
CREATE TABLE stack3 AS SELECT username, password FROM second_db.stack;
```

```mysql
CREATE TABLE indian_city SELECT * FROM city where CountryCode = "IND";
```



# Select 

```mysql
CREATE TABLE car ( 
car_id INT UNSIGNED NOT NULL PRIMARY KEY,
name VARCHAR(20),
price DECIMAL(8,2));

INSERT INTO car (car_id, name, price) VALUES 
(1,'Audi A1','15000'),
(2,'Audi A1','40000'),
(3,'Audi A1','20000'),
(4,'Audi A1','20000');
```

# DISTINCT

The DISTINCT clause after SELECT eliminates duplicate rows from the result set.

```mysql
> SELECT DISTINCT name, price FROM car;
+---------+----------+
| name    | price    |
+---------+----------+
| Audi A1 | 15000.00 |
| Audi A1 | 40000.00 |
| Audi A1 | 20000.00 |
+---------+----------+
3 rows in set (0.00 sec)
```

# Select all columns (*)

```mysql
> SELECT * FROM car;
+--------+---------+----------+
| car_id | name    | price    |
+--------+---------+----------+
|      1 | Audi A1 | 15000.00 |
|      2 | Audi A1 | 40000.00 |
|      3 | Audi A1 | 20000.00 |
|      4 | Audi A1 | 20000.00 |
+--------+---------+----------+
4 rows in set (0.00 sec)
```

> **Best Practice** Do not use * unless you are debugging or fetching the row(s) into associative arrays, otherwise
> schema changes (ADD/DROP/rearrange columns) can lead to nasty application errors. Also, if you give the list of columns you need in your result set, MySQL query planner often can optimise the query.

**Pros:**

1. When you add/remove columns, you don't have to make changes where you did use SELECT *
2. It's shorter to write
3. You also see the answers, so can SELECT * -usage ever be justified?

**Cons:**

1. You are returning more data than you need. Say you add a VARBINARY column that contains 200k per row.
    You only need this data in one place for a single record - using SELECT * you can end up returning 2MB per
    10 rows that you don't need
2. Explicit about what data is used
3. Specifying columns means you get an error when a column is removed
4. The query processor has to do some more work - figuring out what columns exist on the table (thanks
    @vinodadhikary)
5. You can find where a column is used more easily
6. You get all columns in joins if you use SELECT *
7. You can't safely use ordinal referencing (though using ordinal references for columns is bad practice in itself)
8. In complex queries with TEXT fields, the query may be slowed down by less-optimal temp table processing

# WHERE

**Syntax**

`SELECT * FROM table_name WHARE condition`;

```MYSQL
SELECT Name from country where Continent = "Asia"
SELECT Name from country where LifeExpectancy > 60;
```

| Operator | Description                                      |
| :------: | ------------------------------------------------ |
|    =     | Equal                                            |
|    >     | Greater than                                     |
|    <     | Less than                                        |
|    >=    | Greater than or equal                            |
|    <=    | Less than or equal                               |
| <> or != | Not equal                                        |
| BETWEEN  | Between a certain range                          |
|   LIKE   | Search for a pattern                             |
|    IN    | To specify multiple possible values for a column |

# LIKE

The `LIKE` operator is used in a ` WHERE` clause to search for a specified pattern in a column.

There are two wildcards often used in conjunction with the ` LIKE` operator:

-  The percent sign (%) represents zero, one, or multiple characters
-  The underscore sign (_) represents one, single character

| LIKE Operator          | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| WHERE Name LIKE 'a%'   | Finds any values that start with "a"                         |
| WHERE Name LIKE '%a'   | Finds any values that end with "a"                           |
| WHERE Name LIKE '%or%' | Finds any values that have "or" in any position              |
| WHERE Name LIKE '_r%'  | Finds any values that have "r" in the second position        |
| WHERE Name LIKE 'a_%'  | Finds any values that start with "a" and are at least 2 characters in length |
| WHERE Name LIKE 'a__%' | Finds any values that start with "a" and are at least 3 characters in length |
| WHERE Name LIKE 'a%o'  | Finds any values that start with "a" and ends with "o"       |

**Example**

```mysql
 -- Select data where country name Start with In
 > SELECT Name FROM country WHERE Name LIKE "In%";
+-----------+
| Name      |
+-----------+
| Indonesia |
| India     |
+-----------+
2 rows in set (0.00 sec)
```

# AND, OR and NOT Operator

**The SQL AND, OR and NOT Operators**

The `WHERE` clause can be combined with ` AND`, `OR`, and ` NOT` operators.

The `AND` and `OR` operators are used to filter records based on more than one  condition:

- The `AND` operator displays a record if all the conditions separated by `  AND`   are TRUE.
- The `OR` operator displays a record if any of the conditions separated by `  OR` is TRUE.

The `NOT` operator displays a record if the condition(s) is NOT TRUE.

```mysql
-- AND Operator 
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...; 


-- Or Operator
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...; 

-- Not Operator
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition; 
```

# CASE or IF

```mysql
SELECT Name, District,
CASE WHEN Population >= 1000000 THEN 'Above 10L' ELSE 'Below 10L' END AS `Population Above 10L` FROM indian_city ;

-- OR
SELECT Name,
District,
IF(Populcation >= 1000000, 'Above 10L', 'Below 10L') AS `Population Above 10L`
FROM indian_city ;

+--------------------------------+-------------------+----------------------+
| Name                           | District          | Population Above 10L |
+--------------------------------+-------------------+----------------------+
| Mumbai (Bombay)                | Maharashtra       | Above 10L            |
| Delhi                          | Delhi             | Above 10L            |
| Calcutta [Kolkata]             | West Bengali      | Above 10L            |
| Raigarh                        | Chhatisgarh       | Below 10L            |
| Vejalpur                       | Gujarat           | Below 10L            |
+--------------------------------+-------------------+----------------------+

```

# AS

SQL aliases are used to temporarily rename a table or a column. They are generally used to improve readability.

```mysql
> desc indian_city;
+-------------+----------+------+-----+---------+-------+
| Field       | Type     | Null | Key | Default | Extra |
+-------------+----------+------+-----+---------+-------+
| ID          | int      | NO   |     | 0       |       |
| Name        | char(35) | NO   |     |         |       |
| CountryCode | char(3)  | NO   |     |         |       |
| District    | char(20) | NO   |     |         |       |
| Population  | int      | NO   |     | 0       |       |
+-------------+----------+------+-----+---------+-------+
5 rows in set (0.01 sec)


>SELECT Name, District as State from indian_city;
+--------------------------------+-------------------+
| Name                           | State             |
+--------------------------------+-------------------+
| Mumbai (Bombay)                | Maharashtra       |
| Delhi                          | Delhi             |
| Calcutta [Kolkata]             | West Bengali      |
| Raigarh                        | Chhatisgarh       |
| Vejalpur                       | Gujarat           |
+--------------------------------+-------------------+


```

# LIMIT clause

MySQL provides a LIMIT clause that is used to specify the number of records  to return.

```mysql
> SELECT * FROM indian_city LIMIT 3;
+------+--------------------+-------------+--------------+------------+
| ID   | Name               | CountryCode | District     | Population |
+------+--------------------+-------------+--------------+------------+
| 1024 | Mumbai (Bombay)    | IND         | Maharashtra  |   10500000 |
| 1025 | Delhi              | IND         | Delhi        |    7206704 |
| 1026 | Calcutta [Kolkata] | IND         | West Bengali |    4399819 |
+------+--------------------+-------------+--------------+------------+
-- First three records

> SELECT * FROM indian_city ORDER BY Population ASC  LIMIT 3;
+------+----------+-------------+-------------+------------+
| ID   | Name     | CountryCode | District    | Population |
+------+----------+-------------+-------------+------------+
| 1364 | Vejalpur | IND         | Gujarat     |      89053 |
| 1363 | Raigarh  | IND         | Chhatisgarh |      89166 |
| 1362 | Morvi    | IND         | Gujarat     |      90357 |
+------+----------+-------------+-------------+------------+
3 rows in set (0.00 sec)
-- First three orderd by ascending population
```

> **Best Practice** Always use ORDER BY when using LIMIT ; otherwise the rows you will get will be unpredictable.

**With Offset**

`SELECT * FROM table_name LIMIT [offset], row_count`;

- The `offset` specifies the offset of the first row to return. The `offset` of the first row is 0, not 1.
- The `row_count` specifies the maximum number of rows to return.

```mysql 
 SELECT * FROM indian_city ORDER BY Population ASC  LIMIT 10, 3;
+------+------------+-------------+----------------+------------+
| ID   | Name       | CountryCode | District       | Population |
+------+------------+-------------+----------------+------------+
| 1354 | Nagaon     | IND         | Assam          |      93350 |
| 1353 | Bansberia  | IND         | West Bengali   |      93447 |
| 1352 | Chhindwara | IND         | Madhya Pradesh |      93731 |
+------+------------+-------------+----------------+------------+
3 rows in set (0.00 sec)
-- 3 row from 10th row
```

# BETWEEN

You can use BETWEEN clause to replace a combination of "greater than equal AND less than equal" conditions.

```mysql
-- Query with operators
> SELECT * FROM indian_city WHERE Population >= 100000 and Population <= 1000000;
-- Similar query with BETWEEN
> SELECT * FROM indian_city WHERE Population BETWEEN 100000 AND 1000000;

+------+--------------------------------+-------------+-------------------+------------+
| ID   | Name                           | CountryCode | District          | Population |
+------+--------------------------------+-------------+-------------------+------------+
| 1042 | Madurai                        | IND         | Tamil Nadu        |     977856 |
| 1043 | Haora (Howrah)                 | IND         | West Bengali      |     950435 |
| 1332 | Kanchrapara                    | IND         | West Bengali      |     100194 |
| 1333 | Tonk                           | IND         | Rajasthan         |     100079 |
+------+--------------------------------+-------------+-------------------+------------+

```

# Order By

The `ORDER BY` keyword is used to sort the result-set in ascending or  descending order.

The `ORDER BY` keyword sorts the records in ascending order by default. To sort the records in descending order, use the ` DESC` keyword.

**Syntax**

`SELECT column1,column2, .. FROM table_name ORDER BY column1, column2, .. ASC|DESC; `

```mysql
> SELECT * FROM indian_city ORDER BY Population;
+------+--------------------------------+-------------+-------------------+------------+
| ID   | Name                           | CountryCode | District          | Population |
+------+--------------------------------+-------------+-------------------+------------+
| 1364 | Vejalpur                       | IND         | Gujarat           |      89053 |
| 1363 | Raigarh                        | IND         | Chhatisgarh       |      89166 |
| 1362 | Morvi                          | IND         | Gujarat           |      90357 |
+------+--------------------------------+-------------+-------------------+------------+

> SELECT * FROM indian_city ORDER BY Population DESC;
+------+--------------------------------+-------------+-------------------+------------+
| ID   | Name                           | CountryCode | District          | Population |
+------+--------------------------------+-------------+-------------------+------------+
| 1024 | Mumbai (Bombay)                | IND         | Maharashtra       |   10500000 |
| 1025 | Delhi                          | IND         | Delhi             |    7206704 |
| 1026 | Calcutta [Kolkata]             | IND         | West Bengali      |    4399819 |
+------+--------------------------------+-------------+-------------------+------------+
```

The following SQL statement selects all cities from the "indian_city" table,  sorted ascending by the "District" and descending by the "Population" column:

```mysql
> SELECT * FROM indian_city ORDER BY District, Population;
+------+--------------------------------+-------------+-------------------+------------+
| ID   | Name                           | CountryCode | District          | Population |
+------+--------------------------------+-------------+-------------------+------------+
| 1354 | Nagaon                         | IND         | Assam             |      93350 |
| 1284 | Silchar                        | IND         | Assam             |     115483 |
| 1276 | Dibrugarh                      | IND         | Assam             |     120127 |
| 1064 | Guwahati (Gauhati)             | IND         | Assam             |     584342 |
| 1357 | Bettiah                        | IND         | Bihar             |      92583 |
| 1350 | Dehri                          | IND         | Bihar             |      94526 |
| 1337 | Sasaram                        | IND         | Bihar             |      98220 |
| 1230 | Chapra                         | IND         | Bihar             |     136877 |
| 1207 | Munger (Monghyr)               | IND         | Bihar             |     150112 |
+------+--------------------------------+-------------+-------------------+------------+

> SELECT * FROM indian_city ORDER BY District ASC, Population DESC;
```

# IN Clause

The `IN` operator allows you to specify multiple values in a ` WHERE` clause.

The `IN` operator is a shorthand for multiple ` OR` conditions.

```mysql
SELECT column_name(s) FROM table_name WHERE column_name IN (value1, value2, ...); 

-- Or
SELECT column_name(s) FROM table_name WHERE column_name IN (SELECT STATEMENT); 
```

**Example**

```mysql
SELECT * FROM indian_city WHERE District IN ("Bihar", "Gujarat", "Assam");

+------+--------------------+-------------+----------+------------+
| ID   | Name               | CountryCode | District | Population |
+------+--------------------+-------------+----------+------------+
| 1029 | Ahmedabad          | IND         | Gujarat  |    2876710 |
| 1035 | Surat              | IND         | Gujarat  |    1498817 |
| 1040 | Vadodara (Baroda)  | IND         | Gujarat  |    1031346 |
| 1152 | Bihar Sharif       | IND         | Bihar    |     201323 |
| 1185 | Nadiad             | IND         | Gujarat  |     167051 |
| 1354 | Nagaon             | IND         | Assam    |      93350 |
| 1357 | Bettiah            | IND         | Bihar    |      92583 |
| 1362 | Morvi              | IND         | Gujarat  |      90357 |
| 1364 | Vejalpur           | IND         | Gujarat  |      89053 |
+------+--------------------+-------------+----------+------------+

```

# Group By

The `GROUP BY` statement groups rows that have the same values into summary  rows, like "find the number of customers in each country".

The `GROUP BY` statement is often used with aggregate functions (`COUNT()`,  `MAX()`,  `MIN()`, `SUM()`,  `AVG()`) to group the result-set by one or more columns.

# HAVING Clause

# Set Operations

Set operations are supported by SQL to be performed on table data. for these operations special operators knows as Set Operators are used to join the result of multiple `SELECT` Statements.

These operators help to get meaningful results from data, under different specific conditions.  Quries which contain set operators are called **compound queries**.

## Union

The union operator return all distinct rows selected by either query.

```mysql
-- Syntax 
SELECT col_name(n) from table_1 UNION SELECT col_name(n) FROM table_2

-- Example
SELECT * FROM indian_city UNION SELECT * FROM usa_city;
+------+--------------------------------+-------------+----------------------+------------+
| ID   | Name                           | CountryCode | District             | Population |
+------+--------------------------------+-------------+----------------------+------------+
| 1024 | Mumbai (Bombay)                | IND         | Maharashtra          |   10500000 |
| 1025 | Delhi                          | IND         | Delhi                |    7206704 |
| 1026 | Calcutta [Kolkata]             | IND         | West Bengali         |    4399819 |
| 4061 | Fall River                     | USA         | Massachusetts        |      90555 |
| 4062 | Kenosha                        | USA         | Wisconsin            |      89447 |
| 4063 | Elgin                          | USA         | Illinois             |      89408 |
| 4064 | Odessa                         | USA         | Texas                |      89293 |
| 4065 | Carson                         | USA         | California           |      89089 |
| 4066 | Charleston                     | USA         | South Carolina       |      89063 |
+------+--------------------------------+-------------+----------------------+------------+

```

## Union All

The Union All operator returns all rows selected by either query including duplicates.

```mysql
SELECT * FROM table_1 UNION ALL SELECT * FROM table_2;
```

## Intersect

The intersect operator returns only those rows which are common to both the queries.

```mysql
SELECT column_name(n) FROM table_1 INTERSECT SELECT column_name(n) FROM table_2;
```

## Minus

Minus operator display the rows which are present in the first query but absent in the second query with no duplicates and data is arranged in ascending order by default.

```mysql
SELECT column_name(n) FROM table_1 MINUS SELECT column_name(n) FROM table_2;
```

# Joins

A `JOIN` clause is used to combine rows from two or more tables, based on  a related column between them.

## Inner Join

The `INNER JOIN` keyword selects records that have matching values in both tables.

```mysql
-- Syntax
SELECT column_name(s) FROM table1 INNER JOIN table2 ON table1.column_name = table2.column_name;

-- Don't use this because columns will repated
SELECT * FROM city INNER JOIN indian_city ON city.ID = indian_city.ID;

ID,Name,CountryCode,District,Population,ID,Name,CountryCode,District,Population

SELECT city.Name, city.Population  FROM city INNER JOIN indian_city ON city.ID = indian_city.ID;
--------------------------------+------------+
| Name                           | Population |
+--------------------------------+------------+
| Mumbai (Bombay)                |   10500000 |
| Delhi                          |    7206704 |
| Calcutta [Kolkata]             |    4399819 |
| Chennai (Madras)               |    3841396 |
| Hyderabad                      |    2964638 |
| Ahmedabad                      |    2876710 |
+--------------------------------+------------+
```

## Left Join

The `LEFT JOIN` keyword returns all records  from the left table (table1), and the matching records (if any) from the right table (table2). 

```mysql
SELECT column_name(s) FROM table1 LEFT JOIN table2 ON table1.column_name = table2.column_name; 

SELECT column_name(s) FROM table1 LEFT OUTER JOIN table2 ON table1.column_name = table2.column_name; 
```

## Right Join

The `RIGHT JOIN` keyword returns all records from the right table (table2), and the  matching records (if any) from the left table (table1).

```mysql
SELECT column_name(s) FROM table1 RIGHT JOIN table2 ON table1.column_name = table2.column_name; 

SELECT column_name(s) FROM table1 RIGHT OUTER JOIN table2 ON table1.column_name = table2.column_name; 
```

## Cross Join

The `CROSS JOIN` keyword returns all records  from both tables (table1 and table2).

```mysql
SELECT column_name(s) FROM table1 FULL OUTER JOIN table1 ON table1.column_name = table2.column_name; 

SELECT column_name(s) FROM table1 CROSS JOIN table2;

```

> **Note:** `CROSS JOIN` can potentially return very large  result-sets!

> **Note:** The `CROSS JOIN` keyword returns all matching  records from both tables whether the other table matches or not.

## Self Join

A self join is a regular join, but the table is joined with itself.

```mysql
SELECT column_name(s) FROM table1 T1, table1 T2 WHERE condition;
```

