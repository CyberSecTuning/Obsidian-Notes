webfuzz_api example
`python3 api_fuzzer.py http://83.136.254.158:51683 | grep "200"`
## Fuzzing the API

We will use a fuzzer that will use a wordlist in an attempt to discover these undocumented endpoints. Run the commands to pull, install the requirements, and run the fuzzer:

API Fuzzing

```shell-session
xeloki@htb[/htb]$ git clone https://github.com/PandaSt0rm/webfuzz_api.git
xeloki@htb[/htb]$ cd webfuzz_api
xeloki@htb[/htb]$ pip3 install -r requirements.txt
```
## Types of API Fuzzing

There are 3 primary types of API fuzzing

1. `Parameter Fuzzing` - One of the primary techniques in API fuzzing, parameter fuzzing focuses on systematically testing different values for API parameters. This includes query parameters (appended to the API endpoint URL), headers (containing metadata about the request), and request bodies (carrying the data payload). By injecting unexpected or invalid values into these parameters, fuzzers can expose vulnerabilities like injection attacks (e.g., SQL injection, command injection), cross-site scripting (XSS), and parameter tampering.
2. `Data Format Fuzzing` - Web APIs frequently exchange data in structured formats like JSON or XML. Data format fuzzing specifically targets these formats by manipulating the structure, content, or encoding of the data. This can reveal vulnerabilities related to parsing errors, buffer overflows, or improper handling of special characters.
3. `Sequence Fuzzing` - APIs often involve multiple interconnected endpoints, where the order and timing of requests are crucial. Sequence fuzzing examines how an API responds to sequences of requests, uncovering vulnerabilities like race conditions, insecure direct object references (IDOR), or authorization bypasses. By manipulating the order, timing, or parameters of API calls, fuzzers can expose weaknesses in the API's logic and state management.
This API provides automatically generated documentation via the `/docs` endpoint, `http://IP:PORT/docs`. The following page outlines the API's documented endpoint.

![](https://academy.hackthebox.com/storage/modules/280/apispec.png)

The specification details five endpoints, each with a specific purpose and method:

1. `GET /` (Read Root): This fetches the root resource. It likely returns a basic welcome message or API information.
2. `GET /items/{item_id}` (Read Item): Retrieves a specific item identified by `item_id`.
3. `DELETE /items/{item_id}` (Delete Item): Deletes an item identified by `item_id`.
4. `PUT /items/{item_id}` (Update Item): Updates an existing item with the provided data.
5. `POST /items/` (Create Or Update Item): This function creates a new item or updates an existing one if the `item_id` matches.

While the Swagger specification explicitly details five endpoints, it's crucial to acknowledge that APIs can contain undocumented or "hidden" endpoints that are intentionally omitted from the public documentation.

These hidden endpoints might exist to serve internal functions not meant for external use, as a misguided attempt at security through obscurity, or because they are still under development and not yet ready for public consumption.