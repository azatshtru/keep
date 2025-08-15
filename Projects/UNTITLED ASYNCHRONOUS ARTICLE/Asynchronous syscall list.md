Asychronous syscall list

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