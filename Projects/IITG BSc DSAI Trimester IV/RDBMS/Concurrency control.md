---
course: rdbms
week: 11
---
For efficiency, several transactions should happen at once, however this interferes with the isolation principle. Hence we require some concurrency control systems that ensure serialized schedule in a logical way, even if physical serialization is not preserved.

# Lock based protocols
A lock controls access to a data item, a transaction can lock a data item, and while the data item is locked, no other transaction can access that data item. When the current transaction unlocks the data item, then it becomes available for some other transaction to lock and use.

There are two types of locks.
1. exclusive (X): a locked data item can be read and written only by the current transaction.
2. shared (S): a locked data item can only be read by the current transaction, but cannot be written by the current transaction but it is also available for the other transactions to lock and read as well.

An S-locked data item can be S-locked by other transactions, but it cannot be X-locked by the other transactions.

Lock requests are made to the concurrency control manager using the x-lock and s-lock instructions, and the control manager checks if it can grant the lock to a particular transaction.

> Locking does not always guarantee serializability.

Locks can be used to delay the execution of transactions. If a transaction requires access to some data item, which is currently locked by some other transaction, then the transaction will not proceed. Unlocking a transaction too early can sometimes lead to execution of instructions of some other transaction that should've only run after the current transaction was completed. However delaying the unlocking is not always a good option either and can lead to deadlocks more often than not.
# Deadlocks
Let there be two data items A and B and two transactions T1 and T2.
- T1 wants to write something to A, after which T1 will read the value of B. 
- T2 wants to write something to B, after which T2 will read the value of A.
- T1 execution order: 
	1. T1 will first ask the concurrency control manager to grant access to lock A then it will write something to it.
	2. then T1 will ask access to lock B and read its value. 
	3. Then it will unlock A, then it will unlock B.
- T2 execution order:
	 1. T2 will ask access to lock B and write something to it.
	 2. then T2 will ask access to A to read it
	 3. after that it will unlock B, then it will unlock A.
 
A problem can arise when T1 and T2 run concurrently like this. If T1 locks A, and within the same time T2 locks B, then:
1. The second instruction in T1 is to ask access to lock B and read it, but B is currently locked by T2, so T1 cannot proceed further.
1. The second instruction in T2 is to ask access to lock A and read it, but A is currently locked by T1, so T2 cannot proceed further.
This means that no transaction can proceed further as they have locked the required data items for each other to proceed further.
This type of situation is called a deadlock.

One transaction must rollback and release its locks to let the other continue, or both should rollback, or the transaction should be designed in a better way.
## Deadlock prevention by no cycle waits
One method to prevent deadlocks is by ensuring that there are no cycle waits. This can be done in two ways: pre-declaration and graph-based-protocol.
### pre-declaration for deadlock prevention
Require that each transaction locks all its data items before execution.

This allows no other transaction to lock the data items that prevent execution of the current transaction as the current transaction locks all of its required data items beforehand.

However this has a problem as we might not know the required data items beforehand and data-item utilize will be very low, i.e. a transaction might lock a data item that it didn't need to lock for a long time preventing any other transaction to access it during that period.
### graph-based-protocol for deadlock prevention
Impose partial ordering of all data items and require that transaction can only lock the data items in the specified partial order.

The disadvantage is that it is often hard to predict what ordering is needed.
## Deadlock prevention by preemption and rollback
In this method, we assign a unique timestamp to each transaction when it begins, this is called preemption. Then the system uses these timestamps to decide whether a transaction should rollback to prevent a deadlock.
### wait-die deadlock prevention
When transaction T1 requests a data item currently held by T2, then it must only wait if its timestamp began before T2's timestamp, otherwise T1 must die and rollback.
This is a non-preemptive technique, because T1 stops its own execution if wait condition is not satisfied.
### wound-wait deadlock prevention
When transaction T1 requests a data item currently held by T2, then it must only wait if its timestamp began after T2's timestamp, otherwise T2 must die and rollback.
This is a preemptive technique, because T1 stops T2's execution if wait condition is not satisfied.
## Deadlock Detection Algorithms
### Wait-for graph
A Wait-for graph is a directed graph whose vertices are transactions, and there is a directed edge between two vertices if the transaction associated with the first vertex is waiting for a lock held in conflicting mode by the transaction associated with the second vertex.

System is in a deadlock state if and only if the Wait-for graph has a cycle. A cycle detection algorithm for Wait-for graph periodically checks for deadlocks.
## Deadlock Recovery
Some transaction needs to be set as a victim to break the deadlock cycle. A victim transaction is chosen such that it incurs minimal cost.

A total rollback can be done to abort the transaction entirely and restart, however this is not very cost-effective.

A partial rollback is performed where the victim transaction is rolled back only as far as necessary to release locks that another transaction cycle is waiting for.
## Starvation 
If a transaction wants to X-lock a data item but the data item is being S-locked by multiple other transactions causing the current transaction to rollback repeatedly, then this type of scenario is called starvation.
### Deadlock recovery can sometimes cause starvation
When choosing a victim, the oldest transaction should never be chosen.

- If there are multiple older transactions in a deadlock that are S-locking a data item which is needed by some other transaction T1 exclusively so that T1 can proceed.
- If the oldest transaction always keeps getting chosen as a victim, then T1 won't be able to X-lock that data item ever due to continuous rollbacks of the oldest transactions before it.
- When current oldest transaction is rolled back, it is placed somewhere after the transaction after it and the transaction after it becomes the current oldest transaction and so on. Thus rollback keeps on happening repeatedly and T1 never gets access to the data item.
# Two phase locking protocol
Two phase locking protocol ensures serializable schedules but they are still prone to deadlocks.
Two phase locking is done in a growing phase and a shrinking phase.
1. In growing phase, transactions may obtain locks but they cannot release them.
2. In shrinking phase, transactions release the locks they obtained in the growing phase, but they cannot acquire any more locks.

A transaction follows two phase locking if all locking operations precede the first unlock operation. However this, doesn't mean delaying the unlocks to end, its just that whenever the first unlock happens, then no other locks can happen in the schedule.

> Transactions can be serialized in the order of the points where they acquire their final lock.

> Limitations of Two-phase locking protocol:
> 	1. Deadlocks can still happen.
> 	2. Cascading rollback may occur.
## Extensions of Two-phase locking protocol
1. Strict two-phase locking: a transaction must hold all its X-locks till it commits/aborts. This ensures recoverability and avoids cascading rollbacks.
2. Rigorous two-phase locking: a transaction must hold all its locks till commit/abort (not just X-locks). This also ensures recoverability and avoids cascading rollbacks, also when using rigorous two-phase locking, transactions can be serialized in the order in which they commit.
### Lock conversions
Two additional operations are defined:
1. upgrade, which converts an S-lock to an X-lock
2. downgrade, which converts an X-lock to an S-lock

The Two-phase locking protocol can be changed to allow upgrades in the growing phase and downgrades in the shrinking phase for more flexibility which allows us to mitigate even more deadlocks.
# Implementation of a lock manager
Like concurrency control manager, a lock manager can be implemented to grant lock requests to transactions or to implement deadlock checks to rollback in case of a deadlock.

The lock manager maintains an in-memory data structure called **lock table** to record granted locks and pending requests. Lock tables can implement external overflow chaining using linked lists.

Each data item is hashed to obtain a key, which gives an index on which the data item is kept, if another data item has the same index as an already inserted data item, then it is made a next node of the linked list on that index.

Each data item node itself contains another linked list which contains the record of granted or pending lock requests.
# Timestamp Based Ordering Protocol (TSO)
Timestamps are assigned to transactions in a logical way to avoid duplicates. Timestamps are assigned using a logical counter so that serializability order of the transactions is same as the timestamp order, i.e. newer transactions have strictly greater timestamps than the older transactions.

We define two timestamps for each data item:
1. W-timestamp W-TS(Q) is the largest time-stamp of any transaction that executed write(Q) successfully.
2. R-timestamp R-TS(Q) is the largest time-stamp of any transaction that executed read(Q) successfully.
TSO protocol imposes rules on read and write operations so that they are executed in the timestamp order, any out of order operations cause rollbacks.
## Read rules of TSO Protocol
Suppose a transaction T1 issues a read(Q), then schedule should adhere to the following rules:
1) If TS(T1) < W-TS(Q), then T1 wants to read a value of Q that is written by a Transaction that should run after T1, that is why W-TS(Q) is higher than the timestamp of T1. This means T1 will read a different value that is supposed to be written by instruction in the future. Hence, read operation is rejected, and T1 is rolled back.
2) If TS(T1) >= W-TS(Q), then T1 wants to read the latest value of Q that was written by some transaction, hence the read operation is executed, and R-TS(Q) is set to max (R-TS(Q), TS(T1)).
## Write rules of TSO Protocol
Suppose a transaction T1 issues a write(Q), then schedule should adhere to the following rules:
1) If TS(T1) < R-TS(Q), then the value of Q that T1 is producing was supposed to be read by a transaction that happened after T1, that is why R-TS(Q) is higher than the timestamp of T1, i.e. the newer transaction has already read the non-updated value of Q, which was supposed to be updated by T1 in this write instruction. Hence, write operation is rejected, and T1 is rolled back.
2) If TS(T1) < W-TS(Q), then T1 is attempting to write a value of Q that it should've written before the write instruction of a transaction with newer timestamp executed. Hence, write operation is rejected, and T1 is rolled back.
3) Otherwise, the write operation is executed, and W-TS(Q) is set to TS(T1).

## Properties of Schedule under TSO
Using the rules above, the TSO protocol ensures that if two transactions access the same data item Q, then transaction with smaller timestamp always accesses Q before the transaction with larger timestamp. This means that TSO Protocol guarantees serializability.

1. The precedence graph when using TSO protocol always has edges from vertex of transaction of smaller timestamp to vertex of transaction of larger timestamp.
2. The precedence graph when using TSO protocol has no cycles.
3. TSO ensures freedom from deadlocks as no transaction ever waits.
4. The schedule under TSO may not be cascade-free and recoverable.
## Thomas write rule
In the write rule of TSO Protocol, if TS(T1) < W-TS(Q), then we must rollback.
- Suppose that the latest write operation on Q was done by some T2, i.e. W-TS(Q) is set to T2's timestamp.
- However, if the value of Q that was written by both T1 and T2 did not depend on Q itself, then the write operation of T1 doesn't matter because the newer transaction T2 has already written the updated value.
- Since T2 was supposed to perform after T1, the value of Q was going to be updated anyways by T2 and T1 is simply trying to write an obsolete value.
- If no other transaction reads the value of Q in the middle of the write(Q) instruction of T2 and write(Q) instruction of T1, then we can safely ignore the write(Q) instruction of T1 and continue further without rollback.
This is a modification made to the second write rule of TSO Protocol which makes TSO Protocol more concurrency friendly.

obsolete write operations can be ignored under certain circumstances, this is called Thomas write rule.