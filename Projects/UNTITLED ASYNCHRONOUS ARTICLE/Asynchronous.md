# Reading Files
The CPU is usually a lot faster than other hardware components. To read a file,
1. a `read`(unix) or `ReadFile`(Windows) syscall is performed from the user-space which passes the control to the kernel-space. Kernel maintains a list of open file descriptors.
2. It first looks through the cache for the requested file,
3. then it communicates with the file system driver to determine the disk block and offset of the file.
4. It then sends commands to the storage hardware through SATA, NVMe, SCSI, etc.

A lot, but still very fast because the CPU handles it.

However, after sending the commands to the storage hardware, the kernel loses control and the storage hardware is pretty slower compared to the CPU, and it takes a bit longer for it to return the requested file to the kernel's buffer. During this time, the CPU does nothing, which is a bit inefficient, we could be holding out on a lot of useful computation.

> In fact, all hardware components, be it speakers, displays, not just storage, are much slower than the CPU, and interacting with them takes measurable amount of time.

When the Kernel's buffer receives the file from storage, it is copied to the user-space buffer which was provided by the currently running program during the syscall.
# Scheduling
Sometimes we have to execute a routine after some interval of time. Maybe running a server and requiring it to gracefully shut down after serving for 30 more seconds, maybe scheduling a cache cleanup, or a remainder.
1. The program invokes a `sleep` syscall on unix-like systems, `SetTimer` on Windows.
2. Kernel maintains a timer queue where a timer and callback associated to the syscall are placed.
3. Computers have timer hardware called **System Clocks**, which generate periodic kernel interrupts, maybe every millisecond or so.
4. At every timer interrupt, the kernel checks if any timer in the timer queue has expired, it calls the callbacks associated to any expired timers and the execution of program continues.

`sleep` syscall blocks further execution of anything in the program, and no other process can run while we wait for the timer to resolve.

A different syscall `timer_create` can be used to schedule something without blocking the execution of our programs. But.. how do we handle when the scheduled timer resolves? Do we stop execution of everything for a little while, save the state of the program, run the function we scheduled, then resume everything back? Seems a bit too much saving the entire state of the program though. Hmm..
# Network requests and responses
Speed of the internet is limited by humanity's advancements in physics, it just IS sometimes slow. Not just your internet though, maybe you are requesting some resource from a server somewhere, it takes time for the server to look into your query, prepare the appropriate response, then send it back through the internet, also what if their internet is slow? Point is, it takes time to get stuff around the internet. How would we write a web server now?
## Sockets
1. syscalls are made to the kernel for sending or receiving stuff from the internet, creating sockets and listening for connections.

2. kernel maintains sockets, which are structs that contain send and receive buffers along with some other state. On Linux, new sockets are created using the `socket` syscall.

3. Once sockets are created, the data can be written to their write buffers and read from their read buffers using syscalls.
### Network Interface Card
1. On receiving a write syscall, the kernel stores the data we want to transfer in socket's write buffer, encodes it into packets, attaches TCP/UDP/IP/MAC headers, and then passes the packets to the **Network Interface Card (NIC)**, which is a hardware component that converts packets into electrical signals and EM Waves and transfers them over the network.

2. When data arrives in the NIC, it generates an interrupt that notifies the kernel about the arrival of data. Network ring buffer is a memory region where NIC have direct access ( over [DMA](https://en.wikipedia.org/wiki/Direct_memory_access) ) to store incoming data. The kernel stores the arrived data from the NIC's buffer into a queue to sequentially process it, from the queue, the kernel then reads the received packets, validates them, removes headers, reassembles them and puts it into the read buffer of the appropriate socket where it is ready to be read by the user-space program using read syscalls.

That was a lot, it really is a lot, and it takes time.

### `accept` syscall in Linux
The `accept` syscall checks the kernel's pending connection queue, pops the first element, then creates a socket for it. The pending queue is filled when kernel receives interrupts from the NIC during connection requests and copies data from NIC's buffer into its own data structures. Each time `accept` is called it pops the pending queue to create a socket, if there is nothing in the pending queue, then the CPU just waits for something to appear there when interrupts from NIC are received.

Say we create a TCP listener in our web server, and we open a socket listening for a connection using the `accept` syscall. Until all of this process over the network followed by interfacing with the NIC happens, the pending connection queue is populated, the CPU just waits for the connection, doing nothing. We could be doing a lot in that time, we could be serving the other already connected sockets, but instead we just wait for a connection.
# Blocking I/O and Threads
Bottom line of the things discussed above is: while waiting for other hardware components or the network, the CPU stays idle, when it could be doing a bunch of stuff.

This is called Blocking I/O, it makes our software less responsive. How do we solve this and make our software more responsive? The answer is... not threads.

Now, I can talk all about how threads pass the control to the kernel space and all the arguments against them. But in all common sense, threads are pretty cool, they let you run a bunch of things in parallel, you can wait for connection on one thread, serve a connection on different thread, wait for scheduled routines in one thread and a lot... And they are incredibly easy to use and spawn.

However when using threads, while the CPU is blocking in one thread, it *still* does nothing in *that* thread. It is like having 4 glasses with quarter filled Orange Juice, instead of just one glass that is completely filled.

Now that I think about it, if I do my job right and make some really good decisions in the next few paragraphs, we could have 4 completely filled glasses of OJ.