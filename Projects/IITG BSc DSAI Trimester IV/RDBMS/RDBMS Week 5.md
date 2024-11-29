---
course_name: rdbms
trimester_week: 5
---

### Week 5 structure   
1. Join Expressions   
2. Views   
3. Transactions   
4. Integrity constraints   
5. Index Definition in SQL   
6. Authorization   
   
### recap   
- about sql   
- sql ddl: table creation, deletion, domain types   
- modification of database: insert, update , delete   
- select-from-where   
- basic operations: rename, string matching, set operation, ordering   
- null value handling: check for null values, arithmetic, comparison, boolean operations on null values.   
- aggregate functions: group by, having   
- nested subqueries: subqueries in select-from-where   
   
# Joining Relations   
A join operation is a cartesian product which requires that tuples in the two relations match under some condition. They are usually used as subquery expressions in the **from** clause.   
Three types of Join:   
1. Natural Join   
2. Inner Join   
3. Outer Join   
   
## Natural Join   
Natural Join combines two relations based on the common attributes among them. It only selects the tuples that have the same value for all the common attributes in both relations. Natural Join retains only one copy of each common attribute.   
### Example   
Take two relations:   
| ID |  Name | Department |
|:---|:------|:-----------|
|  1 | Alice |         HR |
|  2 |   Bob |      Sales |
|  3 | Carol |         HR |

| ID | Department | Salary |
|:---|:-----------|:-------|
|  1 |         HR |  50000 |
|  2 |      Sales |  60000 |
|  3 |         IT |  55000 |

In both tables, we have two common columns: **ID** and **Department**.   
When you perform a **natural join** on these tables, SQL will match rows where **both** the  `ID`  and  `Department`  columns have the same values.   
The result will be:   
| ID |  Name | Department | Salary |
|:---|:------|:-----------|:-------|
|  1 | Alice |         HR |  50000 |
|  2 |   Bob |      Sales |  60000 |

### SQL syntax:   
```
select A, B
from r1 natural join r2;
```
This saves us an extra clause, normally we would have to check for equality of common attributes between the common relations in an extra where clause.   
### The order of attributes in the resultant relation   
First, the attributes common to both the relations   
Second, the attributes unique to the schema of first relation   
Finally, the attributes unique to the schema of second relation   
## join-using clause   
### limitation of natural join   
If there are three relations, `r1(A, B, C)` `r2(A, M, N)` `r3(N, B, X)`    
Suppose we want to do a query that is equivalent of:   
```
select A, X
from r1, r2, r3
where r1.A = r2.A and r2.N = r3.N;

```
We want to only select tuples that have the same value of attribute A in r1 and r2, which also have the same value of N in r2 and r3.   
A real life scenario might be having three relations: student, takes, course and trying to find out which student took which courses. We might need to perform something like:   
```
select sname, course_name
from student, takes, course
where student.sid = takes.sid and takes.course_id = course.course_id
```
We might think that the query can be performed using the natural join like so:   
```
select A, X
from r1 natural join r2 natural join r3;

```
But, there is a problem, the attribute B is common between r1 and r3 as well. We only want to perform the first natural join based on the attribute A, and the second natural join based on the attribute N.   
But since B is also common among r1 and r3, the first natural join would give the attributes (A, B, C, M, N), then the second natural join with r3 would be performed based on both B and N, instead of only N, hence we might omit more tuples than desired.   
We need more control over the natural join, this is where the `**using**` clause comes in. `using` helps us select the attributes based on which the natural join would be performed.   
```
select A, X
from (r1 natural join r2) join r3 using N;

```
Notice that instead of natural join, we have used join while joining the result of the first natural join with r3.   
join-using helps us specify which columns to check the equality for while joining two relations.   
The first natural join gives results based on the attribute A, the second join gives results based on just N, not B.   
### on clause   
the on clause is used to specify general predicate condition for join, it is like where in select-from-where, and having for group-by-having   
it can be read like join on condition   
```
select *
from r1 join r2 on condition
```
it can be used with using   
```
select *
from r1 join r2 using A on condition
```
The condition for on clause follows the same predicate logic and arithmetic operators as the where clause   
on can be used as a replacement for using, where we can use on to specify that r1.col = r2.col, but a bit extra steps to write   
on can be used to write complex predicates unlike using which only lets us specify the column name   
## Outer Join   
This works the same way as described in relational algebra, and is used to avoid the loss of information. It first performs the regular join, then it adds the tuples of one relation that did not match the other relation during the join to the final resultant relation. It uses `null` for missing tuple values in these unmatched tuples.   
There are three outer joins and they work the same way as described in Relational Algebra.   
1. left outer join   
2. right outer join   
3. full outer join   
   
### syntax for outer joins   
This is written in the from clause, just like natural join and join-using-on   
```
select something
from r1 natural left outer join r2;

select something
from r1 natural right outer join r2;

select something
from r1 natural full outer join r2;

```
### on and using clause with outer join   
on clause and using clause can be used with outer joins by omitting the `natural` keyword   
```
select *
from r1 left outer join r2 on condition
```
This will join based on the provided conditions, but this will add the unmatched tuples at the end of relation, filling the missing values with null.   
The common attributes in both relations that were not used to perform the join with the using clause will not be merged and will appear as r1.col and r2.col in the resultant relation.   
# Views   
A view can be used to hide the internals of the logical structure of the database.    
1. A view is can be seen as an alias for a query.   
2. Views can act as schemas and can be queried from.   
   
A user role can be defined and a view associated to that user role is configured.   
syntax:   
```
create view v as query
```
where query is any sql query.   
> Is updating relations through views allowed?   

In most SQL implementations: **NO**    
however in the ones where it is allowed, it is only allowed on simple views.   
What is a simple view?   
1. from should have only one database relation   
2. select only has names of attributes and no aggregates, expressions, etc.   
3. the attributes not listed in select clause can be set to null   
4. shouldn't have group by or having clause   
   
# Transactions   
Transaction is a sequence of query or update statements. A transaction is a "unit" of work.   
When any SQL statement is executed, it is implicitly implied that a transaction is taking place. But SQL doesn't know when a transaction ends, hence the user needs to define it explicitly.   
A transaction **must end with** one of the following statements:   
1. **COMMIT **: The updates performed in the transaction should become permanent with the database.   
2. **ROLLBACK** : The updates performed by the SQL statements in the transaction are undone.   
   
### An atomic transaction is either fully executed or rolled back as if it never occurred.   
### Isolation is required in concurrent transactions.   
# Integrity Constraints   
Guards against accidental authorized damaged to the database that might result in data inconsistency.   
Constraints applied to a single relation are as follows:   
1. not null   
2. primary key   
3. foreign key   
4. unique   
5. check   
   
other than not null, every constraint is specified after the attribute definition. check takes in a condition and restricts attributes to receive value that do not satisfy the given condition and unique takes in columns and doesn't let those columns have duplicate values.   
### A note about check constraints   
when null is compared with any value, it gives unknown, and unknown is treated as a valid value.   
If we put a constraint check(prices > 2000), and if the prices attribute receives the value null, then the check constraint will still allow the null value because null > 2000 is unknown and unknown is not the same as the boolean value false.   
check constraint doesn't check if the provided condition is true, it only checks if the provided condition is **not false.**   
to restrict the null value, one must explicitly put `**not null**` constraint after the attribute definition   
## Cascading actions   
For the foreign key constraint, we can set cascades. When a value that the foreign key references is deleted or updated from its primary key in the referenced table, we can specify what to do in the referencing table.   
For each referencing table, after specifying the foreign key we can set cascades like so:   
```
foreign key(A) references r2 on delete cascade
```
this `on delete cascade` will delete the value of attribute A in the current relation if that same value of A in the referenced relation r2 is ever deleted.   
similarly we can write `on update cascade` to update the corresponding value if it changes in the referenced relation.   
we can write `on delete set null` to set all the corresponding values in the foreign key as null values if the value is deleted in primary key.   
we can set default values for a column by specifying `default value` right after declaring the column when creating the table.   
we can write `on delete set default` to set the value of foreign key to default value in case the original value in primary key is deleted.   
there is also `on delete restrict` which restricts the deletion of the value from primary key if it is referenced in the foreign key, or `on update restrict` which restricts the updation.   
# Indexing attributes   
If an attribute needs to be queried multiple times and the table is very large (>1000000), then the query would take a lot of time and would be inefficient.   
An index of an attribute is a data structure that allows us to find the tuples in a relation efficiently without scanning everything.   
An index is created with the create index command:   
```
create index <index-name> on <relation-name> (<attribute-name>)
```
# Authorization   
We sometimes want to restrict certain users to specific actions and specific relations or specific views, to do so, we use the authorization commands to grant or revoke privileges.   
## Granting privileges   
```
grant Pr1, Pr2, Pr3, ...
on r1, r2, r3, ...
to u1, u2, ...
```
Here, Pr1,… are the list of privileges, r1,… are the relations or views we want to restrict the privileges on, and u1,… are the list of users or roles which are being granted the privileges.   
the user or role list can have values of a username, a role name or **public** , where public gives access to all users.   
An example of a privilege is **select, **which gives the user the ability to query the relation or view upon which the privilege is granted.   
### Helpful notes/caveats   
1. Granting privilege on a view doesn't imply granting privilege to the underlying relation the view queries.   
2. The person providing the privilege must already hold the said privilege on the specified item.   
   
## Revoking privileges   
syntax:   
```
revoke Pr1, Pr2, Pr3, ...
on r1, r2, r3, ...
to u1, u2, ...
```
### Helpful notes   
1. the privilege list Pr1,… can be replaced b*y `**all**`* to revoke all privileges from the revokee.   
2. if the revokee list u1,… includes public, all users lose the privileges except those who were explicitly granted privileges.   
3. If the same privilege was granted twice to the same user by different grantees, the user may retain the privilege after the revocation.   
4. All privileges that depend on the to-be-revoked privileges are also revoked.   
   
   
## Roles   
A role can be given privileges and can be assigned to users, other roles.   
Roles can be created through the create role command and privileges can be assigned the same way as for the users.   
```
create role dean ;
grant update (budget) on department to dean ;
grant course_instructor to dean ;
grant dean to Amit ;
```
   
