#Window  Function and Recurisve Queries
## In Class Example

```sql
drop table seq;
create table seq(x int );
insert into seq values(1);
insert into seq values(1);
insert into seq values (1);
insert into seq values(2);
insert into seq values(2);
insert into seq values(3);
insert into seq values(3);
insert into seq values(4);
```

```sql
select v, sum(v) over ()  from seq;
```

```sql
select v, sum(v) over w  from seq
WINDOW w as ()
```

```sql
select v, sum(v) over w   from seq
WINDOW w as (RANGE BETWEEN
UNBOUNDED PRECEDING AND CURRENT ROW)
```

```sql
select *, sum(x) over w  from seq
window w as (order by x);
```

```sql
select *, sum(x) over w  from seq
window w as ( order by x range BETWEEN UNBOUNDED PRECEDING and current row);
```

```sql
select *, sum(x) over w  from seq
window w as ( Order by x  rows BETWEEN  current row and UNBOUNDED following);
```

```sql
select *, sum(x) over w  from seq
window w as ( Order by x  range BETWEEN  current row and UNBOUNDED following);
```

```sql
select *, row_number() over w from seq
window w as (   );
```

### SQL RANK() versus ROW_NUMBER()
```sql
select *, rank() over  (order by x),
 row_number() over (order by x)
 from seq;
```
## Leap vs Lag 


``` sql
drop table if exists sales;
create table sales (
product char(10),
y int,
sales decimal(10,2));

insert into sales values ('car', 2022, 10);
insert into sales values ('car', 2021, 9);
insert into sales values ('car', 2020, 8);
insert into sales values ('car', 2019, 12);
insert into sales values ('bike', 2022, 12);
insert into sales values ('bike', 2021, 10);
insert into sales values ('bike', 2020, 11);
insert into sales values ('bike', 2019, 9);
```
```sql
select * from sales cur, sales prev
where cur.product=prev.product and cur.y=prev.y+1;
```

```sql
select product, y, sales, lag(sales) over 
(partition by product order by y )   from sales;
```


## More Example
```sql
CREATE TABLE emp (
id int AUTO_increment Primary key,
ename char(50) NOT NULL,
department char(50),
salary decimal(10, 2)
);
INSERT INTO emp (ename, department, salary) VALUES
('Andy', 'Shipping', 5400),
('Betty', 'Marketing', 6300),
('Tracy', 'Shipping', 4800),
('Mike', 'Marketing', 7100),
('Sandy', 'Sales', 5400),
('James', 'Shipping', 6600),
('Carol', 'Sales', 4600);
```
```sql
SELECT COUNT(*), SUM(salary),
round(AVG(salary), 2) AS avg
FROM emp;
```
```sql
SELECT department, COUNT(*), SUM(salary),
round(AVG(salary), 2) AS avg
FROM emp
GROUP BY department
ORDER BY department;
```
```sql
SELECT department, COUNT(*), SUM(salary),
round(AVG(salary), 2) AS avg
FROM emp
GROUP BY department with Rollup
ORDER BY department;
```
```sql
SELECT ename, salary
FROM emp
ORDER BY salary DESC;
```

```sql
SELECT ename, salary, SUM(salary) OVER ()
FROM emp
ORDER BY salary DESC;
```
```sql
SELECT ename, salary,
round(salary / SUM(salary) OVER () * 100, 2) AS pct
FROM emp
ORDER BY salary DESC;
```


```sql
SELECT ename, salary,
SUM(salary) OVER (ORDER BY salary DESC ROWS BETWEEN
UNBOUNDED PRECEDING AND CURRENT ROW)
FROM emp
ORDER BY salary DESC;
```

```sql
SELECT ename, salary,
round(AVG(salary) OVER (), 2) AS avg
FROM emp
ORDER BY salary DESC;
```

```sql
SELECT ename, salary,
round(AVG(salary) OVER (), 2) AS avg,
round(salary - AVG(salary) OVER (), 2) AS diff_avg
FROM emp
ORDER BY salary DESC;
```
```sql
SELECT ename, salary,
salary - LEAD(salary, 1) OVER
(ORDER BY salary DESC) AS diff_next
FROM emp
ORDER BY salary DESC;
```


```sql
SELECT ename, salary,
salary - LAST_VALUE(salary) OVER w AS more,
round((salary - LAST_VALUE(salary) OVER w) /
LAST_VALUE(salary) OVER w * 100) AS pct_more
FROM emp
WINDOW w AS (ORDER BY salary DESC ROWS BETWEEN
UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
ORDER BY salary DESC;
```

```sql
SELECT ename, department, salary,
round(AVG(salary) OVER
(PARTITION BY department), 2) AS avg,
round(salary - AVG(salary) OVER
(PARTITION BY department), 2) AS diff_avg
FROM emp
ORDER BY department, salary DESC;
```


```sql
SELECT ename, department, salary,
round(AVG(salary) OVER d, 2) AS avg,
round(salary - AVG(salary) OVER d, 2) AS diff_avg
FROM emp
WINDOW d AS (PARTITION BY department)
ORDER BY department, salary DESC;
```


```sql
SELECT ename, department, salary,
salary - LEAD(salary, 1) OVER
(PARTITION BY department
ORDER BY salary DESC) AS diff_next
FROM emp
ORDER BY department, salary DESC;
```

```sql
SELECT ename, department, salary, RANK() OVER s AS dept_rank,
RANK() OVER (ORDER BY salary DESC) AS global_rank
FROM emp
WINDOW s AS (PARTITION BY department ORDER BY salary DESC)
ORDER BY department, salary DESC;
```

## Recursive SQL

```sql
create table parentof
(parent varchar(10),
child varchar(10));
insert into parentof values ('Alice', 'Carol');
insert into parentof values ('Bob', 'Carol');
insert into parentof values ('Carol', 'Dave');
insert into parentof values ('Carol', 'George');
insert into parentof values ('Dave', 'Mary');
insert into parentof values ('Eve', 'Mary');
insert into parentof values ('Mary', 'Frank');

WITH RECURSIVE Ancestor as (
SELECT parent as p FROM parentof WHERE child='Frank'
UNION ALL
SELECT parent from Ancestor a , parentof p
where a.p=p.child)
SELECT * from Ancestor


```

```sql
drop table if exists parentof;
create table parentof
(parent varchar(10),
child varchar(10),
Birthyear int);
insert into parentof values ('Alice', 'Carol',1945);
insert into parentof values ('Bob', 'Carol',1945);
insert into parentof values ('Carol', 'Dave',1970);
insert into parentof values ('Carol', 'George',1972);
insert into parentof values ('Dave', 'Mary',2000);
insert into parentof values ('Eve', 'Mary',2000);
insert into parentof values ('Mary', 'Frank',2020);


WITH RECURSIVE Descendant as (
SELECT CONCAT(parent ,'->', child) as lineage, 
child as c, birthyear, 0 as ParentAge FROM parentof 
WHERE parent='Alice'
UNION ALL
SELECT concat(parent,'->', child) as lineage, 
child , p.birthyear, p.birthyear- d.birthyear as ParentAge
 FROM Descendant d , parentof p
where d.c=p.parent)
SELECT * from Descendant;

```


## References

https://dev.mysql.com/doc/refman/8.0/en/window-functions-usage.html
