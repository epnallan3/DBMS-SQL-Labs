Purchase

| Id | Product | Price | Quantity |
|----|---------|-------|----------|
| 1  | bagel   | 1.0   | 5        |
| 2  | banana  | 0.5   | 6        |
| 3  | Juice   | 1.0   | 4        |
| 4  | donuts  | 1.5   | 8        |
| 5  | bagel   | 1.5   | 7        |
| 6  | banana  | 1.0   | 4        |
| 7  | banana  | 1.0   | 3        |
| 8  | juice   | 3.0   | 5        |

```sql

DROP TABLE IF Exists Purchase;
CREATE TABLE Purchase (
	ID int PRIMARY KEY AUTO_INCREMENT,
	Product char(20),
	Price Decimal(10,2),
	Quantity int
	);

INSERT INTO Purchase(Product, Price, Quantity) values('bagel',1.0,5),
('banana',.5,6), ('juice',1.0,4),
('donuts', 1.5,8), ('bagel', 1.5,7),
('banana',1.0,4),('banana',1,3), ('juice',3.0,5)
;
```
*NOTE:*
AUTO_INCREMENT, and how we insert into the table.
```sql
SELECT SUM(Price*Quantity) as Total_sales
FROM Purchase
WHERE product='bagel'
```
1. Executing from & WHERE

| Id | Product | Price | Quantity |
|----|---------|-------|----------|
| 1  | bagel   | 1.0   | 5        |
| 5  | bagel   | 1.5   | 7        |

2. Computing the aggregate.

| Price| X | Quantity|    
|------|---|---------|
| 1.0  | x | 5 |
| 1.5  | x | 7 |
| 15.5 |   |   |

### COUNT() vs COUNT(DISTINCT)
Run the following two queries:
```sql
SELECT COUNT(*) 
FROM Purchase;
```
```sql 
SELECT COUNT(DISTINCT product)
FROM Purchase;
```

First query returns 8 while the second returns 4.

### Group By clause
The following query would find the total number of the units sold from all products.
```sql
SELECT SUM(Quantity) 
FROM Purchase
```
The query returns 36.
To find the number of units sold for each product.
```sql
SELECT Product, SUM(Quantity) 
FROM Purchase
GROUP BY Product
```



Find products and the sales that has sold more than 10 units.
<details>
  <summary>Solution</summary>

```sql
SELECT Product, SUM(Quantity*Price)
FROM Purchase
GROUP BY Product
HAVING SUM(Quantity)> 10
	

```
|Product | SUM(Quantity*Price)|
|--------|-------|
| bagel  | 15.50 |
| banana | 10.00 |
	
  </details>
    
Find the product with the most units sold.
<details>
<summary>Solution</summary>
  

```sql

	SELECT Product, SUM(Quantity*Price)
FROM Purchase
GROUP BY Product
HAVING SUM(Quantity)= (
	SELECT MAX(t.units) 
	FROM	
	(SELECT SUM(Quantity) as units 
	 FROM Purchase
	 GROUP BY Product) t
);
```
 
</details>
  
To understand the query, let us start with:
```sql
SELECT SUM(Quantity) 
FROM Purchase
GROUP BY Product
```
```sql
SELECT MAX(t.units) 
FROM	(SELECT SUM(Quantity) as units 
		 FROM Purchase
		 GROUP BY Product) t
```
The query above finds the maximum of the total unit sold.
