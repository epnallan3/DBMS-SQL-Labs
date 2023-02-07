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
select product, y, sales, lag(sales) over (partition by product order by y )   from sales;
```

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


select v, sum(v) over ()  from seq;


select v, sum(v) over w  from seq
WINDOW w as ()


select v, sum(v) over w   from seq
WINDOW w as (RANGE BETWEEN
UNBOUNDED PRECEDING AND CURRENT ROW)



select *, sum(x) over w  from seq
window w as (order by x);



select *, sum(x) over w  from seq
window w as ( order by x range BETWEEN UNBOUNDED PRECEDING and current row);




select *, sum(x) over w  from seq
window w as ( Order by x  rows BETWEEN  current row and UNBOUNDED following);


select *, sum(x) over w  from seq
window w as ( Order by x  range BETWEEN  current row and UNBOUNDED following);


select *, row_number() over w from seq
window w as (   );

```

--- adding lag and lead


https://dev.mysql.com/doc/refman/8.0/en/window-functions-usage.html
