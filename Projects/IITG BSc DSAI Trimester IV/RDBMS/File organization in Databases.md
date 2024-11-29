---
course: rdbms
week: 12
---
The database is stored as a collection of files. Each file is logically partitioned into blocks. A block can have a size of 4 to 8 KB and each block can contain multiple records. Each record is entirely contained in a block, i.e. records are not divided between blocks. Each record is a sequence of fields.

Storing fixed size records is easy, they are stored in sequence of memory with equally sized allocations.

Storing variable size fields is a bit more involved, instead of storing the field values in the record, they are stored somewhere else and the location(memory address) and size of the values in memory is stored into a fixed size integer value in its place.
A record that contains mix of fixed and variable sized fields stores the variable size values somewhere else in the memory and stores the location and size of all the variable sized fields into the fixed integer memory in place of the value. The fixed sized records are stored as is.

insertion of records for fixed length: store the ith record starting from the byte k(i-1) where k is the size of each record.

deletion of records for fixed length: if we want to delete ith record from the file, then we can:
1. move all the records after i a place before.
2. move the nth record to the ith place.
3. instead of moving the records, maintain a free list of deleted indices.

organization of files:
- heap: record can be placed in the file wherever there is free space.
- sequential: the records are sequentially ordered, sorted based on a search key, possibly in form of a linked list
- B+ tree data structure and search key indexing.
- data structures using hashing: computing index by hashing a search key
- multitable clustering: records of multiple relations are stored in the same file and related records are stored in the blocks to minimize I/O.

search key is a set of attributes used to lookup records in a data file.
indexing is a mechanism used to speed up access to desired data.
an index-file stores data of type: {search-key: record-pointer}, the record-pointer describes the memory address of the record associated to the search key, the size of index files is usually very less than the size of data files. index-files can be sorted on the search keys like the data files.

hashing the search keys and distributing the records into buckets where each bucket corresponds to a hash index, then storing the search keys into an index files to hash and lookup corresponding records is also used.

indexing evaluation metrics:
1. access types: how efficient is it to search for a value vs how efficient is it to search for a value in some range or category.
2. access time
3. insertion time
4. deletion time
5. space overhead (including the space required to store indices)

there are two types of indices:
1. clustering index: for a sequentially ordered data file, the search keys in the index-file specify the sequential ordering of the data file. search keys are usually taken as the primary key but not always. this is also called primary index. index-sequential file is a sequential data-file ordered on a search key with clustering index-file.
2. secondary index: an index key whose search key specifies an order different from the sequential order of the file. also called non-clustering index.

dense vs sparse index files:
1. in dense index files, there is an index record for each record in the data file. there can be an index record for multiple records in the data file.
2. in sparse index files, there are records for only some records in the data file. to find a record from the data file using the sparse index file using some sequential search key, we find the largest search key K which is smaller than the query search key, then we search the records starting from K until we find the record in the data file.

sparse index is easier to maintain with less overhead for insertions and deletions, but is generally slower than dense index for queries.

- for clustered indexing, it is a better tradeoff to create a sparse index with records for the first file of each block in the data-file.
- for nonclustered index, use sparse index on top of a dense index. for example in an instructor table which is sorted by ID, if we want to get the instructors whose salary is between a range, then it makes sense to make a dense index which uses salaries as search keys, and puts records into buckets corresponding to salaries, then we can use a sparse index on top of the dense index where the entries in the sparse index are ordered by salaries and point to the dense index buckets.

indices impose overhead on database modification as whenever a record from the datafile must be added, deleted or updated, then the corresponding entries in the index file must be updates as well.

sequential scan using clustering index is efficient but it takes more times with nonclustering indices.

When data files are updated, then corresponding indices must be update accordingly to account for deletion or insertion of records. deletion or addition of entries in the dense index is done when the data file is deleted from or inserted to. For sparse indices, certain rules are followed:
1. if record corresponding to sparse entry is deleted, then replace the next record in the data file with that entry, if the next record in data file itself is an entry in the index, then simply remove the current deleted entry.
2. when inserting record into data files, no change needs to be performed for the sparse index files unless a new block is created, then the first search key of the new block is inserted into the sparse index.

In indexed sequential files, performance degrades as file grows and insertions and deletions from the data file cause massive overhead with reorganization of the index files. 

B+ trees index files are automatically indexed with small and local changes when insertion and deletion happens in the data file, without reorganization of the entire file.

A node of a B+ tree contains an array of pointer-key pairs: $(P_i, K_i)$
\[$(P_1, K_1)$, $(P_2, K_2)$, $\dots$, $(P_{n-1}, K_{n-1})$, $P_n$]
- for non-leaf nodes, $P_i$ are pointers to child nodes
- for leaf nodes, $P_i$ are pointers to records or buckets of records 

- pointer-key pairs in a node are ordered according to the keys $K_1<K_2<...<K_{n-1}$
- the search keys of a leaf node is always greater than all the search keys of the preceding leaf node.

- The last pointer $P_n$ in leaf nodes is used to refer to the next sibling node to allow for rapid in-order traversal of the leaf nodes.
- The last pointer $P_n$ in non-leaf nodes is used to refer to the child node that contains keys that are all larger than the current node's keys
- the first pointer $P_1$ in non-leaf nodes is used to point to a child node whose keys are all smaller than the keys of the current node.
- the intermediate pointers in non-leaf node $P_i$ are used to refer to child node whose values are all larger than $K_{i-1}$ and smaller than $K_{i+1}$


B+ trees satisfy the following properties for use in dbms:
1. all the paths from root to every node are of same length, they are balanced.
2. each non-leaf node has between n/2 to n children where n is the order of B+ tree.
3. a leaf node has (n-1)/2 to n-1 values
4. if root is a leaf than it can have 0 to n-1 values, if root is not a leaf then it has atleast 2 children.

searching in B+ trees:
if we want to search for some value V, then we start at the root and follow a recursive algorithm, until we reach the leaf node level:
1. examine the current node, looking for the smallest i, such that $K_i$ >= V
2. if $K_i$ == V, then current node becomes the node pointed by the pointer $P_{i+1}$
3. if $K_i$ > V, then current node becomes the node pointed by the pointer $P_{i}$
4. if no $K_i$ >= V is found, then the node pointed by the last pointer $P_n$ becomes the current node.

while inserting into the B+ tree, all the nonleaf nodes follow the insertion and split rules of B trees, but for the leaf nodes, when splits happen after insertions, the values in leaf nodes are not moved up, they are copied upwards instead.

when a value is deleted from a node, then the node might contain less than the minimum values required by the order of B+ tree after the delete operation, to fix this, we can take values from the sibling nodes, if the values in siblings nodes are also insufficient, then we merge the current node with its sibling, then we split and rearrange as required.
search keys in the parents need to be updated if node merge or split happens.

a lot of problems of index sequential files are solved by B+ trees, B+ trees can not only be used in index files, but also for records organization in the original data file. while storing data files as B+ trees, the leaf nodes store records instead of pointers to records, since records take more space than pointers, the maximum number of records in leaf node is less than the number of pointers in the nonleaf node. insertion and deletion of B+ tree data file is done in the same way as B+ tree index files.