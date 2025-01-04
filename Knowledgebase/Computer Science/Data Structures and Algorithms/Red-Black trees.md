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
# Deletion in Red Black trees
Deletion in Red Black trees happens in two steps:
1. Do a regular binary tree deletion, i.e. replace the node to be deleted with its in-order successor.
2. Call a fixup method to fix any violations of Red-Black tree properties that were made during the deletion.
## Delete fixups
If the original-color of the deleted node was red, then there is no fixup required, as the black-height of the tree remains same. However, if the original color of the deleted node was black, then we do **sibling analysis** to perform fixups.

There are four possible states of the sibling node of the deleted node after the deletion, and there are four corresponding fixes for each of them. These fixes are called according to the condition repeatedly in a loop until Red-Black tree properties are satisfied. More than one fix can happen during one invocation.

- Let the deleted node be **x** and sibling node be **w**.
- let anti-rotate be left-rotate if w is right child, and right-rotate if w is left child.
- let rotate be left-rotate if w is left-child, and right-rotate if w is right-child
- let same-side be right if w is right child, else same-side is left
- let opposite-side be right if w is left child, else opposite-side is right.

1. If w is red
	1. set w's color to black
	2. set w's parent to red
	3. call anti-rotate on x's parent, this will change x's sibling from w to something else.
	4. point w to x's same-side sibling after the anti-rotate
2. If w is black and both w's children are black as well
	1. set w's color to red
	2. point x to x's parent
3. If w is black, w's opposite-side child is red, and w's same-side child is black
	1. set w's opposite-side child to black
	2. set w's color to red
	3. call rotate on w
	4. point w to be x's same-side sibling
	5. do all the steps in the 4th case
4. If w is black, and w's same-side child is red
	1. set w's color to be the same as x's parent's color
	2. set x's parent color to black
	3. set w's same-side child's color to black
	4. call anti-rotate on x's parent
	5. point x to be the root of the tree