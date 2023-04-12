In this short tutorial, we will explore how transcations work in MySQL.



Now, we would create a simple bank table;

```sql
CREATE TABLE Bank(
name varchar(10),
amnt decimal(10,2),
Constraint  AmntC check (amnt>=0) ); 
```

Add few users to the bank

```sql
insert into bank values("Bob", 40);
Insert into bank values ("Joe", 50);
```


By default, in mysql shell (or Workbench) each statement is a sperate transcation and it is auto commited (auto commit)

```sql
SHOW VARIABLES LIKE 'AUTOCOMMIT';

```

To set autocommit to false

```sql
SET AUTOCOMMIT = 0;
```
OR
```sql
SET AUTOCOMMIT = OFF;
```

To transfer 10$ from Bob to Joe

```sql
start transaction;

update bank set amnt=amnt-10 where name='Bob';

update bank set amnt=amnt+10 where name='Joe';

commit;
```

After executing the statements
You should get the following when executing 'select * from bank;'

|name|age|
|----|---|
|Bob|30|
|Joe|60|


Execute the following statemnts
```sql
start transaction;
select * from bank;
update bank set amnt=amnt+10 where name='Bob';
select * from bank;
update bank set amnt=amnt-10 where name='Joe';
select * from bank;
rollback;
```

When we are using rollback, the table is rolled back to the previous state.


Other instances (or connections would not see these updates until **commit** is executed.)


The previous transcation can results in a several pitfalls.

For eaxample, running the following statements
```sql
start transaction;
update bank set amnt=amnt-100 where name='bob';
update bank set amnt=amnt+100 where name='joe';
commit;
```

|name|age|
|----|---|
|Bob|30|
|Joe|160|

Basically 100$ has been magically appeared in Joe account.


To handle that in mysql, we need to create a procedure (so we can use IF and or execption)

```sql
DELIMITER //

CREATE PROCEDURE `TRANSFER`(  
  IN FromACCNT VARCHAR(10),
  IN ToACCNT VARCHAR(10),
 IN amount decimal(10,2))
BEGIN
    
    IF EXISTS(select * from bank where name=FromAccnt and amnt > amount) and EXISTS(select * from bank where name=ToAccnt) THEN
       start transaction;
      update bank set amnt=amnt-amount where name=FromAccnt;
        update bank set amnt=amnt+amount where name=ToAccnt;
      commit;
 
    END IF;
END//
```

TO use it 
```sql
DELIMITER ;

Call TRANSFER('Bob','Joe', 100);
```
This would not be executed.


Another Approach is by using Exception, as follows:

```sql

DELIMITER //

CREATE PROCEDURE Transfer2(
 IN FromACCNT VARCHAR(10),
  IN ToACCNT VARCHAR(10),
 IN amount decimal(10,2))
BEGIN
 DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;  -- rollback any error in the transaction
    END;

   start transaction;
   update bank set amnt=amnt-amount where name=FromAccnt;
   update bank set amnt=amnt+amount where name=ToAccnt;
   commit;
END//

DELIMITER ;
```

```sql
Call TRANSFER2('Bob','Joe', 10);
```
AND
```sql
Call TRANSFER2('Bob','Joe', 100);
```





