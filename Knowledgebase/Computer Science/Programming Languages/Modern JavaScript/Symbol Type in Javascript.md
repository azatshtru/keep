`Symbol` is a primitive type for unique identifiers.
Symbols are created with `Symbol(name)` call with an optional description (name).
Only strings and symbols can be used as object properties in JavaScript.
## assigning symbols as object properties
### symbol properties using the bracket syntax
symbols can be assigned using the bracket syntax `object[symbol]`
```javascript
let user = {
  name: "John",
};
let id = Symbol("id");

user[id]: 123
```
### symbol properties in object literals
symbols can be put into objects directly using the bracket syntax for object literals where we enclose the variable name of the property in square brackets.
```javascript
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // not "id": 123
};
```
## symbols are guaranteed to be unique
```javascript
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false
```
## symbols do not auto-convert to string
In JavaScript, all primitive types are auto-converted to string. But symbols need to be converted explicitly using `.toString()`.
## symbol registry
Symbols are always different values, even if they have the same name. If we want same-named symbols to be equal, then we should use the global registry.

`Symbol.for(key)` returns (creates if needed) a global symbol with `key` as the name inside the global registry.
Multiple calls of `Symbol.for` with the same `key` return exactly the same symbol.

`Symbol.keyFor(symbol)` takes in a symbol and returns its description (name) if the symbol is defined in the global registry, otherwise it returns `undefined`

`symbol.description` can be used to access the description of every symbol regardless if it is defined in the global registry or not.
## symbol property access
string properties are iterated over using `for..in` loops, or `Object.keys()` method. But symbol properties are invisible to `for..in` loops and `Object.keys()`. However, `Object.assign()` still copies all the properties, both symbol and string to another object.
## use cases of Symbols
1. “Hidden” object properties.
    
    If we want to add a property into an object that belongs to another script or a library, we can create a symbol and use it as a property key. A symbolic property does not appear in `for..in`, so it won’t be accidentally processed together with other properties in the other library.
	
    Also it won’t be accessed directly, because another script does not have our symbol, we can access the property of the foreign object only by using the symbol we created in our script. So the property will be protected from accidental use or overwrite in the other script.
    
    So we can “covertly” hide something into objects that we need, but others should not see, using symbolic properties.
	
2. System symbols
	
	There are many system defined symbols that are auto-assigned as property keys to all objects created in JavaScript. A list of such symbol properties can be found in the [Well-known symbols](https://tc39.github.io/ecma262/#sec-well-known-symbols) table.
	
	There is a global system symbol in the registry called `Symbol.hasInstance`, this means that every object will have a hasInstance property. These symbolic properties are used by different javascript constructs like built-in functions and operators and can be used to extend the behaviour of functions, operators and other constructs under JavaScript specification.

> Technically, symbols are not 100% hidden. There is a built-in method [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)that allows us to get all symbols. Also there is a method named [Reflect.ownKeys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) that returns _all_ keys of an object including symbolic ones. But most libraries, built-in functions and syntax constructs don’t use these methods.