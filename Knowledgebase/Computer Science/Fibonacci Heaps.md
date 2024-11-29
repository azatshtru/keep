A fibonacci heap is a data structure that implements a mergeable heap through the following API:   
1. make-heap: creates an empty heap   
2. insert: inserts a node   
3. minimum: returns the minimum node   
4. extract-min: deletes the minimum and returns it   
5. union: merges two heaps   
6. decrease-key: decreases the key for a node, with a value that is less than or equal to the current value   
7. delete: deletes a node   
   
Fibonacci heaps provide a very good amortized time complexity for these operations.   
amortized = average time over a sequence of operations   
|    operation | binary heap (worst case) | fibonacci heap (amortized case) |
|:-------------|:-------------------------|:--------------------------------|
|    make-heap |                     O(1) |                            O(1) |
|       insert |                  O(logn) |                            O(1) |
|      minimum |                     O(1) |                            O(1) |
|  extract-min |                  O(logn) |                         O(logn) |
|        union |                     O(n) |                            O(1) |
| decrease-key |                  O(logn) |                            O(1) |
|       delete |                  O(logn) |                         O(logn) |

used when number of extract-min and delete operations are less.   
used in minimum spanning trees with prim's algorithm and shortest path algorithms like dijkstra's algorithm   
### Basic structure   
A fibonacci heap is a set of multiple min-heaps that are not necessarily binary heaps.   
1. Each node in each min-heap has a pointer to its parent and one of its children, every node is linked to its siblings by circular doubly linked lists.   
2. If a node has no siblings then its doubly linked head and tail pointers point back to itself.   
3. The root of each min-heap itself is a node in a circular doubly linked list called root list   
4. A pointer to the minimum node is always kept.   
5. The total number of nodes is always tracked   
6. For each node, two other values are kept, **degree**, which is equal to the number of direct children of the node and **marked**, which is a boolean indicating if a node has lost a child since the last time its parent was changed.   
   
   
### Insert   
Insert is a lazy operation. Whenever a new node is inserted, it is inserted into the root list, then extract-min handles the heavylifting of consolidating nodes into min-heap ordered trees.   
