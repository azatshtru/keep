File descriptors in Linux can be opened in non-blocking mode, since sockets are also file descriptors representing a connection, they can be opened in non-blocking mode.

The `accept` syscall looks into the queue of pending connections for the socket int the kernel, if there are connections in the queue, it extracts the first connection out of it. If there are no connections, it block the main thread and waits for a connection to appear.

`accept` syscall can be opened in nonblocking mode, and if no connection is present in the queue, instead of blocking, it returns `EAGAIN` or `EWOULDBLOCK`.

In rust a simple web server can be created using `std::net::TcpListener`:
```rust
use std::net::{TcpListener, TcpStream};
use std::io::{self, Read, Write};

fn read_request(stream: &mut TcpStream, data: &mut String) -> Result<bool, io::Error> {
    loop {
        let mut buffer = [0u8; 1024];
        match stream.read(&mut buffer) {
            Ok(0) => return Ok(false),
            Ok(n) => {
                data.push_str(std::str::from_utf8(&buffer[..n]).unwrap());
                if data.get(n - 4..n) == Some("\r\n\r\n") {
                    return Ok(true);
                }
            }
            Err(e) => return Err(e)
        }
    }
}

fn write_response(stream: &mut TcpStream, response: &mut String) -> Result<bool, io::Error> {
    loop {
        match stream.write(&response[..].as_bytes()) {
            Ok(0) => return Ok(false),
            Ok(n) => {
                let response_bytes = response.as_bytes();
                *response = std::str::from_utf8(&response_bytes[n..]).unwrap().to_string();
                if response.len() == 0 {
                    return Ok(true);
                }
            }
            Err(e) => return Err(e)
        }
    }
}

fn process_request(request: String) -> String {
    if request.lines().next().unwrap() == "GET /hello HTTP/1.1" {
        concat!(
            "HTTP/1.1 200 OK\r\n",
            "Content-Length: 13\n",
            "Connection: close\r\n\r\n",
            "Hello, world!"
        ).to_string()
    } else {
        concat!(
            "HTTP/1.1 404 NOT FOUND\r\n",
            "Connection: close\r\n\r\n",
            "Not found :("
        ).to_string()
    }
}

fn main() -> io::Result<()> {
    let listener = TcpListener::bind("0.0.0.0:12112").unwrap();
    loop {
        let (mut stream, _) = listener.accept().unwrap();

        let mut request = String::new();
        if let Ok(true) = read_request(&mut stream, &mut request) {
            println!("{}", request);
            let mut response = process_request(request);
            let _ = write_response(&mut stream, &mut response)?;
            let _ = stream.flush();
        }
    }
}
```

This server, however, blocks the main thread multiple times during execution.
1. The `accept` call of the `TcpListener` blocks the main thread until a connection is received in the kernel's queue.
2. `stream.read` and `stream.write` calls block.

In Linux, file descriptors can be opened in nonblocking mode, where instead of blocking the main thread, they return `EWOULDBLOCK`. In Rust, when `TcpListener` and `TcpStream` are opened in nonblocking mode, instead of blocking, they return an error `WouldBlock`. This can be used as a naive workaround to create asynchronity.

1. We create a state machine enum `ConnectionState` for the stream. The enum has three variants: `Read(Vec<u8>)`, `Write(Vec<u8>)` and `Flush`.
	- The `Vec<u8>` in the `Read(Vec<u8>)` variant stores the bytes of the request that have been read.
	- The `Vec<u8>` in `Write` is used to store the response we want to serve. The bytes of this response vector are consumed as the response is written to the stream.
	- All of this keeps track of the state of the stream while reading or writing.
	
2. We create a vector `connections` of type `Vec<ConnectionState>` that holds all the streams coming through in form of `ConnectionState` state machines.

3. We set the listener and any incoming stream to nonblocking mode.

4. We run `listener.accept()` in nonblocking mode inside the main loop and match the return value, if returned value of `listener.accept()` is a stream, we set the stream to nonblocking mode and add it to the vector, if it is an error of type `WouldBlock`, we do nothing and resume execution of the loop.

5. Inside the main loop, we run another loop that iterates through all the streams in the vector `connections`, for each `ConnectionState` in connections, we:
	1. match the variant of `ConnectionState`
	2. if it is the `Read(Vec<u8>)` variant, then we use the `Vec<u8>` inside `Read` to read the socket stream further in a loop
	3. if the read operation returns `WouldBlock`, we continue on to the next stream in the vector `connections`
	4. After the read operation finished, we transition the `ConnectionState` of the current connection to `Write`
	5. if the state is `Write`, we use the `Vec<u8>` inside it to write to the stream, if the write operation returns `WouldBlock`, we continue on to the next connection in the vector.