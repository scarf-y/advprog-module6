## (1) Handle-Connection, Check Response

The `handle_connection` function is responsible for processing incoming TCP streams and extracting HTTP request headers. It makes use of `BufReader` to improve efficiency by buffering input rather than reading from the stream line by line in real-time.

The function initializes a `BufReader` with the given `TcpStream` and then iterates over the lines using `.lines()`. Each line is unwrapped from its `Result` type to extract the actual string content. The `.take_while(|line| !line.is_empty())` ensures that only the headers are read, stopping at the empty line that separates headers from the request body.

Once the headers are collected into a vector, they are printed to the console. This provides a structured way to inspect incoming HTTP requests before further processing. Understanding this implementation clarifies how Rust handles network I/O efficiently and how it can be extended to parse different types of HTTP requests and generate responses accordingly.

