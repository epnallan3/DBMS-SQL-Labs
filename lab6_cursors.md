# Lab6
## Table of Contents
- [Lab6](#lab6)
  - [Giving commands to mariaDB:](#giving-commands-to-mariadb)
  - [More Advanced Procedures](#more-advanced-procedures)
  - [CURSORS](#cursors)
  - [To DO](#to-do)

## Giving commands to mariaDB:

You know how to enter commands by hand.  You can also use the `SOURCE` command[^1]
	
From the shell (in the same directory from which you will launch the mariadb host), create the file "test.sql".  The contents should be:

```sql
SELECT * FROM inventory;
```


You can also do the same thing (from the shell) via:
```sh
mysql -u <your name> -p --host=ids [your database] < test.sql
```

## More Advanced Procedures

Stored procedures in mariaDB can be more than just a sequence of SQL commands.  There are also much more sophisticated flow control commands

<https://mariadb.com/kb/en/library/control-flow-functions/>

In addition the... *ahem* (need to change the tone of my voice).  
In addition the arguments that are passed to a procedure use an antediluvian (look it up) mechanism that was old when the world was young.  
You probably saw it when you read the material from the last lab... but just in case... here's one of the important parts taken from the MariaDB documentation:

>Each parameter is an IN parameter by default. 
>To specify otherwise for a parameter, use the keyword OUT or INOUT before the parameter name.
>		
>An **IN parameter** passes a value into a procedure. The procedure might modify the value, but the modification is not visible to the caller when the procedure returns. 
>
>An **OUT parameter** passes a value from the procedure back to the caller. Its initial value is NULL within the procedure, and its value is visible to the caller when the procedure returns. An INOUT parameter is initialized by the caller, can be modified by the procedure, and any change made by the procedure is visible to the caller when the procedure returns.
>		
>For each OUT or INOUT parameter, pass a user-defined variable in the **CALL statement** that invokes the procedure so that you can obtain its value when the procedure returns. If you are calling the procedure from within another stored procedure or function, you can also pass a routine parameter or local routine variable as an IN or INOUT parameter.


## CURSORS

To take full advantage we are going to need to understand CURSORS (yes... forbidden cursors).

Start with this link to get a sense of how they work:

<https://mariadb.com/kb/en/cursor-overview/>

When you implement the first example (and every member of your group should implement the example in their database), you will need to modify the code to use **your** database.  We do not have a sufficiently recent version of the mariadDB server to implement the second example... so don't worry about that.

It's possible that you will accidently create an infinite loop so I've enabled our usual power user.  Review this:

<https://mariadb.com/kb/en/show-processlist/>

And then this:

<https://mariadb.com/kb/en/data-manipulation-kill-connection-query/>

You can use the information contained therein to kill any run-away loops you make.

Every member of your group should do the examples 

I'm going to ask you to implement a stored procedure that used cursors. Before doing this you will need to choose two dividing values based upon the prices in the prices table.  Choose these values so that they divide your prices into 3 pieces.  A few pieces of advice:

1. All your `DECLARE`'s must occur at the beginning of the procedure
1. This is a PRIMITIVE language, and you will have to know IN ADVANCE how many columns you are going to be using in your CURSOR and have the same number of variables ready for the values (<https://mariadb.com/kb/en/fetch/> contains the syntax)
1. Do NOT make your variable names the same as your column names (trust me)

**Note:**  The assumption is that the ids in your `price` table link correctly to the ids in your `inventory` table.  If they don't-- you'll have to fix that.

<a name="cursorEx"></a>**Cursor Exercise:**

Create a stored procedure called `mypartition` that takes TWO values as input and does each of the following:

1. CREATE tables low, middle, and high  based off the structure of inventory (You may find the  `IF NOT EXISTS` clause and the `LIKE` clause useful)
1. Removes the current contents of those tables (in case they already existed)… look into `TRUNCATE`
1. Create a cursor that steps through the values in prices.  If the price is below the first value  then insert it into the low table, if the price is greater than the second value then add it to the high table, otherwise add it to the middle table.  (You may find `INSERT ... SELECT` useful) , and you will definitely find the example in the cursor-overview link (from above) useful.
	
Now create a stored procedure called `mypartition2` that takes 3 additional parameters and uses them to return the number of records stored in low, middle, and high respectively.
	
After you have that working, go through and create a  third stored procedure called `mypartition3` that does the same thing as mypartition but without using a cursor (this is actually quite a bit easier) but you don't get the fun and excitement of working with a cursor.

## To DO

- [ ] All the tutorials listed above
- [ ] Read the following links (do sample exercises when appropriate):
  - [ ] https://mariadb.com/kb/en/program-control/
  - [ ] https://mariadb.com/kb/en/cursor-overview/
  - [ ] https://mariadb.com/kb/en/show-processlist/
  - [ ] https://mariadb.com/kb/en/data-manipulation-kill-connection-query/
- [ ] Cursor example in the [mariadb cursor overview](https://mariadb.com/kb/en/cursor-overview/) done by all-- group will be lowest individual score
- [ ] Complete the following exercises:
  - [ ] The [cursor exercise](#cursorEx)
     - [ ] Procedure `mypartition`
     - [ ] Procedure `mypartition2`
     - [ ] Procedure `mypartition3`
     
