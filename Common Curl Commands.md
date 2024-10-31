**Basic GET Request**:

`curl 127.0.0.1`
Retrieves content from a specified URL. By default, `curl` performs a GET request.

**Basic GET path Request**:
`curl 127.0.0.1/path`
Retrieves content from a specified URL path.

**Send POST Request**:

`curl -X POST -H 'User-Agent: example' 127.0.0.1`
Sends data to a server as part of a POST request.

**Follow Redirects**:

`curl -L http://example.com`
Follows any redirects if the server responds with a redirection status (e.g., 301, 302).

**Send JSON Data**:

`curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' http://example.com/api`
Sends JSON data in the request body. The `-H` option sets headers like `Content-Type`.
`-d` is used to specify data.

**Save Output to a File**:

`curl -o filename.html http://example.com`
Saves the output of the request to a specified file.