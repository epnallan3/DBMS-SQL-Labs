<table>
 <tr>
   <td>
 
```sql
create table product
(id  int primary key,
name varchar(10),
price float
);

insert into product values (1,'phone',100);
insert into product values (2,'tv',200);
```

  </td>
 </tr>
</table>

<table>
<tr>
   <td colspan="2"> READ UNCOMMITTED </td>
  </tr>
  <tr>
    <td> Terminal 1 </td> <td> Terminal 2 </td> </tr>

<tr>
<td>
  
```sql 
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
BEGIN;
SELECT * FROM product WHERE id = 1; 
```
</td>
  <td></td>
</tr>

<tr>
<td> </td>
<td>
  
  ```sql

UPDATE product SET price = 1000 WHERE id = 1;
```
 
  </td>
 </tr>
  
 <tr>
    <td>

```sql
BEGIN;
SELECT  * FROM product  WHERE id = 1;
```

 </td>
 <td></td>
  </tr>
  
</table>

This would return the uncommited values;



<table>
 <tr>
   <td colspan="2"> READ COMMITTED </td>
 </tr>
  <tr>
    <td> Terminal 1 </td> <td> Terminal 2 </td> </tr>

<tr>
<td>
  
```sql 
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN;
SELECT * FROM product; 
```

</td>
  <td></td>
</tr>

<tr>
<td> </td>
<td>
  
```sql
BEGIN;
UPDATE product SET price = 1000 WHERE id = 1;
```
 
  </td>
</tr>
  
  <tr>
    <td>

```sql

SELECT  * FROM product;
```

 </td>
 <td></td>
  </tr>
  
 <tr>
   <td>
     
   </td>
   
   <td>
     
```sql
Commit;
```
 
   </td>
 </tr> 
  
 <tr>
   <td>
     
```sql
SELECT * from Product;
``` 
   </td>
   
   <td>
     

 
   </td>
 </tr>   
</table>

