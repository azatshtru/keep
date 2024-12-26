# methods
since functions are first-class objects in Javascript, object properties can be set as functions.
The functions which are object properties are called **methods**.

## defining methods
1. methods can be defined directly:
```javascript
let user = {
  name: "John",
  sayHi: function() {
    alert("Hello");
  }
};
```
2. methods can be defined using the **method shorthand**:
```javascript
let user = {
  name: "John",
  sayHi() {
    alert("Hello");
  }
};
```
3. methods can be defined after the object is declared:
```javascript
let user = {
  name: "John",
  age: 30
};

user.sayHi = function() {
  alert("Hello!");
};
```

4. methods can be defined by setting a property equal to a function:
```javascript
let user = {
  name: "John",
  age: 30
};

// first, declare
function sayHi() {
  alert("Hello!");
}

// then add as a method
user.sayHi = sayHi;
```

# `this` keyword
methods can reference the object using `this` keyword

## `this` is unbound
`this` keyword is evaluated at runtime, therefore, multiple objects can reference the same function as their method property and javascript will assign the value of `this` according to the context at runtime:
```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different `this`
user.f(); // here, this == user
admin.f(); // here, this == admin

admin['f']() // methods can be accessed using the quote syntax, too
```

## calling a function with `this` directly (not as a method)
```javascript
function sayHi() {
  alert(this);
}

sayHi();
```
In strict mode, `this` is undefined. In the example above, if we try to access `this.name`, there will be an error.

In non-strict mode, the value of `this` in such case will be the global object (usually, the window in a browser). This is a historical behavior that "use strict" fixes.

## `this` in arrow functions
If we use `this` in an arrow function, the value of `this` is taken from the outer scopes.

For instance, here arrow() uses this from the outer user.sayHi() method:
```javascript
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi();
```

//////////////////////////// TASKS ///////////////////////////////

What is the result of the `alert` call in the following code:
```javascript
function makeUser() {
  return {
    name: "John",
    ref: this
  };
}

let user = makeUser();

alert( user.ref.name ); // What's the result?
```
> Answer: error
> this is because evaluation of `this` is lazy. In the above code, `this` is only evaluated when `makeUser` is called, when the call happens, the value of `this` at that instant is `undefined` because `makeUser` was called as function, not as a method. And when we try to access the `name` property of `undefined`, we get error.

> [!INFO] Final notes
> impure object methods usually return the object to allow chaining using the factory design pattern.
