# Introduction to Modern JavaScript
## Javascript engines
Javascript engine is a software component that executes JavaScript code. Mozilla Firefox used SpiderMonkey, and Google Chrome uses V8.
## What can JavaScript do?
Send requests, react to events, cookies, manipulation of styles and HTML, and localStorage
## What can't JavaScript do?
JavaScripts limitations are based on user's safety.
1. It doesn't know about other tabs
2. It can't write to the devices hard disk from the browser.
3. For two different websites to communicate, explicit headers need to be passed.
These limitations do not apply in server environments.
## Why is JavaScript unique? Why are we using it?
It is the only browser technology supported by major browsers with a rich ecosystem, so we got no choice. That said, simple things are done simply in JavaScript because it was made to do simple things.

Since JavaScript lacks type checking and other programming style features, there are different languages and tools that are transpiled to JavaScript, like Flow, TypeScript and Brython.
# Specification
Javascript is a developing language and hence the specification changes. The released versions are based on [ECMAScript spec](https://www.ecma-international.org/publications/standards/Ecma-262.htm). Latest draft for it between releases can be found on [https://tc39.es/ecma262/](https://tc39.es/ecma262/). 

The manual for development use and examples is on [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference).

As features are changing, the compatibility manual can be found at [caniuse](https://caniuse.com) and the compatibility table is [https://compat-table.github.io/compat-table/es6/](https://compat-table.github.io/compat-table/es6/)
## semicolons?
It is always better to use semicolons than not, even though the interpreter automatically puts semicolons at reasonable places, it will miss more often than not.
# `use strict`
This is the defining pillar of modern JavaScript.

Older javascript had some features before the spec, for backward compatibility they are kept. To explicitly use modern javascript without the old javascript features, it is always better to use `use strict`. Modern features like classes automatically use this.
# basic variable naming conventions
1. Along with the standard naming, `$` and `_` can also be used in variable names. 
2. Use camelCase for variables and runtime constants.
3. Be descriptive and consistent about variable names.
4. Use SCREAMING_CASE to declare hardcoded constants.
# Data Types
There are 8 basic data types, can be checked using `typeof`.
1. **Number** represents any number, Infinity and NaN. NaN operated with anything results in NaN except `NaN ** 0` which is 1. `1/0` is `Infinity`.
2. **BigInt** type represents numbers bigger than $2^{53}$ and is created by appending `n` after the number.
3. **Strings**, can be created using single/double quotes or backticks, use backticks for formatting.
4. **Boolean**
5. **null** represents lack of value in memory.
6. **undefined** represents unassigned variables, no variable should be explicitly assigned to undefined as per convention.
7. **Object** represents data.
8. **Symbol** type is used to create unique identifiers for objects.

Javascript contains a `typeof` operand which can be used before a value to get the data type. For example, `typeof 8n` will return `"bigint"`, `typeof true` returns `"boolean"`.

> [!warning] typeof interpreter error
> `typeof null` returns `"object"` even though `null` is a type of its own. This is an error that is kept for compatibility.

> [!info] the `"function"` type
> `typeof alert` returns `"function"`. the function type is a subset of the object type. functions are also objects.
# alert, prompt, confirm
all of these pause the execution of the script until the user continues.
1. alert is used to display something to the user, it returns `undefined`
2. prompt is used to ask the user for a string, if the user provides the string, prompt returns it, if the user cancels, then prompt returns null.
3. confirm is used to ask some question to the user, with two buttons: *Ok* and *Cancel*, if the user presses Ok, then true is returned otherwise false.

```javascript
let result = prompt(question, [default]);
alert(result);
```
# Primitive Type conversion
## String conversion
Things are converted to strings using the function `String(value)`.
- Functions that need string form of the value (for example, `alert`) automatically convert the passed value to string before use.
## Numeric conversion
Values are converted to numbers using the function `Number(value)`.

| Value          | Becomes…                                                                                                                                                                                                             |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| undefined      | NaN                                                                                                                                                                                                                  |
| null           | 0                                                                                                                                                                                                                    |
| true and false | 1 and 0                                                                                                                                                                                                              |
| string<br>     | Whitespaces (includes spaces, tabs \t, newlines \n etc.) from the start and end are removed. If the remaining string is empty, the result is 0. Otherwise, the number is “read” from the string. An error gives NaN. |
Arithmetic operators and functions that require numbers automatically convert the passed value to a number. For example, `"6" / "2"` in javascript is `3`.

If something cannot be converted to a number, for example `Number("hello")`, then the `Number` function returns `NaN`.
## Boolean conversion
If anything almost feels like a "false" (values like `0, null, undefined, NaN, ""`), then the function `Boolean(value)` gives false, otherwise true.

> There are exceptions, `Boolean("0")` and `Boolean(" ")` return true.
# Arithmetic Operators
## precedence
The usual arithmetic operators with their usual precedences exist in JavaScript. Operated left to right if two operators have same precedence.

usual arithmetic precedence is 
1. unary
2. exponentiation (`**`)
3. multiply/divide/modulo
4. addition/subtraction
## The unary `+`
The unary `+` acts like Numeric conversion: `+null` returns `0`, `+true` returns `1`, `+"3"` returns `3`.
## Arithmetic operators on strings
- Strings are concatenated using `+` operator.
- If string is plused to any other value, then the other value is also converted to string. `"6"+3` returns `"63"`. Further, conversions are done left to right when required, hence `4+4+"2"` gives `"82"` and `"2" + 5 + 5` gives `"255"`.
- Any other arithmetic operator with string converts the string to a number. For example, `"6"-2` will give `4` and `"6"/"3"` will give `2`.
# Assignment operator
anything on the right of assignment operator goes inside the variable to the left of the assignment operator.
## Assignment operator returns the value.
This can be used to chain assignments: `a = b = c = 5 + 4`
## increments and decrements
Increments `++` and decrements `--` hold higher precedence than most arithmetic operators and are used to increase or decrease a number by one.
they also return a number.
Postfix increment or decrement returns the number before the increment or decrement.
Prefix returns the number after increment or decrement.
# Comma
returns the value of the last expression. For example `(1+2, 3+4, 5+7)` returns `12` because the last expression `5+7` gives `12`
Comma has the least precedence, less than assignment, as an example, in `a = 1 + 2, 3 + 4`, javascript evaluates `+` first, summing the numbers into `a = 3, 7`, then the assignment operator ` = ` assigns `a = 3`, and the rest is ignored. It’s like `(a = 1 + 2), 3 + 4`.
# Comparison Operators
All comparison operators return a boolean value.
## String comparison
To compare strings, JavaScript uses a lexicographical ordering, in other words, if a word comes first in the dictionary, it is smaller than the word that comes later.

> [!info] the dictionary is based on unicode.
> This means that "A" and "a" are not same, infact "a" is greater than "A", because it has a higher index in Unicode.
## Comparison between different types
When comparing values of different types, JavaScript converts them to numbers. This can lead to results like this:
```javascript
let a = 0;
alert( Boolean(a) ); // false

let b = "0";
alert( Boolean(b) ); // true

alert(a == b); // true!
```
## Strict equality
Since the ` == ` operator converts operands to numbers before adding them, it cannot be used reliably to check for equality, `'' == false` gives `true` because both the empty string and false convert to zero.

The ` === ` operator returns true only when both the operands are equal, it first checks the types, if types are same, then it converts the operands to numbers and compares them. But if types are different, it simply returns false without checking further.

The strict non-equality operator `!==` is also used to reliably check non-equality in place of the non-equality operator `!=`.
## Equality with null, NaN and undefined
The ` == ` operator converts every operand to a number, except null and undefined, for null and undefined, a different equality rule follows: The values null and undefined equal ` == ` each other and do not equal any other value, i.e. `null == undefined` returns true even though `null` is `0` when converted to a number and `undefined` is `NaN`, But `null == 0` returns `false` even though null is also zero as a number.

The strict equality ` === ` between null and undefined returns false because they have different types.

> [!info] Strange result: null vs 0
> ```
> alert( null > 0 );  // (1) false
> alert( null == 0 ); // (2) false
> alert( null >= 0 ); // (3) true
> ```
> The reason is that an equality check == and comparisons ` > < >= <= ` work differently. Comparisons convert `null` to a number, treating it as `0`. That’s why `null >= 0` is true and `null > 0` is false.
> 
> On the other hand, the equality check == for `undefined` and `null` is defined such that, without any conversions, they equal each other and don’t equal anything else. That’s why `null == 0` is false.

NaN returns false for all comparisons.

> [!CAUTION]
> Be careful when using comparisons like > or < with variables that can occasionally be null/undefined. Checking for null/undefined separately is a good idea. Treat any comparison with `undefined/null` except the strict equality === with exceptional care.
# Conditionals
if statements work in javascript similar to most languages, the expression inside the parentheses is converted to boolean, there is also the ternary operator `?:` (with lower precedence than comparison operators and logical operators), and some guidelines:
1. always put braces after ifs
2. Try not to use ternary as a replacement for ifs, but only when conditionally assigning a value, although this mostly depends on taste.

There is also the switch statement, which runs in the standard C way, if break if provided, then it breaks, otherwise, it runs till the end of switch. The switch statement checks equality with the cases strictly. Several cases in switch can be grouped by writing something like `case 1: case 2:`
# Logical operators
- or `||`, and `&&`, not `!` work the same way as in most languages, but they have some extra features in JavaScript.
- precedence of `&&` is higher than `||`.
- the operands are converted to booleans automatically based on the type conversions explained before.
## chaining with or `||`
or (`||`) returns the first truthy value from an expression, if no value is truthy, then the last operand is returned.
This can be used to set default values..
`let userDisplayName = (firstName || lastName || nickName || "Anonymous");`

This will check if `firstName` exists and it will assign it to `userDisplayName`, if not, it will check the second operand `lastName`, if it is empty, then `nickName`, and if all are empty, then `"Anonymous"` is assigned to it.
## chaining with and `&&`
and (`&&`) works the same way as `or`, but it returns the first falsy value as is, if no falsy value is found than the last operand is returned.
## boolean conversion using double not `!!`
a *double not* (`!!`) is used to convert any value to boolean. This is like the unary `+`, because unary `+` is arithmetic operator, it converts the operand to numbers, similarly the first NOT converts the value to boolean and returns the inverse, and the second NOT inverses it again. In the end, we have a plain value-to-boolean conversion.
```javascript
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```
## Nullish coalescing logical operator `??`
This works the same as logical or `||`, but instead of returning the first truthy value like or, it returns the first *defined* value, a defined value is any value that isn't null or undefined. This is used to quickly check and provide a default value if some value is null.

Nullish coalescing operator has same precedence than logical or, but it cannot be used in the same expression with logical and, or logical or without putting parentheses around the expression. This is a design decision in JavaScript to ensure safety while using this operator.

```let x = 1 && 2 ?? 3; // Syntax error```
```let x = (1 && 2) ?? 3; // Works```

# Loops
JavaScript supports three types of loops: `for`, `while`, and `do..while`.
Loops can be labeled, for example `someLoop: for(let i = 0; i < n; i++)`
Usual `break` and `continue` directives exist, the directives can be paired with loop labels to break the loop under that label. 

# Functions
A function declaration looks like this:
```javascript
function name(parameters, delimited, by, comma) {
  /* code */
}
```
- Values passed to a function as parameters are copied to its local variables, hence parameters can be modified without modifying the arguments.
- Functions can have default parameters using `parameter=default_value` if an argument isn't passed to a function, the parameter equals the default value. 
- If no arguments are passed to the function, then they are replaced by undefined if no default value is specified.
- If undefined is explicitly passed to a function, then it is replaced by default value.
- A function may access outer variables. But the code outside of the function doesn’t see its local variables.
- A function can return a value. If it doesn’t, then its result is undefined.

> To make the code clean and easy to understand, it’s recommended to use mainly local variables and parameters in the function, not outer variables.
>
> It is always easier to understand a function which gets parameters, works with them and returns a result than a function which gets no parameters, but modifies outer variables as a side effect.
## Function naming
A name should clearly describe what the function does. When we see a function call in the code, a good name instantly gives us an understanding what it does and returns.
A function is an action, so function names are usually verbal.
There exist many well-known function prefixes like create…, show…, get…, check… and so on. Use them to hint what a function does.
## Function expressions
Functions are values. They can be assigned, copied or declared in any place of the code.
If the function is declared as a separate statement in the main code flow, that’s called a “Function Declaration”.
If the function is created as a part of an expression, it’s called a “Function Expression”.
Function Declarations are processed before the code block is executed. They are visible everywhere in the block.
Function Expressions are created when the execution flow reaches them.
In most cases when we need to declare a function, a Function Declaration is preferable, because it is visible prior to the declaration itself. That gives us more flexibility in code organization, and is usually more readable.

So we should use a Function Expression only when a Function Declaration is not fit for the task.

```javascript
const a = function() {
	//some code;
}
```
## Arrow functions
```javascript
//single line
(parameters) => code;

//multiline
(parameters) => {
	code;
}
```
