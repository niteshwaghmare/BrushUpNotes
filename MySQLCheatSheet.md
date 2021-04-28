# MySQL CheatSheet

[TOC]



# Login

```shell
$ sudo mysql -u username -p 
```

# Database Operations

```mysql
-- creates a database
CREATE DATABASE database_name; 
CREATE DATABASE IF NOT EXISTS database_name; -- avoids Error 1008

-- Delete Database
DROP DATABASE IF EXISTS database_name; -- Drops a database if it exists, avoids Error 1008 
DROP DATABASE database_name; -- If database does not exist, ERROR 1008 will occur

-- See your current databases
SHOW DATABASES;

USE database_name; -- set it as the current database
```

# Data Types

| MEDIUMBLOB                                               | String (0 - 16777215)                                        |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| LONGTEXT                                                 | String (0 - 429496­7295)                                     |
| LONGBLOB                                                 | String (0 - 429496­7295)                                     |
| TINYINT x                                                | Integer (-128 to 127)                                        |
| SMALLINT x                                               | Integer (-32768 to 32767)                                    |
| MEDIUMINT x                                              | Integer (-8388608 to 8388607)                                |
| INT x                                                    | Integer (-2147­483648 to 214748­3647)                        |
| BIGINT x                                                 | Integer (-9223­372­036­854­775808 to 922337­203­685­477­5807) |
| FLOAT                                                    | Decimal (precise to 23 digits)                               |
| DOUBLE                                                   | Decimal (24 to 53 digits)                                    |
| DECIMAL                                                  | "­DOU­BLE­" stored as string                                 |
| DATE                                                     | YYYY-MM-DD                                                   |
| DATETIME                                                 | YYYY-MM-DD HH:MM:SS                                          |
| TIMESTAMP                                                | YYYYMM­DDH­HMMSS                                             |
| TIME                                                     | HH:MM:SS                                                     |
| [ENUM](http://dev.mysql.com/doc/refman/5.0/en/enum.html) | One of preset options                                        |
| [SET](http://dev.mysql.com/doc/refman/5.0/en/set.html)   | Selection of preset options                                  |

> **Note:** Integers (marked x) that are "­UNS­IGN­ED" have the same range of values but start from 0 (i.e., an UNSIGNED TINYINT can have any value from 0 to 255).

# Table Operations

## Create Table

```mysql
-- Create Normal Table
CREATE TABLE table (field1 type1, field2 type2);

-- Create Table with index
CREATE TABLE table (field1 type1, field2 type2, INDEX (field));

-- Create Table with primary key
CREATE TABLE table (field1 type1, field2 type2, PRIMARY KEY (field1));

-- Create Table with multiple primary key
CREATE TABLE table (field1 type1, field2 type2, PRIMARY KEY (field1,field2));


CREATE TABLE table1 (fk_field1 type1, field2 type2, ...,
  FOREIGN KEY (fk_field1) REFERENCES table2 (t2_fieldA))
    [ON UPDATE|ON DELETE] [CASCADE|SET NULL]

-- create table with foregin key
CREATE TABLE table1 (fk_field1 type1, fk_field2 type2, ...,
 FOREIGN KEY (fk_field1, fk_field2) REFERENCES table2 (t2_fieldA, t2_fieldB))
 
 -- Create table if not exists 
CREATE TABLE table IF NOT EXISTS;

-- Create table by cloning 
CREATE TABLE cloned_table_name LIKE table_name;
CREATE TABLE cloned_table_name SELECT * FROM table_name;

-- create a table from another table in the same database with some attributes
CREATE TABLE cloned_table_name  AS SELECT col_name_1, col_name_2 FROM table_name;

-- Create table by using where condition
CREATE TABLE cloned_table_name SELECT * FROM table_name WHERE condition;
CREATE TABLE cloned_table_name SELECT col_name, col_name2, .. WHERE condition;

```

The following constraints are commonly used in SQL:

- `NOT NULL` - Ensures that a column cannot have a NULL value
- `UNIQUE` - Ensures that all values in a column are different
- `PRIMARY KEY` - A combination of a `NOT NULL` and `UNIQUE`. Uniquely identifies each row in a table
- `FOREIGN KEY` - Prevents actions that would destroy links between tables
- `CHECK` - Ensures that the values in a column satisfies a specific condition
- `DEFAULT` - Sets a default value for a column if no value is specified
- `CREATE INDEX` - Used to create and retrieve data from the database very quickly

```mysql
-- Create Table with constraints
CREATE TABLE user(
	id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  phone VARCHAR(255),
  country VARCHAR(255) DEFAULT "India",
  age INT CHECK (age>=18),
  PRIMARY KEY (id),
  FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

## Get Table Info

```mysql
DESC table_name;
```

## Insert into Table

```mysql
-- Specify both the column names and the values to be inserted
INSERT INTO table_name (column1, column2, column3, ...) VALUES (value1, value2, value3, ...);

-- Make sure the order of the values is in the same order as the columns in the table
INSERT INTO table_name VALUES (value1, value2, value3, ...);

-- Multiple values
INSERT INTO table_name (column1, column2, column3, ...) VALUES 
(value1, value2, value3, ...),
(value1, value2, value3, ...),
(value1, value2, value3, ...)..;
```

## Select table

```mysql
SELECT * FROM table;
SELECT * FROM table1, table2;
SELECT field1, field2 FROM table1, table2;
SELECT ... FROM ... WHERE condition
SELECT ... FROM ... WHERE condition GROUPBY field;
SELECT ... FROM ... WHERE condition GROUPBY field HAVING condition2;
SELECT ... FROM ... WHERE condition ORDER BY field1, field2;
SELECT ... FROM ... WHERE condition ORDER BY field1, field2 DESC;
SELECT ... FROM ... WHERE condition LIMIT 10;
SELECT DISTINCT field1 FROM ...
SELECT DISTINCT field1, field2 FROM ...
```

## Alter table

```mysql
-- add new column
ALTER TABLE table_name ADD COLUMN field1 type1 contraints;
ALTER TABLE table_name ADD COLUMN field1 Datatype contraints, field2 type1; 

-- Drop column
ALTER TABLE table_name DROP COLUMN field1;
ALTER TABLE table_name DROP COLUMN field1, field2;

-- modify type column
ALTER TABLE table_name MODIFY COLUMN field1 type1 contraints; 

-- change type and name of column
ALTER TABLE table_name CHANGE old_field1 new_field1 type1 contrainsts; 

-- add new column after existing column 
ALTER TABLE table_name ADD COLUMN field1 type1 AFTER another_column(A);

-- add new column at first location
ALTER TABLE table_name ADD field1 type1 FIRST

-- Droping Primary key
ALTER TABLE table_name DROP PRIMARY KEY;

-- drop foreign key
ALTER TABLE table_mame DROP FOREIGN KEY FK_name;

-- Change auto-increment value
ALTER TABLE your_table_name AUTO_INCREMENT = 101;

-- Rename Table
RENAME TABLE old_table_name TO new_table_name;
ALTER TABLE old_table_name RENAME TO new_table_name;
```

## Drop/Truncate Table

```mysql
DROP TABLE table_name
TRUNCATE TABLE table_name
```

## Indexing

```mysql
-- Creating index
CREATE INDEX index_name ON table_name (column1, column2, ...);

-- Creates a unique index on a table. Duplicate values are not allowed
CREATE UNIQUE INDEX index_name ON table_name (column1, column2, ...);

-- Droping index
ALTER TABLE table_name DROP INDEX index_name;
```

# Constraints

## NOT NULL

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL,
    field3 type NOT NULL,
    field4 type
);

ALTER TABLE table_name MODIFY field1 type NOT NULL;
```

## UNIQUE

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL UNIQUE,
    field3 type NOT NULL,
    field4 type,
    UNIQUE (field1)
);

-- UNIQUE constraint on multiple columns
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL,
    field3 type,
    field4 type,
    CONSTRAINT constraint_name UNIQUE (field1, field2)
);

-- Alter 
ALTER TABLE table_name ADD UNIQUE (field_name);
ALTER TABLE table_name ADD CONSTRAINT constraint_name UNIQUE (field1, field2);
```

## PRIMARY KEY

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL UNIQUE,
    field3 type NOT NULL,
    field4 type,
    PRIMARY KEY (field_name)
);

-- PRIMARY KEY constraint on multiple columns
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL,
    field3 type,
    field4 type,
    CONSTRAINT constraint_name PRIMARY KEY (field1, field2)
);

-- Alter 
ALTER TABLE table_name ADD PRIMARY KEY (field_name);
ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY KEY (field1, field2);

ALTER TABLE table_name DROP PRIMARY KEY;
```

## FOREIGN KEY

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL UNIQUE,
    field3 type NOT NULL,
    field4 type,
    PRIMARY KEY (field_name)
    FOREIGN KEY (field_name) REFERENCES table_name2(field_from_table2)
);

-- Foreign key on multiple column with some rules 
CREATE TABLE Vineyard (
    VineyardID smallint auto_increment,
    VineyardName VARCHAR(45) NOT NULL,
    FarmerID    smallint,
    GrapeID smallint,
    ComeFrom    varchar(45) NOT NULL,
    HarvestedAmount int,
    RipenessPercent int,
    PRIMARY KEY (VineyardID),
    FOREIGN KEY (FarmerID) REFERENCES Worker(WorkerID)
        ON DELETE SET NULL
        ON UPDATE CASCADE,
    FOREIGN KEY (GrapeID) REFERENCES Grape(GrapeID)
        ON DELETE SET NULL
        ON UPDATE CASCADE
)Engine=InnoDB;
```

## CHECK

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL UNIQUE,
    field3 type NOT NULL,
    field4 type,
    PRIMARY KEY (field_name)
    FOREIGN KEY (field_name) REFERENCES table_name2(field_from_table2)
    field5 type CHECK (field5 >= condition)
);

-- CHECK constraint on multiple columns
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL,
    field3 type,
    field4 type,
    CONSTRAINT constraint_name CHECK  (condition1 AND condition2)
);

-- Alter 
ALTER TABLE table_name ADD CONSTRAINT constraint_name CHECK (conditions);

ALTER TABLE table_name DROP CHECK constraint_name;
```

## DEFAULT

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL,
    field2 type NOT NULL UNIQUE,
    field3 type NOT NULL,
    field4 type DEFAULT 'value'
);

ALTER TABLE table_name ALTER field_name SET DEFAULT 'Value';

ALTER TABLE table_name ALTER field_name DROP DEFAULT;
```

## AUTO_INCREMENT

```mysql
CREATE TABLE table_name (
    field1 type NOT NULL AUTO_INCREMENT,
    field2 type NOT NULL,
    field3 type,
);

ALTER TABLE table_name AUTO_INCREMENT=100;
```

