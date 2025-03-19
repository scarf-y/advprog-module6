## (1) Handle-Connection, Check Response

The `handle_connection` function is responsible for processing incoming TCP streams and extracting HTTP request headers. It makes use of `BufReader` to improve efficiency by buffering input rather than reading from the stream line by line in real-time.

The function initializes a `BufReader` with the given `TcpStream` and then iterates over the lines using `.lines()`. Each line is unwrapped from its `Result` type to extract the actual string content. The `.take_while(|line| !line.is_empty())` ensures that only the headers are read, stopping at the empty line that separates headers from the request body.

Once the headers are collected into a vector, they are printed to the console. This provides a structured way to inspect incoming HTTP requests before further processing. Understanding this implementation clarifies how Rust handles network I/O efficiently and how it can be extended to parse different types of HTTP requests and generate responses accordingly.


<br/>

## (2) Returning HTML

![Commit 2 screen capture](/assets/images/commit2.png)


With the updated implementation, the function now serves an actual HTML file as a response. It constructs an HTTP response by setting the status line (`HTTP/1.1 200 OK`), reading the contents of `hello.html`, and including the content length in the response headers. The response is then written to the stream using `write_all(response.as_bytes())`, ensuring the client receives the HTML page properly.

This modification demonstrates how Rust can handle basic HTTP responses efficiently. By serving static files, the server is now more functional, laying the groundwork for more advanced features like handling different request methods, dynamic content generation, or even implementing a full web framework.



