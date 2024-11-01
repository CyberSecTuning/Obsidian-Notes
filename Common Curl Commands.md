###### **Basic GET Request**:

`curl 127.0.0.1`
Retrieves content from a specified URL. By default, `curl` performs a GET request.

###### **GET path Request**:

`curl 127.0.0.1/path`
Retrieves content from a specified URL path.

###### **GET cookie request:**

`curl -X GET http://127.0.0.1 -b "cookie=5b54b83a66fa354bba88d92c416342f1"`

###### **GET stateful request:**

In this process, you are using `curl` to interact with a stateful server at `http://127.0.0.1`. The server tracks the "state" of the session, and each request increments the state, depending on the presence of cookies.

```
hacker@talking-web~level37:~$ curl -X GET http://127.0.0.1 -c cookies.txt
state: 1
hacker@talking-web~level37:~$ curl -X GET http://127.0.0.1 -b cookies.txt
state: 2
hacker@talking-web~level37:~$ curl -X GET http://127.0.0.1 -b cookies.txt -c cookies.txt
state: 2
hacker@talking-web~level37:~$ curl -X GET http://127.0.0.1 -b cookies.txt
state: 3
hacker@talking-web~level37:~$ curl -X GET http://127.0.0.1 -b cookies.txt -c cookies.txt
state: 3
hacker@talking-web~level37:~$ curl -X GET http://127.0.0.1 -b cookies.txt
pwn.college{YhI2XU_dEsYAm1IyQBbO0MHD2zT.dRDMzMDLwYjN5czW}
```

This process demonstrates how `curl` can be used to interact with stateful web applications by sending and saving cookies across multiple requests. The server uses these cookies to track your session and control access to information (in this case, revealing the flag when the correct state was reached).

###### **Send POST Request**:

`curl -X POST -H 'User-Agent: example' 127.0.0.1`
Sends data to a server as part of a POST request.

###### **Send POST parameter Data:**

```
curl -X POST -d "a=208cb9a10d6bd24647646a3f2d719e9f&b=abd1b44c%209cbdb3e9%26e258541e%239e2c9428" http://127.0.0.1
```

###### **Follow Redirects**:

`curl -L http://example.com`
Follows any redirects if the server responds with a redirection status (e.g., 301, 302).

###### **Send JSON Data**:

`curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' http://example.com/api`
Sends JSON data in the request body. The `-H` option sets headers like `Content-Type`.
`-d` is used to specify data.
`curl -X POST -H "Content-Type: application/json" -d '{"a":"bfcf82d831f004a6ca5d82bc833544e0"}' http://127.0.0.1`

###### **Complex Json data:**
```
curl -X POST -H "Content-Type: application/json" -d '{"a":"a8ab4191852668f9c121e82633682237","b":{"c":"33c3de30","d":["bd8fe5fc","91df0455 d93c4cc1&404ca2ac#b75719c6"]}}' http://127.0.0.1:80
```

**URL Encoding Curl path** 

![[Pasted image 20241031103532.png]]
- `&` becomes `%26`

As shown in the URL below, there are spaces in the URL, so encoding is needed. SP = %20 and must fill the spaces to produce the correct path.
![[Pasted image 20241031103637.png]]

To specify an argument in a HTTP request simply add `?` and specify the argument, such as:
![[Pasted image 20241031105410.png]]

###### **Passing multiple arguments into a path using URL encoding:** 

`curl "http://127.0.0.1:80/?a=2905291f9d001723077870dd5b04edf6&b=0752f85a%204b1c9b21%26ab0c48b3%23649d3861"`

###### **Including form data in POST HTTP request:**
`curl -X POST -d "a=a561d55bde1a8d3fbf8f143e9e82f51a" http://127.0.0.1`
- `-X POST` tells `curl` to use the POST method.
- `-d "a=a561d55bde1a8d3fbf8f143e9e82f51a"` specifies the POST parameter, where `a` is the parameter name and `a561d55bde1a8d3fbf8f143e9e82f51a` is the value.
- `http://127.0.0.1` is the URL of the local server on port 80.













**Save Output to a File**:

`curl -o filename.html http://example.com`
Saves the output of the request to a specified file.

