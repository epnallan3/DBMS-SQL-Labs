# Lab 2.1


Getting Started:
### Step 1: Create the database
```sql
DROP DATABASE IF EXISTS lab2;
CREATE DATABASE lab2;
USE lab2;
```
### Step 2: Add Tables
```sql
DROP TABLE IF EXISTS Enrolled;
DROP TABLE IF EXISTS Student;
DROP TABLE IF EXISTS Course;

CREATE TABLE  Student (
    		sid int,
		    fname char(20),
		    lname char(20),
		    gpa float,
		    PRIMARY KEY (sid)
);
CREATE TABLE  Course (
	 	cid int Primary key,
  		code char(10),
  		name char(40),
  		credithours smallint
);  
CREATE TABLE  Enrolled (
    sid int,
    cid int,
    grade char(2),
    PRIMARY KEY (sid,cid)
);

ALTER TABLE Enrolled ADD FOREIGN KEY (sid) references Student(sid);
ALTER TABLE Enrolled ADD FOREIGN KEY (cid) references Course(cid);
```
### Step 3: Add data
```sql
INSERT INTO Student VALUES(1,'Amie','Massey',3);
INSERT INTO Student VALUES(2,'Abigail','Larsen',NULL);
INSERT INTO Student VALUES(3,'Cora','Rowland',4);
INSERT INTO Student VALUES(4,'Alesha','Ferrell',3.5);
INSERT INTO Student VALUES(5,'Nina','Lowery',NULL);
INSERT INTO Student VALUES(6,'Scarlet','Lane',2.6);
INSERT INTO Student VALUES(7,'Lukas','Reeves',3.6);
INSERT INTO Student VALUES(8,'Ray','Chang',3.2);
INSERT INTO Student VALUES(9,'Ioan','Reid',2.5);
INSERT INTO Student VALUES(10,'Ryan','Peck',3.1);
INSERT INTO Student VALUES(11,'Marco','Lowery',3.8);
INSERT INTO Student VALUES(12,'Pearl', 'Mayo',3.7);
INSERT INTO Student VALUES(13,'Elmer', 'Cain',2.7);
INSERT INTO Student VALUES(14,'Isla', 'Mccall',2.2);
INSERT INTO Student VALUES(15,'Kate', 'Kimmel',NULL);

INSERT INTO Course VALUES(1,'CS101', 'Computer Programming I',3);
INSERT INTO Course VALUES(2,'CS102', 'Computer Programming II',3);
INSERT INTO Course VALUES(3,'CS321', 'Data Structures',4);
INSERT INTO Course VALUES(4,'CS5710', 'Network',4);
INSERT INTO Course VALUES(5,'CS4550', 'Database',4);
INSERT INTO Course VALUES(6,'CS5990', 'Capstone Project',4);
INSERT INTO Course VALUES(7,'CS6020', 'Data Mining',4);

INSERT INTO Enrolled VALUES(1,1, 'A+');
INSERT INTO Enrolled VALUES(2,1, 'A-');
INSERT INTO Enrolled VALUES(3,1, 'B');
INSERT INTO Enrolled VALUES(4,1, 'C');
INSERT INTO Enrolled VALUES(5,1, 'D');
INSERT INTO Enrolled VALUES(6,1, 'A');
INSERT INTO Enrolled VALUES(7,1, 'B-');
INSERT INTO Enrolled VALUES(8,1, 'C+');
INSERT INTO Enrolled VALUES(9,1, 'D');
INSERT INTO Enrolled VALUES(10,1, 'C+');
INSERT INTO Enrolled VALUES(11,1, 'A-');
INSERT INTO Enrolled VALUES(12,1, 'A');
INSERT INTO Enrolled VALUES(13,1, 'B+');
INSERT INTO Enrolled VALUES(14,1, 'C+');
INSERT INTO Enrolled VALUES(15,1, 'A');
INSERT INTO Enrolled VALUES(1,2, 'A');
INSERT INTO Enrolled VALUES(2,2, 'C');
INSERT INTO Enrolled VALUES(3,2, 'B+');
INSERT INTO Enrolled VALUES(4,2, 'C-');
INSERT INTO Enrolled VALUES(5,2, 'D+');
INSERT INTO Enrolled VALUES(6,2, 'A-');
INSERT INTO Enrolled VALUES(7,2, 'C-');
INSERT INTO Enrolled VALUES(8,2, 'C');
INSERT INTO Enrolled VALUES(9,2, 'D+');
INSERT INTO Enrolled VALUES(10,2, 'C+');
INSERT INTO Enrolled VALUES(11,2, 'A-');
INSERT INTO Enrolled VALUES(12,2, 'A');
INSERT INTO Enrolled VALUES(13,2, 'B-');
INSERT INTO Enrolled VALUES(14,2, 'C');

INSERT INTO Enrolled VALUES (1,3,'C-');
INSERT INTO Enrolled VALUES (2,3,'B-');
INSERT INTO Enrolled VALUES (3,3,'D+');
INSERT INTO Enrolled VALUES (4,3,'C');
INSERT INTO Enrolled VALUES (5,3,'A+');
INSERT INTO Enrolled VALUES (6,3,'D+');
INSERT INTO Enrolled VALUES (7,3,'F');
INSERT INTO Enrolled VALUES (8,3,'A-');
INSERT INTO Enrolled VALUES (9,3,'D+');
INSERT INTO Enrolled VALUES (10,3,'D');
INSERT INTO Enrolled VALUES (11,3,'A+');
INSERT INTO Enrolled VALUES (12,3,'A');
INSERT INTO Enrolled VALUES (13,3,'B-');
INSERT INTO Enrolled VALUES (14,3,'C');
INSERT INTO Enrolled VALUES (15,3,'B-');

INSERT INTO Enrolled VALUES (1,4,'F');
INSERT INTO Enrolled VALUES (2,4,'A');
INSERT INTO Enrolled VALUES (3,4,'D-');
INSERT INTO Enrolled VALUES (4,4,'A-');
INSERT INTO Enrolled VALUES (5,4,'B-');
INSERT INTO Enrolled VALUES (6,4,'A-');
INSERT INTO Enrolled VALUES (7,4,'D+');
INSERT INTO Enrolled VALUES (8,4,'B+');
INSERT INTO Enrolled VALUES (9,4,'C+');
INSERT INTO Enrolled VALUES (10,4,'B');
INSERT INTO Enrolled VALUES (11,4,'F');
INSERT INTO Enrolled VALUES (12,4,'F');
INSERT INTO Enrolled VALUES (13,4,'A+');
INSERT INTO Enrolled VALUES (14,4,'A+');

INSERT INTO Enrolled VALUES (1,5,'A+');
INSERT INTO Enrolled VALUES (2,5,'A+');
INSERT INTO Enrolled VALUES (3,5,'B');
INSERT INTO Enrolled VALUES (4,5,'D+');
INSERT INTO Enrolled VALUES (5,5,'C-');
INSERT INTO Enrolled VALUES (6,5,'B-');
INSERT INTO Enrolled VALUES (7,5,'B');
INSERT INTO Enrolled VALUES (8,5,'B+');
INSERT INTO Enrolled VALUES (9,5,'A');
INSERT INTO Enrolled VALUES (10,5,'B');
INSERT INTO Enrolled VALUES (11,5,'D');
INSERT INTO Enrolled VALUES (12,5,'B');
INSERT INTO Enrolled VALUES (13,5,'B');
INSERT INTO Enrolled VALUES (14,5,'A+');

INSERT INTO Enrolled VALUES (1,6,'A+');
INSERT INTO Enrolled VALUES (2,6,'D');
INSERT INTO Enrolled VALUES (3,6,'D-');
INSERT INTO Enrolled VALUES (4,6,'C-');
INSERT INTO Enrolled VALUES (5,6,'C');
INSERT INTO Enrolled VALUES (6,6,'C-');
INSERT INTO Enrolled VALUES (7,6,'A-');
INSERT INTO Enrolled VALUES (8,6,'D');
INSERT INTO Enrolled VALUES (9,6,'B');
INSERT INTO Enrolled VALUES (10,6,'B-');
INSERT INTO Enrolled VALUES (11,6,'F');
INSERT INTO Enrolled VALUES (12,6,'C+');
INSERT INTO Enrolled VALUES (15,6,'F');
```
### Query I
Find all name of students that have got A or A+ in Database Course, ordered by their name.
We need all the three tables: Student for his(her) name, Enrolled for the grade and Course for database course name.
As we have three tables, we need two join conditions

<details>
  <summary>Solution</summary>
  
  ```sql
  SELECT concat(fname,' ',lname) name 
FROM Student s, Course C, Enrolled E
WHERE C.cid=E.cid 
  AND s.sid=E.sid
  AND (E.grade='A+' OR E.grade='A')
  AND C.name like '%DATABASE' 
  ORDER BY name
```
  NOTE:
Make sure to understand all parts of the solution up there.
</details>


### Query 2
Find all countries that manufacture some product in the ‘Gadgets’ category.
Assume you have the following data:

```sql
DROP TABLE IF EXISTS Company;
DROP TABLE IF EXISTS Product;

CREATE TABLE Product(
	PName char(20) Primary key,
  	Price double,
  	category char(20),
  	Manufacturer char(20)
);

CREATE TABLE Company(
	CName char(20) Primary key,
  	StockPrice double,
  	Country char(20)
);

ALTER TABLE Product ADD FOREIGN KEY (Manufacturer) references Company(CName);

INSERT INTO Company VALUES('GizmoWorks',25,'USA');
INSERT INTO Company VALUES('Canon',65,'Japan');
INSERT INTO Company VALUES('Hitachi',15, 'Japan');

INSERT INTO Product VALUES('Gizmo',19.99,'Gadgets','GizmoWorks');
INSERT INTO Product VALUES('Powergizmo',29.99,'Gadgets','GizmoWorks');
INSERT INTO Product VALUES('SingleTouch',149.99,'Photography','Canon');
INSERT INTO Product VALUES('MultiTouch',203.99,'Household','Hitachi');
SELECT Country FROM Product, Company
WHERE Manufacturer=CName AND Category='Gadgets'
```

This would return:
Country
USA
USA
How to get USA only once.


### Query 3: An Unintuitive Query
Prepare the database as follows:
```sql
DROP TABLE IF EXISTS R;
DROP TABLE IF EXISTS T;
DROP TABLE IF EXISTS S;

CREATE TABLE R(A int);
CREATE TABLE S(A int);
CREATE TABLE T(A int);

INSERT INTO R VALUES (1);
INSERT INTO R VALUES (1);
INSERT INTO R VALUES (2);
INSERT INTO R VALUES (3);
INSERT INTO R VALUES (5);
INSERT INTO R VALUES (7);
INSERT INTO R VALUES (7);
INSERT INTO R VALUES (8);

INSERT INTO S VALUES (1);
INSERT INTO S VALUES (1);
INSERT INTO S VALUES (2);
INSERT INTO S VALUES (3);

INSERT INTO T VALUES (1);
INSERT INTO T VALUES (2);
INSERT INTO T VALUES (3);
INSERT INTO T VALUES (7);
```
Run this query
```sql
SELECT  DISTINCT R.A
FROM R, S, T
WHERE R.A=S.A OR R.A=T.A
```
This would return the following
| A |
|---|
| 1 |
| 2 |
| 3 |
| 7 |

Remove all tuples from S
```sql 
SET SQL_SAFE_UPDATES = 0;
DELETE FROM S;
SET SQL_SAFE_UPDATES = 1;
```
MySQL has something called safe mode, to disable it. We run SET SQL_SAFE_UPDATES = 0; and to enable it again SET SQL_SAFE_UPDATES = 1;
Rerun the query
```sql
SELECT DISTINCT R.A
FROM   R, S, T
WHERE  R.A=S.A OR R.A=T.A
```

### Multiset Operations
MySQL does not have INTERSECT or EXCEPT keyword. In the next lecture, we will explain an alternative.
*Compare these two queries *
```sql
SELECT  R.A 
FROM   R, S 
WHERE  R.A=S.A
UNION  
SELECT R.A 
FROM   R, T  
WHERE  R.A=T.A 
```
```sql
SELECT  R.A
FROM   R, S
WHERE  R.A=S.A
UNION ALL
SELECT R.A
FROM   R, T
WHERE  R.A=T.A
```


#### Try the following SQL code
```sql
DROP TABLE IF EXISTS Product;
DROP TABLE IF EXISTS Company;

CREATE TABLE Product(
  PName char(20) Primary key,
    maker char(20),
    factory_loc char(20)
);
CREATE TABLE Company(
  CName char(20) Primary key,
    hq_city char(20)
);

ALTER TABLE Product ADD FOREIGN KEY (maker) references Company(CName);

INSERT INTO Company VALUES('X co.','Seattle');
INSERT INTO Company VALUES('Y inc.','Seattle');
INSERT INTO Company VALUES('A co.', 'Tokyo');

INSERT INTO Product VALUES('X','X co.','US');
INSERT INTO Product VALUES('A','A co.','China');
INSERT INTO Product VALUES('A+','A co.','US');
INSERT INTO Product VALUES('Y','Y inc.','China');
```
Compare the three queries below:
```sql
SELECT hq_city
FROM   Company, Product
WHERE  maker = cname 
  AND factory_loc='US'
AND hq_city in (
SELECT hq_city
FROM   Company, Product
WHERE  maker = cname
AND factory_loc='China'
);
```
```sql
SELECT maker, hq_city
FROM   Company, Product
WHERE  maker = cname 
  AND factory_loc='US'
AND (maker,hq_city) in (
SELECT maker, hq_city
FROM   Company, Product
WHERE  maker = cname
AND factory_loc='China'
);
```
```sql
SELECT DISTINCT hq_city
FROM   Company, Product
WHERE  maker = cname 
       AND cname IN (
		SELECT maker
	  	FROM   Product
	  	WHERE  factory_loc = 'US')
	 AND cname IN (
		SELECT maker
	  	FROM   Product
	  	WHERE  factory_loc = 'China');
```

### To Delete or update multiple rows
To disable safe updates or delete
```sql
SET SQL_SAFE_UPDATES=0;
```

To enable them back

```sql
SET SQL_SAFE_UPDATES=0;
```




