Run the following queries, to create the schema:
```sql
drop table if exists product;
drop table if exists supplier;

create table supplier(
id int,
sname varchar(30) not null,
primary key (id),
unique(sname)
);

create table product(
id int primary key, 
pname varchar(30) not null,
price float,
supplierid int,
foreign key (supplierid) references supplier(id)
);
```
Insert Data
```sql
insert into supplier(id, sname) values (1, 'bike Ltd');
insert into supplier(id, sname) values (2, 'bike USA');
insert into supplier(id, sname) values (3, 'bikes4all');

insert into product values (1,'girl bike', 110, 1);
insert into product values (2,'boy bike', 110, 2);
insert into product values (3,'mountain bike', 510, 2);
insert into product values (4,'bike', 180, NULL );
insert into product values (5,'ebike', 280, NULL );
```
Try to insert the following
```
insert into supplier(id, sname) values (4, 'bike Ltd');
```
The previous SQL insert should not work, as we have specified that  the supplier name to be unique.


Q1:

Find all products with unknown supplier.

A supplier of a product is recorded in product table
to find out products with unknown supplier, we need to find those that are NULL.
<details>
  <summary>Solution</summary>

```sql
  SELECT * from product where supplierid IS NULL
```  
</details>
  
Q2:
Find all suppliers with no product listed.
<details>
  <summary>Solution</summary>

  ```sql
SELECT * from Supplier s
WHERE NOT exists (select * from product p
	where p.supplierid=s.id)
``` 

</details> 

Q3:
For each product list the supplier information.

<details>
  <summary>Solution</summary>

```sql
SELECT *
FROM   supplier s, Product p
WHERE  s.id = p.supplierid
 ```
Another way to write the above query
is by using join clause, as follow:
  
```sql
select * from product p 
join supplier s 
on p.supplierid=s.id;
```
 However, running the above query, you are not returning
information about Products with no supplier information (e.g., products with id 4 and 5).
```sql
  select * from product p 
left outer join supplier s 
on p.supplierid=s.id;
```
  
 </details> 
 
 Q4:
Find supplier (along with the supplied products)

<details>
  <summary>Solution</summary>

```sql
select * from product p
right outer join supplier s 
on p.supplierid=s.id;
```
</details>
  Q5
Find supplier and products including not matched
<details>
  <summary>Solution</summary>

```sql
select * from product p 
full outer join supplier s 
on p.supplierid=s.id;
```
Mysql does not support full outer join. It is can executed as follow:
```sql
select * from product p 
left outer join supplier s 
on p.supplierid=s.id
union
select * from product p 
right outer join supplier s 
on p.supplierid=s.id;
```
  </details>
  
