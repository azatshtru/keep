# constructor functions
constructor functions are used to define similar objects in an ergonomic manner.

constructor functions can be defined as:
```javascript
function User(name) {
  this.name = name;
  this.isAdmin = false;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}
```
.. and can be used to create an object later using the `new` operator:
```javascript
let user = new User("Jack");
```
1. Conventionally, names of constructor functions are written in PascalCase.
2. any function except arrow functions can be used to create constructor functions
3. if `return` statement is used in a constructor function, it is usually ignored. But if the `return` statement returns an object, then that object is returned.

## the `new` operator
calling the function with `new` creates an empty object inside the function being called. Then the empty object is assigned to `this` and the function executes, modifying the properties of the empty object. Lastly, the empty object is returned. The `User` constructor function defined above is somewhat equivalent to:
```javascript
function User(name) {
  // this = {};  (implicitly)

  // add properties to this
  this.name = name;
  this.isAdmin = false;

  // return this;  (implicitly)
}
```

## `new.target`
when used inside a function, `new.target` returns true if the function was called with the `new` operator, otherwise it returns false.

```javascript
function User() {
  alert(new.target); // alerts "true" if User is called with `new`, otherwise false
}

// without "new":
User();

// with "new":
new User();
```
