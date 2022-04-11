# Neo4J

We can think of the fundamental building block of SQL data as being **the record**, 

and the fundamental building block of mongoDB as **the document** (JSON object).  

The fundamental building block of neo4J is the graph.

Recall, that a graph can be thought of as a collection of nodes connected by edges.  These edges can be directed or undirected.  For the purposes of this database, the edges are directed.  You'll recall how this works from data structures, but here's a picture  (again from wikipedia) of a typical directed graph:

<img src="http://personal.morris.umn.edu/~dolan118/cs4453/images/DAG.png">

The Neo4J people have provided this handy reference graphic:

	http://visual.ly/whats-graph-database
	
It probably won't make much sense until AFTER you do the first set of tutorials that I suggest below.

## Getting things running

It's relatively easy to set-up a Neo4J server but allowing multiple people to use it simultaneously without accidently changing somebody else's data ends up being a real problem, so instead each group is going to spin-up their own copy on their current machine.  Here's how:

1. **Copy** the file `~dolanp/Public/neo4j.unix.tar.gz` to `/tmp` on your current machine.
2. Unzip it: `tar -xvf neo4j.unix.tar.gz`
3. This should create the subdirectory `neo4j-community-2.3.1/`
4. Change into that subdirectory `cd neo4j-community-2.3.1/`
5. Start a version of neo4J:  `bin/neo4j console`

You can connect to your neo4J server using a web-browser.  Use the address `http://localhost:7474/`.  The first time you do so, you will be asked to log in (if you look closely you will see the username and password are both `neo4j`.  On your first log-in you will need to change the default password.

After logging into the system you will see a variety of options that you can choose to start learning how to use neo4j.  We'll go through those in a minute.  First a brief aside:  There are two major, interactive, interfaces to a neo4J system.  One is a browser based (the main one that we will be using), and the other is more of a classic shell interface.  

Because of the nature of the database's paradigm, the browser based approach offers nice graphical flexibility and that is  where we will begin.  Go to localhost:7474 and log in.  Now go through their tutorials-- the first is started by clicking on the button "Start Learning"  It's about 7 slides.  A key thing to notice is that client-side commands begin with a colon, and server side commands are given using a language called CYPHER.

When you reach the end of those 7 slides you have a few options-- click on the button for `Intro` which should start a guided tour of about the same length.  At the end of that you will see `Concepts`, but you've already gone through that.  If you want the reinforcement, read again about nodes, relationships, and properties as they are used in neo4J.  Note the following correspondence:

SQL    | mongo  |neo4J 
-------|--------|-----
record |document|node
	
I don't think you should take the correspondence too far because SOMETIMES (like when you are trying to make sense of the neo4j version of a query) it is easier to think of a node as a column…. but in general, the above correspondence makes the most sense to me.

In some ways, labels play the same role as tables in SQL and collections in mongo; however, labels are more general since the same node may be given multiple labels.  

Relationships in neo4J are similar to the idea of relationships that we developed when drawing our ER diagrams (remember that?)… however, again, the idea is more flexible and abstract in neo4J.

Now go forward and try out the Cypher query language tutorial.  I want EVERYBODY to type this stuff in at least once.  To keep out of each other's way, whenever you are asked to add a node you should provide an extra label specifying who **you** are.  So if the tutorial says:

```
	CREATE (ee:Person { name: "Emil", from: "Sweden", klout: 99 })
```

Then you need to change the labels to look like this:
```
	CREATE (ee:Person:Dolan { name: "Emil", from: "Sweden", klout: 99 })
```
Where you will replace `Dolan` with your own last name.  Technically, you only need to do this for the CREATE commands, but if you don't add your last name to the `MATCH` commands then you will start picking up nodes created by other students as well.  Please note that there is a SPACE after the command names it is NOT `CREATE(blah)` NOR is it `MATCH(blah)` more blah.  Just to be explicit… if you want your examples to look like those in the tutorial (or at least as close as possible), then you will want your `MATCH` commands to look like this:

```
	Match (ee:Person:Dolan) WHERE ee.name="Emil" RETURN ee;
```

instead of like this:

```
	Match (ee:Person) WHERE ee.name="Emil" RETURN ee;
```

Note:  the order of the labels does not matter.  This will work just as well:

```
	Match (ee:Dolan:Person) WHERE ee.name="Emil" RETURN ee;
```

A WARNING.  Although you can have the tutorial type the command for you, it will be MUCH more enlightening if you type the commands yourself.

A few words on the syntax too (do the tutorial first or read what I have to say concurrently, but I doubt this will make much sense if you don't try to do the commands yourself first).  One thing to notice is that a node looks like a node in the syntax.  Look for expressions of the form:   `(stuff)`.  They represent nodes.   The only place this breaks down is in the `RETURN` clause of a `MATCH`. Look closely at this expression:

```
	MATCH (ee:Person)-[:KNOWS]-(friends) WHERE ee.name = "Emil" RETURN ee, friends
```

the `(ee:Person)` is, kinda-sorta like specifying a COLUMN in SQL.  Also notice that syntax involving edges (which capture the idea of a relationship between nodes) looks a lot like an edge.  Consider that last example again:

```
	MATCH (ee:Person)-[:KNOWS]-(friends) WHERE ee.name = "Emil" RETURN ee, friends
```

You have two nodes `(ee:Person)` and `(friends)`.  One of the nodes can be thought of as being named `ee`, and the other as being named `friends`.  The colon after `ee` requires that any node to be associated with `(ee)` must have a `Person` label.    
The edge `-[:KNOWS]->` means that the `ee` node must be joined to the friends node by a relation with a `KNOWS` label.

Notice some of the similarities between neo4J syntax and SQL:

* `WHERE` clauses
* existence and meaning of the keyword `DISTINCT`
* object.property notation (also a key feature of Javascript and hence Mongo)
* A bothersome amount of case-sensitivity in unexpected locations
* A strong order-dependency on the clauses (a `LIMIT` clause occurs AFTER a `RETURN` clause)

In just a moment I will ask you to do the final tutorial-- `The Movie Database`-- **BUT PAY CLOSE ATTENTION TO WHAT I HAVE TO SAY FIRST**:

* Typing everything in this tutorial is unreasonable, however just using the data as provided in the tutorial won't get you credit. 
* For the movie database, copy and paste the rather large amount of text into a text document and make the following two global replacements:
		1. Replace `:Movie` with `:Movie:lastname` (where lastname is your lastname)
		2. Do the same, replacing `:Person` with `:Person:lastname`
* Do NOT delete the nodes at the end of the tutorial (else I will not be able to give you credit for it), instead, call me over and show me your work.

If you've followed the meaning of the syntax, then modifying the command, as provided in the tutorial, to only include nodes labelled with your last name will be pretty easy.  For example, I would modify 

```
	MATCH (tom {name: "Tom Hanks"}) RETURN tom
```

To read:

```
	MATCH (tom:Dolan {name: "Tom Hanks"}) RETURN tom
```

Pay close attention to the syntax.  In particular, be sure you understand the usage of  `-->` , `<--`, and `--`
Also notice the ability to name an edge an extract data from it in the `RETURN` clause.

Now let's test your understanding.  I'm going to ask you to modify two of the queries.  
	1. Modify the following query so that it also returns the name of the movie:
```
		MATCH (tom:Person {name:"Tom Hanks"})-[:ACTED_IN]->(m)<-[:ACTED_IN]-(coActors) RETURN coActors.name
```
	2. Modify the following query to find the PEOPLE who acted within 3 hops of Keanu Reeves:
```
		MATCH (bacon:Person {name:"Kevin Bacon"})-[*1..4]-(hollywood)
		RETURN DISTINCT hollywood
```

Here's a great, condensed reference:  http://docs.neo4j.org/refcard/2.0/.  Pay close attention to the overlap in the neo4J language and SQL.  (There's a LOT here.  I expect you to skim)

With that beginning out of the way, you are now ready go into the slower-paced, but more detailed sets of tutorials.  The first tutorial in this section again uses a movie database so I want you to add a label not just for your last name, but ALSO label nodes with Tutorial.  That way you can keep this work separate from your earlier work.  You will get the opportunity to try out more clauses in these tutorials:

```
	http://docs.neo4j.org/chunked/stable/tutorials.html
```

Start with 4.  See how far you get. (I'd like us to get to the end of 5 by the end of the week).  Remember to modify the commands so that a label with YOUR last name also gets created.  Also notice that in THIS tutorial, the actors are described with the label Actor instead of Person.  So you WILL need to add some more nodes.

