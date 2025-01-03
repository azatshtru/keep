A balanced binary search tree.
# Properties
1. A node is either red or black.
2. The root and leaves are all black.
3. If a node is red, then all its children are black.
4. All paths from any node to any leaf in its descendants contain the same number of black nodes.
## height and black-height in Red-Black trees
**black-height** of a node is the number of black children it has in any one descendant path to its leaves.

- Maximum height of a Red-Black tree of n nodes is log(n).

- The longest path to a descendant leaf node is twice the length of shortest path to the a descendant leaf node.
## color of nodes
Nodes require one storage bit to keep track of their color (either red or black).
## relationship of nodes in Red-Black trees
```
      y
     / \
    x   z
     \
      a
```
x is a's parent, y is a's grandparent and z is a's uncle.
## arrangement of nodes in Red-Black trees
1. left triangle arrangement
```
      y
     / 
    x   
     \
      a
```
2. right triangle arrangement
```
      y
	   \
	    x   
	   / 
      a
```
3. left line arrangement
```
      x
     /
    y
   / 
  a   
```
4. right line arrangement
```
     y
      \
       z
	    \
	     a
```
# Rotations in Red Black trees
Red-Black trees are rotated to bring larger subtrees up and smaller subtrees down, so as to balance the binary search tree. There are two rotations:
1. left-rotate
2. right-rotate
Rotations are done in O(1)
## Left-rotation
Let there be a node x, and its right child be y.
Performing a left rotation on some node makes it the left child of its right child, and makes the left child of its right child into its right child.

i.e. x becomes the left child of y, and the already existing left child of y becomes x's right child.

```
	x                                              
     \                                             
      y                                                         
     / \                                                     
    a   b                                                     

After Left Rotation:

      y
     / \
    x   b
     \
      a
```
## Right-rotation
Performing right rotation on some node makes it the right child of its left child, and it makes the right child of its left child into the node's left child.
```
      x
     /
    y
   / \
  a   b

After Right Rotation:

    y
   / \
  a   x
     /
    b
```
# Insertion in Red Black trees
We insert new node similar to a regular binary tree insertion and color it red, and then do a couple of fix-ups to fix the violations of the Red-Black tree properties that were violated during the insertion.

After insertion of a node Z, there are four scenarios and corresponding actions to take in the fix-up:
1. if Z is root -> recolor Z to be black.
2. if Z's uncle node is red -> recolor Z and parent nodes recursively to fix violations of properties of Red Black trees.
3. if Z's uncle is black and Z is in a triangle arrangement with its parent and grandparent -> rotate Z's parent on opposite side of Z.
4. if Z's uncle is black and Z is in a line arrangement with its parent and grandparent -> rotate Z's grandparent on opposite side of the Z and recolor.

Redblack trees insert fixups demonstrated:
```
Z is node, A is parent, B is grandparent, U in uncle

Left Line arrangement, B needs to be right rotated (opposite direction of Z) if the above condition meets:

      B
     / \
    A   U
   / 
  Z

Right Line Arrangement, B needs to be left rotated (opposite direction of Z) if the above condition meets:

    B
   / \
  U   A
	   \
	    Z
    
Left Triangle arrangement, A needs to be left rotated (opposite direction of Z) if the above condition meets:

      B
     / \
    A   U
     \
	  Z 
	
Right Triangle Arrangement, A needs to be right rotated (opposite direction of Z) if the above condition meets:

	  B
	 / \
	U   A
	   /
	  Z
```
