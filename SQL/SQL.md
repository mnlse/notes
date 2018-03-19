# Basics
## Format
~~~SQL
SELECT *
FROM Customers
WHERE Country='Poland';
~~~

## Basics
What is a database?
* A collection of data
* A method of accessing and manipulating data

MySQL, PostgreSQL, SQLite3 are database management systems (DBMS).

What is a computer database?
A structure of computerized data with an accessible interface.

## SQL vs MySQL
SQL - Structured Query Language

It's the language we use when we "talk" to our databases.

A quick preview:

~~~SQL
SELECT *
FROM users
WHERE Age >= 18;
~~~

MySQL - Database Management System (DBMS)
Working with MySQL is primarily writing SQL.

Once SQL is learned, it's fairly easy to switch to another DBMS (PostgreSQL or SQLite)
What makes DBMSes unique is the features they offer, not the language.

## Starting and stopping
### PostgreSQL
~~~bash
systemctl list-units | grep postgresql
systemctl stop postgresql@9.6-main
systemctl start postgresql@9.6-main

#or
su - postgres
pg_lsclusters

Ver Cluster Port Status Owner    Data directory               Log file
9.6 main    5432 online postgres /var/lib/postgresql/9.6/main /var/log/postgresql/postgresql-9.6-main.log

# pg_ctlcluster <version> <cluster> <action>

pg_ctlcluster 9.6 main stop
pg_ctlcluster 9.6 main start
~~~

### MySQL
~~~bash
mysql-ctl start
mysql-ctl stop
~~~


## Logging in
### PostgreSQL
~~~bash
su - postgres
psql
>> CREATE DATABASE testing;
>> DROP DATABASE testing;
~~~
To quit: `\q` or `ctrl+d`


### MySQL
~~~bash
mysql-ctl cli
>> CREATE DATABASE testing;
>> DROP DATABASE testing;
~~~
To quit: `exit;` or `quit;` or `\q`

## Listing databases
### PostgreSQL
~~~bash
postges=# \list
Name                      |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
--------------------------+----------+----------+-------------+-------------+-----------------------
development               | user     | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
mattnelsen                | user     | UTF8     | en_US.UTF-8 | en_US.UTF-8 |

~~~

### MySQL
~~~bash
show databases;
~~~

## Getting help
### PostgreSQL
`\h`

### MySQL
`help;`

## Creating databases
~~~SQL
CREATE DATABASE my_db;
\list
~~~
`\l = \list`

## Dropping databases
~~~SQL
DROP DATABASE my_db;
\list
~~~

## Connecting to a database
### PostgreSQL
~~~SQL
CREATE DATABASE my_db;
\connect my_db
~~~
### MySQL
~~~SQL
USE my_db
~~~

## Tables
A (relational) database is a bunch of tables.

Tables hold the columns, which hold the data.

Databases are made of many tables.

## Data types
varchar limit = 255 characters

## Creating a Table
~~~SQL
CREATE TABLE cats (
  name VARCHAR(100),
  age INT
);
~~~

## Showing tables
### MySQL
~~~SQL
SHOW TABLES;
~~~

### PostgreSQL
~~~SQL
\dt
\displaytables
~~~

## Showing columns
### MySQL
~~~SQL
SHOW COLUMNS from cats;
~~~

### PostgreSQL
~~~
\d+ cats;
~~~

## Dropping a table
~~~SQL
DROP TABLE cats;
~~~

## Inserting data
~~~SQL
INSERT INTO cats(name, age)
VALUES('Jetson', 7)

INSERT INTO cats(age, name)
VALUES('Oldie', 17)
~~~

## Showing data
~~~SQL
SELECT * FROM cats;
Jetson | 7
Oldie  | 17
~~~

## Inserting multiple rows
~~~SQL
INSERT INTO cats(name, age)
VALUES('Jetson', 7),
      ('Oldie', 17),
      ('Peanut', 4);
~~~

## Playing around
~~~SQL
CREATE TABLE people (
  first_name VARCHAR(255),
  last_name VARCHAR(255),
  age INT
  );

>> CREATE TABLE

INSERT INTO people(first_name, last_name, age)
VALUES('Tina', 'Belcher', 13);

INSERT INTO people(first_name, last_name, age)
VALUES('Bob', 'Belcher', 42),
      ('Matt', 'Nelsen', 20),
      ('Sample', 'Name', 21);
~~~

~~~SQL
INSERT INTO people(first_name, last_name, age)
VALUES('Thomas', 'Jackson', 'seventeen');
>> ERROR:  invalid input syntax for integer: "seventeen"
>> LINE 2: VALUES('Thomas', 'Jackson', 'seventeen');
                                        ^ because it's a string
~~~

## NULL and NOT NULL
A value is NULL:
~~~SQL
INSERT INTO cats(name)
VALUES('Jack');

>> INSERT 0 1

SELECT * FROM cats;

Jack |
      # ^ NULL
~~~

`NOT NULL` doesn't permit empty values. It is specified during table definition:

~~~SQL
CREATE TABLE cats2(
  name VARCHAR(255) NOT NULL,
  age INT NOT NULL
);
~~~

~~~SQL
INSERT INTO cats2(name)
VALUES('Jack');
# >> ERROR:  null value in column "age" violates not-null constraint
~~~

## Default values
~~~SQL
CREATE TABLE cats3(
  name VARCHAR(255) DEFAULT 'unnamed',
  age INT DEFAULT 0
);
~~~

## Not null and default
~~~SQL
CREATE TABLE cats4(
  name VARCHAR(255) NOT NULL DEFAULT 'unnamed',
  age INT NOT NULL DEFAULT 0
);
~~~

Isn't this redundant?
The answer: we can still manually set something to be null
~~~SQL
INSERT INTO cats3(name, age)
VALUES('Jack', NULL)

# >> INSERT 0 1

INSERT INTO cats4(name, age)
VALUES('Jack', NULL)

>> ERROR:  null value in column "age" violates not-null constraint
~~~

## Unique
### MySQL
~~~SQL
CREATE TABLE cats5(
  cat_id INT NOT NULL,
  name VARCHAR(255) DEFAULT 'no name',
  age INT,
  PRIMARY_KEY (cat_id)
  );
~~~

### PostgreSQL
~~~SQL
CREATE TABLE cats5(
  cat_id INT NOT NULL PRIMARY KEY,
  name VARCHAR(255) DEFAULT 'no name',
  age INT,
  );


INSERT INTO cats5(cat_id, name, age)
VALUES(0, 'Jack', 17),
      (1, 'Matt', 13),
      (2, 'Irene', 4);
~~~

You notice that you have to specify cat_id every time.
To fix it:

## Serial / Autoincrement
Use the `SERIAL` function in PostgreSQL / `AUTO_INCREMENT` in MySQL

### PostgreSQL
~~~SQL
CREATE TABLE cats6(
  cat_id SERIAL NOT NULL PRIMARY KEY,
  name VARCHAR(255),
  age INT
);
~~~

### MySQL
~~~SQL
CREATE TABLE cats6(
  cat_id INT NOT NULL AUTO_INCREMENT
  name VARCHAR(255),
  age INT,
  PRIMARY_KEY (cat_id)
);
~~~

## Playing around
### PostgreSQL
~~~SQL
CREATE TABLE employees(
  id SERIAL NOT NULL PRIMARY KEY,
  last_name VARCHAR(255) NOT NULL,
  first_name VARCHAR(255) NOT NULL,
  middle_name VARCHAR(255),
  age INT NOT NULL,
  current_status VARCHAR(255) NOT NULL DEFAULT 'employed'
);
~~~

# CRUD
## Read
~~~SQL
SELECT * FROM cats;

SELECT name, age FROM cats;
~~~

### Where
~~~SQL
SELECT * FROM cats WHERE age=4;

SEELCT * FROM cats WHERE name='Egg';
~~~

### Aliases
~~~SQL
SELECT name AS n FROM cats;
~~~

## Update
~~~SQL
UPDATE cats SET breed='Shorthair'
WHERE breed='Tabby';
~~~

~~~SQL
UPDATE cats set age=14
WHERE name='Misty';
~~~

## Delete
~~~SQL
DELETE FROM cats WHERE name='Egg';
~~~

To delete all rows:
~~~SQL
DELETE FROM cats;
~~~


