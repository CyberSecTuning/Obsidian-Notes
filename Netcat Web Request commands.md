
`nc example.com 80`
GET /path HTTP/1.1 
Host: example.com

![[Pasted image 20241031100436.png]]
*Figure: example path request using nc*
**GET /d7f82b09e8c5a5d0cafa5bf043b4ad69 HTTP/1.1**: This is the request line, specifying the HTTP method (GET), path, and HTTP version (HTTP/1.1).

![[Pasted image 20241031112731.png]]

![[Pasted image 20241031115846.png]]- `POST / HTTP/1.1`: Specifies the POST method to the root (`/`) using HTTP/1.1.
- `Host: 127.0.0.1`: Indicates the host (required for HTTP/1.1 requests).
- `Content-Type: application/x-www-form-urlencoded`: Specifies form-encoded data.
- `Content-Length: 34`: Specifies the length of the body (`a=9ecbd149fcc806f32f4e29ddf6b3f482` has 34 characters).
- `a=9ecbd149fcc806f32f4e29ddf6b3f482`: This is the body content, which should match exactly as specified. **there must be a blank line between the headers and the body** to separate them. This blank line is essential in the HTTP protocol for indicating the start of the request body.