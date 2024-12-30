> designed to handle large amounts of data, typically in a database

1. volume of data can be so large that it doesn't fit into the primary memory, and needs to be stored in the second memory.
2. The amount of time it takes to retrieve data from secondary memory is much higher than primary memory.
3. In B-trees, a single node contains a lot of data (roughly equivalent to a memory page).
4. When traversing nodes of a B-tree, the node (and a lot of data along with it) is loaded into the primary memory, which reduces disk operations.
# Properties of B-trees
1. Each node has n keys and n+1 pointers to children nodes.
2. Keys are stored in increasing order in the nodes.
3. satellite-data is stored alongside keys.
4. the child node pointed by the pointer between kth-key and (k+1)th key stores keys that are all greater than kth key and less than (k+1)th key.
5. the child node pointed to by the first pointer contains keys all less than the first key. (similar for the last pointer.)
6. All leaves are at the same depth
## degrees of B-tree nodes
The nodes must have a degree $t \geq 2$.
### lower and upper bound on number of keys for B-tree nodes
For internal nodes, there must be more than t-1 keys t children, and less than 2t-1 keys and 2t children.
## height of a B-tree
height of a B-tree is generally proportional to the number of disk operations required, and is generally $h \leq \log_t{(n+1)/2}$

If the base of log is higher, the log function increases slowly, therefore, we can reduce the amount of disk operations by choosing a suitably higher degree $t$ of the B-tree.
# BTree-Search
We start from a node, then find the key from the list of keys that matches the search key, if we can't find the search key in the keylist of the node, we pick the child next to the largest key from the keylist that is smaller than the search key as the new node and repeat recursively.