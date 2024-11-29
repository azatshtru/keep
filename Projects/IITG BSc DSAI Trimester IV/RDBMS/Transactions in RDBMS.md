---
course: rdbms
week: 10
---

A transaction is the smallest unit of work that accesses or modifies values in a database.

A transaction contains **read** and **write** operations, which read and write to some temporary buffer, which is then applied to the database after the transaction finishes.

Transactions should be designed to deal with issues involving system crashes and concurrent execution.
## States of Transactions
A transaction can be: 
1. Active: currently running
2. Partially completed: changes made but not committed
3. Commited
4. Failed: cannot proceed further due to some error
5. Aborted: database is rolled back to its original state before transaction began.
## Atomicity
Either all the commands in the transaction run successfully, or the DBMS should return to the state before the transaction began.
## Consistency
1. Explicit integrity constraints should not be violated after transactions.
2. Implicit integrity constraints, like the sum of balances of all the accounts before and after a transaction involving transfer of money should not be violated. 
During transactions, the integrity constraints may be violated, but they should be intact before and after the transaction. 
There should not be any loss of information before and after the transaction.
## Isolation
No transaction should interrupt the other. This can be easily achieved by running transactions one at a time serially.

Although, concurrently running transactions has its benefits, like high CPU utilisation and faster transaction processing on average. To still keep transactions isolated and utilize the benefits of concurrency, **concurrency control schemes** are used.
### [[Serializability]]
## Durability
During transaction, if system crashes or outages occur, they shouldn't affect the transaction, and changes must persist until the transaction is completed.
### Recoverable schedules
If a transaction $T_j$ reads data from an item previously written by transaction $T_i$, then $T_i$ must commit before $T_j$ otherwise if $T_j$ is committed before $T_i$ and error/outage happens before the commit of $T_i$ then the operations performed by $T_j$ will be based on the uncommitted values that changed during $T_i$ temporarily but were not committed, and we might get inconsistencies.
### Cascading Rollback
If one transaction in a Schedule aborts, then every transaction in that shedule must rollback. This can, however, lead to undoing of large amounts of important computation intensive work, to avoid rolling back large amounts of work, commits should be performed frequently after transactions in a Recoverable schedule manner, i.e. commits of one transaction should be performed before the read operation of another transaction begins so that the other transaction reads the committed updated value.

Every cascadeless schedule is recoverable and is desired.