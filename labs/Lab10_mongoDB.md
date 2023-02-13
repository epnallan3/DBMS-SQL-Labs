# Lab10
Mongo WANTS sandwich!

## What **IS** NoSQL

As a first answer, it's easier to say what it is *not*.  NoSQL databases are not relational databases.  There are many database systems that fall under this description (such as MongoDB).  But that does not do full justice to the term... it's also a broad term that covers a philosophy/community of developers that coalesced around the need to develop highly scalable techniques to deal with the data arising from the growing world wide web.

Many SQL database systems support a standard of data reliability called [**ACID**](https://en.wikipedia.org/wiki/ACID):

* A is for **Atomic**:  a change is all or nothing-- this protects against things like update anomalies that could happen if multiple fields need to be updated at the same time.  (Proper data normalization fixes this too-- but that's not always desireable, nor feasible... besides there are more subtle inconsistencies that can still arise.)   MariadDb *can* support [**transactions**](https://mariadb.com/kb/en/mariadb/transactions/) which allow a series of SQL commands to be executed atomically.  It's all fairly self-explanatory, and you can follow the link I provided for more information.)
* C is for Consistency:  If you define rules (constraints, triggers, etc), then you want to be guaranteed that an update won't leave your system in an inconsistent state.  Also, if you have multiple servers, it may be important that all your servers contain the same information.  (more on that in a bit)
* I is for Isolated:  Performing two updates at the same time, should have the same effect as performing them serially.
* D is for Durability:  If the power goes out, or the system crashes, your data should be the same when you bring the system back online.

MariaDB allows you to specify various [`STORAGE ENGINES`](https://mariadb.com/kb/en/mariadb/storage-engines/).  Some of these are ACID compliant, others are not.  

The problem is that scaling up to larger *distributed* systems introduces some new problems.  The **CAP (or Brewer's) Theorem** (due to University of California, Berkeley computer scientist Eric Brewer) says that a distributed system can only guarantee two of the following three characteristics:

* Consistency (all nodes see the same data at the same time)
* Availability (a guarantee that every request receives a response about whether it succeeded or failed)
* Partition tolerance (the system continues to operate despite arbitrary partitioning due to network failures)

This is more subtle [than you might think](http://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed), but it was a major impetus for the develpment of NoSQL databases.

Because of CAP, most NoSQL database systems give up consistency (and thus ACID compliance) in favor of **eventual consistency**.   Which is why my father-in-law was concerned about having his Facebook comments disappear temporarily.

## Starting Mongo and some initial exploration

Today we are going to begin our exploration of mongoDB.  This database is NOT a relational database, instead it is a document-oriented database "... designed for storing, retrieving, and managing document-oriented information...".  Instead of being designed around tables it is designed around the idea of a document (thanks Wikipedia).   And when I say document you really want to be thinking JSON object.

We will use the javascript based shell.  You may start your mongoDB session with the command 
```sh
mongo
```

You may specify the database to which you wish to connect this way:
```sh
mongo database_name
```

However, this is a local connection.  You are going to want to connect to my mongo server.  Since I'm sharing an IP address that is visible to the outside world I am NOT going to make this repository public, and I hope that you don't share the address either:

```sh
 mongo 146.57.34.125:2111
```

You may see a couple of server warning messages... you can safely ignore them:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/connected.png">

## Mongo as a JavaScript Interpreter

This is a full-blown JavaScript interpreter that has a few extra goodies thrown in.  Let's play with that first by creating an object and reminding ourselves about how to interact with such things:

```js
var myobject={first: "Peter",
	last: "Dolan",
	age: 42,
	freak: true,
	currentClasses : [{name: "data management", number: "cs1251"},
				 {name: "data bases", number: "cs4453"},
				 {name: "calculus II", number: "math1102"}],
	sister: {name: "Amy", 
		residence: "Briton",
		 children: ['Luzzeta', 'Ruthie', 'Ollie']}
}
```

You don't need the tabs when you're entering-- and it's fine to use <enter> or <return> in the middle of things:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/firstObj.png">

You have just created an object with 5 properties:

*last, age, freak, currentClasses, and sister*

There are two ways to access a property:

* Dot notation
* Bracket notation

Here's an example of dot notation:

```js
myobject.age
```

And here's an example of bracket notation:
```js
myobject["age"]
```

Javascript variables or in our case, properties, contains values.  Values can be, broadly speaking, categorized by their data type.  JavaScript has seven datatypes, but I'm only going to list five of them to avoid a non-productive, lengthy explanation:

* undefined
* boolean
* number
* string
* object

Some constructions in JavaScript are special enough that you can think of them as being *subtypes*.  For example, it's best to think of arrays as subtypes of *object*.  (Interestingly, in JavaScript, it's best to think of *functions* as being subtypes of *object* too!)

In any event, it should be clear that the following holds for our example object:

* The property `.last` contains a *string*.
* The property `.age` contains a *number*.
* The property `.freak` contains a *boolean*.
* The property `.currentClasses` contains an *array of objects*.
* The property `.sister` contains an *object*.

The advantage of the bracket notation is that you can use a variable between the hard brackets `[]`.    

The hard brackets are also used to denote an array like the following:

```js
example = [1, 2, 3, "bob", false]
```
Arrays in javascript are not strongly typed and you can mix and match contents to your heart's content.  Also, arrays are 0-based.  Make certain the contents of the screen capture below makes sense:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/arrayOne.png">

We can continue stacking  these things together:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/arrayTwo.png">
	
It is also very important to notice that we can put objects inside of objects... ad infinitum:
	
<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/arrayThree.png">
	
And

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/arrayFour.png">
	
This ability to stack objects inside of objects is part of what gives mongoDB its flexibility.

# Mongo as a Database System

## The Database level

Similarly to mariaDB (and mySQL), the highest level of organization is the database.  The variable 'db' contains the current database and provides a mechanism by which to interact with it.  (the variable db is an object and so has properties and methods)

To see the current database use the command `db`.  Give it a try.

The equivalent of `SHOW DATABASES` is 

```js
show dbs
```

To change the default database (`USE <database>` in SQL-land)

```js
use <name>
```

You can use a database that does not exist.  When you insert data that database becomes real (more on that in a bit).

## Tables vs Collections

Roughly speaking, the following table translates SQL nomenclature to MongoDB:

SQL      | Mongo
---------|-------
database | database (or db)
table    | collection
row      | document

So, to repeat, the equivalent of a table in mongoDB is a *collection* and the equivalent of a row is a JSON object (called a *document* in mongoDB).  Unlike SQL there is no fixed schema hence no need for an elaborate `CREATE TABLE` command.  To create a collection you need merely insert something into it.  What you insert is a document (in other words an object).    Let's do that, and do it in such a way that it also creates a database for you to use as your own:

```js
use <your SQL database name>
db.books.insert({book: "Caves of Steel", author: "Isaac Asimov"})
```

**Note:** We will discuss creating a bit later in this lab.

The equivalent of `SHOW TABLES` is 

```js
show collections
```

Give that a try now.  You will notice that there are two collections:

* books
* system.indexes

We have a couple of... well... complications to deal with.  In SQL we did not use periods (.) in the table name.  We *could* use a construction like `<database>.<table>`, but the period was only to separate the database from the table.  Things get particularly interesting in Mongo, because JavaScript (as we were reminded above) has a dot-notation convention.  

Mongo collections are allowed (and encouraged) to use periods to suggest a hierarchy, but that is **only** a suggestion.  So, in our set of collections we have a collection named `system.indexes`.   You should interpret this as a collection, but you could also choose to view it as a subcollection of 'system'.... or not-- it's really up to you.  

### Collections contain Documents

Let's add a few more documents to our 'books' collection, but let's make *these* documents a bit different

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/bookdbOne.png">

You don't have to add things that are identical to what I've shown-- but I want you to add objects that are all books, all have some similarities in their structure, but also have some differences.

### Extracting Documents from Collections (Mongo Queries)

Okay, if the collection 'books' is the analog of a table named books, then the equivalent of `SELECT * FROM BOOKS` is:

```js
db.books.find()
```

You will notice that a *new* property named `_id` has been added to every document in the collection.  This is something that Mongodb does (it is the equivalent of a `PRIMARY KEY`).

The equivalent of the `LIMIT n` clause from SQL (things like `SELECT * FROM books LIMIT 2`) is

```js
db.books.find().limit(2)
```

There is a special method emulating the clause `LIMIT 1`:

```js
db.books.findOne()
```

The method `.find()` can take an argument-- the argument is called a query object.  The mongoDB version of

```sql
SELECT * FROM books where book="Caves of Steel"
```

is the following:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/selectOne.png">

Give it a try with your own books collection.  

The second argument to .find() is an object that controls which properties are displayed.  A value of 1 means display and  a value of 0 means do NOT display.  (this matters because "_id" is always included unless specifically disallowed.  The SQL 

```sql
SELECT book FROM books
```

can be realized as
	
<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/selectTwo.png">

You can replicate

```sql
SELECT * FROM books WHERE book IN (<list>)
```

like this:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/selectThree.png">

The key thing to notice is that the value associated to the property book is an object with property $in whose associated value is (in this case) an array of strings.    (Recall that `[....]` refers to an array and `{...}` refers to an object in this context)

### Operators

The expression $in is called an operator and there are quite a few of them.  Here is a link to a page containing several that allow us to emulate things like >, >=, <, <=, in, not in, etc:

<http://docs.mongodb.org/manual/reference/operator/query/>

Now work through the [CRUD (Create, Read, Update, and Delete) tutorials](https://docs.mongodb.com/manual/crud/). Make sure you do the "sub" pages in the documentation (e.g., "Query an Array"). Stop when you get to "Bulk Write Operations" – we're not going to mess with that. Be sure to **do the work in your own database** so that I can give you credit. This **will require you to make a few changes to the commands**, since they refer to databases like `inventory`, which you do _not_ want to be manipulating. **Each individual should do this**, although you are welcome to work together.  Pay close attention to what is an object and what is an array (especially in the part about `$or`).  Also, ctrl-J (sometimes repeated a few times) will lets you break-out of a command that you are in the middle of entering.
	
You might find the idea of an *upsert* to be nice (particularly since it relates to a problem you had to solve when working on the node.js project)

A few notes:

* The difference between `.save()` and `.insert()` is that `.save()` will do an update if your provide the `_id` and a matching document is already in the collection, whereas `.insert()` will fail.
* We're not going to wade into this here, but MongoDB does support _transactions_ to ensure that complex actions are atom. Feel free to look at [the documentation on transactions](https://docs.mongodb.com/manual/core/transactions/) if you'd like to learn more.

## Reinforcement

Finally, do this tutorial:

<http://openmymind.net/mongodb.pdf>

This will probably take at least one lab. Some of the material is a repeat, but the reinforcement will be helpful. (It may look like a lot of pages, but many of them are almost entirely blank. You could probably compress it to something like 1/3 the pages if you wanted to.)

## To Do

* Create a Mongo database using the same name as you did in your MariaDB database.
* Create a collection called `books` in your Mongo database
* Add several documents to your `books` collection.  At a mininimum:
    * All your documents should have the 'book' property
    * Most of your documents should have an 'author' property
    * You should have at least 10 documents
    * It would be good to have some variety in your properties-- so not all the documents share the same properties
* Do these tutorials:
    * <https://docs.mongodb.com/manual/crud/>
    * <http://openmymind.net/mongodb.pdf>
