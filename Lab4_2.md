We will try out using mysql connector with python
```
pip install mysql-connector-python
```

Connect to DB

```python
import mysql.connector 
cnx=mysql.connector.connect(user='root', password='1234', host='127.0.0.1', database='dbms')
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