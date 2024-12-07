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
4. A pointer to the minimum node `min` is always kept.
5. The total number of nodes is always tracked   
6. For each node, two other values are kept, **degree**, which is equal to the number of direct children of the node and **marked**, which is a boolean indicating if a node has lost a child since the last time its parent was changed.   
### Insert   
Insert is a lazy operation. Whenever a new node is inserted, it is inserted into the root list using circular doubly linked list insertion, then the key of the new node is compared with the minimum node's key and the minimum node `min` is updated if its key is greater than the newly inserted node.
Later `extract-min` handles the heavylifting of consolidating nodes into min-heap ordered trees.   
1. Insert the new node to the root list using circular doubly linked list insertion.
2. Update `min` by comparing with the key of the newly inserted node.
### Union
Union combines two Fibonacci Heaps, it connects the last element of the first heap to the first element of the second heap, effectively combining their root lists. It then updates the node count to be the sum of the node counts of the two heaps and updates the minimum pointer by comparing the minimum pointers of the two heaps and pointing the new `min` to the lesser of the two.
1. Create a new Fibonacci Heap
2. Set the root of the new heap to the root of the first heap.
3. Connect the last element of the first heap to the first element of the second heap using circular doubly linked list operations.
4. Set the node count of the new heap to the sum of the node counts of both the heaps.
5. Set the `min` of the new heap to the `min` with the lesser key among the two heaps.
### Extract Min
1. Add all the children of `min` the node pointed to the root list and deparent them from the `min` node.
2. remove the `min` node from the root list and set the `min` pointer to the node right next to the `min` node.
3. consolidate the heap:
	1. create an auxillary array `aux` of length $\lfloor 2log(n)\rfloor$ where $n$ is the number of nodes in the Fibonacci Heap.
	2. Iterate through each node in the root list and put the nodes with degree $d$ into the $d^{th}$ index of the auxillary array.
	3. While iterating, If degree of two nodes in the root list is same, then make the node with larger degree the child of the node with smaller degree.
	At the end of consolidate, no two nodes in the root list will have the same degree.
### Decrease Key
1. Decrease the key of the node
2. Check if the key of the decreased node is less than the key of its parent, if it is, then call `cut` on the decreased node, followed by `cascading_cut` on the parent node of the decreased node.
3. `cut(decreased_node)`: remove the decreased node along with its children and attach it to the root list, set `marked` of the decreased node to False and decrease the degree of its parent node by 1.
1. `cascading_cut(parent_node)`: if parent node's `marked` is set to False, then set it to True, otherwise `cut` the parent node, followed by the `cascading_cut` of the parent node's parent node. `cascading_cut` is repeated recursively until one of the parent node is found to be already `marked` or we reach the root list.
