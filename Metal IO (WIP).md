# Reading Metal I/O source
> `cfg` is related to conditional platform-dependent compilation in Rust. mio uses cfg at a lot of places to generalize platform dependent behavior.
## Token
Token is a tuple struct around usize, it stores a usize integer. It is used to identify which event belongs to which socker, etc.
## Poll
`Poll` polls the operating system for readiness of ***events*** on all ***registered*** values. `Poll` is backed by the selector provided by the operating system.

| OS            | Selector                                                                           |
| ------------- | ---------------------------------------------------------------------------------- |
| Android       | [epoll](https://man7.org/linux/man-pages/man7/epoll.7.html)                        |
| DragonFly BSD | [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue&sektion=2)               |
| FreeBSD       | [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue&sektion=2)               |
| iOS           | [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue&sektion=2)               |
| illumos       | [epoll](https://man7.org/linux/man-pages/man7/epoll.7.html)                        |
| Linux         | [epoll](https://man7.org/linux/man-pages/man7/epoll.7.html)                        |
| NetBSD        | [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue&sektion=2)               |
| OpenBSD       | [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue&sektion=2)               |
| Windows       | [IOCP](https://docs.microsoft.com/en-us/windows/win32/fileio/i-o-completion-ports) |
| macOS         | [kqueue](https://www.freebsd.org/cgi/man.cgi?query=kqueue&sektion=2)               |
[`signalfd`](https://man7.org/linux/man-pages/man2/signalfd.2.html)

On windows, polling creates a new IoCompletionPort by calling the Windows API function of IOCP then it wraps it in a selector wrapper, which is then wrapped in a registry, a registry can have a waker, the registry is stored in the Poll wrapper.
## Events
Events is a wrapper around a vector of `CompletionStatus`.