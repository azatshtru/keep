Take observations, and write everything down.
- Make a summary of the problem statement
- Write down and grasp the size of constraints
- think about the direction the constraints and problem want you to take, if you can take multiple directions, which is the most optimal one

Simulation
- Take an example from the observation and work through it, write down the steps you would take to get to the output using that example input only
- after working through one example, generalize, replace numbers with variables and think about the newer checks you need to keep track of
- proofs are just generalizations using variables

Recurrence relations:
- the significant **actions** you can take at any point during the function are described by recursive calls to that function
- during each action the parameters of the function change in order to reduce the recursive state
- each return call is the amount contributed to the recursive state
- a recursive function should be named exactly to describe what it does
- never think more than one layer deeper into the recursion, don't simulate or visualize, if the function's promises are correct, you will get the required value no matter what, believe in the recursive function
- don't forget to add base cases and checks
- A RECURRENCE FUNCTION MUST HAVE ZERO SIDE EFFECTS other than modifying the memoization array

Time Complexity
- time complexity of recurrence relations is calculated by multiplying the bounds of parameters to the recurrence function with cached complexity, cached complexity is the time complexity obtained when we assume that the recursive calls to deeper levels are O(1), we assume constant time with recursive calls because we use memoization in dynamic programming