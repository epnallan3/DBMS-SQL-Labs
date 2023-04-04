# Interactting with Python

We will use mysql connector with python
```
pip install mysql-connector-python
```

Connect to DB

```python
import mysql.connector 
cnx=mysql.connector.connect(user='root', password='1234', host='127.0.0.1', database='dbms')
```

You may need to create a non-root account to access the DBMS (you can allow root to remotely access DB but this is not a greate idea).

To create a user 
```sql
CREATE USER 'sammy'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'sammy'@'localhost' WITH GRANT OPTION;

```

Make sure to use a database in your local dbms.

To run a query
```python
query='select * from company;'
cursor = cnx.cursor()
cursor.execute(query)
```

Print the results:
```python
for (o) in cursor:
  print(o)
```

Closing the connection
```python 
cnx.close()
```

Insert data into database
```python
iquery='insert into  company values('BMW',10,'Germany');'
cursor = cnx.cursor()
cursor.execute(iquery)
```
If you execute the following, you will see the newly inserted tuple. 
```python
query='select * from company;'
cursor = cnx.cursor()
cursor.execute(query)
for (o) in cursor:
  print(o)
```

if you reconnect to the database
You will see the updates are not committed.


To make sure the updates is commited
```python
iquery='insert into  company values('BMW',10,'Germany');commit;'
cursor = cnx.cursor()
cursor.execute(iquery)
```
Becuase we have commited the changes, it would presist.





