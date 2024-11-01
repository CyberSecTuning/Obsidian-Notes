
`nc example.com 80`
GET /path HTTP/1.1 
Host: example.com

![[Pasted image 20241031100436.png]]

###### **GET Path request using nc**

`GET /d7f82b09e8c5a5d0cafa5bf043b4ad69 HTTP/1.1`: This is the request line, specifying the HTTP method (GET), path, and HTTP version (HTTP/1.1) to reveal the flag.

![[Pasted image 20241031112731.png]]


**GET cookie request:**
```
hacker@talking-web~level35:~$ nc 127.0.0.1 80
GET / HTTP/1.1
Cookie: cookie=756b5f4cce37ba85e28c06e268eee639
```


**POST request for 'a'**
![[Pasted image 20241031115846.png]]- `POST / HTTP/1.1`: Specifies the POST method to the root (`/`) using HTTP/1.1.
- `Host: 127.0.0.1`: Indicates the host (required for HTTP/1.1 requests).
- `Content-Type: application/x-www-form-urlencoded`: Specifies form-encoded data.
- `Content-Length: 34`: Specifies the length of the body (`a=9ecbd149fcc806f32f4e29ddf6b3f482` has 34 characters).
- `a=9ecbd149fcc806f32f4e29ddf6b3f482`: This is the body content, which should match exactly as specified. **there must be a blank line between the headers and the body** to separate them. This blank line is essential in the HTTP protocol for indicating the start of the request body.

Multiple parameters passed through nc, new line is created through manually inputting with nc, to avoid we much use echo or printf. 

![[Pasted image 20241101014030.png]]
```
echo -e "POST / HTTP/1.1\r\nHost: 127.0.0.1\r\nContent-Type: application/x-www-form-urlencoded\r\nContent-Length: 79\r\n\r\nrun=a1fffa49e3c9bad006f1eaddd4d0e10e&b=b3001b27b9b7bf3c17a29b19bd92c62b3b1abde5" | nc 127.0.0.1 80
```
Make sure the wc for the 'Content-Length:' is correct using `echo "parameter" | wc -c`

**HTTP POST sending JSON data:**
```
POST / HTTP/1.1 
Host: 127.0.0.1 
Content-Type: application/json
Content-Length: 40 

{"a":"bfcf82d831f004a6ca5d82bc833544e0"}
```

**Complex JSON data POST request:**
```
POST / HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Content-Length: 116

{"a":"1b37d70415444ca70923f51016acecba","b":{"c":"64b5a87e","d":["640294b7","03d6e06f c6efd9af&971af87b#38ac63be"]}}
```


###### **Moving through Stateful server cookies:**

Each GET request made creates a new session ID cookie, simply change the cookies with each state.

```
hacker@talking-web~level38:~$ nc 127.0.0.1 80
GET / HTTP/1.1
Host: 127.0.0.1           

HTTP/1.1 302 FOUND
Server: Werkzeug/3.0.6 Python/3.8.10
Date: Fri, 01 Nov 2024 11:08:39 GMT
Content-Length: 9
Location: /
Server: pwn.college
Vary: Cookie
Set-Cookie: session=eyJzdGF0ZSI6MX0.ZyS2tw.0F3obocR2ZSNlstRz9J2aeWiNsA; HttpOnly; Path=/
Connection: close

state: 1
^C
hacker@talking-web~level38:~$ nc 127.0.0.1 80
GET / HTTP/1.1
Host: 127.0.0.1
Cookie: session=eyJzdGF0ZSI6MX0.ZyS2tw.0F3obocR2ZSNlstRz9J2aeWiNsA

HTTP/1.1 302 FOUND
Server: Werkzeug/3.0.6 Python/3.8.10
Date: Fri, 01 Nov 2024 11:09:25 GMT
Content-Length: 9
Location: /
Server: pwn.college
Vary: Cookie
Set-Cookie: session=eyJzdGF0ZSI6Mn0.ZyS25Q.QgqFJw7AGBNnOXrBbYlLUvkmIgw; HttpOnly; Path=/
Connection: close

state: 2
^C
hacker@talking-web~level38:~$ nc 127.0.0.1 80
GET / HTTP/1.1
Host: 127.0.0.1
Cookie: session=eyJzdGF0ZSI6Mn0.ZyS25Q.QgqFJw7AGBNnOXrBbYlLUvkmIgw

HTTP/1.1 302 FOUND
Server: Werkzeug/3.0.6 Python/3.8.10
Date: Fri, 01 Nov 2024 11:09:56 GMT
Content-Length: 9
Location: /
Server: pwn.college
Vary: Cookie
Set-Cookie: session=eyJzdGF0ZSI6M30.ZyS3BA.73F1vxnZxnSCS9AnLYvDLjNx0kI; HttpOnly; Path=/
Connection: close

state: 3
^C
hacker@talking-web~level38:~$ nc 127.0.0.1 80
GET / HTTP/1.1
Host: 127.0.0.1
Cookie: session=eyJzdGF0ZSI6M30.ZyS3BA.73F1vxnZxnSCS9AnLYvDLjNx0kI

HTTP/1.1 200 OK
Server: Werkzeug/3.0.6 Python/3.8.10
Date: Fri, 01 Nov 2024 11:10:18 GMT
Content-Length: 58
Server: pwn.college
Vary: Cookie
Set-Cookie: session=eyJzdGF0ZSI6NH0.ZyS3Gg.W5O33XpTSBORfIePf5_tm65uphc; HttpOnly; Path=/
Connection: close

pwn.college{weLaSF2X1xZKbT-JVnW1MUurp18.dVDMzMDLwYjN5cz
```