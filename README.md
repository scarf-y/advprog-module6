
## Commit 1 Reflection Notes

### (1) Handle-Connection, Check Response

The `handle_connection` function is responsible for processing incoming TCP streams and extracting HTTP request headers. It makes use of `BufReader` to improve efficiency by buffering input rather than reading from the stream line by line in real-time.

The function initializes a `BufReader` with the given `TcpStream` and then iterates over the lines using `.lines()`. Each line is unwrapped from its `Result` type to extract the actual string content. The `.take_while(|line| !line.is_empty())` ensures that only the headers are read, stopping at the empty line that separates headers from the request body.

Once the headers are collected into a vector, they are printed to the console. This provides a structured way to inspect incoming HTTP requests before further processing. Understanding this implementation clarifies how Rust handles network I/O efficiently and how it can be extended to parse different types of HTTP requests and generate responses accordingly.


<br/>

## Commit 2 Reflection Notes

### (2) Returning HTML

![Commit 2 screen capture](/assets/images/commit2.png)


With the updated implementation, the function now serves an actual HTML file as a response. It constructs an HTTP response by setting the status line (`HTTP/1.1 200 OK`), reading the contents of `hello.html`, and including the content length in the response headers. The response is then written to the stream using `write_all(response.as_bytes())`, ensuring the client receives the HTML page properly.

This modification demonstrates how Rust can handle basic HTTP responses efficiently. By serving static files, the server is now more functional, laying the groundwork for more advanced features like handling different request methods, dynamic content generation, or even implementing a full web framework.


<br/>

## Commit 3 Reflection Notes

### (3) Validating Request and Selectively Responding

![Commit 3 screen capture](/assets/images/commit3.png)

With the updated implementation, the function can now return different HTML pages based on the request. If the request is for the root URL (`GET / HTTP/1.1`), it serves `hello.html`; otherwise, it responds with `404.html` and a `404 NOT FOUND` status. This allows the server to differentiate between valid and invalid requests.

A key improvement in this update is the refactoring of redundant code. Previously, the `if` and `else` blocks contained repeated logic for reading files and writing responses. Now, these differences, status line and filename are assigned to variables first, allowing the rest of the response construction to be handled uniformly. This makes the code more concise, improves maintainability, and ensures that changes to file handling or response writing only need to be updated in one place. The use of tuple destructuring in `let (status_line, filename) = ...` also makes the logic clearer by separating decision-making from execution.


<br/>

## Commit 4 Reflection Notes

### (4) Simulation of Slow Request

A new addition is the ability to simulate a slow request using `thread::sleep(Duration::from_secs(5))`. When a request is made to `/sleep`, the server delays the response for 5 seconds before serving `hello.html`. To accommodate this new behavior, the implementation switched from `if` statements to a `match` expression, making it clearer how different request patterns are handled. Unlike `if`, `match` requires explicit referencing to compare string literals. This modification highlights the limitations of our simple server, as real-world web servers would handle concurrent requests more efficiently instead of blocking execution during delays.


<br/>

## Commit 5 Reflection Notes

### (5) Multithreaded server using Threadpool

Now that we've introduced a `ThreadPool` to manage multiple threads, our server can handle multiple requests concurrently without blocking on a single slow request. The `ThreadPool::new(4)` ensures that at most four threads are spawned, preventing excessive resource usage. Each incoming connection is passed to the pool's `execute` method, which is designed to delegate the request to an available worker thread. Currently, `execute` is a placeholder and does not yet assign tasks to threads, but it lays the foundation for a structured approach to parallel request handling. Once fully implemented, this will significantly improve our server's ability to manage multiple clients efficiently.