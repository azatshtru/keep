---
course: rdbms
week: 7
---
#rdbms #database-design
#### two ways to design data
1. bottom up (design by synthesis), consider all the relationships between all the attributes at once.
2. top down (design by analysis), build a table naturally, decompose if required.
### Checking if the decomposition is good
Parts made from a lossy decomposition of a relation cannot be combined back to make the original table. Mathematically, $R \subset \Pi_{R_1}(R)\bowtie \Pi_{R_2}(R)$. 
For a decomposition to be lossless, the natural join of the decomposed parts should result in the original relation. Mathematically, $R=\Pi_{R_1}(R)\bowtie \Pi_{R_2}(R)$.
A way to decide if a database design in "good" is checking the **Functional dependencies**.
## Formal definition of Functional dependency
Let $a$ be a set of columns of some relation $R$, and $b$ be another.
The functional dependency $a\to b$ holds **if and only if** for all instances of the relation, the values of all the tuples in the columns of $a$ uniquely determine the values of the columns of $b$.
Functional dependencies can be used to derive constraints on a relation (including key constraints), and they can be used to check if an instance of a relation follows the constraints.
### Definition of Superkey and Candidate key using Functional dependencies
A set of columns $K$ is a superkey of a schema $R$, if and only if $K\to R$, i.e. the columns of K uniquely determine the values of all the columns of R.
For a set of columns $K$ to be a candidate key, $K\to R$ and $a\to R\space\nexists\space a \subset K$, i.e. no subset $a$ of columns $K$ can uniquely determine $R$, only the entirety of $K$ can, in other words, K is minimal superkey.
### Trivial FD
A column $A$ is a functional dependency on itself ($A\to A$), in fact, any set of columns of which $A$ is a part is a functional dependency of itself ($AB\to A$).
In general, A functional dependency for two subset of columns $a$ and $b$, such that $a\to b$, is said to be **trivial** if $b\subset a$.
### Closure of set of FDs
If we have a set of functional dependencies, say $F$. Then the **closure** of $F$, say $F^+$, is the set of all the functional dependencies that are logically implied by $F$.
Elements of $F^+$ can be computed by using **Armstrong's Axioms**.
#### Armstrong's axioms
A functional dependency is relflexive, augmented, and transitive. Take three subsets of columns of a relation $R$, say $a$, $b$ and $c$, then we can define the axioms.
1. Reflexivity: If $b\subseteq a$, then $a \to b$.
2. Augmentation: If $a \to b$, then $ac \to bc$, where $ac$ is the concatenations of columns of $a$ and $c$. and $bc$ is the concatenation of $b$ and $c$.
3. Transitivity: If $a\to b$ and $b\to c$, then $a \to c$.
Some additional rules can be derived from these axioms:
1. Union: If $a\to b$ and $a\to c$, then $a\to bc$
2. Decomposition: If $a\to bc$, then $a\to b$ and $a\to c$
3. Pseudo-transitivity: If $a\to b$  and $cb \to d$, then $ac\to d$.
#### An example using Armstrong axioms to derive elements of $F^+$
$R=(A, B, C, G, H, I)$
Let there be a set of functional dependency $F$ defined on $R$:
$$F = \{A\to B, A\to C, CG\to H, CG\to I, B\to H\}$$
This logical implies:
1. $A\to H$, by transitivity ($A\to B, B\to H$)
2. $AG \to I$, by augmentation and transitivity($A\to C, AG\to C\to CG, CG\to I$)
3. $CG\to HI$, by augmentation and transitivity (do it yourself.)
### Lossless Decomposition Check using FDs
For a decomposition to be lossless, it should follow at least one of the dependencies stated below:
1. $R_1\cap R_2 \to R_1$
1. $R_1\cap R_2 \to R_2$
The set of common columns of both the decompositions must be a functional dependency of any one of the decompositions.

> To test FDs, sometimes a lot of computationally expensive cartesian products need to be performed. A decomposition can be performed such that it becomes very computationally hard to check or enforce FDs, this type of decomposition doesn't preserve FDs.
### Closure of Attribute sets
Given a set of attributes $\alpha$, the closure of $\alpha$, i.e. $\alpha^+$ is the set of attributes that are logically implied by the functional dependencies of the relation of which $\alpha$ is a subset of.

Uses of Closures of attribute sets:
1. To check if $\alpha$ is the superkey, check if $\alpha^+$ contains all attributes of $R$.
2. To check if $\alpha$ is a candidate key, check if $\alpha$ is the superkey and no subset of $\alpha$ is a superkey itself.
3. To check if $\alpha \to \beta$ holds, just check $\beta \subseteq \alpha^+$
4. To find the closure of functional dependencies, i.e. $F^+$, for each attribute $A_i$ of $R$, compute union of all $A_i^+$
### Canonical cover of FDs
A functional dependency in $F$ is **extraneous** if it can be removed without changing $F^+$. The set of functional dependencies made by removing these redundant dependencies is called the canonical cover of $F$. Canonical covers require less computation to check for violations of $F$. Canonical covers are denoted by $F_c$
#### weaker and stronger reduction of functional dependencies
If after reducing a dependency, the resulting dependency cannot directly infer the original dependency without the help of other dependencies in $F_c$ , then the resulting dependency is called a weak result.

for example, let $F = \{A\to B, B\to C, A\to CD\}$. The last dependency can be decomposed as $A\to C$ and $A\to D$. Since the first two dependencies in $F$ imply $A\to C$ indirectly, we can remove it from the last dependency. The resulting canonical cover $F_c = \{A\to B, B\to C, A\to D\}$ has $A\to D$ as the last dependency. $A\to D$ cannot directly imply the original dependency $A\to CD$ without the help of neighbouring dependencies in $F_c$ , hence $A\to D$ is a weak result.

We may get stronger result when we get a dependency $a\to b$ in the canonical cover $F_c$ that logically implies some dependency $c \to d$ in $F$ but that dependency $c \to d$ in $F$ cannot directly determine the same dependency $a\to b$ in $F_c$ without help of other dependencies in $F$.

for example, let $F = \{A\to B, B\to C, AC\to D\}$, this can be reduced to $F_c = \{A\to B, B\to C, A\to D\}$, the last dependency $A\to D$ in $F_c$ directly implies $AC\to D$ of $F$, but $AC\to D$ cannot directly imply $A\to D$ without the help of the first two dependencies of $F$.

## Multivalued Dependencies
$a \to\to b$ holds on $R$ if for four tuples $t_1, t_2, t_3, t_4$:
1. $t_1[a]= t_2[a]= t_3[a]= t_4[a]$
2. $t_3[b]=t_1[b]$ and $t_4[b]=t_2[b]$
3. $t_3[R-a-b]=t_2[R-a-b]$ and $t_4[R-a-b]=t_1[R-a-b]$

tabularly,

|  a  |  b  | R - a - b |
|:---:|:---:|:---------:|
|  A  |  B  |     P     |
|  A  |  C  |     Q     |
|  A  |  B  |     Q     |
|  A  |  C  |     P     |
Multivalued dependencies can be decomposed into two separate relations:

| a   | b   |
| --- | --- |
| A   | B   |
| A   | C   |

| a   | R - a - b |
| --- | --------- |
| A   | P         |
| A   | Q         |
