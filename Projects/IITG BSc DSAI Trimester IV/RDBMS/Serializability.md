---
course: rdbms
week: 10
---

# Schedule
Schedule is a sequence of instructions that specify the chronological order in which instructions of concurrent transactions should happen.

A transaction that ran successfully will have a commit at the end, while an unsuccessful one will have abort.

- A schedule is **serialized** if transactions happen one after another but not at the same time.
- A schedule is **concurrent** if transactions happen at the same time. A concurrent schedule is **serializable** if it is equivalent to the serialized schedule.
## conflicting vs non-conflicting instructions
Two instructions are only compatible if both read from the database, or both operate on different values in the database, otherwise they are conflicting.

Two non-conflicting instructions can run concurrently, otherwise one transaction needs to wait for the other transaction's *write* to finish before proceeding further.
### Conflict Serializability and Conflict Equivalence
Two schedules are said to be conflict equivalent, if one of them can be transformed into the other without moving/swapping conflicting instructions.

A schedule is said to be conflict serializable if it is _conflict equivalent_ to a serialized schedule.
### View Equivalence and View Serializability
Two schedules $S$ and $S'$ with same set of instructions are view equivalent when:
1. the first read instruction in both schedules should come from the same transaction.
2. if a transaction $T_j$ in schedule $S$ reads some value $v$ while executing some read instruction such that $v$ was written by some write instruction of transaction $T_i$, then in schedule $S'$, the transaction $T_j$ must also read the value written by some write instruction of transaction $T_i$
3. the final write instructions in both schedules write the same value

In other words, since both schedules are made from the same set of instructions, view equivalence is when all the read instructions of one schedule read the same value as the corresponding read instructions of the other schedule.

A schedule is said to be view serializable if it is _view equivalent_ to a serialized schedule.

Every conflict serializable schedule is also view serializable but not the other way around. A view serializable schedule that is not conflict serializable contains blind writes.
# Precedence Graph
A precedence graph is a [[Directed Graph]] where the vertices are transactions and directed edges are drawn from transaction $T_i$ to $T_j$ if $T_j$ accesses (reads/writes) some data that was previously accessed by $T_i$
## Algorithms to test for Serializability
A schedule is conflict serializable if and only if it's precedence graph is acyclic.
[[Cycle detection algorithms]] ($O(v^2)$ or $O(v+e)$) can be used to detect conflict serializability.

Detecting View Serializability is a [[NP-complete]] and extending cycle detection for detecting conflict serializability to view serializability gives an algorithm of exponential time complexity.
## Algorithms to find Serializability order
If we know that the precedence graph is acyclic, then the precedence order can be found by [[Topological Sorting]] in $O(v + e)$ time. 