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
# B-tree Search
We start from a node, then find the key from the list of keys that matches the search key, if we can't find the search key in the keylist of the node, we pick the child next to the largest key from the keylist that is smaller than the search key as the new node and repeat recursively.
# B-tree Insert
Insertion requires searching for the to-be-inserted key, then inserting at the appropriate spot in the keylist of one of the leaf nodes. However, if the leaf node is already at maximum capacity (2t - 1), then we must split the node into two nodes with t-1 keys each, and we must add the median key of the splitted node to the parent node at some appropriate spot, the pointers around this median key will be used to point to the splitted nodes.

Still, parent nodes can be full as well, therefore to avoid recursively splitting nodes, we split the full nodes on our way down the tree while searching for the spot to insert the new key.
# B-tree deletion
### Steps for Deletion:
1. **Find the Key**:
    - Traverse the B-tree to locate the key to delete.
2. **Delete Based on internal or leaf node**:
    - If the key is in a **leaf node**, simply remove it.
    - If the key is in an **internal node**, replace it with its in-order predecessor or successor.
3. **Handle Underflows**:
    - Borrow a key from a sibling if possible.
    - Merge nodes if borrowing isn’t possible.

The inorder successor of a key is the smallest key on its right child node.
The inorder predecessor of a key is the largest key on its left child node.