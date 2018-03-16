# MySQL & PostgreSQL
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
VALUES ('Tina', 'Belcher', 13);

INSERT INTO people(first_name, last_name, age)
VALUES ('Bob', 'Belcher', 42),
       ('Matt', 'Nelsen', 20),
       ('Sample', 'Name', 21);
~~~
