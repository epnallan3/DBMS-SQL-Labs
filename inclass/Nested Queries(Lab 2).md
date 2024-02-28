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




# Lab2.2

Run the following query
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
      WHERE  factory_loc = 'China')
```
To understand the previous query, you may break it into three parts.

Q1:
```sql
    SELECT maker
    FROM   Product
      WHERE  factory_loc = 'US'
     ``` 
Q2:
```sql
SELECT maker
      FROM   Product
      WHERE  factory_loc = 'China'
```
Q3:
```sql
SELECT *
FROM   Company, Product
WHERE  maker = cname 
```

Another way:
```sql
SELECT DISTINCT hq_city
FROM   Company, (SELECT maker
      FROM   Product
      WHERE  factory_loc = 'US') US, 
      (SELECT maker
      FROM   Product
      WHERE  factory_loc = 'China') Ch
WHERE  us.maker = cname 
AND    ch.maker= cname 
```
Another one:
```sql
SELECT DISTINCT hq_city
FROM   Company, Product p1,   Product p2
WHERE  p1.maker = cname 
AND    p2.maker= cname 
AND  p1.factory_loc = 'US'
AND  p2.factory_loc = 'China';
```
### Example 

```sql
DROP table if exists company;
DROP table if exists Product;
DROP table if exists Purchase;

CREATE TABLE Company (
cname char(20) Primary key,
city char(20)
);

CREATE TABLE Product (
  pname char(20) ,
  maker char(20),
  price double,
  Foreign key (maker) REFERENCES company(cname),
  primary key (pname, maker)
);

CREATE TABLE Purchase 
(id int primary key,
 product char(20),
 buyer varchar(30),
Foreign key (product) REFERENCES Product(pname)
 );

INSERT INTO Company VALUES('X co','Seattle');
INSERT INTO Company VALUES('Y co','Jeffersonville');
INSERT INTO Company VALUES('A co','Seattle');
INSERT INTO Company VALUES('A inc','LA');
INSERT INTO Company VALUES('B co','NYC');
INSERT INTO Product VALUES('X','X co',20);
INSERT INTO Product VALUES('X+','X co',10);
INSERT INTO Product VALUES('Y','Y co',5);
INSERT INTO Product VALUES('A','A co',8);
INSERT INTO Product VALUES('T','A co',18);
INSERT INTO Product VALUES('A','A inc',5);
INSERT INTO Product VALUES('AA','A co',30);
INSERT INTO Product VALUES('AAA','A co',20);
INSERT INTO Product VALUES('B','B co',8);
INSERT INTO Purchase VALUES(1,'A','Joe Blow');
INSERT INTO Purchase VALUES(2,'A','Joe Smith');
INSERT INTO Purchase VALUES(3,'A','John Oliver');
INSERT INTO Purchase VALUES(4,'A','John Oliver');
INSERT INTO Purchase VALUES(5,'Y','Joe Blow');
INSERT INTO Purchase VALUES(6,'X','Joe Blow');
INSERT INTO Purchase VALUES(7,'A','Joe Blow');
INSERT INTO Purchase VALUES(8,'A','Joe Blow');
INSERT INTO Purchase VALUES(9,'A','Joe Blow');
```

### Query Q1
Cities where one can find companies that manufacture products bought
by "Joe Blow”.
```sql
SELECT  c.city
FROM   Company c
WHERE  c.cname  IN (
  SELECT pr.maker
      FROM   Purchase p, Product pr
      WHERE  p.product = pr.pname 
     AND p.buyer = 'Joe Blow')
```    
Note that we got Seattle twice for X co and A co

### Query Q2
```sql
SELECT c.city
 FROM   Company c, 
        Product p, 
        Purchase pu
 WHERE  c.cname = p.maker
   AND  p.pname = pu.product
   AND  pu.buyer = 'Joe Blow'
```
Compare the  previous two queries.

### Question 1
Find products that are more expensive than all those produced by “X co”
<details>
  <summary>Solution</summary>


```sql
SELECT pname
FROM   Product
WHERE  price > ALL(
  SELECT price
     FROM   Product
     WHERE  maker = 'X co')
```
  </details>
  
 ### Question 2 
Find ‘copycat’ products, i.e. products made by competitors with the same names as products made by ‘A co’

 <details>
  <summary>Solution</summary>
  
```sql
 SELECT p1.pname
FROM   Product p1
WHERE  p1.maker = 'A co'
   AND EXISTS(
  SELECT p2.pname
      FROM   Product p2
      WHERE  p2.maker <> 'A co'
     AND p1.pname = p2.pname)
```
</details>
  