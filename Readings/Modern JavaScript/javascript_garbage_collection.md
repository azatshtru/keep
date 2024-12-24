# reachability
1. a value in javascript is either reachable or non-reachable
2. The garbage collector is a background process that monitors all objects and cleans non-reachable ones from the memory.

## roots
roots are the base set of reachable values, following values are classified as roots
- The currently executing function, its local variables and parameters.
- Other functions on the current chain of nested calls, their local variables and parameters.
- Global variables.
there are some other, internal ones as well

any other value is considered reachable if it is reachable from roots, for example, any object that is referenced by a property of an object at root is considered reachable.

# mark-and-sweep
garbage collector uses mark-and-sweep algorithm to determine which objects are reachable and which aren't. mark-and-sweep is a basic Breadth-First Search:
1. The garbage collector takes roots and “marks” (remembers) them.
2. Then it visits and “marks” all references from them.
3. Then it visits marked objects and marks their references. All visited objects are remembered, so as not to visit the same object twice in the future.
4. …And so on until every reachable (from the roots) references are visited.
5. All objects except marked ones are removed.

# some GC optimizations
1. **Generational collection** – objects are split into two sets: “new ones” and “old ones”. In typical code, many objects have a short life span: they appear, do their job and die fast, so it makes sense to track new objects and clear the memory from them if that’s the case. Those that survive for long enough, become “old” and are examined less often.

2. **Incremental collection** – if there are many objects, and we try to walk and mark the whole object set at once, it may take some time and introduce visible delays in the execution. So the engine splits the whole set of existing objects into multiple parts. And then clear these parts one after another. There are many small garbage collections instead of a total one. That requires some extra bookkeeping between them to track changes, but we get many tiny delays instead of a big one.

3. **Idle-time collection** – the garbage collector tries to run only while the CPU is idle, to reduce the possible effect on the execution.
