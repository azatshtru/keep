---
course_name: rdbms
trimester_week: 1
---

### Substructure   
1. Structure of Relation Schema   
2. Relational Database Schema   
3. Different Keys   
4. Schema Diagrams   
   
A table is also called relation and the words can be used interchangeably.   
# Relation Schema structure and Instance   
Relations schema defines the design and structure of a relation or table in the database.   
$A\_1, A\_2,\dots , A\_n$ are attributes.   
$R(A\_1, A\_2,\dots , A\_n)$ is a relation schema   
Example: instructor = (ID, name, dept\_name)   
The rows in a table are also called tuples. In the instructor table defined by the schema above, every instructor is represented by a different tuple and every instructor holds the three specified attributes in the tuple. Order of rows, i.e. which row comes after which is not relevant in a relation.   
### Instance   
At any point of time whatever the table contains is called the ***Instance*** of the relation at that time.   
### Structure of a Relational Database   
A relational database consists of a collections of relations (or tables) each of which is assigned a unique name. For example, in a database for a university, there can be multiple tables, an Instructor table, Student Record table, Department Info table, etc. with some shared attributes that connect those tables together.   
### Attribute domain   
The set of allowed values for each attribute is called the domain of that attribute (for example, the salary attribute cannot be less than zero). Normal attribute values are indivisible and represent a single data, in other words they are atomic.   
Non-atomic attribute domains can also exist, for example, a contact\_numbers attribute can store multiple phone numbers.   
### Null values   
A special value "NULL" is a member of every domain. It indicates that the value is unknown. Avoid the null value as much as possible.   
# Keys   
Keys are used to uniquely identify a tuple in a relation or to connect two relations.    
## Super Key   
Let $R(A\_1, A\_2,…,A\_n)$ be a relation schema.   
Let $K\subset R$ be a subset of attributes.   
Then, $K$ is a superkey of $R$ if values of $K$ are sufficient to identify a unique tuple in $R$.   
A basic assumption for all relations is that duplicates of tuples cannot exist, hence, every tuple in some way is uniquely identifiable.   
For example, in the university instructor relation, the Instructor\_ID can be super key, the subset of { Instructor\_ID, Instructor\_name } can be another super key, the subset of {Instructor\_ID, name, department} can be yet another superkey, because they can all be used to uniquely identify any instructor tuple, but {Instructor\_name, department} cannot be a superkey because two instructors with the same name could be working in the same departments, hence not uniquely identifiable.   
## Candidate Key   
A Superkey is a candidate key if it is minimal, i.e., it contains the minimal number of attributes to uniquely identify the tuple.   
In the university instructor example, the Instructor\_ID in itself is a candidate key.   
## Primary Key   
One of the candidate key is selected as a primary key based on a few properties:   
1. The attribute values should never or very rarely change.   
   
While writing relations, the primary key attributes are usually underlined.   
instructor(id, name, department, years of experience)   
since ID and name are unique, and rarely change, they can be taken as primary key.   
## Foreign Key   
Foreign key is a set of attributes in a table that refers to the primary key of another table, effectively linking the two tables. The table that is being referred to is called the referenced relation, the table that is referring to the referenced table is called the referencing relation.   
### Foreign Key constraint   
Value in the referencing relation must appear in the referenced relation.   
### Referential integrity constraint   
The values appearing in specified attributes of any tuple in the referencing relation must appear in specified **non-primary** key attributes of the referenced relation.   
While foreign key constraint and referential integrity constraint sound almost similar, they have a very small difference that is in foreign key constraint, only primary key attribute's values must exist, while in referential integrity, any referenced attribute's values must exist in across different tables.   
Sometimes, the primary key can consist of multiple attributes, if a table references just one of those attributes, then it is not referencing the entire primary key, hence the foreign key constraint is not being applied, but since that attribute is a part of the primary key, then it must hold all the values the foreign key is referencing, hence the need for referential integrity constraint.   
# Database schema   
Database schema is a logical structure of the whole database.   
Database instance is a snapshot of the data in the database at any given instant.   
> example of University Database Schema:   

- instructor(ID, name, dept\_name, salary)   
- student(ID, name, dept\_name, total\_cred)   
- department(dept\_name, building, budget)   
- advisor(s\_id, i\_id)   
- takes(ID, course\_id, sec\_id, semester, year, grade)   
- classroom(building, room\_number, capacity)   
- time\_slot(time\_slot\_id, day, start\_time, end\_time)   
- teaches(ID, course\_id, sec\_id, semester, year)   
- section(course\_id, sec\_id, semester, year, building, room\_number, time\_slot\_id)   
   
A schema diagram is used to describe the Database schema. [Refer to the Week 1's last video: Schema diagram and conclusion at the 2-5 minute mark]   
