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
