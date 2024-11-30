# Sentinel Nodes in Linked Lists   
In Linked lists, a sentinel node is used to halve the number of checks while searching. A sentinel node is never destroyed and acts as a dummy node which is attached at the end of the linked list and has a pointer to the first node.   
When searching for something, the sentinel node is assigned its value. Without sentinel node, on every iteration, two things are checked:   
1.  If the current node has the searched value   
2. If the current node is the last node in the linked list   
   
Since, the sentinel node will hold the value of the search, only the first condition needs to be checked when using a sentinel node. (And it makes for a cool looking code where the for loop is left empty) After search, a preprocessing step can be used to check if the arrived node is sentinel or not to check if the search was successful or not.   
   
