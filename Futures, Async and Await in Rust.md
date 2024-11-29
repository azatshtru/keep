### Preemptive vs Cooperative multitasking
Computers only do one thing at a time. The illusion of multitasking can be achieved in two ways.
1. If the computer arbitrarily gives up control of the currently running task, saves the execution state and switches to some other task, then it is called preemptive multitasking.
2. If the tasks themselves have some logic to give up control back to the computer willingly during some waiting operation, so that other tasks can execute in the time before the original task regains control and continues execution, then that is called cooperative multitasking. In cooperative multitasking, every task needs to be cooperative otherwise it won't work.

### Non-blocking IO
The computer has a Network Interface Card (NIC) which is abstracted to the operating system via the kernel in form of sockets. A socket acts as a file, in which, if data is written using the `send` syscall, then it is sent over the network, and data that comes from the network is stored in the NIC ready to be read using the `recv` syscall.
A ***future*** is a container for a value that is not available yet, but might be in the future.
When a request is sent over the network, instead of sleeping while waiting for the value, we can return a _not ready future_ and continue running other code. As soon as the future becomes ready, the code can use it. This doesn't block the execution of the code.

## Futures in Rust
A future is defined as a trait in Rust. The associated type `Output` defines the type of the asynchronous value that will be returned in the future.
```rust
pub trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context) -> Poll<Self::Output>;
}
```
The [`poll`](https://doc.rust-lang.org/nightly/core/future/trait.Future.html#tymethod.poll) method allows to check if the value is already available. It returns a [`Poll`](https://doc.rust-lang.org/nightly/core/future/trait.Future.html#tymethod.poll) enum, which looks like this:
```rust
pub enum Poll<T> {
    Ready(T),
    Pending,
}
```
