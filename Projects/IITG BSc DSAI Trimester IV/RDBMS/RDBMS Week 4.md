---
course_name: rdbms
trimester_week: 4
---

## union   
Union corresponds with the union operation in relational algebra.   
Two queries can be unioned to combine their resulting tuples into a single relation and remove duplicates.   
## intersect   
similar to union, the intersect operation is used to get the tuples that are present in both queries   
## except   
this is same as set difference operator in relational algebra    
## null values   
it is possible for a tuple to have null values, values that do not exist or are unknown   
### where clauses can have predicates to select based on the existence of null   
the predicate `is null` can be used to check the existence of null value in a tuple   
the predicate `is not null `is used to check the absence of null   
### operations with null   
any arithmetic operation involving null results in null   
any comparison operation other than `is null`  or ` is not null `result in **unknown**   
### operations with unknown   
|   false | and | unknown |   false |
|:--------|:----|:--------|:--------|
| unknown | and | unknown | unknown |
|    true | and | unknown | unknown |
|   false |  or | unknown | unknown |
| unknown |  or | unknown | unknown |
|    true |  or | unknown |    true |

## basic aggregate functions   
sum, min, max, avg, count   
aggregate functions take a set and return some value based on that set   

# Aggregation using `group by`
  If we want to apply aggregation on tuples by grouping them based on a certain attribute, we use `group by`.

  example:
  ```sql
  select avg(price) from product
  group by product\_name
  ```
  > [!WARNING]
  > While working with `group by` make sure that the attributes that do not appear in the `group by` clause, also do not appear in the `select` clause without an aggregate function around them. In the above example, no other attribute than the product\_name must appear with the select clause without being surrounded by aggregate function.

# `having`
  `having` is used to apply conditions to `group by`, `having` is used to apply conditions to groups instead of tuples. `where` is used to apply conditions to tuples.

  here is an example:
  ```sql
  select avg(price) from product
  group by product\_name
  having avg(price) < 200
  ```

# Nested subqueries
  In `select` `from` and `where`, the predicates can be replaced by a subquery.
  - in `select`, the predicate can be replaced by a subquery that generates a single value. most of the time, this is replaced by `group by`. As an example, if we want to find the number of professors in each department we can either do a `select count(prof\_id) from professor group by department`, or we can do the following:
    ```sql
    select department, (select count(*) from professor where professor.department = department.department) as prof\_count
    from department
    ```
  - in `from`, the predicate can be replaced by any valid subquery indicating that we want to select from the resulting tuples of the subquery. most of the time, this is replaced by `having`.
  - in `where`, any part of the predicate can be replaced by a subquery, the `in` operator is used to match attribute values in the resulting subquery.
    as an example, suppose we have to find the coursed offered in fall 2017 and in spring 2018
    ```sql
    select distinct course\_id
    from class
    where trimester = "fall" and trim\_year = 2017
      and course\_id in (select course\_id
                        from class
                        where trimester = 'spring' and trim\_year = 2018);
    ```
    note that the above operation in this case is same as performing two seperate selects and using `intersect` between them.

# `some` and `all` clauses
  some gives true if any value that satisfies the condition exists in an attribute, all gives true if all values of attribute satisfy the condition.
  some is equivalent to first doint the cartesian join, and then selectively picking tuples based on some condition.

  example usages:
  1. find names of instructors with salary greater than that some **(at least one)** instructor in biology department
  ```sql
  select id
  from instructor
  where salary > some(select salary
                    from instructor
                    where department = 'biology') 
  ```
  2. find name of instructors who have salary greater than all of the instructors in the biology department.
  ```sql
  select id
  from instructor
  where salary > all(select salary
                    from instructor
                    where department = 'biology') 
  ```

# `exists` clause
  this is used in where clause to check if the predicate subquery is non-empty.
  ```sql
  select something from someplace 
  where exists (select another\_thing 
                from another\_place);
  ```

# `not exists` clause
  works in the exact same way as `exists`, but returns false if subquery is non-empty.

### A fairly complex query involving `not exists`
  There are three relations:
  1. student(student\_id, sname, department, credits)
  2. course(course\_id, cname, department, credits)
  3. takes(student\_id, course\_id, semester, year, building, credits)

  We have to select all students who have taken all courses offered in the Biology department.

  ```sql
  select distinct s.student\_id, s.sname
  from student as S
  where not exists ((
    select course\_id from course
    where department = "Biology"
  ) except (
    select T.course\_id from takes as T
    where S.student\_id = T.student\_id
  ));
  ```

  Explanation:
  1. the inner subquery
  ```sql
    select course\_id from course
    where department = "Biology"
  ```
  selects all the course\_id under the biology department

  2. the other inner subquery
  ```sql
    select T.course\_id from takes as T
    where S.student\_id = T.student\_id
  ```
  selects all the course\_id in the takes relation where student\_id of the student matches student\_id of takes, effectively, it selects the course\_id of the courses which the student has taken, if the student has taken all the courses, it will select all the courses.

  3. inner subquery 2 is set-minused from inner subquery 1, therefore, if a student has taken all the courses from biology department, then their course\_id will be selected by the second subquery, and since the first subquery also returns all the course\_id in biology, the set-minus will return an empty table.

  4. the table obtained in step 3 is checked if its empty or not by the `not exists` clause, if the student has taken all the biology courses, the table in step 3 would be empty, and not exists will return true, otherwise false.

  5. Finally, if `not exists` returns true for a student, then that student will be included in the final resulting query, otherwise not. And `not exists` will only return true if the student has taken all the courses in biology as we saw in the fourth step.

# `unique` clause
this is used to check whether a subquery has any duplicate tuples in its result, if it doesn't then `unique` returns true, otherwise false.

### A fairly complex query using `unique`
  We have two relations:
  1. section(course\_id, semester, year, building)
  2. course(course\_id, title, department, credits)

  We want to find out the course\_id of courses that were offered at most once in the year.

  ```sql
  select course\_id
  from course as T
  where unique(
    select course\_id
    from section as R
    where T.course\_id = R.course\_id and R.year = 2017
  )
  ```

  Explanation:
  1. The inner query selects the course\_id where the course\_id of the section relation in the subquery matches the course\_id of the section relation in outer query, if the course was taught more than once, then it will match multiple times in the subquery.

  2. `unique` is performed on the subquery to check whether any tuples are repeating, if the course was taken more than once, the inner tuples from the inner subquery would have duplicating tuples and `unique` will return false and the query will not include the course\_id in the outer relation that was currently being evaluated.


# insertion using select-from-where
  `insert into` can be combined with select-from-where to (conditionally) add multiple records to a table.

  As an example, suppose we have two tables, product and machinery, and product have a complexity attribute. 
  Lets say we want to add any product that has a complexity greater than 5 into the machinery table, we can write a query for it as follows:

  ```sql
  insert into machinery(id)
  select id, complexity
  from product
  where complexity > 5;
  ```

# deletion
  - to delete everything from a relation, use `delete from r`
  - usual deletion works by adding a where clause after `delete from r` to conditionally delete tuples from relation.
  - deletion involving aggregates and their condition requires subqueries
  ```sql
  delete from product
  where price < (select avg(price) from product);
  ```
  - the `in` operator can be used to search through a list of tuples, and can be used while deleting to delete if some attribute's value is in some list. the list item can be generated using a subquery.
  ```sql
  delete from relation
  where something in (list\_item1, list\_item2, …)
  ```

# updating tuples in relation
### simple updates
basic syntax:
```sql
update relation\_name
set attribute\_name = some\_expression;
```
as an example, lets give a 5% salary raise to all the employees.
```sql
update employee
set salary = salary * 1.05;
```
### conditional updates
a where clause can be combined with update-set to conditionally update a tuple if one of its attribute satisfies the condition. syntax is simple:
```sql
update r
set A = something
where condition is true;
```
to include aggregate function in where, as before, one needs to use subqueries, as where clause can not directly use aggregate functions:
```sql
update r
set A = some\_expression
where (select aggregate(something) from somwhere where some\_condition is true);
```

### significance of order of sequence of updates
  Taking a complex problem, say we want to increase the salary of employees who earn 100,000 by 3%, while every other employee must get a raise of 5%. This can be done in two steps:

  **Step 1**:
  ```sql
  update employee
  set salary = salary * 1.03
  where salary > 100000
  ```
  **Step 2**:
  ```sql
  update employee
  set salary = salary * 1.05
  where salary <= 100000
  ```

  pretty straightforward, but can the order of updates be switched, can we do **Step 2** before **Step 1**.

  If we increase the salary of all the employees with salary <= 100000 first, then there might exist some employees whose salary was already closer to 100000 and upon getting a raise, the salary of these employees will become higher than 100000. 

  When we do the second update and increase salary of all the employees whose salary was higher than 100000 by 3%, then the employees who weren't initially higher than 100000, but became higher due to the first increase would also get a raise, this will result in some employees getting a raise two times. Hence the steps cannot be swapped in this situation.

  The order of updates matters.

### case statements
  case statements work similar to switch case in languages like C. They are used to combine multiple updates together and execute updates based on conditions without overlap.

  Taking the same problem from the previous section, we can solve it in one command using case statements.

  ```sql
  update employee
  set salary = case
    when salary <= 100000 then salary * 1.05
    else salary * 1.03
  end;
  ```

  case statements need to end with `end`, conditions can be specified using `when` and value at those conditions can be specified using `then`.

  multiple conditions can be specified as follows:
  ```sql
  case
    when condition1 then result1
    when condition2 then result2
    when conditionN then resultN
    else result
  end; 
  ```   
