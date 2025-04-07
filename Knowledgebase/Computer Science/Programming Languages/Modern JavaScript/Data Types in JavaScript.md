# Number
Numbers are stored as described by the IEEE-754 specification as in most languages like C, Perl, Ruby etc.
## Ways to write number literals
1. The underscore can be used to separate digits to make the number literal more readable. For example, `1_000_000_000` is completely valid.
2. For scientific notation, use `e` after the number to denote powers of ten multiplied by the number. For example, 2 billion can be written as `2e9`, i.e. $2 \times 10^9$. 7.3 million can be written as `7.3e6`, smaller numbers can be written with negative powers of 10, for example 5.7 nanoseconds are `5.7e-9`.
3. Hexadecimal numbers can be written following `0x`, for example 255 is `0xff` or `0xFF`. Octals are followed by `0o`, and binary is followed by `0b`.
## `.toString(base)`
`num.toString(base)` converts a number to a string in the numeral system with the given `base`.
A useful case for `36` is when we need to turn a long numeric identifier into something shorter.
## `parseInt(str, base)` and `parseFloat(str, base)`
Normal numeric conversion with unary `+` or `Number(...)` fail when the string inside isn't completely a number. For example `+"100px"` will fail because it contains characters `px`. If we want to extract out the numeric part in the prefix of a string, then we can use `parseInt` or `parseFloat`.

`parseFloat(str)` parses the string as an float.
`parseFloat(str)` parses the string as an int.

```javascript
alert( parseInt('100px') ); // 100
alert( parseFloat('12.5em') ); // 12.5

alert( parseInt('12.3') ); // 12, only the integer part is returned
alert( parseFloat('12.3.4') ); // 12.3, the second point stops the reading
```
There are situations when `parseInt/parseFloat` will return `NaN`. It happens when no digits could be read..
```javascript
alert( parseInt('a123') ); // NaN, the first symbol stops the process
```
### optional second argument of `parseInt`
`parseInt` has a second optional argument, `parseInt(str, base)` parses the string `str` into an integer in numeral system with given `base`, `2 ≤ base ≤ 36`.
```javascript
alert( parseInt('0xff', 16) ); // 255
alert( parseInt('ff', 16) ); // 255, without 0x also works

alert( parseInt('2n9c', 36) ); // 123456
```
## Rounding and floating-point precision
There are several built-in functions for rounding:
- `Math.floor` rounds down, for example, `3.1` becomes `3`, and `-1.1` becomes `-2`.
- `Math.ceil` rounds up, for example: `3.1` becomes `4`, and `-1.1` becomes `-1`.
- `Math.round` rounds to the nearest integer: `3.1` becomes `3`, `3.6` becomes `4`. In the middle cases `3.5` rounds up to `4`, and `-3.5` rounds up to `-3`.
- `Math.trunc` removes anything after the decimal point without rounding: `3.1` becomes `3`, `-1.1` becomes `-1`.
### round off after n digits of precision
To round after n decimal places of precision, use `.toFixed(precision)`, this returns a string, which can be converted back to number if required.
```javascript
let num = 12.34;
alert( num.toFixed(1) ); // "12.3"
```

```javascript
let num = 12.36;
alert( num.toFixed(1) ); // "12.4"
```

```javascript
let num = 12.34;
alert( num.toFixed(5) ); // "12.34000", added zeroes to make exactly 5 digits
```
### loss of precision
Since numbers are stored in 64 bits as per IEEE-754, there are cases when there aren't enough bits to contain the number precisely. This can happen when the number is too big, or the decimal part of number has infinite digits or more digits after decimal that can be contained by the IEEE-754 float.

This leads to cases like:
1. `alert( 1e500 ); // Infinity`

2. `alert( 0.1 + 0.2 == 0.3 ); // false` because `alert( 0.1 + 0.2 ); // 0.30000000000000004` due to precision errors.
3. `alert(9999999999999999); // shows 10000000000000000` i.e. a self-increasing number!
4. There exist two zeroes because one bit is assigned for sign, therefore `0` and `-0` are both different numbers.
## `isFinite` and `isNan`
- `isNaN(value)` converts its argument to a number and then tests it for being `NaN`
- `Number.isNaN(value)` checks whether its argument belongs to the `number` type, and if so, tests it for being `NaN`, i.e. it is more strict than `isNan`
- `isFinite(value)` converts its argument to a number and then tests it for not being `NaN/Infinity/-Infinity`
- `Number.isFinite(value)` checks whether its argument belongs to the `number` type, and if so, tests it for not being `NaN/Infinity/-Infinity`, i.e. it is more strict.
## [Built-in Math Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math)
The library is very small but can cover basic needs like 
- `Math.random()` returns a random number from 0 to 1 (not including 1).
- `Math.max(a, b, c...)` and `Math.min(a, b, c...)` returns the greatest and smallest from the arbitrary number of arguments.
- `Math.pow(n, power)` returns `n` raised to the given power.
# String
Strings can be created using single/double quotes or backticks, use backticks for formatting.

Only backticks can be used for multiline strings. Multiline strings can also be created using single/double quotes with `\n`.

To get the length of the string, use the `.length` property.

Strings are immutable, a new string must be created everytime a change is required. All the string methods and the string concatenation `+` return new strings instead of modifying the old one.
## escape sequence
1. `\r\n` carriage return line feed (CRLF)
2. `\n` newline
3. ```\` \' \"``` quotes
4. `\\` backslash
5. `\t` tab
6. `\b \f \v` Backspace, Form Feed, Vertical Tab
## accessing characters
- characters at some index can be accessed directly using square brackets as `str[index]`
- characters can also be accessed using `str.at(index)` function, which allows python like negative indexing.
- characters of strings can be looped over using `for..of` loops.
## accessing substrings
### `.slice(start, end)`
`str.slice(start, end)` returns the part of the string from `start` to (but not including) `end`. Supports negative indexing. If the `end` argument is not given, it returns substring until the end of the string.
```javascript
let str = "stringify";
alert( str.slice(0, 5) ); // 'strin', the substring from 0 to 5 (not including 5)
alert( str.slice(2) ); // 'ringify', from the 2nd position till the end
alert( str.slice(-4, -1) ); // 'gif'
```
### `str.substring(start, end)`
returns the part of the string _between_ `start` and `end` (not including `end`).
This is almost the same as `slice`, but it allows `start` to be greater than `end` (in this case it simply swaps `start` and `end` values).
### `str.substr(start, length)`
Returns the part of the string from `start`, with the given `length`.
```javascript
let str = "stringify";
alert( str.substr(2, 4) ); // 'ring', from the 2nd position get 4 characters
alert( str.substr(-4, 2) ); // 'gi', from the 4th position get 2 characters
```

> `.substr` isn't specified in the core JavaScript specification but in Annex B, although all browsers support it.
## searching characters, substrings
### `str.indexOf(substr, pos)`
It looks for the `substr` in `str`, starting from the given position `pos`, and returns the position where the match was found or `-1` if nothing can be found.
```javascript
let str = 'Widget with id';
alert( str.indexOf('id') ) // 1, occurrence from the zeroeth index
alert( str.indexOf('id', 2) ) // 12, occurrence after the 2nd index
```
We can call `.indexOf` in a loop and increment `pos` to find all the occurrences.
### `str.lastIndexOf(substr, pos)` 
Similar to `.indexOf` but starts searching from the end.
### `.includes(substr)`, `.startsWith(substr)`, `.endsWith(substr)`
All of these return a boolean that indicates if the string includes, starts with or ends with a substring.
## `.toLowerCase()` and `.toUpperCase()` are used to change the case of string.
## String Comparisons
The strings are compared using the lexicographical order of UTF-16 characters which JavaScript uses for its strings.

The method `str.codePointAt(pos)` can be used to get the integer value of a character according to UTF-16.

The method `String.fromCodePoint(code)` returns a character by its numeric `code`

The UTF-16 ordering is:
```
ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~ ¡¢£¤¥¦§¨©ª«¬­®¯°±²³´µ¶·¸¹º»¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜ
```

However, the above ordering is not accurate as it is not language accurate. For example uppercase `Z` comes before lowercase `a`, while in real language a should come before z. Also, characters like `Ö` come after `Z`, when in real language in some countries it should come before it.

Therefore, standard `<` `>` comparisons cannot reliably produce real language accurate comparisons. Hence, the internationalization standard [ECMA-402](https://www.ecma-international.org/publications-and-standards/standards/ecma-402/) provides a special method [str1.localeCompare(str2)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare) which returns an negative integer, zero, or positive integer indicating whether `str1` is less, equal or greater than `str2` respectively according to the language rules.

This method actually has two additional arguments specified in [the documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare), which allows it to specify the language (by default taken from the environment, letter order depends on the language) and setup additional rules like case sensitivity or should `"a"` and `"á"` be treated as the same etc.
## [String Library](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
contains other useful functions like `.trim()` and `.repeat(n)`
Additionally, there are [[Regular Expressions in JavaScript]]
Also, [[JavaScript - Unicode, surrogate pairs, diacritical marks, Oh my!]]
# Array
Array is a an object, suited to storing and managing ordered data items. It is declared as..
```javascript
let arr = [item1, item2...];
```

> There is another (**not recommended**) way to declare arrays of given size using `new Array(item1, item2, ...)`
## Accessing elements of an array
Elements can be accessed or modified using `arr[i]` syntax where `i` is the index of the element in the array.

To get the element, an alternate method is the `.at(i)` function. `.at` allows for python-like negative indexing.
## Array internal memory representation
Arrays are stored in contiguous memory for faster access. However, if out of bounds index is accessed, then the memory representation decomposes to that of a regular object giving up all the benefits of contiguous memory location.
## array to primitive conversion
Arrays do not implement `Symbol.toPrimitive`, neither a viable `valueOf`, they implement only `toString` conversion. The `toString` conversion returns comma-separated list of elements of the array. for example,
```javascript
let arr = [1, 2, 3];
alert( arr ); // 1,2,3
alert( String(arr) === '1,2,3' ); // true
```

When performing operations with arrays, if the other operand is an object, then object operation rules are applied, otherwise if the other operand is a primitive, then primitive operation rules are applied.

For example, When the binary plus `"+"` operator adds something to a string, it converts it to a string as well, therefore we get results like..
```javascript
alert( [] + 1 ); // "1"
alert( [1] + 1 ); // "11"
alert( [1,2] + 1 ); // "1,21"
```
This makes sense if we convert the arrays to their `toString` representation.
```javascript
alert( "" + 1 ); // "1"
alert( "1" + 1 ); // "11"
alert( "1,2" + 1 ); // "1,21"
```
## Length property of arrays
The `length` property is the array length or, to be precise, its last numeric index plus one. It is auto-adjusted by array methods. If we shorten `length` manually, the array is truncated.

If we set `array.length = 0`, then array is cleared.
## `push` `pop` `shift` `unshift`
We can use an array as a deque with the following operations,
1. `push(...items)` adds `items` to the end.
2. `pop()` removes the element from the end and returns it.
3. `shift()` removes the element from the beginning and returns it.
4. `unshift(...items)` adds `items` to the beginning.

However, unlike a deque which supports all these operations in O(1), JavaScript still implements these on a regular array, therefore while push and pop are in O(1), shift and unshift suffer from O(n) time complexity.
## `arr.splice()`
> `arr.splice(start, deleteCount, elem1, elem2, elem3, ...)` is the Swiss army knife for array manipulation

`arr.splice(start, deleteCount, elem1, elem2, ...)` takes two required arguments: `start` and `deleteCount` and deletes `deleteCount` amount of elements starting from the `start` index. If optionally, variadic arguments `elem1`, `elem2`, ... are specified then it also inserts the elements `elem1`, `elem2`, ... after the start position.

If `deleteCount` is set to zero, then splice can be used as insertion.
splicing also supports negative indexing for the `start` position.
## slicing
```javascript
arr.slice(start, end)
```
It returns a new array copying to it all items from index `start` to `end` (not including `end`). Both `start` and `end` can be negative.
If `end` argument is not specified, then it returns a subarray starting from `start` until the end of the array.
If no arguments are specified, then the array is simply copied as is, this can be used to quickly create a copy of the array.
## concatenation
```javascript
arr.concat(arg1, arg2...)
```
`arg1`, `arg2`, `...` can either be individual values or they can be arrays themselves. If any argument of `concat` is an array, then its elements are copied to `arr` instead.
```javascript
let arr = [1, 2];
arr.concat(3, [4, 5], 6, 7) // arr becomes [1, 2, 3, 4, 5, 6, 7]
```
### `Symbol.isConcatSpreadable` and array-like objects
An object can look like an array:
```javascript
let arrayLike = {
  0: "something",
  1: "else",
  length: 2
};
```
When concatenated to an array however, this object will be added as is. If we want this object to behave like an array during concatenation, i.e. its elements shall be concatenated instead of the whole array. Then we need to add a special property to this object, which is `Symbol.isConcatSpreadable`

If an array-like object contains the `Symbol.isConcatSpreadable` property set to true, then its property-values are added to the array separately instead of the whole object.
```javascript
let arr = [1, 2];

let arrayLike = {
  0: "something",
  1: "else",
  [Symbol.isConcatSpreadable]: true,
  length: 2
};

alert( arr.concat(arrayLike) ); // 1,2,something,else
```
## Looping over array
To loop over the elements of the array:
- `for (let i=0; i<arr.length; i++)` – works fastest, old-browser-compatible.
- `for (let item of arr)` – the modern syntax for items only,
- `for (let i in arr)` – never use.
## `arr.forEach`
The `arr.forEach` method allows to run a function for every element of the array.
For instance, alert each element of the array:
```javascript
["Bilbo", "Gandalf", "Nazgul"].forEach(alert);
```
Function expressions or Arrow functions are used to run more elaborate complex functions for each element.
```javascript
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
  alert(`${item} is at index ${index} in ${array}`);
});
```
The return value of the function (if it returns any) is thrown away and ignored.
## Comparing arrays
Regular comparison operators like ` == `, `<`, `>` do not work with arrays because arrays are objects and object comparison rules are applied instead of comparing the array element-wise. Hence it is wise to loop over the arrays and compare manually element-wise instead.
### comparing arrays with primitives
the array is first converted to primitive using `toString`, we get results like..
`alert(0 == []) // true`
## Searching arrays
### `arr.indexOf` and `arr.lastIndexOf`
`arr.indexOf(item, from)` looks for `item` starting from index `from`, and returns the index where it was found, otherwise `-1`.
`indexOf` uses strict equality ` === ` for comparison.
`arr.lastIndexOf` is the same as `arr.indexOf` but searches from the end.
### `arr.includes`
If we only need to check that item is in the array without finding it, then we can use `arr.includes(item, from)` which looks for `item` starting from index `from`, returns `true` if found.

> [!info] searching Nan
> If an array contains `NaN`, then `indexOf` cannot find it because `NaN` doesn't equal itself. But `arr.includes` can be used to check if `NaN` is in the array because `arr.includes` was introduced later to JavaScript and contains modern NaN equality comparison algorithms.
### `arr.find`, `arr.findIndex`, `arr.findLastIndex`
`arr.find(func)` takes in a function `func`. `func` is called for each item one by one. `func` should return a boolean. `arr.find` returns the first element in the array that returns true when supplied to `func`. 

When supplying each element to the given function, `find` also additionally supplies the element's index and a reference to the array to `func`, i.e. `func` can take upto three arguments if for some reason the index of the element and a reference to the array is required: `(item, index, array) => {}`

`arr.findIndex(func)` returns the index of the first element for which `func` returns true. `arr.findLastIndex(func)` is same as `arr.findIndex` but returns the last element that returns true for the given `func`
## `arr.sort(compareFunc)`
sorts the array by comparing elements according to the given `compareFunc`. 

`compareFunc` should take two elements, and return a negative number if the first is bigger than the second, otherwise it should return a positive number if second is bigger than the first, and if both are equal then `compareFunc` should return 0.

If no `compareFunc` is provided, then each item of the array is converted to string and sorted lexicographically.

-  common idiomatic syntax for sorting numbers: `arr.sort((a, b) => a - b)`
-  common idiomatic syntax for sorting strings: `arr.sort((a, b) => a.localeCompare(b))` even though javascript automatically sorts lexicographically, this method is preferred because it covers discrepancies with Unicode.

under the hood, sorting used optimized quicksort or Timsort.
## `arr.reverse()`
... reverses the order of elements of the array.
## split and join
`str.split(delim)` is a function of strings, not arrays. It splits the string into an array by the given delimiter `delim`.
In the example below, we split by a comma followed by a space:
```javascript
let names = 'Bilbo, Gandalf, Nazgul';
let arr = names.split(', '); // ['Bilbo', 'Gandalf', 'Nazgul']
```
If split is supplied with empty string `''` then it splits the string and returns an array containing each character as a separate element.

split also has a optional second argument which takes in an integer. It is a limit on the array length. If it is provided, then the extra elements are ignored.
```javascript
let arr = 'Bilbo, Gandalf, Nazgul, Saruman'.split(', ', 2);
alert(arr); // Bilbo, Gandalf
```

`arr.join(glue)` is a function of array, not string. It joins the array elements into a string using the `glue` provided.
```javascript
let arr = ['Bilbo', 'Gandalf', 'Nazgul'];
let str = arr.join(';'); // glue the array into a string using ;
alert( str ); // Bilbo;Gandalf;Nazgul
```
## map, filter, reduce
`arr.map(func)` calls the function `func` for each element of the array and returns the array of results.
```javascript
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6
```

`arr.filter(func)` returns a subarray of `arr` which contains only the elements that return true when supplied to `func`

`arr.reduce` accumulates a result over the array and returns it at the end.
```javascript
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```
Arguments:
- `accumulator` - is the result of the previous function call, equals `initial` the first time (if `initial` is provided).
- `item` - is the current array item.
- `index` - is its position.
- `array` - is the array.

```javascript
let arr = [1, 2, 3, 4, 5];
let result = arr.reduce((sum, current) => sum + current, 0);
alert(result); // 15
```

If there’s no `initial` provided, then `reduce` takes the first element of the array as the initial value and starts the iteration from the 2nd element.
## `thisArg` in array functions
Almost all array methods that call functions - like `find`, `filter`, `map`, with a notable exception of `sort`, accept an optional additional parameter `thisArg`.
```javascript
arr.find(func, thisArg);
arr.filter(func, thisArg);
arr.map(func, thisArg);
// thisArg is the optional last argument
```
### what does thisArg do?
```javascript
let army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    return user.age >= this.minAge && user.age < this.maxAge;
  }
};

let users = [
  {age: 16},
  {age: 20},
  {age: 23},
  {age: 30}
];

// find users, for who army.canJoin returns true
let soldiers = users.filter(army.canJoin, army);

alert(soldiers.length); // 2
alert(soldiers[0].age); // 20
alert(soldiers[1].age); // 23
```

If in the example above, if we used `users.filter(army.canJoin)` instead of `users.filter(army.canJoin, army);`, then `army.canJoin` would be called as a standalone function, with `this=undefined`, thus leading to an instant error.

A call to `users.filter(army.canJoin, army)` can be replaced with `users.filter(user => army.canJoin(user))`, that does the same.
## `Array.isArray`
Since arrays are based on objects, `typeof` does not help to distinguish a plain object from an array.
```javascript
alert(typeof {}); // object
alert(typeof []); // object
```
To check if some variable is an array, `typeof` won't work. Instead, `Array.isArray(value)` is used.
```javascript
alert(Array.isArray({})); // false
alert(Array.isArray([])); // true
```
## some and every
`arr.some(fn)`
The function `fn` is called on each element of the array similar to `map`. If any of the results are `true`, `some` returns `true`, otherwise `false`.

`arr.every(fn)`
The function `fn` is called on each element of the array similar to `map`. If and only if all results are `true`, `every` returns `true`, otherwise `false`.

We can use `every` to compare arrays:
```javascript
function arraysEqual(arr1, arr2) {
  return arr1.length === arr2.length && arr1.every((value, index) => value === arr2[index]);
}

alert( arraysEqual([1, 2], [1, 2])); // true
```
## [Arrays Manual on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
### arr.fill(value, start, end)
... fills the array with repeating `value` from index `start` to `end`.
### arr.copyWithin(target, start, end) 
... copies its elements from position `start` till position `end` into _itself_, at position `target` (overwrites existing).
### [arr.flat(depth)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)/[arr.flatMap(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)
... are used to create a new flat array from a multidimensional array.