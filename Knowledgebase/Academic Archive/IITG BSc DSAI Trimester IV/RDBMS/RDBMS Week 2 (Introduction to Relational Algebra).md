---
course_name: rdbms
trimester_week: 2
---

## Relational Query Language   
A qwuery language is used to request information from a database.   
There are two types of query languages: procedural and non-procedural (declarative).   
In procedural query language, the user provides the procedure and instructions to perform a series of operations on the database to calculate the desired result. example: **relational algebra, PL/SQL**   
In non-procedural (declarative) language, the user describes the desired information without giving specific procedure for obtaining the information. example: **relational calculus, SQL**.   
## What is relational algebra?   
Relational algebra provides theoretical foundations for Relational Databases and SQL.   
Some basic operations of relational algebra:   
1. select $\sigma$   
2. project $\Pi$   
3. cartesian product $\times$   
4. natural join $\bowtie$   
5. union $\cup$   
6. intersection $\cap$   
7. set difference $-$   
8. assignment $\leftarrow$   
9. rename $\rho$   
   
These takes relations as input and give other relations as outputs.   
## select $\sigma$   

$$
\sigma_p(r)
$$
The select operation takes in a relation $r$ as input and under a predicate $p$, selects the tuples from the relation that satisfy the predicate as output.   
For example, $\sigma_{\text{dept\_name='physics'}}(\text{instructor})$ takes in the instructor relation as input under the predicate that the selected tuples must have `dept\_name = "physics"`. If only two tuples contain dept\_name as physics, then the select operation will return a relation containing only those two tuples.   
### predicate operators   
The predicate is written as a set of conditions.   
The conditions use comparison operators: $=, \gt, \lt, \leq, \geq, \neq$   
some sample conditions: `experience < 5, dept\_name = "physics", etc.`    
Conditions can be combined using connectives: $\land, \lor, \lnot$   
for example if we're looking for the professors with less than 5 years of experience in the physics department, we'd write:   
`experience < 5 $\land$ dept\_name = "physics`"   
### comparisons between two attributes   
We can also compare two attributes in the predicate.   
For example if we want to know how many department names match their building name and there are two attributes called `dept\_name`  and `building\_name`, then the predicate will be: `dept\_name=building\_name`   
And the entire query to get the professors who have more than 6 years of experience in the departments whose building name is same as the department name itself would look like:   

$$
\sigma_{\text{dept\_name=building\_name }\land\text{ years > 6}}(\text{instructors})
$$
## project $\Pi$   
Project is used to select specified attributes from the relation.   
For example, if there is a professors table with 5 attributes and we only want to look at the names and ages of professors, then $\Pi\_{\text{name, age}}(\text{professors})$ will output a relation that only contains two columns: name and age.   
## Compositions of relational algebra operators   
Taking an example, if we want to find the name and age of all the professors whose salary is greater than 85000, and the professors table contains five columns (id, name, age, salary, department), then we can compose the project and select operators from above to get the resultant relation as follows:   

$$
\Pi_{\text{name, age}}(\sigma_{\text{salary>85000}}(\text{professors}))
$$
The **select ($\sigma$)** operation inside will operate first and will give an intermediate table that will be fed to the **project ($\Pi$)** operator outside.   
Note that $\Pi(\sigma( r ))$ is valid, because the output of select ($\sigma$) includes the name and age attributes that the project ($\Pi$) operator can choose. But if this was other way around, i.e. $\sigma(\Pi( r ))$, then the project operator inside would have only selected the name and age attributes and the select operator outside would not have been able to filter the tuples based on salary attribute since it wasn't selected by the project operation inside.   
Operations cannot be composed in any order and special care must be taken. If the project operation was selecting the salary as well, then the order of composition wouldn't have mattered.   
## cartesian product $\times$   
Taking an example,   
There are two tables $X(A, B)$ and $Y(C, D, E)$; The table X has two attributes A and B; The table Y has three attributes C, D and E.   
Let there be three tuples in the relation X:   
|    $A$ |    $B$ |
|:-------|:-------|
| $A\_1$ | $B\_1$ |
| $A\_2$ | $B\_2$ |
| $A\_3$ | $B\_3$ |

Let there be two tuples in relation Y:   
|    $C$ |    $D$ |    $E$ |
|:-------|:-------|:-------|
| $C\_1$ | $D\_1$ | $E\_1$ |
| $C\_2$ | $D\_2$ | $E\_2$ |

The cartesian product of the above two relations $X \times Y$ will be all the possible pairings of the tuples in both the tables; a table with 5 columns (A, B, C, D, E) and 6 rows.   
The first tuple of $X$, i.e. $(A\_1, B\_1)$ will pair with both the tuples of $Y$, i.e. $(C\_1, D\_1, E\_1)$ and $(C\_2, D\_2, E\_2)$:   
|      A |      B |      C |      D |      E |
|:-------|:-------|:-------|:-------|:-------|
| $A\_1$ | $B\_1$ | $C\_1$ | $D\_1$ | $E\_1$ |
| $A\_1$ | $B\_1$ | $C\_2$ | $D\_2$ | $E\_2$ |

In a similar fashion, the second tuple of $X$, ($A\_2, B\_2$) will pair with both the tuples of $Y$.   
And same for the third tuple of $X$, it will also pair with both the tuples of $Y$.   
Since $X$ has three tuples, and each of them pair with the two tuple of $Y$, the tuples in the resulting table are $2\times3=6$   
The resulting table is:   
   
|      A |      B |      C |      D |      E |
|:-------|:-------|:-------|:-------|:-------|
| $A\_1$ | $B\_1$ | $C\_1$ | $D\_1$ | $E\_1$ |
| $A\_1$ | $B\_1$ | $C\_2$ | $D\_2$ | $E\_2$ |
| $A\_2$ | $B\_2$ | $C\_1$ | $D\_1$ | $E\_1$ |
| $A\_2$ | $B\_2$ | $C\_2$ | $D\_2$ | $E\_2$ |
| $A\_3$ | $B\_3$ | $C\_1$ | $D\_1$ | $E\_1$ |
| $A\_3$ | $B\_3$ | $C\_2$ | $D\_2$ | $E\_2$ |

### Selective Cartesian Product   
Sometimes, we do not want every possible pairing of the tuples in the two tables, for example, if there are two relations: a professor relation and a course relation, doing the cartesian product among those will pair every professor with every course, which is undesired.   
To resolve this, after performing a cartesian product, we can use the output relation of the cartesian product as input to a select operator under a predicate which will remove any undesired tuples of the cartesian product.   
For example, if the professor table has a course ID attribute and the courses table also has a course ID attribute for every course, and we cross join (perform a cartesian product on) the tables, then we only want to select the tuples from the cartesian product that have the professors table's course ID same as the courses table's course ID.   
For two relations $r, s$, the selective cartesian product based on some predicate $\theta$ can be written as:   

$$
\sigma_\theta(r \times s)
$$
## Natural Join $\bowtie$   
The natural join is the selective cartesian product, just using a fancy bowtie symbol.   
For two relations $r, s$, the natural join operation under some predicate $\theta$ is defined as:   

$$
r\bowtie_\theta s=\sigma_\theta(r \times s)
$$
### Associativity of Natural Join   
Natural join is naturally (*get it?*) associative, which means for three relations $A, B, C$, we have:   

$$
A\bowtie(B\bowtie C)=(A\bowtie B)\bowtie C
$$
### "verbs as relations"   
A good practice is to keep the "noun" tables independent from each other and join them using "verb" tables.   
For example instead of the professor table containing an attribute for the course ID which will hard link the courses table and the professor table, we should create a teaches table instead which will do the sole job of determining which professor teaches which course.   
The three tables can be described as:   
Professor(prof\_id, name, department, salary)   
Teaches(prof\_id, course\_id, semester)   
Course(course\_id, course\_name, department, credits)   
Instead of the professor table storing a direct reference to the course\_id, we have completely delinked the courses and the professors table. Only the Teaches table has information about linking the both, effectively making professors and courses independent and managing everything easier.   
Because if a professor is not teaching any course currently and the professor table has a course\_id attribute, then that attribute will be empty for the non-teaching professor's tuple, which is not a good thing to have. Creating a verb table like Teaches prevents many other issues like that.   
Whenever we want to see what professor teaches what course we can do a simple natural join of the three tables based on the predicate that prof\_id and course\_id gotta be the same across tables.   
### Outer Join   
If there is an instructor that isn't teaching any course currently, then that will lead to the natural join operation ignoring the tuple for that professor across the tables. In many cases this isn't a problem, but if it is required to have information about all the professors, or in a general case if it is required to have information about a set of tuples that would be otherwise ignored under the predicate in natural join, one uses the outer join operation to account for those.   
There are three types of outer join operations:   
1. Left outer join $⟕$   
2. Right outer join $⟖$   
3. Full outer join $⟗$   
   
Each outer join will make sure that all of the respective table's attributes are present in the resultant joined table, albeit, with null values.   
For example, the left join $r \space ⟕\space s$ for two relations $r, s$ will first do a regular natural join, then it will take all the tuples of the left relation $r$ that were not included in the natural join, and it will fill those unincluded tuples with null values for all the right relation attributes in the join and then it will add those tuples to the result of the natural join.   
Applying the same logic to the right join, it will include all the right relation's columns, the ones that are not included in the natural join will still be added with the corresponding attributes of the left relation as null.   
For a full outer join, all the attributes from the left and right relation will be added and the ones that were not included in the natural join will have null as attribute values for the attributes of the opposite relation.   
## set operations   
If two relations have the same number of attributes (arity), and the domains (types) of all the attributes are compatible in both the tables, then set union, intersection and difference can be performed among those relations' tuples.   
### set union $\cup$   
set union of two relations $r, s$ will take all the tuples from both the relations and return them in a single relation by removing duplicates. It is written as $r \cup s$   
### set intersection $\cap$   
set intersection will give only the tuples that appear in both relations.   
### set difference $-$   
set difference will give only the tuples that appear in one relation but not in the other.   
## assignment operator $←$   
Relational algebra expressions can get too long sometimes, if a result of any complex relational operation needs to be used multiple times, then the expression becomes too unreadable very quickly.    
Relational operators can be used to assign intermediate results to symbols or aliases. These symbols and aliases can be used to refer to the intermediate result in the expressions that follow.   
A query can be written as a series of assignments followed by a relational expression.   
For example, if someone wants to find the names of all the professors in physics and music departments, then:   

$$
\text{phy} \leftarrow \sigma_{\text{department=`physics'}}(\text{professors})
\\\text{mus} \leftarrow \sigma_{\text{department=`music'}}(\text{professors})
\\\Pi_{\text{prof\_name}}(\text{phy}\cup\text{mus})
$$
The intermediate select queries being too long were assigned to two aliases `phy`  and `mus`. Which were later used in the final query.   
## rename $\rho$   
While the assignment operator is used purely as a cosmetic renaming of relational expressions and relations to make readability easier, the rename operator serves as a functional way to resolve ambiguities while performing relational algebra.   
Imagine if there is a professor table with 5 attributes like so:   

$$
\text{professor(id, name, department, age, salary)}
$$
And we want to find names of all the professors who earn more than the professor whose `id` is 12112.   
A way one might approach this is by first using the select operator to get a relation that contains just the tuple of the professor 12112. Then doing the natural join of 12112's one-tuple relation with the entire professor relation under the predicate that the salary must be greater than 12112's salary. Then finally we can get just the names of all those highly paid professor by doing a project operation.   
This way has a problem, which is that both, the selected 12112's one-tuple relation and the professor table contain attributes with same aliases and hence both contain a salary attribute. Therefore, when performing the natural join, we don't know how to specify which relation's salary must be greater than the other. To resolve this issue, rename operation comes in the picture.   
Before doing the natural join or select, we rename the relations while selecting:   

$$
\sigma_{\text{W.id=12112}}(\rho_{\text W}(\text{professor}))
$$
This expression above means that prior to selecting the professor whose id is 12112 from the professor table, we are renaming the professor table from which the selection is happening to $W$ and we are saying that the id attribute from this $W$ table must be equal to 12112.   
Similarly, while performing the natural join, we can rename the other professor table to, say $Y$:   

$$
(\rho_{\text Y}(\text{professor}))\Join_{\text{Y.salary > W.salary}}(\sigma_{\text{W.id=12112}}(\rho_{\text W}(\text{professor})))
$$
The above operation first renames the professor table to $Y$, then it performs a natural join with the relation obtained from the select operation in which we renamed the professor table to $W$.   
This natural join happens under the predicate that salary from the $Y$ professor table shall be larger than the salary from the $W$ professor table. (remember that the W professor table just contains one tuple, which is the professor whose id is 12112).   
Finally, to get just the names of the professors who are earning higher than 12112, we can perform a project operation with the names from the $Y$ professor table.   
The final query would look like:   

$$
\Pi_{\text{Y.name}}((\rho_{\text Y}(\text{professor}))\Join_{\text{Y.salary > W.salary}}(\sigma_{\text{W.id=12112}}(\rho_{\text W}(\text{professor}))))
$$
The rename operation can be used to rename relations or attributes of relations.   
If we want to rename the relation $r$ to $W$, we will write:   

$$
\rho_W( r )
$$
If we want to rename the attributes of the relation $r$ to $A\_1, A\_2,\dots,A\_n$, then we will write:   

$$
\rho_{(A_1, A_2,...,A_n)}( r )
$$
If we want to rename both the relation and its attributes, then we will write:   

$$
\rho_{W(A_1, A_2,...,A_n)}( r )
$$
Two syntactically different queries that give the same result are called equivalent queries   
   
