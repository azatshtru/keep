---
course: java programing
week: 11
---
- Polymorphism can be implemented by inheritance using method overloading.
- abstract classes and abstract methods can be implemented, which are meant to be extended and overloaded by other classes in Java.
- abstract methods are not defined during declaration of abstract class, they are overloaded when the abstract class is extended.
- abstract classes cannot be instantiated into objects like normal classes
- `final` keyword can be put before the method declaration to disallow overloading of the method in derived classes. this is called early binding. `final` can also be used with class properties.
# interfaces
interfaces are defined as: 
```Java
<access> interface <interface_name> {
    <function_declarations>;
    <variable_definitions>;
}
```
- access is public/private 
- if interface is public, methods cannot be private, methods cannot have different access than interfaces. 
- the variables defined in interfaces are final and static, i.e. they cannot be overloaded and their values are shared across instances and no object creation is required to access them.
- `implements` keyword can be used in when declaring classes to implement the interface for them. the classes are then required to implement the interface functions on their own.
- if a class implements an interface but doesn't implement all of the interface's methods, then the class declaration should be preceded with an `abstract`keyword.
- interfaces can extend and inherit from other interfaces like classes can extend other classes using the `extends` keyword.