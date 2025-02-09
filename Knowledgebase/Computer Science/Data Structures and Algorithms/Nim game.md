Nim game is a two player game that doesn't involve randomness and the state of everything in the game is known to both players at all times.
# Single Heap Nim game
Given a heap with $n$ sticks, both players remove 1, 2 or 3 sticks each turn, the player that removes the last stick wins.

A given playthrough of nim game with single heap with 10 sticks:
• Player A removes 2 sticks (8 sticks left).
• Player B removes 3 sticks (5 sticks left).
• Player A removes 1 stick (4 sticks left).
• Player B removes 2 sticks (2 sticks left).
• Player A removes 2 sticks and wins.

A winning state for player A is achieved when, regardless of whether the opponent plays optimally or not, player A will win if he plays optimally.

A losing state is achieved for player A when, if the opponent always plays optimally, player A will always lose regardless of whether he plays optimally or not.

- state 0 is losing state
- state 1 is winning state because player can remove 1 stick and win
- state 2 is winning state because player can remove 2 sticks and win
- state 3 is winning state because player can remove 3 sticks and win
- state 4 is losing state because if the player removes 1, 2 or 3 sticks, there will be with 3, 2 and 1 sticks left respectively which the opponent can remove and win.
- state 5 is a winning state because the player can remove 1 stick to put the opponent in state 4 which is a losing state.

In fact, in a Nim game with single heap and 1, 2, 3 are the only possible amount of sticks that can be removed, every multiple of 4 is a losing state.

## State graph
Any nim game heap can be represented using a state graph, where each vertex of the graph represents the states (the number of sticks left in the heap), and there are directed edges between vertices $v_i$ and $v_j$ if it is possible to move from $v_i$ to $v_j$ by following the rules of the particular nim game.

For example, to represent the nim game of single heap with 10 sticks, we would have a graph with 10 vertices, and each of the vertex would have edges to at most 3 other vertices. vertex with number 10 would have edges to vertices with numbers 9, 8 and 7. Similarly, the vertex with number 5 would have edges to vertices 4, 3 and 2. The vertex with 2 would have edges to vertex containing 1, etc.

## Nim game of single heap where you can only remove factors less than the heap size
There is another type of nim game with single heap, where you can only remove the number of sticks that are multiples of the amount of sticks left in the heap.
For example, if there are 8 sticks left in the heap, then you can remove either 1, 2 or 4 sticks from the heap because 1, 2, 4 are the multiples of 8.

This type of num game can be represented with a graph, where there is a directed edge between vertices $v_i$ and $v_j$ if $v_j$ can be obtained by subtracting a multiple of $v_i$ from $v_i$

It is useful to represent states of Nim game heaps using graphs, to visualize which states can change to which states in the heap.
# Nim game with multiple heaps
In a regular nim game, there are n heaps, and on each turn, a player can select one of the heaps and remove any amount of sticks from it. The player who removes the last stick wins the game.

A nim game with multiple heaps can be represented using an array, which hold the initial number of sticks in each heap. For example a nim game with 3 heaps of size 12, 10 and 5 would be represented as `[10, 12, 5]`. Each of these heaps can be represented using a state graph.
# Nim sum
Any nim state can be classified as a winning state or losing state using the nim sum. At any turn in the game, we can `xor` the amount of sticks remaining in each heap, if the xor is zero, it is a losing state, otherwise a winning state.

For example, in a nim game, `[10, 12, 5]`, the initial state is a winning state, because $10 \oplus 12 \oplus 5 = 3$

To optimally play the game from a winning state, we can do the following:
1. find a heap-size which which has a one bit at the same position as the leftmost one bit of the nim sum.
	- In the above case, the nim sum is 3, i.e. $0011$, the leftmost one bit is the second bit from right.
	- the heap sizes are 10, 12 and 5, i.e. $1010$, $1100$ and $0101$.
	- It can be seen that 10 has a one bit at the second position from the right, which is at the leftmost one bit of the nim sum 3, therefore we select 10.
2. remove $k$ amount of sticks from heap which was selected in the previous step where $k$ is the xor of nim sum with the heap-size selected in the previous step substracted from that selected heap-size
	- we selected 10 in the previous step, and 3 is the nim sum. $10\oplus 3 = 9$
	- we subtract the xor with the selected heap-size, i.e. $10 - 9 = 1$
	- therefore, we remove 1 stick from the heap with size 10.
	
Doing these two steps puts the game in a losing state, since after removing one stick from the heap of size 10, the current nim state is `[9, 12, 5]` and $9\oplus 12\oplus 5 = 0$, since the new nim sum is 0, the opponent gets into a losing state.

Why does performing these two steps transitions from a winning state to a losing state? Prove it yourself, or search online. In essence, we are choosing a move which is making the nim sum zero for the next player and putting him in a losing state.

Every nim game always reaches a state where each heap has atmost one stick left in each heap, i.e. each heap contains either 1 or zero sticks.
# Misère game
In the Misère game, the goal is opposite, the person who removes the last stick loses. The Misère game can be played using the optimal way of the nim game until a state is reached where each heap would contain atmost one stick after the next move. When this state is achieved, the strategy can be switched to remove stick from a heap so that after the move, there are odd number of heaps with a single state.
# Sprague-Grundy theorem
This theorem generalizes the Nim game to any two player game where there is no randomness and the state is known to both players at all times.

At each turn, we can calculate a Grundy number corresponding to the state of the game, if the number is zero, the state is a losing state, otherwise it is a winning state.
## Grundy Number
The grundy number of the current state depends on the grundy numbers of the states to which we can move from the current state. It is calculated as:
$$G = \text{mex}(\{g_1, g_2, g_3, \dots, g_n\})$$
$g_1, g_2, g_3, \dots, g_n$ are the grundy numbers of the states to which we can move from the current state.
The mex function gives the smallest non-negative number which is not in the set $\{g_1, g_2, g_3, \dots, g_n\}$

For example if states are {0, 1, 3, 5}, then mex({0, 1, 3, 5}) = 2 because 2 is the smallest positive number not in the set.

If we are in a winning state, we can make the optimal move and move into a state whose grundy number is zero, so that the opponent is put in a losing state.
## Grundy Number of Subgames
If there are multiple subgames of a two player game and at each turn, a player has to choose a subgame and perform an action, then this setup corresponds to the nim game with multiple heaps.

We can proceed in this setup as a nim game by calculating the xor of grundy numbers of all the subgames to determine if its a losing state or a winning state, and then choosing a subgame and an action such that the xor becomes 0 for the next player in the next move.

The xor of grundy numbers is called the Nim sum.
# Grundy's game
A game in which subgames are created after each move can be modelled on top of Grundy's game.

In Grundy's game, there are heaps with sticks, and at each move, a player has to choose a heap and divide it into two heaps with unequal amount of sticks. During a turn, if there are no heaps left which can be divided into unequal amount of heaps, then the player during that turn loses and the other player wins. This losing state is achieved when the remaining heaps only contain either one or two sticks.
## Grundy number for Grundy's game
If after a move, $m$ subgames are generated in a Grundy's game, then the Grundy number of that state of the game can be calculated as the mex of the grundy numbers of all the possible sets of subgames that can be generated from that state:
$$G = \text{mex}(\{a_1, a_2, a_3, \dots, a_n\})$$
where $a_i$ is the xor of grundy numbers of all the $m$ subgames generated in that possible set:
$$a_i = g_1 \oplus g_2 \oplus \dots \oplus g_n$$
As an example take a grundy's game with initial one heap with 8 sticks.
There are 3 possible sets of subgames we can generate here:
We can divide the heap in three possible ways:
1. (1, 7)
2. (2, 6)
3. (3, 5)

Then we can recursively calculate $a_i$ for each of these possible sets of subgames as $a_{(1, 7)}$, $a_{(2, 6)}$, $a_{(3, 5)}$
where $a_{(3, 5)} = f(3)\oplus f(5)$ and so on...
here $f(x)$ is the grundy number of state x, calculated recursively by applying the same procedure.
Then we can calculate the mex of all the $a_i$, i.e. $\text{mex}(\{a_{(1, 7)}, a_{(2, 6)}, a_{(3, 5)}\})$ to obtain $f(8)$ i.e. the grundy number for the current state.

The grundy numbers in this setup can be calculated effectively using Dynamic Programming:
f(1)=0
f(2)=0
f(3)=1
f(4)=0
f(5)=2
f(6)=1
f(7)=0
f(8)=2