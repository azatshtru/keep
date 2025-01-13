---
course: java programing
week: 10
---

1. Inheritance using extends, superclass, subclass
2. subclasses cannot access the private properties and methods of the superclass. to access private properties of superclass in the subclass, one should make public accessor (set, get) functions in the superclass.
3. A variable of superclass type can refer to a value of it's subclass types in Java.
	```Java
	Vehicle v = new Truck();
	```
	 `new Truck()` creates a `Truck` type object, but it can still be stored in a `Vehicle` type variable if `Vehicle` is a superclass of `Truck`.
4. `super(args)` can be called from inside the subclass, this method calls the constructor of the superclass. the properties of the superclass can be accessed using the `super.memberName` syntax. the methods of superclass can be called using `super.methodName(args)` syntax.
5. method overriding, java understands whether the method of superclass or subclass is being called depending on the method signature and call structure. If a method is called on an object, java checks if the method signature of the current class matches with the call, if not, then it checks whether the superclass's method signature matches with the call.