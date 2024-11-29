---
course_name: rdbms
trimester_week: 3
---

### Overview of SQL   
- History   
   
> Sequel was developed as part of the IBM System R project in the early 1970   

> It was later renamed to Structured Query Language and standardized in 1986   

SQL has the following components:   
1. DDL is used to define schema structure, deleting and modifying relations, set and maintain indices for relations and physical storage structure of each relation   
2. DML is used to add and delete and modify tuples   
3. Integrity - DDL contains commands for specifying integrity constraints   
4. View definition - DDL contains commands for defining views   
5. Transaction control commands are used to specify beginning and ending of transactions   
6. Authorization commands are used to specify user access rights to relations and views, this is also part of DDL   
   
## Basic Domain types in SQL   
### char and varchar   
- char(n) is fixed length character string, if shorter string is provided, then extra spaces are appended to make the string n characters long.   
- varchar(n) is variable length character string which can hold maximum n characters, no extra space is appended at the end, this saves space. If the equality of two same char(n) and varchar(n) is checked, then the answer depends on the database implementation.   
   
### int, smallint and numeric, also real, double precision and float   
- int is the standard 4 byte integer   
- smallint is 2 byte integer   
- numeric(p,d) is a decimal point number with total p digits and d digits to the right of decimal   
- real is machine dependent low precision float   
- double precision is machine dependent high precision float   
- float(n) is a float with n digits of precision, n is anything between 1 and 53, the default value is 53. The SQL Server takes any n from 1<=n<=24 as 24 and any value between 25 and 53 is taken as 53.   
   
### date   
- dates stored in YYYY-MM-DD format   
   
### constant   
- a constant is a literal or scalar that represents a specific value   
   
# SQL Commands to define tables and manipulate table data   
SQL keywords are case insensitive, while relation names and attribute names are case sensitive depending on DBMS and OS.   
### create table   
```
create table r (
	A1 D1,
	A2 D2,
	...
	An Dn,
	integrity-constraint_1
	integrity-constraint_2
	...
	integrity-constraint_n
)

```
where r is the name of the relation   
each Ai is an attribute name and Di is the attribute data type   
at the end are integrity constraints   
### integrity constraints   
`primary key(A1, A2, …, An)` is used to set some attributes as primary key   
`foreign key(A1, A2, …, An) references r` is used to set columns A1, A2, … as foreign keys that reference some relations r   
`not null` is specified right after specifying an attribute to mark it so that it doesn't accept null values   
A sample relation definition:   
```
create table instructor (
	id int not null,
	name varchar(25) not null,
	age int,
	course_id varchar(25),
	primary key(id),
	foreign key(course_id) references course
);
```
### insert into table   
To insert a record into table, the syntax is:   
```
insert into table_name(A1, A2, ...)
values(v1, v2, ...)
```
where A1, A2, … are the attributes we want to insert the values for, and v1, v2, … are the tuple values for the specified attributes   
### delete from table   
`insert into` can be replaced by `delete from` to delete tuples from the relation.   
### drop table   
to delete the entire table from memory, use `drop table r`    
### alter table   
`alter table r add A D` is used to add a column named `A` with data type `D` into relation `r`. All the values in the new attribute are assigned null for each tuple.   
`alter table r drop A` is used to drop a column named `A` from the relation `r`. Dropping attributes are not supported by many databases.   
# SQL Queries   
### introduction to select from where   
```
select A1, A2, ...
from r1, r2, ...
where P
```
this is used to select attributes A1, A2, … from the relations r1, r2, … where a certain predicate P is true.   
The select clause corresponds to the projection operator $\Pi$ of relational algebra.   
## select   
### distinct   
the distinct clause after select can be applied to remove duplicate tuples from the query result.   
`select distinct A1, A2, … from table`    
to explicitly specify to not remove duplicates, the `all` keyword is used inplace of distinct   
### select \* is used to select everything from the given relations   
### arithmetic expressions in select   
the select clause can contain arithmetic expressions for operating on attributes.   
for example, if there are two attributes of type `int` A1 and A2 in a relation r, and we want to display the sum of all the tuples in both the attributes, then we can write `select A1+A2 from r`.   
another example, if we have two columns, one with price and one with units sold, and we want to see the total sales of each product, then we can do `select price\*units from product`    
if we want to calculate the monthly average of sales in a year, we can divide the column by a constant 12, `select (price\*units)/12 from product`    
### as   
as clause is used to rename the resulting attribute of a query.   
for example, the last command in the previous section will display the resulting monthly sales attribute heading as `(price\*units)/12`, instead we want to rename it to av\_monthly\_sales. For that, we can write   
`select (price\*units)/12 as av\_monthly\_sales from product`    
general syntax:   
```
select A1 as B1, A2, A3 as B2 from relation
```
## where   
where corresponds to the select $\sigma$ operation in relational algebra.   
### operators for where clause   
where used logical connectives `and` `or` `not`    
and comparison operators `<` `>` `=` `>=` `<=` `<>`    
arithmetic operators `+` `-` `\*` `/`    
continuing from the sales example, to select all the products whose sales was greater than 200000, we can write:   
`select product from sales where (price\*units) >= 200000`   
### String matching in where clause   
String matching is performed using the `like` keyword. Like uses percent and underscore to match strings.   
percent % matches any substring   
underscore \_ matches any character   
to match all the product names containing "dar", we can write:   
```
select * from product
where name like '%dar%'
```
this means that match any substring after or before dar, i.e. any string that contains 'dar'.   
If % itself needs to be matched then we use an escape `\%`    
patterns are case sensitive.   
[[SQL supports other string operations like concatenation, case conversion, extracting substrings and finding string length.]]
## from   
from corresponds to the cartesian product $\times$ of relational algebra   
`select \* from r1, r2` will give the cartesian product of r1 and r2 as described in relational algebra   
the attribute in the resultant relation are named as `relation\_name.attribute\_name` to avoid ambiguity among the attributes of the cartesian join.   
combined with the where clause, the from clause gives a natural join instead of a cartesian join. (because where is $\sigma$, and from is $\times$)   
> To only get the cartesian product where a column $A$ is same in two relations $r_1$ and $r_2$, we will write: select * from r1, r2 where r1.A = r2.A    

### as   
Like the select clause, the from clause also has an `as` which is used to rename relations for usage in the following where clause.   
for example, if there is a university professor table, where we want to list all the professors who have a higher salary than the professors in computer science department, then we can write:   
```
select distinct S.name
from professor as T, professor as S
where T.department = "CS" and T.salary < S.salary
```
This provides a great example for a lot of things   
1. Since we know that the from clause is a cartesian product, we use from clause to perform a cartesian join of the professor table by itself, to avoid ambiguity in the following where clause, we rename the first professor table to T, and the other one to S, their corresponding columns will be renamed as T.colname and S.colname in the cartesian join.   
2. In the where clause, we first select only the professors from the first professor table whose department is computer science, then using the logical and operator we combine it with the condition that the selected professors in the T professor table must have less salary than the S professor table.   
3. In the first line, we select distinctly that we want the names of professors from the second S professor table.   
   
Note: The keyword `as` in the from clause can be omitted, and the same query can be written as follows:   
```
select distinct S.name
from professor T, professor S
where T.department = "CS" and T.salary < S.salary
```
The `as` acts like a rename $\rho$ operation in relational algebra   
## order by   
To sort the resulting table into ascending or descending we use the order by clause   
```
select name, age, salary
from person
order by salary, age desc
```
the above command will sort in descending order based on the salary and age attributes of person   
by default it is sorted in ascending order.
