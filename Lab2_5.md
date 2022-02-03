 ```sql
 create table student (SID int primary key,Name varchar(10),Major
 varchar(4)); 
 CREATE TABLE enrolled (SID int,cid varchar(5),primary key
 (sid,cid)); 
 insert into student values (40,'alex','cs'), (41, 'john','cs'), (42, 'mat', 'cs'), (43, 'zack', 'math');
 insert into Enrolled values (40,'cs101'),(40,  'cs102'),(41, 'cs101'),(41,  'cs404'),(41,'cs405'), (42,'cs101'),(42,'cs102'),(42,'cs203'),(43,'cs101'),(43,'cs102');
```

Find students who are taking all courses taken by student 40

<details>
  <summary>Solution</summary>

```sql
SELECT DISTINCT NAME FROM STUDENT S WHERE major='cs’ AND NOT EXISTS
 (SELECT * FROM ENROLLED E2	WHERE SID=40 and CID NOT IN (SELECT CID
 FROM ENROLLED E WHERE E.SID=S.SID) )
```
  
```sql
  SELECT DISTINCT NAME FROM STUDENT S WHERE major=‘cs’ AND  NOT EXISTS
     (SELECT * FROM ENROLLED E2  WHERE SID=40 AND NOT EXISTS ( SELECT *
     FROM ENROLLED E WHERE E.SID=S.SID AND E2.CID=E.CID) )
```
</details>
