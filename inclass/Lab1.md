If MYSQL does not work, you can use  [SQLFiddle](http://sqlfiddle.com/)

Step 0: Create Database
-----------------------

To create the database, write down `CREATE DATABASE Test;`  
This would create the database.  
To connect to the database. `USE Test;`

Step 1: Creating Relational Schema
----------------------------------

1.  Remove the student table, if exists.

```sql
DROP TABLE IF EXISTS Students;
```

2.  Removing the enrolled table, if exists
```sql
DROP TABLE IF EXISTS Enrolled;
```

3.  Create the `Enrolled` and `Students` table, only if they do not exist.

```sql
   CREATE TABLE IF NOT EXISTS Students (
  		sid char(10),
  	    name char(20),
  	    gpa float,
  	    PRIMARY KEY (sid)
  );
  CREATE TABLE IF NOT EXISTS Enrolled (
  sid char(10),
  cid char(10),
  grade char(2),
  PRIMARY KEY (sid,cid),
  FOREIGN KEY (sid) references students(sid)
  );
  ```

**_NOTE:_**  
You need to create the table `Students` before table `Enrolled`. Why?

4.  Insert data into tables

```sql
INSERT INTO Students VALUES('s1', 'student1', 3.1);
INSERT INTO Students VALUES('s2', 'student2', 3.2);
INSERT INTO Students VALUES('s3', 'student3', 2.2);
INSERT INTO Students VALUES('s4', 'student4', NULL);

INSERT INTO Enrolled VALUES('s1','cs101', 'A+');
INSERT INTO Enrolled VALUES('s1','cs102', 'A-');
INSERT INTO Enrolled VALUES('s1','cs103', 'B');
INSERT INTO Enrolled VALUES('s1','cs104', 'B');
INSERT INTO Enrolled VALUES('s2','cs101', 'A+');
INSERT INTO Enrolled VALUES('s2','cs103', 'A');
INSERT INTO Enrolled VALUES('s3','cs101', 'A-');
INSERT INTO Enrolled VALUES('s3','cs102', 'C');
INSERT INTO Enrolled VALUES('s3','cs105', 'B');
```

**_NOTE:_**  
Due to the foreign key constraint, students needed to be INSERTed before INSERTing the enrollment in the the Enrolled table.

5.  Try to run the following command

 INSERT INTO Enrolled VALUES('s5','cs105', 'B');

This should not work, because there is no students with sid of ‘s5’

6.  Make sure to inspect data in the tables.
```sql
SELECT * FROM Students;
```
&
```sql
SELECT * FROM Enrolled;
```
7.  Try to delete student with sid=‘s3’
```sql
DELETE  FROM Students WHERE sid='s3';
```
The command could not be executed, due to the foreign key constraint. Mysql reports the following error:

>     Error Code: 1451. Cannot delete or update a parent row: a foreign key constraint fails (`test`.`enrolled`, CONSTRAINT `enrolled_ibfk_1`  FOREIGN KEY (`sid`) REFERENCES `students` (`sid`))
>     

8.  However, you can delete student `s4` as there are no records (in the enrolled table).

```sql
DELETE  FROM Students WHERE sid='s4';
```

9.  To Delete student with sid ‘s3’, first we delete his(her) enrolled courses
```sql
DELETE  FROM Enrolled WHERE sid='s3';
```

Then, you can delete the student
```
DELETE  FROM Students WHERE sid='s3';
```

10.  Inspect data in the tables.
```
SELECT * FROM Students;
```

&
```
SELECT * FROM Enrolled;
```

**_NOTE:_**  
We have tested the default behavior for foreign keys.

11.  We will create a foreign key with cascades:
```sql
 DROP TABLE IF EXISTS Enrolled ;
 DROP TABLE IF EXISTS Students;
 CREATE TABLE IF NOT EXISTS Students (
    		sid char(10),
		    name char(20),
		    gpa float,
		    PRIMARY KEY (sid)
    );
    CREATE TABLE IF NOT EXISTS Enrolled (
		    sid char(10),
		    cid char(10),
		    grade char(2),
		    PRIMARY KEY (sid,cid),
    FOREIGN KEY (sid) references students(sid) ON DELETE cascade
    );
  
	INSERT INTO Students VALUES('s1', 'student1', 3.1);
	INSERT INTO Students VALUES('s2', 'student2', 3.2);
	INSERT INTO Students VALUES('s3', 'student3', 2.2);
	INSERT INTO Students VALUES('s4', 'student4', NULL);
	
	INSERT INTO Enrolled VALUES('s1','cs101', 'A+');
	INSERT INTO Enrolled VALUES('s1','cs102', 'A-');
	INSERT INTO Enrolled VALUES('s1','cs103', 'B');
	INSERT INTO Enrolled VALUES('s1','cs104', 'B');
	INSERT INTO Enrolled VALUES('s2','cs101', 'A+');
	INSERT INTO Enrolled VALUES('s2','cs103', 'A');
	INSERT INTO Enrolled VALUES('s3','cs101', 'A-');
	INSERT INTO Enrolled VALUES('s3','cs102', 'C');
	INSERT INTO Enrolled VALUES('s3','cs105', 'B');
```

12.  Inspect the tables:
```sql
SELECT * FROM Students;
```
&
```
SELECT * FROM Enrolled;
```
13.  However, because we are using cascade in the foreign key, we can delete student with sid=`s3`.
```
DELETE  FROM Students WHERE sid='s3';
```
14.  Inspect the tables:
```
SELECT * FROM Students;
```
&
```
SELECT * FROM Enrolled;
```




## Lecture Example


```sql
CREATE  database Lecture3;
USE Lecture3; 

create table product(
       pname        varchar(20) primary key, -- name of the product
       price        decimal(10,2),               -- price of the product
       category     varchar(20),             -- category
       manufacturer varchar(20) NOT NULL     -- manufacturer
);
insert into product values('Gizmo', 19.99, 'Gadgets', 'GizmoWorks');    
insert into product values('PowerGizmo', 29.99, 'Gadgets', 'GizmoWorks');  
insert into product values('MultiTouch', 203.99, 'Household', 'Hitachi'); 
insert into product values('SingleTouch', 149.99, 'Photography', 'Canon')

```

### Exercise 1
Try writing a query to get an output table of all the products with "Touch" in the name, showing just their name and price, 
and sorted alphabetically by manufacturer.


Next, write a query that returns the distinct names of manufacturers that make products with "Gizmo" in its name



### Exercise 2

Try some of these queries but first guess what they return.

```sql
SELECT DISTINCT category FROM product ORDER BY category;
SELECT category FROM product ORDER BY pname;
SELECT DISTINCT category FROM product ORDER BY pname;
```

### Multi Table join

```sql
create table company (
    cname varchar(20) primary key, -- company name uniquely identifies the company.
    stockprice decimal(10,2), -- stock price is in money 
    country varchar(10)); -- country is just a string
insert into company values ('GizmoWorks', 25.0, 'USA');
insert into company values ('Canon', 65.0, 'Japan');
insert into company values ('Hitachi', 15.0, 'Japan');
```

## Another Example
```sql
drop table if exists product; 
drop table if exists company;

create table company (
    cname varchar(20) primary key, -- company name uniquely identifies the company.
    stockprice decimal(10,2), -- stock price is in money 
    country varchar(10)); -- country is just a string
insert into company values ('ToyWorks', 25.0, 'USA');
insert into company values ('ToyFriends', 65.0, 'China');
insert into company values ('ToyCo', 15.0, 'China');

create table product(
       pname varchar(10), -- name of the product
       price decimal(10,2), -- price of the product
       category varchar(10), -- category
       manufacturer varchar(10), -- manufacturer
       primary key (pname, manufacturer),
       foreign key (manufacturer) references company(cname));
insert into product values('Pikachu', 19.99, 'Toy', 'ToyWorks');
insert into product values('Pikachu', 19.99, 'Toy', 'ToyFriends');
insert into product values('Pokeball', 29.99, 'Electronic', 'ToyCo');
insert into product values('Bulbasaur', 149.99, 'Toy', 'ToyFriends');
insert into product values('Charizard', 203.99, 'Toy', 'ToyCo');
insert into product values('PokeCamera', 19.99, 'Electronic', 'ToyWorks');
```

Our goal is to answer the following question:
Find all categories of products that are made by Chinese companies

## In Class Exerices


```sql
DROP TABLE IF EXISTS Movies;
CREATE TABLE Movies(title VARCHAR(50), year INT, director VARCHAR(50), length INT);
INSERT INTO Movies VALUES('Database Wars', 1967, 'John Joe', 123);
INSERT INTO Movies VALUES('The Databaser', 1992, 'John Bob', 190);
INSERT INTO Movies VALUES('Database Wars', 1998, 'John Jim', 176);
```

## Exercise
Can you write the movie query from lecture as a single SFW query?

Recall that we are trying to find all movie titles that were used for more than one movie, and our schema for the movies table is:
```
title VARCHAR(50)
year INT
director VARCHAR(50)
length INT
```
Let's try to write the nested query that solves this from lecture:

use ANY to write it.


use join to write it.



# Another example

```sql
DROP TABLE IF EXISTS Movies;
CREATE TABLE Movies(title VARCHAR(50), year INT, director VARCHAR(50), length INT);
INSERT INTO Movies VALUES('Database Wars', 1967, 'John Joe', 123);
INSERT INTO Movies VALUES('The Databaser', 1992, 'John Bob', 190);
INSERT INTO Movies VALUES('Database Wars', 1998, 'John Jim', 176);
```

## Exercise #1
Can you write the movie query from lecture as a single SFW query?

Recall that we are trying to find all movie titles that were used for more than one movie, and our schema for the movies table is:

title STRING
year INT
director STRING
length INT

Let's try to write the nested query that solves this from lecture:

use ANY to write it.


use join to write it.



you need to create a database to do that
```sql
create database test;
```
test is the name of the database

then to use that database

```sql
use test; 
```
Please note that you need to create the database only once.
```sql
create table product(
       pname        varchar(20) primary key, -- name of the product
       price        decimal(10,2),               -- price of the product
       category     varchar(20),             -- category
       manufacturer varchar(20) NOT NULL     -- manufacturer
);
insert into product values('Gizmo', 19.99, 'Gadgets', 'GizmoWorks');    X
insert into product values('PowerGizmo', 29.99, 'Gadgets', 'GizmoWorks');  X
insert into product values('MultiTouch', 203.99, 'Household', 'Hitachi'); 
insert into product values('SingleTouch', 149.99, 'Photography', 'Canon');
```


## Exercise #1
Try writing a query to get an output table of all the products with "Touch" in the name, showing just their name and price, and sorted alphabetically by manufacturer.

Let's look at the products first:


Write your query here:

Next, write a query that returns the distinct names of manufacturers that make products with "Gizmo" in the name:



## Exercise #2

Try some of these queries but first guess what they return.
```sql
SELECT DISTINCT category FROM product ORDER BY category;
SELECT category FROM product ORDER BY pname;
SELECT DISTINCT category FROM product ORDER BY pname;
```

## Exercise # 3
```sql
drop table if exists product; -- This needs to be dropped if exists, see why further down!
drop table if exists company;

create table company (
    cname varchar(20) primary key, -- company name uniquely identifies the company.
    stockprice decimal(10,2), -- stock price is in money 
    country varchar(10)); -- country is just a string
insert into company values ('ToyWorks', 25.0, 'USA');
insert into company values ('ToyFriends', 65.0, 'China');
insert into company values ('ToyCo', 15.0, 'China');

create table product(
       pname varchar(10), -- name of the product
       price decimal(10,2), -- price of the product
       category varchar(10), -- category
       manufacturer varchar(10), -- manufacturer
       primary key (pname, manufacturer),
       foreign key (manufacturer) references company(cname));
insert into product values('Pikachu', 19.99, 'Toy', 'ToyWorks');
insert into product values('Pikachu', 19.99, 'Toy', 'ToyFriends');
insert into product values('Pokeball', 29.99, 'Electronic', 'ToyCo');
insert into product values('Bulbasaur', 149.99, 'Toy', 'ToyFriends');
insert into product values('Charizard', 203.99, 'Toy', 'ToyCo');
insert into product values('PokeCamera', 19.99, 'Electronic', 'ToyWorks');
```

Our goal is to answer the following question:
Find all categories of products that are made by Chinese companies









## References

Use these references for review and to learn some new things that you'll be needing:

  * data types:
    [https://dev.mysql.com/doc/refman/8.0/en/data-types.html]
  * Create tables:
    [https://dev.mysql.com/doc/refman/8.0/en/create-table.html]
  * Inserting data (contains more than you need right now):
    [https://mariadb.com/kb/en/mariadb/how-to-quickly-insert-data-into-mariadb/]



   

