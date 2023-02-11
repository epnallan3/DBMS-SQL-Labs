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
