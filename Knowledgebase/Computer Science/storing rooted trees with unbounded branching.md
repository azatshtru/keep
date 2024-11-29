To store a rooted tree with unbounded branching, a naive way is to use lists in every node to keep track of children in every node. But this can cause memory management and scaling issues.   
Another way is to let every node store two pointers:   
1. The leftmost child of the node   
2. The right sibling to the node   
   
When arriving at a node, we can check other nodes by going to the right siblings and we can go down by using the left sibling.   
   
