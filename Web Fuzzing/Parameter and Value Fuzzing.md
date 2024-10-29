[[Web Fuzzing Cheat Sheet]]
[[Wenum]]

# Parameter and Value Fuzzing

---

Building upon the discovery of hidden directories and files, we now delve into parameter and value fuzzing. This technique focuses on manipulating the parameters and their values within web requests to uncover vulnerabilities in how the application processes input.

Parameters are the messengers of the web, carrying vital information between your browser and the server that hosts the web application. They're like variables in programming, holding specific values that influence how the application behaves.

## GET Parameters: Openly Sharing Information

You'll often spot `GET` parameters right in the URL, following a question mark (`?`). Multiple parameters are strung together using ampersands (`&`). For example:

Code: http

```http
https://example.com/search?query=fuzzing&category=security
```

In this URL:

- `query` is a parameter with the value "fuzzing"
- `category` is another parameter with the value "security"

`GET` parameters are like postcards – their information is visible to anyone who glances at the URL. They're primarily used for actions that don't change the server's state, like searching or filtering.

## POST Parameters: Behind-the-Scenes Communication

While `GET` parameters are like open postcards, POST parameters are more like sealed envelopes, carrying their information discreetly within the body of the HTTP request. They are not visible directly in the URL, making them the preferred method for transmitting sensitive data like login credentials, personal information, or financial details.

When you submit a form or interact with a web page that uses POST requests, the following happens:

1. `Data Collection`: The information entered into the form fields is gathered and prepared for transmission.
    
2. `Encoding`: This data is encoded into a specific format, typically `application/x-www-form-urlencoded` or `multipart/form-data`:
    
    - `application/x-www-form-urlencoded`: This format encodes the data as key-value pairs separated by ampersands (`&`), similar to GET parameters but placed within the request body instead of the URL.
    - `multipart/form-data`: This format is used when submitting files along with other data. It divides the request body into multiple parts, each containing a specific piece of data or a file.
3. `HTTP Request`: The encoded data is placed within the body of an HTTP POST request and sent to the web server.
    
4. `Server-Side Processing`: The server receives the POST request, decodes the data, and processes it according to the application's logic.
    

Here's a simplified example of how a POST request might look when submitting a login form:

Code: http

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=your_username&password=your_password
```

- `POST`: Indicates the HTTP method (POST).
- `/login`: Specifies the URL path where the form data is sent.
- `Content-Type`: Specifies how the data in the request body is encoded (`application/x-www-form-urlencoded` in this case).
- `Request Body`: Contains the encoded form data as key-value pairs (`username` and `password`).

## Why Parameters Matter for Fuzzing

Parameters are the gateways through which you can interact with a web application. By manipulating their values, you can test how the application responds to different inputs, potentially uncovering vulnerabilities. For instance:

- Altering a product ID in a shopping cart URL could reveal pricing errors or unauthorized access to other users' orders.
- Modifying a hidden parameter in a request might unlock hidden features or administrative functions.
- Injecting malicious code into a search query could expose vulnerabilities like Cross-Site Scripting (XSS) or SQL Injection (SQLi).

