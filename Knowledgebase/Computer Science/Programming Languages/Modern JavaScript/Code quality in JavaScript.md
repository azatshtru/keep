## Debugging in the browser
Debugging is done through the sources tab. It includes:
1. setting, managing breakpoints
2. a tab for **watch**ing values of expressions
3. a tab for looking at **scope**s of variables, local and global
4. a **console** window to run commands and console.log expressions while the code is paused
5. a tab for visualizing **call stack**
6. tracing execution via step, step into, step over, step out
#### Difference between "step" and "step into"
For non-asynchronous code, there is no difference. "Step” command ignores async actions, such as `setTimeout` (scheduled function call), that execute later. The “Step into” goes into their code, waiting for them if necessary.

## Code style, Ninja code and general guidelines
>"none set in stone"
### Code style
1. [IdiomaticJS](https://github.com/rwaldron/idiomatic.js)
2. [StandardJS](https://standardjs.com/rules)
3. [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
### [Ninja code](https://javascript.info/ninja-code)

### General guidelines
...

### Comments

#### A comment is (almost always) redundant if it explains code.
#### Good comments 
1. give an high-level overview of the structure of the code.
2. describe a mathematical, or non-intuitive solution or logic used in a function that is not immediately obvious
3. describe the example usage of a function right before function's definition
#### Bad comments
Comments that tell how code works or what it does. These comments are redundant and can be replaced by appropriate function naming and function composition.

### Testing using Mocha and Chai

### Polyfills and Transpilers
To get newer JavaScript API functions in older JavaScript functions, a polyfill like [core.js](https://github.com/zloirock/core-js) is used.
To get newer JavaScript language constructs in older JavaScript versions, a transpiler like [Babel](https://babeljs.io/)is used. A transpiler converts language constructs of newer JavaScript versions to older version compatible code.