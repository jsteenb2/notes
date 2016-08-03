#Relational Databases
--

**What is a database?**
Its a system for storing data

**Why do we think of a database as a "system" and not just a dump for data?**

Because it does much more than just *save* data.  It allows you to scale, update with accuracy.  Provides security and so forth.

*Databases provide* **STRUCTURE**

**What are 5 reasons to use a database instead of a spreadsheet for storing your application's data?**
 
 1. size
 2. ease of updating
 3. accuracy
 4. security
   * enforced security behind the scenes
 5. redundancy
   * In data entry, not with having backup of the data
   * Keeps data itself non-redundant
 6. importance
   * Allows data to grow without worrying about massive fudges

**What is a relational database?**
A collection of data tables which have relationships that identify between each table.

**What is the RDBMS?**
**R**elational **D**ata**B**ase **M**anagement **S**ystem
Its the software that actually runs the db behind the scenes, like a reference library, more so than - library itself.

Available dbs:

* Microsoft SQL Server
* Oracle db
* MySQL (Oracle)
* SQLite (OS)
* PostgreSQL (OS) 

All use SQL

**How is an object represented within a table?**
As a single row, where the cols are the best

**What are the advantages of having your database be very picky about what kind of data goes into it?**
Keeps the referential integrity. More you know about ur data the faster you can make it run. Strict enforcement of the data makes it fast to allocate precisely(think # of bytes) for a given column and faster b/c it can make lots of assumptions about those cols when forming queries.

**What is a "schema"?**
Its the definition of how the table is setup

**What are "database keys" and why are they important?**
Are the uniq IDs for a row. Are absolutely uniq per table.

**What is the "primary key"?**
The primary key is really just the ID, first column.


Data Relationships
----

**Why is it a Bad Idea to try to stuff all your data into a single table?**
Breaks the single responsibility principle of SOLID.  A table should relate to one type of object.

**Which column forms the backbone of all table relationships?**
The primary key

**What is a "one-to-many" relationship?**
Where one thing has many things that are related to it.  Like user with many blog posts.


**What is a "foreign key"?**
Foreign key is what connects the two tables.  The foreign key represents the primary key from the related object. *Foreign key* always goes on the "many" side of the relationship.

**How can you use the foreign key to find all the "many" items for a particular "one"?**
Just search for the particular key in the tables columns. SImple filtering.

**What is a "one-to-one" relationship and how is it expressed using foreign keys?**
Where an object has a very big attribute that itself can be broken out into another table. That one-to-one relationship is because each object has a single relationship with that attribute.
Like a user and their profile. A profile would take up a lot of space in the user table.

**Why are 1:1 relationships rarely used?**
Its likely that if a 1:1 relationship exists that the object can be broken up and make that relationship between two tables.

**What is a "many-to-many" relationship?**
A relationship that has many to many.  Like person loves many dogs and dogs love many people.

Accomplish this through a 3rd table. Called a join table.

**How is the X:X relationship actually expressed?**
With a join table generally. A table with keys for cols that match according to how you set up the tables relationship.

**What is a "join table" and why is it useful?**
Is the above, last answer.

**Are there requirements for how you name your Foreign Keys?**
Its best to make them have a semantic tag. So you can tell the difference.  Ex) Writer column & contributor column. Both are user objects.

**What are a few conceptual tests you can run to determine if a relationship is 1:1, 1:X, or X:X?**

1. It's How Many You Could Have in the future
2. Think it through both ways
  * Ask the hypothetical questions as if you were the object and walk through its lifetime
3. Beware the word "Typically"
4. Multiple Relationships are Fine and Natural



Data Model Design
----

**Why is it so important to think through your data model ahead of time?**
B/c its significanlty harder to make changes to a db after its populated.  If you architect it correctly from the get-go, you'll be much better off.

**What are the first steps to take when thinking through your data model?**

1. Understand your needs
  * Important to be as specific as possible when detailing relationships
2. Determine your entities
  * psuedocoding for dbs
3. Determine Each Entity's Attributes


**What is "Entity Relationship Modeling"?**
psuedocode for dbs

**What is an "Entity"?**
A specific kind of data (objects or nouns)

**What is an "Attribute"?**
A column, like name, email, phone, body ... etc.

**How can you figure out if maybe an attribute should really be an entity of its own?**
By looking closer at the attributes. Something like a string column that holds state name might be better served as a table of itself, same with cities. 
Ask the following:

1. How strict and structured does this data need to be?
 * If you can't allow errors, probably best to make a table and hold the specifics you need. 
2. Will I potentially change the label of this thing later?
 * Like a city changing its name
 * Don't wnat to go through entire table seraching for attr that needs to be updated
3. Will I use this piece of data in multiple other tables?
 * What if have many uses?
 * Reduce redundancy best as possible
4. Determine attribute types
5. Model relationships b/t entities

**How granular should your attributes be?**
Be as granular as possible.  More granular you are lesss likely to make mistake.  Smaller is better.
 
 * first\_name & last_name, not name

**What is the "Type" of each attribute?**
is the data type, ex) String, Integer, Decimal, Date.. etc.


**What are some helpful ways of modeling the relationships between entities?**
Use and Entity Relationship (ER) diagram (white board it up)

1. Figure out your system goals and data needs
2. Determine all your entities
3. Determine the attributes for each entity
4. Determine the specific type of each attribute
5. Model the relationships between entities
6. Profit

**How do the Entities, Attributes and Relationships you've defined become an actual (proposed) database schema?**

1. Setup table for ea Entity
2. Create cols for attr
3. Add any foreign keys as need
  * Create join table if you have a many-to-many relationship

Data Normalization
----

**What is "normalization"?**
Checking for redundancy and/or risk for collateral changes

**What is the main goal of normalization (in 3 words or less)?**
reduce redundancy & dependencies
--> DRY for dbs

**What are the "normal forms"?**
tests to make sure you haven't created data redundancy

**Why are the normal forms practically useful to you as someone trying to design a data model?**
Way of identifying where and how to break up data into tables.  They are tests that you run to identify shortcomings

**What is the most common solution to a normal form violation?**
create a new table

**Why shouldn't you include derived columns in your table?**
You can fall prey to having to edit multiple entries when you only want to change one.  Like the category column that depends on the title.  Break the category into its on table and reference the ID there.

Another reason, if you store a product of two other attrs for instance, and change one of them, then you need to redo the calculation.  The calc can be done in SQL or ruby so do it at runtime!  IF this is the case, axe the col that is using table data to confirm itself.

**Why should you be careful about normalizing data like zip codes?**
Becuase zip codes can be across multiple cities. Zips are not necessarily redundant with City/State info.  Same with phone nums for work and cell.

SQL Basics: Working with Database Schemas
----
**What is SQL?**
**S**trucutred **Q**uery **L**anguage

 * Structure means fast

**What does it mean (practically speaking) that SQL is a "declarative" language?**
means you only tell it what you want from query, no how to worry about. It optimizes the brains for you!

**Why is it generally better to let SQL do the work on a query than to load it into Ruby first?**
Becuase it optimizes the crap out of it to best fulfill the query request

**Is SQL case-sensitive?**
Not case sensitive

**Is it okay for a query to span multiple lines?**
Yup, no problem

**Why is SQLite3 a useful development and practice database?**
Its a lightweight db, doesn't require server connection to run.

**How do you install SQLite3?**
Install the gem

```ruby
$ gem install sqlite3
```

**How do you use the SQLite3 program from the command line?**

```SQL
# quitting
.quit  # or CTRL + D

#break out of loop
CTRL + C

#type commands and end with ;
COMMAND SEQUENCE HERE;

#succesful command, so no output

# to execute press
ENTER

```

**Why is it useful to add indexes to table columns?**
Allows you to perform faster queries on that column. B/c its loaded into memory.
 
 * Can add *significant* performance gains

SQL Basics: CRUD and Beyond
----
**Does order matter in an SQL query?**
Nah, doesn't matter, but is best to follow community standards

**What are the "Statement", "Table", and "Conditions" of the query ```SELECT * FROM User WHERE ID = 3```?**
SELECT = statement/action
FROM   = table
WHERE  = conditions

**How do you CREATE data in SQL?**
with the following

```sql
# Generic Syntax
INSERT INTO TableName
    (column1,column2,columnN)
    VALUES (value1,value2,valueN);

# Example
INSERT INTO User
    (Name,Email)
    VALUES ('beorn','beorn@app.com');
```
Then specify table, your values, and specific columns you'd like to add


**What happens if you try to insert the "wrong" kind of data into a column?**
WHole process blows up! Only good data!


**How do you READ data in SQL?**

```sql
SELECT Name FROM User;

# Selecting multiple columns
SELECT Name,Email FROM User;

# Selecting all columns
SELECT * FROM User;
```

**How do you specify that you want to return all columns?**
use *

**How do you return just a single column?**
just put in col name

**What is returned by an SQL query?**
returns new table with only columns that fit search criteria

**What does the WHERE clause do?**
Filters for a specific set of rows (like an if statement)

```sql
SELECT *
    FROM User
    WHERE Name = 'sven';  # SINGLE quotes around strings!

SELECT Email
    FROM User
    WHERE UserID = 1; # No quotes for Integers

SELECT *
    FROM Post
    WHERE CreatedAt > '2014-08-10 00:00:00';  # Timestamp
```

**How do you specify a WHERE on a subset of values?**
after table, see above

**How do you "fuzzy match" text to search your database for?**
Use LIKE operator, and the % wildcard (can use _ as single char wildcard)

```sql
SELECT Name
    FROM User
    WHERE Name LIKE 'S%';  #=> Sven!

SELECT Name
    FROM User
    WHERE Name LIKE `Sv_n`; #=> Sven!

SELECT Name
    FROM User
    WHERE Name LIKE `%n%`; #=> Sven and Hans!
```
beware, is less efficient using LIKE

**Comparison Operators**

```sql
unary + - ~ NOT
||
*    /    %
+    -
<<   <>   &    |
<    <=   >    >=
=    ==   !=   <>   IS  IN  LIKE  GLOB  BETWEEN
```

**How do you fuzzy match any number of characters? Just one?**
Use % for many, or _ for one

**How do you query for NULL values or values that are not NULL?**
Use operator IS

```sql
# Check for NULL
SELECT *
    FROM Post
    WHERE Body IS NULL;

# Check for NOT NULL
SELECT Name
    FROM User
    WHERE Name IS NOT NULL;
```

**How do you find only the unique values of a column?**
Use DISTINCT operator, is unpredictable when removing multiples.. beware

**How do you order your results?**
Use ORDER BY operator and ASC(default) or DESC

```sql
SELECT Name
    FROM User
    ORDER BY name;  # default is ASCending

SELECT Name
    FROM User
    ORDER BY email DESC;
```

**What is the default order for results?**
Ascending - ASC

**How do you limit results to just a specific number of rows?**
Use the LIMIT operator

```sql
# Most recent 5 posts
SELECT *
    FROM Posts
    ORDER BY CreatedAt DESC
    LIMIT 5;
```

**What happens if you try to order results on multiple columns?**
Just provide to ORDER BY operator, in order of precedence

```sql
SELECT Name
    FROM User
    WHERE UpdatedAt >= '2014-09-10 00:00:00'
    ORDER BY Name, Email;
```

**How do you UPDATE data in SQL?**
Use UPDATE operator and make sure to include WHERE so you edit specifically what you want. With no WHERE clause the UPDATE operation can apply to a whole column(s)!

```sql
# General case
UPDATE TableName
    SET Column = Value
    WHERE Condition;

# Flagging inappropriate usernames
UPDATE User
    SET Name = '(removed)'
    WHERE Name LIKE '%smelly%';
```

**Why is it important to use a WHERE clause when updating?**
See answer above.

**How do you DELETE data in SQL?**
Use the DELETE clause, and ABSOLUTELY REMEMBER the **WHERE** clause!!!!!

```sql
# General case
DELETE FROM table
    WHERE condition;

# Delete old values
DELETE FROM Post
    WHERE UpdatedAt < '2014-08-10 00:00:00';
```


**Why is it important to use a WHERE clause when deleting?**
B/c without it you'll be deleting every row!

**How can you test your DELETE ahead of time to be sure it's not going to be destructive?**
Use SELECT first to see what rows will be deleted then substitute DELETE when ready

**How do you delete just a single attribute of a single record?**
Use UPDATE to change single value in single row, DELETE always removes a whole row

**What are database transactions and why are they useful?**
A wrapper for running series of operations. Can specify either whole thing succeeds or entire thing fails

**CODE REVIEW**

```sql
# ******* CREATE *******

INSERT INTO User
    (Name,Email)
    VALUES ('beorn','beorn@app.com');


# ******* READ *******

SELECT * FROM User WHERE ID = 1

# Selecting multiple columns
SELECT Name,Email FROM User;

# Selecting all columns
SELECT * FROM User;

# Narrowing with WHERE, AND, OR
SELECT Body
    FROM Post
    WHERE   Title='Hans Posts First'
        AND CreatedAt < '2014-09-10 00:00:00'
        OR Title='Some Other title';

# Multiple values to find within
SELECT Name
    FROM User
    WHERE Name IN 
        ('Sven','Olga','Lars');  #=> Sven!

# Fuzzy matching
SELECT Name
    FROM User
    WHERE Name LIKE 'S%';  #=> Starts with 'S'

# Check for NULL
SELECT *
    FROM Post
    WHERE Body IS NULL;

# Check for NOT NULL
SELECT Name
    FROM User
    WHERE Name IS NOT NULL;

# Just unique values in that column
SELECT DISTINCT name
    FROM User;

# Ordering results
SELECT Name
    FROM User
    ORDER BY email DESC;

# Limiting to the most recent 5 posts
SELECT *
    FROM Posts
    ORDER BY CreatedAt DESC
    LIMIT 5;

# Ordering by multiple columns
SELECT Name
    FROM User
    WHERE UpdatedAt >= '2014-09-10 00:00:00'
    ORDER BY Name, Email;


# ******* UPDATE *******

# Flagging inappropriate user names
UPDATE User
    SET Name = '(removed)'
    WHERE Name LIKE '%smelly%';


# ******* DESTROY *******

# Delete old values
DELETE FROM Post
    WHERE UpdatedAt < '2014-08-10 00:00:00';
```

SQL Join Queries
----
**What is JOINing and why is it useful?**
Pulling data from multiple tables to perform operations on.

**How do you specify the table name of a column as part of a statement or condition?**
use the ```JOIN``` command

**How do you know you'll need to use a JOIN?**

```sql
# General Case
SELECT Column1,Column2...ColumnN
    FROM TableA JOIN TableB
        ON TableA.Column2 = TableB.ColumnX
    WHERE conditions;

# Our Example:
# Author names of recent posts
SELECT User.Name
    FROM User JOIN Post
        ON User.UserID = Post.AuthorID
    WHERE Post.CreatedAt > '2014-08-10 00:00:00';
```

**What (again) is a foreign key?**
Marks the connection/relationship between different tables. FK used in a one to many relationship, and is held by the many side.

**Why are foreign keys so important for joins?**
Allows the matching of data to its counterpart. Fills in the key with the actual model that is being referenced.

**On a normal JOIN, which rows would get duplicated? Omitted?**
Things are omitted when they have no relationship in the dataset being called.  Say, recent authors show up for recent posts, but authors who haven't posted in a long while will be omitted when JOINing recent results.

Also can have duplication of a foreign key in our query.  Say with the example above a author makes several recent posts.  He will show up several times in the JOIN.

**Which relationships between tables really lend themselves to joining?**
Works great for one-to-many relationships.  Pretty much anytime you are using FKs

**How do you specify which columns to join on?**


**Why does this mean that your foreign key names don't matter?**
B/c we select where we join ON, we are responsible for mapping the FKs to the right primary of the other table.

**What is a LEFT OUTER JOIN?**
Means that the word LEFT of hte JOIN statement has higher priority (guarentees all rows are included).

```sql
# General Case
SELECT columns
    FROM TableA LEFT OUTER JOIN TableB
    ON TableA.column1 = TableB.columnX;

# Our Example:
SELECT * 
    FROM User LEFT OUTER JOIN Post
    ON User.UserID = Post.AuthorID;
```

Its like in a ven diagram with the two circles.  You keep all the left circle and the part of the right circle that overlaps with the left circle.

**When might the left table gain rows? Lose rows?**
Left table may gain rows if the right side has multiple matches with the left side.  The left side duplicates to accomadate the extra matches.  LEFT table can grow but never shrink in a LEFT OUTER JOIN.  Right table can lose rows if the row is never referenced by the left table. Usuallly this row is filled with NULLs on the right side if there is no match.

**What is a FULL OUTER JOIN (aka FULL JOIN)?**
When you keep both sides of the tables, and fill in any rows that are missing on either side with NULLs.  Potentially large # of NULLs.

```sql
# General case
SELECT * 
    FROM TableA OUTER JOIN TableB
    ON TableA.Column1 = TableB.Column2;

# Our Example: 
# List all authors and posts
SELECT *
    FROM User OUTER JOIN Post
    ON User.UserID = Post.AuthorID;
```

**Summary of OUTER JOINs**

THere are many ways to do it. 

1. INNER JOIN, aka JOIN
2. LEFT OUTER JOIN, keep all rows from left  and matching rows from right
3. RIGHT OUTER JOIN, same as left but with priority of right instead of left
4. FULL OUTER JOIN, keeps all rows from all tables. Can create wasted space with fulfillment of nulls

**How can you join the tables of a many-to-many relationship?**

```sql
# General Case
SELECT TableFoo.ColumnA,TableBaz.ColumnB
    FROM TableFoo 
    JOIN TableBar ON TableFoo.ID = TableBar.FooID
    LEFT OUTER JOIN TableBaz ON TableFoo.ColX = TableBaz.ColY
    WHERE condition;

# Example 1: Posts that Contributor 3 has Contributed to
SELECT User.Name,Post.*
    FROM User JOIN ContributorPost
        ON User.UserID = ContributorPost.ContributorID
    JOIN Post
        ON Post.PostID = ContributorPost.PostID
    WHERE User.UserID = 3;

# Example 2: Contributors to Post 4
# Note that the JOIN stuff is identical to
# the previous example.  
SELECT User.Name
    FROM User JOIN ContributorPost
        ON User.UserID = ContributorPost.ContributorID
    JOIN Post
        ON Post.PostID = ContributorPost.PostID
    WHERE Post.PostID = 4;

# Example 3: Titles of Posts with Contributors
# Here, the join does all the work we need
# by filtering out any Posts that don't have
# a row in the join table, which means they
# don't have a corresponding Contributor
SELECT Post.title
    FROM Post JOIN ContributorPost
        ON Post.PostID = ContributorPost.PostID;
```

**What are the conceptual steps that are run in order to perform a many-to-many join?**

STEPS:

1. JOIN the left tables together as normal(as above)
2. JOIN the resulting table to the right table as if a fresh join
3. Perform the ```SELECT``` and ```WHERE``` on the resulting table

**CODE REVIEW**

```sql
# JOIN aka INNER JOIN (common)
# Authors of recent posts 
# (one-to-many)
SELECT User.Name
    FROM User JOIN Post
        ON User.UserID = Post.AuthorID
    WHERE Post.CreatedAt > '2014-08-10 00:00:00';

# LEFT OUTER JOIN (uncommon)
# Keep all users but only those posts
# which have authors set
# (one-to-many)
SELECT * 
    FROM User LEFT OUTER JOIN Post
    ON User.UserID = Post.AuthorID;

# OUTER JOIN aka FULL OUTER JOIN (rare)
# List all authors and posts
# (one-to-many)
SELECT *
    FROM User OUTER JOIN Post
    ON User.UserID = Post.AuthorID;

# Multiple Joins
# Posts that Contributor 3 has Contributed to 
# (many-to-many)
SELECT User.Name,Post.*
    FROM User JOIN ContributorPost
        ON User.UserID = ContributorPost.ContributorID
    JOIN Post
        ON Post.PostID = ContributorPost.PostID
    WHERE User.UserID = 3;
```

SQL Aggregate Functions and Groups
----

**What is an "Aggregate Function"?**
Performs operations on all the rows of a single column.

**How many records are returned by running an aggregate function on your query?**
Usually returns one row in teh case of 

```sql
AVG/MIN/MAX/COUNT
```

**What's the conceptual/implicit order in which the parts of an 
SQL query are run?**

1. Grab the 1st table we're working with (**FROM**)
2. Prepare the table by **JOIN**ing tables if necessary
3. Narrow down the table to a suitable sub-table based on **WHERE**
4. Sort the resulting table if needed with **ORDER BY**
5. Gather similar rows together with **GROUP BY**
6. Execute aggregate functions on the grouped columns
7. Filter the table returned by your grouping and aggregates using **HAVING**
8. Decide which columns to return **SELECT**

**How many rows is returned by COUNT?**
Returns 1 row

**What column(s) are returned by COUNT?**
The sum of every row that meet the filter criteria

```sql
# Count up all users whose name starts with 's'
SELECT COUNT(*)
	FROM User
	WHERE Name LIKE 's%';  
```

**Does it matter which column you supply to COUNT?**
No, it counts based on the WHERE criteria

**Does it matter which column you supply to MAX, MIN, AVG, or SUM?**
YES, these aggregate functions are operating on these specific columns

```sql
# MAX
SELECT MAX(Age)
	FROM User;

# SUM
SELECT SUM(SocialShares)
	FROM Post
	WHERE AuthorID = 356;

# MIN
SELECT MIN(WordCount)
	FROM Post
	WHERE SocialShares > 100;
```

**Can you run multiple aggregate calculations at once?**
Yes, yes you can

```sql
# All posts AND the maximum post in one line
SELECT COUNT(*),MAX(CreatedAt)
    FROM Post;
```

**Where in the conceptual order of SQL execution do aggregates fall?**
Aggregate functions wait until you join and make the end tables you will be performing aggregation on.

```sql
# first narrows down POST by where criteria, and then runs aggregates
SELECT COUNT(*),MAX(CreatedAt)
    FROM Post
    WHERE AuthorID = 2
        AND CreatedAt > '2014-08-10 00:00:00';
```

**What does GROUP BY do?**
Couples an aggregate function with another row. Plays matchmaker. Doesn't run if no aggregates used.
Pretty much produces aggregate results based on a columns info.  Like matching author to COUNT of posts she's made.

**Can you run GROUP BY without an aggregate function?**
NO

**Does GROUP BY run (conceptually) before or after WHERE?**
AFTER

**Why might you run GROUP BY on multiple columns?**
Effectively nests them within each other. For visualizing.

```sql
SELECT User.Name, Category.Name, COUNT(Post.*)
    FROM Post 
    JOIN User ON Post.AuthorID = User.UserID
    JOIN Category ON Category.CategoryID = Post.CategoryID
    GROUP BY User.UserID, Category.CategoryID;
```

Another example where our Author now has posts that are apart of a single overall category(one-to-many).

```sql
# Want COUNT of all Posts written by a given Author presented for each Category
SELECT User.Name, Category.Name, COUNT(Post.*)
    FROM Post 
    JOIN User ON Post.AuthorID = User.UserID
    JOIN Category ON Category.CategoryID = Post.CategoryID
    GROUP BY User.UserID, Category.CategoryID;
```

**How do you declare an Alias and why might it be useful?**
Use the **AS** operator.  Just renames the result to the alias.  Can also use the shortcut of leaving out **AS**

```sql
# Column alias
SELECT column_name AS alias_name
	FROM table_name;

# Table alias
SELECT column_name
	FROM table_name AS alias_name;
	
# Both aliases
SELECT MAX(p.CreatedAt) AS LatestPostDate
    FROM User AS u
    JOIN Post AS p ON u.UserID = p.AuthorID
    WHERE p.AuthorID = 4;
    
# with shortcut
SELECT MAX(p.CreatedAt) AS LatestPostDate
    FROM User u
    JOIN Post p ON u.UserID = p.AuthorID
    WHERE p.AuthorID = 4;
```
NOTE: you can't do math immediately with aliases you declar in your SELECT clause.

```sql
# Not going to work...
SELECT COUNT(users.id) as u_count,
	   SUM(users.age) as sum_age, 
	   sum_age/u_count as avg_age
```

**What other clause is HAVING very similar to?**
Very similar to **WHERE** clause.  **HAVING** only runs on aggregated columns and **WHERE** only runs on individual rows prior to aggregation

**Can you use HAVING without GROUP BY?**
NO

**Where in the conceptual order of execution does HAVING fall?**
LAST

```sql
# General Case
SELECT column_name, aggregate_function(column_name)
	FROM table_name
	WHERE column_name operator value
	GROUP BY column_name
	HAVING aggregate_function(column_name) operator value;
	
# example case
SELECT  User.Name AS author, 
        Category.Name AS category, 
        COUNT(Post.*) AS p_count
    FROM Post 
    JOIN User ON Post.AuthorID = User.UserID
    JOIN Category ON Category.CategoryID = Post.CategoryID
    GROUP BY User.UserID, Category.CategoryID
    HAVING p_count >= 2;
```

**CODE REVIEW**

```sql
# Count up all users whose name starts with 's'
SELECT COUNT(*)
FROM User
WHERE Name LIKE 's%'; 

# MAX
SELECT MAX(Age)
FROM User;

# SUM
SELECT SUM(SocialShares)
FROM Post
WHERE AuthorID = 356;

# MIN
SELECT MIN(WordCount)
FROM Post
WHERE SocialShares > 100;

# Multiple aggregates and mixing
# with normal columns are fine
SELECT Title,COUNT(*),MAX(CreatedAt)
    FROM Post;

# Counting up all recent Authors' Posts
# By GROUPing them together
SELECT AuthorID, COUNT(*)
FROM Post
GROUP BY AuthorID
WHERE CreatedAt > '2014-08-10 00:00:00';

# Count of posts by Author for each Category
# (Multi-column grouping)
# Used with joins from a one-to-many relationship
SELECT User.Name, Category.Name, COUNT(Post.*)
    FROM Post 
    JOIN User ON Post.AuthorID = User.UserID
    JOIN Category ON Category.CategoryID = Post.CategoryID
    GROUP BY User.UserID, Category.CategoryID;

# Table and Column Aliases
SELECT MAX(p.CreatedAt) AS LatestPostDate
    FROM User AS u
    JOIN Post AS p ON u.UserID = p.AuthorID
    WHERE p.AuthorID = 4;

# Count of posts by Author for each Category
# BUT only those where that count is 2 or more
SELECT  User.Name AS author, 
        Category.Name AS category, 
        COUNT(Post.*) AS p_count
    FROM Post 
    JOIN User ON Post.AuthorID = User.UserID
    JOIN Category ON Category.CategoryID = Post.CategoryID
    GROUP BY User.UserID, Category.CategoryID
    HAVING p_count >= 2;
```

SQL Sub-Queries
----

**What is a sub-query?**
An additional SQL query that you nest inside another query

**What are "Independent" subqueries?**
A subquery that is completely self-contained.  THey run completely BEFORE the enclosing query is begun.

**How do you put a subquery into another query?**
Most common to add it to the **WHERE** clause. Often used to divide your table by some fn of its other values. 

```sql
# Generic Subquery Case
SELECT column_name
FROM   table1
WHERE  column_name OPERATOR
          (SELECT column_name
          FROM table1
          WHERE condition);

# Example:
# Posts with above average views
SELECT *
    FROM Post
    WHERE Views > (
        /* This comment should describe the subquery */
        /* because otherwise you'll never remember it */
        /* In this case, it's the average of views */
        SELECT AVG(Views)
        FROM Post
    );

# Example:
# Posts with the same category as
# the most recent post
SELECT *
    FROM Post
    WHERE Category = (
        /* Grab the latest post's category */
        /* Note that we're returning ONE value */
        SELECT Category
        FROM Post
        LIMIT 1
        ORDER BY CreatedAt DESC
    );

```

**When is the independent subquery run in the order of operations?**
FIRST

**What should your subquery output?**
A table

**How do you write a comment in SQL?**
Using:

```sql 
 /* Commented stuff here */ 
``` 
 
 see example above

**On a scale from Important to Very Important, how important is it to comment your subqueries so you can remember what they actually do later?**
VERRRRY IMPORTANT

**How do you know if you need to use a subquery?**
You'll know when your initial work doesn't lead to the right place. Occaional nesting may be needed if the first-order query returns a table that is inadquate.

When you have to calculate value within an existing query (only done with subquery), they you know to use a subquery

**What is a "correlated" subquery?**
Subquery that relies on the values in the enclosing query. 


###SQL-Flavored ActiveRecord
- - -

**In what ways is Active Record an abstraction of SQL?**
Sets up database and provides interface to do operations on your db.  Is the ORM that interacts through SQL behind the scenes.

**In what ways does Active Record provide you with functionality that goes beyond just a mere abstraction?**
Gives you flexibility in creating db tables.  Migrations create and edit tables. THey make it possible to rollback db changes with ease.

Also, when you use active REcord, you are getting back objects in an array instead of a SQL table.  Much easier to work with in your app.

**AR** goes a step further and provides you with the ability to do operations before or after the SQL query is run.  Like determining if the input meets validation criteria..etc.

Also operates **lazy**ily and only does operations if there is a specific request for it. Also handles caching for you so you never actually hit the db mult times.

###Rails Migrations in Full
- - -

**What are the two ways to create a migration from the command line?**

```bash
# creates skeleton migration
$ rails g model 

# if you name it correctly will generate helpful input in your migration file
$ rails g migration YourMigrationName
```

**Why are the timestamps in your migration filenames useful?**
Allows you to rollback/undo changes to your db and uniquely identifies it.

**How does Rails know if you've run a migration?**
The ```schema_migrations``` table keeps track of all changes to the db. Contains the timestamps from migrations.

**What does the t.timestamps line do in the migration file?**
Adds ```created_at``` and ```updated_at``` columns.

**What does the change method do?**
Houses everything that will be done in migration. Use change if you want to make the action reversible.  List of supported reversing methods found [here](http://guides.rubyonrails.org/migrations.html#using-the-change-method).

**What does it mean for a migration action to be "Reversible"?**
Means you can remove the columns or other additions that were done in migration.  If you drop column in migration, you lose that information in that column forever, not reversible.

**What do the up and down methods do?**
Up is when you are migrating.

Down is when you are rolling back.

Rails takes care of interpreting which one you want to apply with the ```change``` method

**Why is reversing a migration something to be very careful about?**
Because you can very easily lose data that you may need.

**What method lets you create a new table?**

```ruby
create_table :products do |t|
```

**What method drops that table?**

```ruby
drop_table()
```

**What method lets you create a new column?**

```ruby
add_column :table_name, :column_name, :type
```

**What method lets you change a column?**

```ruby
change_column :table_name, :column_name, :type
```

**What method lets you remove a column?**

```ruby
remove_column :table_name, :column_name
```

**What method lets you add an index to a column?**

```ruby
add_index :posts, :author_id

# can add index when change/creating table
change_table :users do |t|
  t.string :last_name, :null => false
  t.string :email, :null => false, :index => true
end
```

**How do you specify that the index must be unique too?**
Want to add indexes to any columns with heavy use.

Just add the unique hash with a true value.

```ruby
add_index :posts, :title, :unique => true
```

**How do you give a column a default value?**

**How do you specify that a column cannot be null?**

**How do you migrate a specific number of steps?**

```ruby
# perform all un-finished migrations
$ rake db:migrate

# migrate up to a specific file's timestamp
$ rake db:migrate:up VERSION=20140901090707

# migrate # of steps
$ rake db:migrate STEP=3
```

**How do you roll back a specific number of steps?**

```ruby
# Roll back the last run migration
$ rake db:rollback

# Roll back the last 3 migrations
$ rake db:rollback STEP=3

# Roll back to a specific migration timestamp
# Note that this uses MIGRATE!
$ rake db:migrate:down VERSION=20140901090707
```

**Why is it a bad idea to modify migration files if they've been run and checked into source control?**
Because things may be different when you are working with others. If others already rake that migration, you will not be able to get the desired results likely.  Instead opt to create a new migration that will remove_column or undo whatever you desired.

**How do you blow away and reset your database?**

```ruby
# drop your old database
$ rake db:drop

# setup the new database by running migrations and seeds
$ rake db:setup

# perform both steps at once
$ rake db:reset
```
####[Rails Guide on Migrations](http://guides.rubyonrails.org/migrations.html)

###Code Review

```ruby
# migrate and roll back
$ rails g migration YourMigrationName
$ rake db:migrate
$ rake db:migrate:up VERSION=20140901090707
$ rake db:rollback
$ rake db:rollback STEP=2
$ rake db:migrate:down VERSION=20140901090707

# set up and reset the database
$ rake db:setup
$ rake db:drop
$ rake db:reset

# Lots of example migration methods
# ... you'd probably break these up into separate
# migrations in reality
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name, :null => false
      t.string :nickname, :default => "Awaiting nickname..."
      t.string :email, :null => false
      t.datetime :birthday

      t.timestamps
    end

    add_index :posts, :title, :unique => true

    remove_column :posts, :author_name
    add_column :posts, :author_id, :index => true, :null => false

  end
end
```

###Queries and CRUD Operations
- - - 

#####Do all methods in your Models require database interaction?
No, not all.

#####How do you create a new model instance in memory?
Use the same way you instantiate a class object in RUby(SomeClass.new...)

```ruby
$ rails g model Post
```

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
end


# db/migrate/12345678901234_create_posts.rb
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      # We've filled these in:
      t.string :title, :null => false
      t.text :body, :null => false

      t.timestamps
    end
  end
end
```

#####What happens if you try to set an attribute that isn't a column in the table your Model reflects?
You'll get a NoMethodError

#####How do you save that to the database?

```ruby
p.save   # returns false if it fails
p.save!  # throws error if it fails
```

#####How can you view the SQL that runs for a given Active Record query?
Check what is happening in the Rails Server (```$ rails s```) or Rails console.

#####How do you UPDATE a record in AR?

```ruby
> p = Post.first
  Post Load (0.6ms)  SELECT  "posts".* FROM "posts"   
      ORDER BY "posts"."id" ASC LIMIT 1
#=> #<Post id: 1, title: "updatefoo", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 06:16:21"> 

> p.update(:title => "updatefoo")
  Post Load (0.3ms)  SELECT  "posts".* FROM "posts"   
      ORDER BY "posts"."id" ASC LIMIT 1
   (0.2ms)  begin transaction
  SQL (88.5ms)  UPDATE "posts" SET "title" = ?, "updated_at" = ?  
      WHERE "posts"."id" = 1  [
          ["title", "updatefoo"], 
          ["updated_at", "2014-09-24 06:16:21.242536"]
      ]
   (2.5ms)  commit transaction
#=> true

# also
# The longer way
p = Post.first
p.title = "newtitle"
p.save

# The shorter way
p = Post.first
p.update( title: "newtitle" )
```

#####How do you DESTROY a record in AR?

```ruby
> p = Post.last
  Post Load (0.3ms)  SELECT  "posts".* FROM "posts"   
      ORDER BY "posts"."id" DESC LIMIT 1
#=> #<Post id: 22, title: "Foo", author_name: nil, body: "Bar", created_at: "2014-09-24 04:10:25", updated_at: "2014-09-24 04:10:25"> 
> p.destroy
   (0.3ms)  begin transaction
  SQL (0.7ms)  DELETE FROM "posts" WHERE "posts"."id" = ?  [["id", 22]]
   (2.2ms)  commit transaction
#=> #<Post id: 22, title: "Foo", author_name: nil, body: "Bar", created_at: "2014-09-24 04:10:25", updated_at: "2014-09-24 04:10:25">

# destroy all of an models instances
Post.destroy_all
```

#####How do you destroy all the records in AR?

```ruby
Post.destroy_all
```

#####How do you find an object by its ID?
Use the find class method on the model.

```ruby
> p = Post.find(1)
  Post Load (0.5ms)  SELECT  "posts".* FROM "posts"  
    WHERE "posts"."id" = ? LIMIT 1  [["id", 1]]
#=> #<Post id: 1, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31"> 
```

#####How do you find multiple objects by their ID?

```ruby
> p = Post.find([1,4])
  Post Load (0.3ms)  SELECT "posts".* FROM "posts"  
    WHERE "posts"."id" IN (1, 4)
#=> [
        #<Post id: 1, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">, 
        #<Post id: 4, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">
    ] 
```

#####What is returned by Find?
A new object instantiated with the attributes found in the table.

#####How do you find by a specific attribute?

```ruby
# Two syntaxes
Post.find_by(:title => "Foo Title")
Post.find_by_title("Foo Title")

# Search on multiple attributes
Post.find_by(:title => "Foo Title", :author_name => "foo")

# Example
> p = Post.find_by_title("Foo Title")
  Post Load (0.2ms)  SELECT  "posts".* FROM "posts"  WHERE "posts"."title" = 'Foo Title' LIMIT 1
#=> #<Post id: 1, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31"> 
```
Only returns first match b/c it has SQL LIMIT 1 on SELECT query.

#####How do you return ALL the records?

```ruby
> Post.all
  Post Load (0.4ms)  SELECT "posts".* FROM "posts"
#=> #<ActiveRecord::Relation [
        #<Post id: 1, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">,  
        ...
        #<Post id: 10, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">, 
        ...
    ]> 
```

#####What are two ways of writing a WHERE query?

```ruby
 posts = Post.where("created_at > '2014-09-24 04:00:00'")
  Post Load (0.4ms)  SELECT "posts".* FROM "posts"  
    WHERE (created_at > '2014-09-24 04:00:00')
#=> #<ActiveRecord::Relation [#<Post id: 22, title: "Foo", author_name: nil, body: "Bar", created_at: "2014-09-24 04:10:25", updated_at: "2014-09-24 04:10:25">]> 

# Pull out the first result
> p = posts.first
#=> #<Post id: 23, title: nil, author_name: nil, body: nil, created_at: "2014-09-24 09:26:04", updated_at: "2014-09-24 09:26:04"> 

# supply a string
Post.where("id > 3")

# supply a symbol, only works with simple queries
Post.where(:id => 3)
```

#####What does it mean to "substitute your parameters"?
Means you shouldn't hardcode the information in but instead rely on the information that is provided from your input sources (like forms*).

#####When should you do this? How?
When you want to protect against SQL injection attacks

```ruby
# NOT preferred -- directly passing in parameters
Post.where("id = #{params[:id]}")

# Good option: use a string
Post.where( "name = ? AND email = ?", 
            params[:name],params[:email])

# Most legible option: use a hash
Post.where("id = :id", {:id => params[:id]})
```

#####How would you search for records between two dates?
Ruby's own SQL like queries

```ruby
# BETWEEN
> posts = Post.where(:created_at => (Time.now.midnight - 1.day)..Time.now.midnight)
    SELECT "posts".* FROM "posts"  
    WHERE ("posts"."created_at" BETWEEN 
      '2014-08-29 07:00:00.000000' AND '2014-08-30 07:00:00.000000')

# IN
> posts = Post.where(:id => [1,4])
    SELECT "posts".* FROM "posts"  
    WHERE "posts"."id" IN (1, 4)

# NOT
# ...just chain it after the Where
# and pass in the same inputs
> posts = Post.where.not(:id => [1,4])
    SELECT "posts".* FROM "posts"  
        WHERE ("posts"."id" NOT IN (1, 4))
```

#####How do you sort your results?
Use the order method and provide it your search criteria

```ruby
# Defaults to Ascending
Post.order(:created_at)
Post.order("created_at")

# Specify a direction
Post.order(:created_at => :asc)
Post.order("created_at DESC")

# You can order on multiple fields, just like in SQL
Post.order(:created_at => :asc, :author_name => :desc)
Post.order("created_at => DESC, author_name => ASC")
Post.order(:created_at => :desc).order(:author_name => :desc)
```

#####How do you select just a subset of attributes to return?
The return is no longer a full/real objects, but instead are fake representations of the objects and don't have IDs.

```ruby
> Post.select(:title).where("id <= 2")
  Post Load (0.3ms)  SELECT "posts"."title" FROM "posts"  
    WHERE (id <= 2)
#=> #<ActiveRecord::Relation [
        #<Post id: nil, title: "Foo Title">, 
        #<Post id: nil, title: "Foo Title">
    ]> 
```

#####How do you return just unique records?
Use ```DISTINCT``` but the Ruby variety.

```ruby
> unique_post_titles = Post.select(:title).distinct
  Post Load (0.5ms)  SELECT DISTINCT "posts"."title" FROM "posts"
#=> #<ActiveRecord::Relation [
        #<Post id: nil, title: "Foo Title">, 
        #<Post id: nil, title: "some title">, 
        #<Post id: nil, title: "Foo">
    ]>
```

#####How do you return only a certain number of records?
Use ```LIMIT``` or Offset.

```ruby
> three_posts = Post.limit(3)
  Post Load (0.2ms)  SELECT  "posts".* FROM "posts"  LIMIT 3
#=> #<ActiveRecord::Relation [
        #<Post id: 1, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">, 
        #<Post id: 2, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">, 
        #<Post id: 3, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">
    ]> 
```

#####How do you return that number but offsetted by some other number?
Chain the offset method onto it.

```ruby
> three_later_posts = Post.limit(3).offset(2)
  Post Load (0.2ms)  SELECT  "posts".* FROM "posts"  LIMIT 3 OFFSET 2
#=> #<ActiveRecord::Relation [
        #<Post id: 3, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">, 
        #<Post id: 4, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">, 
        #<Post id: 5, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">
    ]> 

```

###Working with Relations
- - -

#####What is a Relation?
A relation is a collection of AR objects.  Like an array but different.

#####What Ruby object does a Relation mimic?
An array

#####Why are Relations useful?
Allows you to run a chain of ActiveRecord methods and not immediately execute them in order like every other chain of normal methods.  Stops every method from immediately running an SQL query.

#####What is "Lazy evaluation"?
A relation is a holding area where you can pile up more and more SQL statements and clauses. It tacks on queries and executes them only when they are actually being called to use.

###Digging Deeper Into Queries
- - - 

#####How do you count up all the Posts?
Produces an integer result.  If you run Count inside the SQL query you will get back a relation.

```ruby
Post.count
#=> 4624
Post.where("created_at > ?",Time.now - 1.day).count
#=> 4
User.avg(:age)
#=> 32
User.where("created_at > ?",Time.now - 1.day).max(:age)
#=> 58

> Post.select("COUNT(author_id), title").where("id < 5").group(:title)
  (0.7ms)  SELECT COUNT(author_id), title 
                FROM "posts"  
                WHERE (id < 5) 
                GROUP BY title
+----+------------+-------+
| id | title      | count |
+----+------------+-------+
|    | Foo Post 0 | 1     |
|    | Foo Post 1 | 1     |
|    | Foo Post 2 | 1     |
|    | Foo Post 3 | 1     |
+----+------------+-------+
4 rows in set
```

#####Does the order in which you put your aggregate functions onto a query chain matter in Active Record?
Yes. If you are running aggregate with AR helpers, you need to run it last since it returns either an integer or hash, not a Relation!

```ruby
 Error!
> Post.where("id < 5").count.group(:title)
   (0.3ms)  SELECT COUNT(*) FROM "posts"  WHERE (id < 5)
#=> NoMethodError: undefined method `group' for 21:Fixnum
```

#####How do you use having in Active Record?
Must combine it with a group.  Can't be used with count and sum (since they return integer). If you want to use with them you'll have to mix it in in the SQL.

```sql
> posts = Post.select("count(*) AS post_count, title")
            .where("id < 25")
            .group(:title)
            .having("post_count >= 1")
    Post Load (0.2ms)  SELECT count(*) AS post_count, title FROM "posts"  WHERE (id < 25) GROUP BY title HAVING post_count >= 1

> posts.each { |p| puts "#{p.title}: #{p.post_count}"}
Foo Title: 19
some title: 1
updatefoo: 1
```

Notice after alias is created it can be called in later methods that work on the Model objects.

#####Can you use having with one of AR's aggregate calculation functions? 
Yes, but you have to include it in an SQL query specifically or it won't work.

#####Why does this break your mental model just a bit?
B/c the sum/count methods don't return relations.

#####How do you execute joins the manual way in AR?
Pass it in as a SQL query string to the ```joins``` method.

```ruby
Post.joins("JOIN users ON posts.author_id = users.id")
```
Otherwise, just call joins with the models name and then the attribute you want to join on.

#####How to you execute your own SQL manually in AR?
Use the ```find_by_sql``` method and pass it a string that is the SQL query.

```ruby
> query = "SELECT *
>             FROM posts
>             WHERE id < 3"
> posts = Post.find_by_sql(query)
  Post Load (0.8ms)  SELECT * FROM posts 
                        WHERE id < 3
#=> [
        #<Post id: 1, title: "updatefoo", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 06:16:21">, 
        #<Post id: 2, title: "Foo Title", author_name: "Foo Name", body: "Lorem Ipsum is simply dummy text of the printing a...", created_at: "2014-09-24 03:05:31", updated_at: "2014-09-24 03:05:31">
    ] 
```
Note: returns normal array filled with Model objects.

#####How do you check whether an object was saved to the DB previously?
Use the ```persisted``` method.

```ruby
p = Post.new
p.persisted?
#=> false
p.save
p.persisted?
#=> true
```

#####How can you ask if there are any records matching the specfied query?
Use the ```exists?``` method.

```ruby
>Post.exists?(1)
    Post Exists (0.2ms)  SELECT  1 AS one FROM "posts"  WHERE "posts"."id" = 1 LIMIT 1
#=> true 
```

#####How can you ask if there are multiple records matching the specified query?
You can use the ```many?``` method or if looking for any match, use the ```any?``` method.

```ruby
# via a model
# ie. "are there any/many posts in the DB?"
Post.any?
Post.many?

# via a relation
# ie. "does this query return any/many posts?"
Post.where(:published => true).any?
Post.where(:published => true).many?
```

#####CODE REVIEW

```ruby
# Aggregate functions
Post.count
User.avg(:age)      # on a single column
Post.where("created_at > ?",Time.now - 1.day).count
User.where("created_at > ?",Time.now - 1.day).max(:age)

# Grouping Aggregate
Post.where("id < 22").group(:title).count

# Aliases (same as SQL)
Post.select("count(*) AS post_count, title")

# Having to filter aggregates
# here: return the titles of those posts
# which are below ID 25 and where the title
# has been repeated more than once
posts = Post.select("count(*) AS post_count, title")
            .where("id < 25")
            .group(:title)
            .having("count(*) >= 2")

# Joining the old-fashioned way
Post.joins("JOIN users ON posts.author_id = users.id")

# Running bare-metal SQL
Post.find_by_sql("SELECT * FROM posts")

p.persisted?    # is this particular object saved?
Post.exists?(3) # is there a Post with ID 3?
Post.any?       # are there any posts?
Post.many?      # are there more than one post in the DB?
Post.where(:published => true).any?
Post.where(:published => true).many?
```