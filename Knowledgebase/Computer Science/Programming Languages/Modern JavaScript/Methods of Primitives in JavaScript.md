In JavaScript, objects can have methods which allow us to manipulate them or get data from them in a structured manner. However, the primitive types like strings, symbols, numbers, BigInt, booleans, undefined, null do not have methods.

To allow manipulation and ease-of-use of primitives, JavaScript has object wrappers. If a method is called on a primitive, an object wrapper for it is created, then the method is executed, then the object wrapper is immediately destroyed to keep the primitive light.

For instance, there exists a string method [str.toUpperCase()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) that returns a capitalized `str`.

Here’s how it works:
```javascript
let str = "Hello";

alert( str.toUpperCase() ); // HELLO
```

Here’s what actually happens in `str.toUpperCase()`:
1. The string `str` is a primitive. So in the moment of accessing its property, a special object is created that knows the value of the string, and has useful methods, like `toUpperCase()`.
2. That method runs and returns a new string (shown by `alert`).
3. The special object is destroyed, leaving the primitive `str` alone.

null and undefined have no methods or object wrappers, hence they are "the most primitive"

> Calling methods on number literals can be performed by surrounding them with parenthesis like `(12345).toString()` or by using two dots like `12345..toString()`