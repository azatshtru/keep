Every reference in rust has an associated lifetime, which indicates how long the reference will be kept in the memory. Explicit lifetimes are specified using the lifetime syntax.

|               Type of reference               | Lifetime specifier |
| :-------------------------------------------: | ------------------ |
| Immutable reference without explicit lifetime | `&str`             |
|  Immutable reference with explicit lifetime   | `&'a str`          |
|   Mutable reference with explicit lifetime    | `&'a mut str`      |
Lifetime syntax in Rust is used to connect lifetimes of references in the parameters to the lifetimes of the references that are returned from the function. Lifetime syntax cannot change the lifetimes of the references, it only specifies how the lifetimes of references are related to each other.
Lifetimes on function or method parameters are called _input lifetimes_, and lifetimes on return values are called _output lifetimes_.

Example,
```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
	if x.len() > y.len() {
		x	
	} else {
		y	
	}
}
```
This syntax tells Rust that this function returns a reference whose lifetime is valid as long as both of the parameter reference's lifetime is valid, in other words this returns a reference with a lifetime same as the smaller of the lifetimes of the parameters.

Two string references `x` and `y` are passed into this function, It is possible that both references have different lifetimes. Since the function can return either `x` or `y`, the compiler has no way of knowing the lifetime of the returned reference. If the returned value is assigned to a variable, the compiler cannot understand when the ***referenced*** value of the returned reference is deallocated and when should it make the returned reference invalid (to avoid memory leaks).

If we specify `'a`, then the compiler can understand that the lifetime of the return value is valid as long as both of the parameters are valid, and both parameters are valid as long the parameter with the smaller lifetime is valid.
## Lifetime elision
Rust assigns lifetimes implicitly using three rules, if these rules are not met, then the compiler gives and error and demands explicit lifetime annotations.
1. The first rule is that the compiler assigns a lifetime parameter to each parameter that’s a reference. In other words, a function with one parameter gets one lifetime parameter: `fn foo<'a>(x: &'a i32)`; a function with two parameters gets two separate lifetime parameters: `fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`; and so on.
2. The second rule is that, if there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters: `fn foo<'a>(x: &'a i32) -> &'a i32`.
3. The third rule is that, if there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters. This third rule makes methods much nicer to read and write because fewer symbols are necessary.
## Lifetimes in Struct definitions
Structs can hold references as their members if we specify the lifetimes.
```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```
This means that the struct `ImportantExcerpt` cannot outlive the reference member `part`

Lifetimes can be specified for `impl` blocks like so:
```rust
impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {announcement}");
        self.part
    }
}
```
There are two input lifetimes, so Rust applies the first lifetime elision rule and gives both `&self` and `announcement` their own lifetimes. Then, because one of the parameters is `&self`, the return type gets the lifetime of `&self` using the third elision rule, and all lifetimes have been implicitly accounted for.

## `'static` lifetime
One special lifetime is `'static`, which denotes that the affected reference _can_ live for the entire duration of the program. All string literals have the `'static` lifetime, which we can annotate as follows:
```rust
let s: &'static str = "I have a static lifetime.";
```
The text of this string is stored directly in the program’s binary, which is always available. Therefore, the lifetime of all string literals is `'static`.