# Lab 1.3

```sql
DROP TABLE IF EXISTS Movies;
CREATE TABLE Movies(title VARCHAR(50), year INT, director VARCHAR(50), length INT);
INSERT INTO Movies VALUES('Database Wars', 1967, 'John Joe', 123);
INSERT INTO Movies VALUES('The Databaser', 1992, 'John Bob', 190);
INSERT INTO Movies VALUES('Database Wars', 1998, 'John Jim', 176);
```

## Exercise #1
Can you write the movie query from lecture as a single SFW query?

Recall that we are trying to find all movie titles that were used for more than one movie, and our schema for the movies table is:

title STRING
year INT
director STRING
length INT

Let's try to write the nested query that solves this from lecture:

use ANY to write it.


use join to write it.
