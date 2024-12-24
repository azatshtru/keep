# object syntax
```javascript
let user = {
  name: "John",
  age: 30,
  "likes birds": true, // multiword properties must be quoted
};
```

- trailing commas do not result in an error and can be put to reduce chances of syntax errors
- object keys need to be strings, if they aren't strings they are converted to strings, for example, 0 is converted to "0"

# empty objects
```javascript
let user1 = new Object(); // object constructor syntax
let user2 = {}; // object literal syntax
```

## accessing properties
1. dot syntax to access properties `user.name`, `user.age`
2. properties can also be accessed using square brackets `user["name"]` `user["likes birds"]`
3. if a property doesn't exist, then it returns `undefined`
4. to check if a property exists in an object, use the `in` operator: `"key" in object`
5. using the in operator is preferred over checking for undefined because a key might store `undefined` as its value.

## adding/set properties
- dot syntax to add/set properties:
    `user.isAdmin = true;`

## deleting properties
- delete operator to remove a property
    `delete user.isAdmin;`

## computed properties
```javascript
let foo = "baz"
let bar = {
   [foo]: 42
}
```
Here, property [foo] takes the value "baz", this is equivalent to:
```javascript
let foo = "baz"
let bar = {}
bar[foo] = 42
```

## property value shorthand
JavaScript has property value shorthand, which means: `return { name: name, age: age, }`
can be written as: `return { name, age, }`

> `object.__proto__` is a special property that cannot be set to a non-object value

# for..in loop
for..in loop can be used to iterate over all the properties of an object
```javascript
for (let key in object) {
}
```

> do not rely on the order of properties in an object, usually properties show up in the order they are added to the object, but if keys can be directly converted to integers, then they show up in increasing numerical order.

