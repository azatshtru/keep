# object references
primitive types are copied when assigned to other variables, but objects are never copied, instead, another reference is created that points to the same object in memory.

## object comparision
two object references are equal **if and only if** they point to the same object in the memory.
```javascript
let a = {};
let b = a; // copy the reference

alert( a == b ); // true, both variables reference the same object
alert( a === b ); // true

//////////////////////////////////////////////////////////////////////

let a = {};
let b = {}; // two independent objects

alert( a == b ); // false
```
## `const` object can be modified
```javascript
const obj = { name: "Pete" }
```
here obj is a constant object reference, hence it cannot refer to anything else, but `{ name: "Pete" }`, (i.e. the object it references) can still be modified:
```javascript
obj.name = "John"
```
# object cloning
## cloning an object using a for..in loop
an object can be copied by using a for..in loop:
```javascript
let user = {
  name: "John",
  age: 30
};

let clone = {}; // create a new empty object

// copy all user properties into it
for (let key in user) {
  clone[key] = user[key];
}
```

## `Object.assign(dest, ...sources)`
`Object.assign` assigns the properties of source objects to dest object

```javascript
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// copies all properties from permissions1 and permissions2 into user
Object.assign(user, permissions1, permissions2); // user = { name: "John", canView: true, canEdit: true }
```
- If a property of any source object already exists in the destination object, then it is overwritten.
- Object.assign also returns the destination object.
- copying using `Object.assign` `let clone = Object.assign({}, user);`

## cloning using spread syntax
An object can be cloned using the spread syntax: `let clone = {...user}`

## issues with the cloning methods described above
However, these methods have a few issues:
1. If a property of the original object references itself, then the same property in the cloned object will reference the original object, not the clone.
2. If a property of the original object has another deeper object, then the cloned object will not clone the deeper object, the same property in the cloned object will refer to the deeper object of the original object.
```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};
let clone = Object.assign({}, user);
```
In the example above, `clone.sizes == user.sizes` will return true, i.e. both `clone` and `user` refer to the same `sizes` object, `sizes` object is not copied.

## a better clone: `structuredClone`
`structuredClone(object)` method takes in an object and creates a deep copy of the object, it handles self-references and deeper objects.

However, `structuredClone` cannot handle function properties like `f: function() {}`
To resolve the issue with function properties, we can use a combination of cloning methods, or use the `_.cloneDeep(obj)` method of the JavaScript extenstion library [lodash](https://lodash.com)

