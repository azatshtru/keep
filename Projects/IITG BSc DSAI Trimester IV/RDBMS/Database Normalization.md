---
course: rdbms
week: 8
---
### Lossless decomposition
For a decomposition to be lossless, it should follow at least one of the dependencies stated below:
1. $R_1\cap R_2 \to R_1$
1. $R_1\cap R_2 \to R_2$
The set of common columns of both the decompositions must be a functional dependency of any one of the decompositions.
### Dependency preservation
Let $R_i$ be a decomposed relation of some larger relation $R$. Let $F^+$ be the closure of functional dependencies of $R$.
Let $F_i$ be a set of dependencies associated to a decomposed relation $R_i$.
A set of decomposition of $R$, i.e. $\{R_1, R_2, ..., R_n\}$ is dependency preserving if and only if
$$(F_1\cup F_2 \cup ...  \cup F_n)^+ = F^+$$
A set of decomposition of $R$ is dependency preserving if the closure of union of functional dependencies of all the decompositions of $R$ is equal to the closure of the functional dependencies of the entire relation $R$.


Normal forms describe the state of data integrity and redundancy in data. Higher the normalization, more integrated and less redundant the database design.

# 1NF
A relation is 1NF if all attributes of it are atomic. An attribute is atomic if it disallows composite and multivalued attributes. To turn a relation into 1NF, remove the attributes that violate 1NF and put them into separate relations.
# 2NF
A relation is 2NF if it is 1NF and every non-candidate attribute is fully functionally dependent on the candidate key. If some attributes are dependent on a subset of candidate key, then the relation can be decomposed into new relations containing the subsets of candidate key and their dependent attributes.
As an example take the relation $R(\underline A, \underline B, C, D, E, F, G)$
Here, let $AB$ be the candidate key and let the functional dependencies of this relation be
1. $A\to CD$
2. $B \to EF$
3. $AB \to G$
Since C and D only depend on A and not on B, and E and F only depend on B, not on A, this relation can be decomposed so that the non-candidate key attributes in the decompositions completely depend on the candidate key. Therefore the decomposition is:
1. $R_1(\underline A, C, D)$
1. $R_2(\underline B, E, F)$
1. $R_3(\underline A, \underline B, G)$
In $R_1$, the non candidate key attributes $C$ and $D$ completely depend on $A$, similarly in $R_2$, the $E$ and $F$ completely depend on $B$, and same for $R_3$, hence this decomposition is in 2NF form.

# 3NF
If the relation is 2NF and no attribute is transitively dependent on the candidate key.
Mathematically, For any two subsets of attributes $a$ and $b$, such that $a\to b$ is in $F^+$, any of the following holds:
1. $a \to b$ is trivial FD
2. $a$ is a superkey of $R$
3. Each attribute in the set difference of $b-a$ is in the candidate key for $R$
To make schema 3NF:
1. Identify non-key attributes that depend on other non-key attributes.
2. Create separate tables for these dependencies.

Example,
Let a relation be $R(\underline A, B, C, D, E)$ and FDs on it be:
1. $A\to BC$
2. $C\to DE$
All of the non-candidate attributes $BCDE$ are dependent on the candidate key $A$, therefore the relation is in 2NF. $DE$ is functionally dependent on $A$, not directly but transitively through $C$. therefore this not in 3NF and can be decomposed into the following:
1. $R_1(\underline A, B, C)$
2. $R_2(\underline C, D, E)$
In the decompositions ($R_1$ and $R_2$ respectively), the non-candidate key attributes ($BC$ and $DE$ respectively) are directly dependent on the candidate keys ($A$ and $C$ respectively).

# BCNF
Boyce Codd Normal Form follows 3NF and for every subsets of columns $a$ and $b$, such that $a \to b$ is a FD, $a$ must be a superkey of the relation.
To convert schema to BCNF, decompose where the violation of candidate key happens, then add the remaining attributes to preserve some of the candidate keys.

Example,
Let there be a relation $R(A, B, C)$ with functional dependencies:
1. $B\to C$
2. $AC \to B$
Candidate keys can be $AC$ or $AB$. Whatever we pick to be the primary key from these candidate keys, 3NF will be satisfied for this relation because both $AB$ and $AC$ can be used to determine $ABC$ non-transitively.

In $AC\to B$, $AC$ is a candidate key, therefore $AC\to B$ does not violate BCNF.
In $B \to C$, $B$ can only determine $C$ and not $A$, therefore $B$ is not a superkey. BCNF requires every functional dependency to be a superkey (recall the definition of BCNF above). Therefore $B\to C$ violates BCNF.

To resolve to BCNF, we decompose where the violation of candidate key happens. Since $B\to C$ violates BCNF, we make a relation $R_1(\underline B, C)$ where $B$ is the superkey of $R_1$.

The remaining attribute $A$ needs to be modelled in a separate relation such that it preserves a candidate key. We have two possible candidate keys $AC$ and $AB$ as given above.

If we make a relation $R_2(\underline A, \underline C)$, then we will preserve the candidate key $AC$, but we will lose the ability to connect $R_1$ to $R_2$ since $R_1$ contains $B$ as a primary key which has no corresponding foreign key in $R_2$. We get: $R_1 \cap R_2 = \phi$ which doesn't satisfy the conditions for lossless decompositions as described above.

Instead if we make $R_2(\underline A, \underline B)$, then we preserve the candidate key $AB$ instead, and since attribute $B$ in relation $R_2$ can be used to connect to the primary key $B$ in $R_1$. We get $R_1 \cap R_2 = B$, since $B$ is a functional dependency of $R_1$, i.e. $B\to R_1$, this implies $R_1 \cap R_2 \to R_1$. Hence, $R_2(\underline A, \underline B)$ satisfies the conditions for lossless decomposition and is a valid decomposition.

Finally the decompositions to BCNF are:
1. $R_1(\underline B, C)$
2. $R_2(\underline A, \underline B)$

Notice that while the functional dependency $B\to C$ is preserved, $AC\to B$ is not preserved. Also, the candidate key $AC$ is lost because it became redundant based on the functional dependencies. However, while the preservation of functional dependencies is ideal, it is not mandatory.

# 4NF
If set is in 3NF and for every multivalued dependency $A\to\to B$, $A$ is a superkey. 4NF removes multivalued dependencies by decomposing the relation.

As an example, the following relation with multiple dependency $\text{Student\_ID}\to\to\text{Course}$

| Student_ID | Course  | Activity |
| ---------- | ------- | -------- |
| S1         | Math    | Soccer   |
| S1         | Math    | Debate   |
| S1         | Science | Soccer   |
| S1         | Science | Debate   |
can be decomposed into:
1. **Student-Course Table**:

| Student_ID | Course  |
| ---------- | ------- |
| S1         | Math    |
| S1         | Science |
2.  **Student-Activity Table**:

| Student_ID | Activity |
| ---------- | -------- |
| S1         | Soccer   |
| S1         | Debate   |
