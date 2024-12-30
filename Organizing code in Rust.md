A crate is a smallest unit of compilation in Rust. The Rust compiler compiles crates, one at a time.
Crates can be binary crates, the ones that are compiled into executable programs.
Crates can be library crates, the ones that provide functionality to other Rust code.

> Cargo is a binary crate.

A package is a bundle of multiple crates that provide some functionality.
A package can contain multiple binary crates and one library crate.
- root of library crate is defined as the `src/lib.rs` file.
- Rust assigns library crate the same name as the project. Stuff in the library crate can be accessed by using the name of the project.
- `src/main.rs` is taken as the root binary crate.
- other binary crates are put in `src/bin/`