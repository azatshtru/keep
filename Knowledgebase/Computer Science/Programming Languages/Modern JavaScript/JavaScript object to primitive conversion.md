# conversion hints
A hint can be one of three strings:
1. "string"
2. "number"
3. "default"

hints are determined by JavaScript engine specification, based on context

there are contexts where objects need to be implicitly converted to primitives like strings or numbers, for example, in `obj * 3`, `obj` needs to be converted to a number to carry out the operation, so JavaScript determines the conversion hint to be a `"number"`.

similarly, in `alert(obj)`, `obj` needs to be converted to string to be printed, so JavaScript engine determines the hint to be `"string"`.

Based on the hints, JavaScript carries out object to primitive conversions based on a conversion algorithm.
# primitive conversion

The primitive conversion algorithm is used to determine how to convert objects to primitives based on hints.

1. Call `obj[Symbol.toPrimitive](hint)` if the method exists,
2. Otherwise if hint is `"string"`, try calling `obj.toString()` or `obj.valueOf()`, whatever exists.
3. Otherwise if hint is `"number"` or `"default"` try calling `obj.valueOf()` or `obj.toString()`, whatever exists.
## `Symbol.toPrimitive`

`Symbol.toPrimitive` is a globally defined system symbol. We can use this symbol as the object key for a method of any object we create.

`Symbol.toPrimitive` takes a hint and we can overload `Symbol.toPrimitive` and write our own logic for primitive conversion based on the hint.

the hint is determined and supplied by the JavaScript engine as described above.

```javascript
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};
```
In the above object literal, we define a third property with key `Symbol.toPrimitive` and set it to a method that returns the `name` property if the hint passed to the method is "string", and returns `money` property if the hint is "number" or "default".
## `toString/valueOf`
In objects, two methods can be defined to carry out conversions as well. These methods are called after the JavaScript engine tries calling `Symbol.isPrimitive` and finds that it isn't overloaded for the object at hand.

If the hint is "string", then JavaScript engine calls the `toString()` method on the object, and if `toString` is not found, then it tries calling the `valueOf` method on the same object.

If the hine is "number" or "default", then JavaScript calls the `valueOf()` method on the object, if `valueOf` is not found, then it calls the `toString` method on the same object.
## default behavior of valueOf and toString
1. `valueOf` by default returns the object itself.
2. `toString` returns `[object Object]` by default.
## overloading valueOf and toString
`valueOf` and `toString` can be method overloaded inside the object to return primitive values based on the hint.

```javascript
let user = {
	name: "John",
	money: 1000,

	toString() {
		return `{name: "${this.name}"}`;
	}

	valueOf() {
		return this.money;
	}
}
```
This achieves the same result as overloading `Symbol.isPrimitive` and returning based on hint, however there is a major difference:

`Symbol.isPrimitive` gives an error if an object is returned from it.
`toString` and `valueOf` do not give errors if an object is returned from them.
Therefore, using `Symbol.isPrimitive` is safer.