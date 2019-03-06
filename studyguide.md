# CS336 (DB) Studyguide
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
* Attribute: circle
* Relationship: diamond

#### Relationship
* No constraints: simple edge 0..*
* At least one relationship for each instance: thick edge 1..*
* At most one relationship for each instance: edge with arrow 0..1
* Exactly one relationship for each instance: think edge with arrow 1..1

#### Cardinality
Cardinality is designated by two numbers at the ends of the line linking a entity to a relationship.

![ERD Cardinality Example](https://www.smartdraw.com/entity-relationship-diagram/img/martin-style.jpg?bn=1510011143)

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
```
Here we note that for two people to be dating, they must both like one another. On the otherhand, friendship only requires one person to like the other.
