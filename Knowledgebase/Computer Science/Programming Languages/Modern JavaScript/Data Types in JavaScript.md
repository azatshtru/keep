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
Also, [[Unicode, surrogate pairs, diacritical marks and normalization, Oh my!]]
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
## Looping over array
To loop over the elements of the array:
- `for (let i=0; i<arr.length; i++)` – works fastest, old-browser-compatible.
- `for (let item of arr)` – the modern syntax for items only,
- `for (let i in arr)` – never use.
## Comparing arrays
Regular comparison operators like ` == `, `<`, `>` do not work with arrays because arrays are objects and object comparison rules are applied instead of comparing the array element-wise. Hence it is wise to loop over the arrays and compare manually element-wise instead.
### comparing arrays with primitives
the array is first converted to primitive using `toString`, we get results like..
`alert(0 == []) // true`