# CS336 (DB) Studyguide
**Disclaimer: I do not own any of the images used in this studyguide, all credit goes to the original creators.**
## Entity Relationships
### One to One
Also denoted 1:1, means that one object is related to at most one other object. Think of it as a coupling. Example: spouse - you can either have zero or one spouse.

One to one relationships are generally implemented as attributes of an entity aka columns in a table.
### One to Many
Also denoted 1:n, means that one object can have a relationship with many other objects. On the contary n:1, means many objects have a relationship wiht one object. Example: make and model - a particular make can have many models and many models belong to a single make.

One to many relationships are generally implemented with a parent table and child table which uses foreign keys that reference the parent's primary key.
### Many to Many
Also denoted n:n, means that many objects can have a relationship with many other objects. Example: authors and books, an author can write many books and a book can by written by multiple authors.

Many to many relationships generally require a bridge table to connect the two entities.
## EER Diagrams
EER Diagrams organizes entities and displays their relationships.
![ERD Example](https://www.smartdraw.com/entity-relationship-diagram/img/erd.jpg?bn=1510011143)
### Notation
#### Entity
* Entities: squares
* Attributes: circle
* Relationships: diamond
* Weak Entity: double line

#### Relationships and Cardinality
* No constraints: simple edge 0..*
* At least one relationship for each instance: thick edge 1..*
* At most one relationship for each instance: edge with arrow 0..1
* Exactly one relationship for each instance: think edge with arrow 1..1
![ERD Cardinality Example](https://www.smartdraw.com/entity-relationship-diagram/img/martin-style.jpg?bn=1510011143)

#### Participation
##### Full
Each entity is involved in a relationship, meaning they cannot be null. Represented by double lines.
##### Partial
An entity doesn't always have to be involved in a relationship meaning it could be null. Represented by a single line.
![participation](https://www.tutorialspoint.com/dbms/images/er_relation_participation.png)

#### Inheritance
Represents the connection between parent and sub entities just like in OOP. Designated by a trianglular shape. 
![inheritance](https://www.tutorialride.com/images/dbms/generalization.jpg)

Disjoint entities mean an object(entry) can only be one type of subclass, example: `Class: Animal --> Subclasses: Tiger, Lion, Elephant`. It is not possible for an animal to be a Tiger and a Lion at the same time.

Cover entities means an object(entry) can belong to multiple subclasses, example: `Class: Employee --> Subclasses: Engineer, Manager`. It is possible for an employee to be both an engineer and a manager.

## Datalog
Datalog is a subset of prolog that can be used to simulate a database management system functions such as querying information.

###Syntax
AND denoted as `,`  
OR denoted as `;`  
NOT denoted as `not` or `\`

Facts denoted `<factname>(<item1>,<item2>,...).` can represent entries in a database table.

```prolog
likes(cat, fish).
likes(cat, fish)?
> true
likes(cat, broccoli)?
> false
blue(sky).
blue(sky)?
> true
blue(ground?
> false
```

Rules denoted `<rulename> :- <fact1>, <fact2>...` allow you to construct more facts.

```prolog
likes(ryan, brit)
likes(brit, ryan)
likes(dan, josh)

dating(X, Y) :-
	likes(X, Y),
	likes(Y, X).

friendship(X, Y) :-
	likes(X, Y);
	likes(Y, X).
	
dating(ryan, brit)?
> true
dating(dan, josh)?
> false
```
Here we note that for two people to be dating, they must both like one another. On the otherhand, friendship only requires one person to like the other.
## Relational Algebra
### Selection
Represented by sigma `σ`. Allows you to choose certain columns in a table with an condition to filter the rows extracted. 

Example: Given the following table named People,

| name | age |
|------|-----|
| Jim  | 30  |
| Joe  | 20  |
| Josh | 45  |
**Select everyone older than 25.**  
σ<sub>age > 25</sub>
 
**Using the same table select everyone under 40 with the name Joe.**  
σ<sub>age < 40 ∧ name = 'Joe'</sub>
### Projection
Represented by pie `Π`. Allows us to pick certain columns. We can use it in conjunction with select operator further specify what information we need.

Example: Given the following table named Car,

| make   | model  | year |
|--------|--------|------|
| honda  | accord | 2008 |
| toyota | camry  | 2016 |
| nissan | altima | 2010 |

**Get the make and model of all cars made before 2015.**  
Π<sub>make, model</sub>(σ<sub>age < 2015</sub>)

Note that select and project operators can be nested.

### Note About Duplicates
Note that relational algebra does not have duplicates because it works with sets but SQL does as it works with multisets and bags.

### Cross-Product
The cross product operator `×` combines two relations which is equivilent to merging the columns of two tables. Alone cross products aren't very useful but they are the basis of joins.

Example: We have a Employee, Department and Works_For table

| ename | eid |
|-------|-----|
| Jon   | 111 |
| Sue   | 222 |

| dname       | did |
|-------------|-----|
| Sales       | 001 |
| Engineering | 002 |

| eid | did |
|-----|-----|
| 111 | 001 |
| 222 | 002 |

Works_For × Employee

| Employee.eid | did | ename | Works_For.eid |
|--------------|-----|-------|---------------|
| 111          | 001 | Jon   | 111           |
| 222          | 002 | Sue   | 222           |

Works_For × Department

| eid | Works_For.did | dname       | Department.did |
|-----|---------------|-------------|----------------|
| 111 | 001           | Sales       | 001            |
| 222 | 002           | Engineering | 002            |

### Natural Join
The join operator `⋈` allows you to take the cross product of two tables and enforces equality on attributes with the same name and eliminates one copy of duplicate attributes.

Example: We have a Employee, Department and Works_For table

| ename | eid |
|-------|-----|
| Jon   | 111 |
| Sue   | 222 |

| dname       | did |
|-------------|-----|
| Sales       | 001 |
| Engineering | 002 |

| eid | did |
|-----|-----|
| 111 | 001 |
| 222 | 002 |

**Lets find all of the names and ids of employees that work in department 002.**  
Π<sub>ename, eid</sub>(σ<sub>did = 002</sub>(Employee ⋈ Department))

### Theta Join
The theta join (⋈<sub>θ</sub>) is the same as the natural join but it only keeps tuples that satisfy the theta condition. If we wanted to join two tables A and B but we wanted to only keep tuples where A column X in A is greater than column Y in B, we could do: Π<sub>A.X, B.Y</sub>(σ<sub></sub>(Employee ⋈ Department))

### Union
The union operator `∪` combines all the tuples between two tables and discards the duplicates.

There are 3 rules that must be followed for a union operation to be valid:

1. The two tables must have the same number of attributes
2. The attribute domains must be compatible
3. Duplicate tuples must be removed

### Difference
The set difference is denoted by `-` means that we want the difference between two relations. 

Example: We have tables, Player and Winner

| pid | rank  |
|-----|-------|
| 0   | 5     |
| 1   | 1     |
| 2   | 0     |

| pid | score  |
|-----|--------|
| 0   | 100    |
| 1   | 20     |

**Find all the players that have not won.**  
Π<sub>pid</sub>Player - Π<sub>pid</sub>Student

Sometimes we need to use a join back to get more than one attribute.  
**Find all the players that have not won and their rank.**  
Π<sub>pid, rank</sub>((Π<sub>pid</sub>Player - Π<sub>pid</sub>Student) ⋈ Student)

### Intersection
The intersection symbol `∩` defines a relation that has a set of tuples that are in both tables.

Example: We have tables Marine and Marauder

| name   | hp  | c_shields |
|--------|-----|-----------|
| Jim    | 100 | true      |
| Tychus | 50  | false     |

| name  | hp  | concus |
|-------|-----|--------|
| John  | 200 | true   |
| James | 100 | true   |


**Find hp amounts that are the same between marines and marauders.**  
Π<sub>hp</sub>Marine ∩ Π<sub>hp</sub>Marauder

### Rename
The rename operator `ρ` allows you to rename tables or their columns. This allows you to perform self joins and unify schema differences.

Example: We have the table User

| name  | id  | age |
|-------|-----|-----|
| Jay   | 340 | 20  |
| Jack  | 215 | 25  |
| Jason | 056 | 20  |

**Find the pairs of names of users that are the same age.**  
σ<sub>n1 < n2</sub>(ρ<sub>U1(n1, i1, age)</sub>User ⋈ ρ<sub>U2(n2, i2, age)</sub>User)

### Expression Trees
You can illustrate relational algebra expressions by creating expression trees which show the steps taken for a query.
![expr-tree](http://i.imgur.com/3ycZB83.jpg)

### Further Reading
[https://www.guru99.com/relational-algebra-dbms.html](https://www.guru99.com/relational-algebra-dbms.html)
[https://www.youtube.com/watch?v=GkBf2dZAES0&t=391s](https://www.youtube.com/watch?v=GkBf2dZAES0&t=391s)

## SQL

### SELECT, FROM
The `select` keyword allows you to choose columns from a table. The `from` keyword allows you to choose a table to select from.

```sql
SELECT column1, column2, ... 
FROM table_name;
```

### WHERE
The `where` keyword allows you to specify a condition to filter the rows returned.

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```
### AND, OR, NOT
The `and`, `or`, and `not` keywords supplement the `where` keyword to allow multiple conditionals. They can be combined together.

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;

SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;

SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
```

### ORDER BY
The `order by` keyword allows you to you to order the tuples returned from a query. Defaults to ascending. It is possible to specify multiple columns so they are used to split ties in ordering.

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```

### IN
In allows you to specify multiple values for a `where` statement.

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

### GROUP BY
The group by operator allows you to group identitcal data from a column into a single one and then do an operation on the other non grouped columns, usually aggregate functions.

```sql
SELECT column1, column2
FROM table_name
WHERE [ conditions ]
GROUP BY column1, column2
ORDER BY column1, column2
```

### HAVING
Basically a `where` keyword but to use with aggregate functions.

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

### INSERT INTO
Allow you to insert records into a table.

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

### UPDATE
Allow you to update records in a table.

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### DELETE
Allow you to delete existing records from a table.

```sql
DELETE FROM table_name WHERE condition;
```

### INNER JOIN
Also called equal, or natural joins, it selects tuples with matching values in both tables.

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;
```

### LEFT JOIN
A type of outer join, it returns all of the records from the left table and only the matched records from the right. As a result, some attributes from the left table may be null.

```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
```

### RIGHT JOIN
Also a type of outer join, it returns all of the records from the right table and only matched records from the left. Some attributes from the right table may be null.

```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;
```

### FULL JOIN
Another type of out join, the full join combines both the left and right join which means all values are returned. There may be null attributes on both sides.

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name;
```
### Constraints
#### NOT NULL
This constraint means an attribute cannot be left null.

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```
#### UNIQUE
This constraint means an attribute must be unique.

```sql
CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```

#### Primary Key
An attribute designated primary key must both be not null and unique. It allows a row to be specifically identified.

```sql
CREATE TABLE Persons (
    ID int NOT NULL PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```

##### Composite
A composite primary key means that several columns are used in conjunction to uniquely identify an entity.

```sql
CREATE TABLE Score_Board (
    ContestID int,
    PlayerID int,
    Score int,
    Date date,
    PRIMARY KEY (ContestID, PlayerID, Score, Date),
    FOREIGN KEY (ContestID) REFERENCES Contest (id),
    FOREIGN KEY (PlayerID) REFERENCES Player (id)
);
```

#### Foreign Key
Allows you to reference another table's primary key.

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int NOT NULL,
    PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
);
```
```sql
CREATE TABLE Score_Board (
    ContestID int,
    PlayerID int,
    Score int,
    Date date,
    PRIMARY KEY (ContestID, PlayerID, Score, Date),
    FOREIGN KEY (ContestID) REFERENCES Contest (id),
    FOREIGN KEY (PlayerID) REFERENCES Player (id)
);
```
##### Composite
It's posible to have a composite foreign key.

```sql
CREATE TABLE Viewer (
	ID int PRIMARY KEY,
	ContestID int,
   	PlayerID int,
   	Score int,
   	Date date,
   	Viewers int,
   	FOREIGN KEY (ContestID, PlayerID, Score, Date) 
   	REFERENCES Score_Board (ContestID, PlayerID, Score, Date)
);
```
#### On Delete, On Update
Typically, when deleting an entry that has a primary key that is being referenced by a child entry, there will be an referential integerty violation.
##### Restrict
The default response, the dbms will issue a warning and execution will stop.
##### Cascade
If the parent entry is updated or deleted, the children entry will also be updated or deleted.
##### Set Null
Will set the foreign key attribute in the child that references the parent entry to null.

```sql
CREATE TABLE BRDG_TBL (
	pk int PRIMARY KEY,
	fk1 int,
	fk2 int,
	fk3 int,
	FOREIGN KEY (fk1) REFERENCES TBL1(pk1) ON DELETE RESTRICT ON UPDATE RESTRICT,
	FOREIGN KEY (fk2) REFERENCES TBL2(pk2) ON DELETE CASCADE ON UPDATE CASCADE,
	FOREIGN KEY (fk3) REFERENCES TBL3(pk3) ON DELETE SET NULL ON UPDATE SET NULL
);
```

## ERD To Schema
Our goal is to change the drawn relationships from a diagram into actual tables with attributes in SQL.
### Reification
Reification is the way we achieve the goal of going from ERD to Schema. A good example would be the implentation of inheritance.

![inhr](https://thuruinhttp.files.wordpress.com/2011/08/image4.png)  
Given the following diagram, we implement it several ways.

```sql
-- Denormalized Approach
CREATE TABLE Person (
	id int PRIMARY KEY,
	name varchar(50) NOT NULL,
	age int NOT NULL,
	type enum(Student, Lecturer) NOT NULL,
	grade int,
	salary numeric
);

-- Normalized Approach
CREATE TABLE Person (
	id int PRIMARY KEY,
	name varchar(50) NOT NULL,
	age int NOT NULL,
	type enum(Student, Lecturer) NOT NULL,
);
CREATE TABLE Student (
	sid int PRIMARY KEY,
	pid int,
	grade int,
	FOREIGN KEY pid REFERENCES Person(id)
);
CREATE TABLE Lecturer (
	lid int PRIMARY KEY,
	pid int,
	salary numeric,
	FOREIGN KEY pid REFERENCES Person(id)
);
``` 
Both approaches have their own psotivies and negatives but satisfies the goal of creating a concrete implementation.
### Normalization
Normalization refers to the practice of structuring a rdd so that it reduces data redundancy and improves data integrity.
#### Data Anomalies
When a table has too many columns that are non-essential, there runs the risk of many of them being null which leads to problems.
##### Insert
![daex1](https://277dfx2bm2883ohl6u2g3l59-wpengine.netdna-ssl.com/wp-content/uploads/2014/06/Intro-Insert-Anomaly.png)
In this situation we can't capture any data until we know the EmployeeID because it is the primary key.
##### Update
![daex2](https://277dfx2bm2883ohl6u2g3l59-wpengine.netdna-ssl.com/wp-content/uploads/2014/06/Intro-Update-Anomaly.png)
There is redundant information being stored in the office number column. If on update they are not all changed exactly there will be an update anomoly.
##### Deletion
![daex3](https://277dfx2bm2883ohl6u2g3l59-wpengine.netdna-ssl.com/wp-content/uploads/2014/06/Intro-Deletion-Anomaly.png)
If we delete employee John Hunt, we will lose all information related to him, in our case the New York office. This is probably an unwanted consequence.

**Referenced from: [https://www.essentialsql.com/get-ready-to-learn-sql-database-normalization-explained-in-simple-english/](https://www.essentialsql.com/get-ready-to-learn-sql-database-normalization-explained-in-simple-english/)**