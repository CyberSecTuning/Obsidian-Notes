[[Web Applications Cheat Sheet]]

The following are some of the most common [HTTP response codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status):

| Code                        | Description                                                                                                         |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Successful responses**    |                                                                                                                     |
| `200 OK`                    | The request has succeeded                                                                                           |
| **Redirection messages**    |                                                                                                                     |
| `301 Moved Permanently`     | The URL of the requested resource has been changed permanently                                                      |
| `302 Found`                 | The URL of the requested resource has been changed temporarily                                                      |
| **Client error responses**  |                                                                                                                     |
| `400 Bad Request`           | The server could not understand the request due to invalid syntax                                                   |
| `401 Unauthorized`          | Unauthenticated attempt to access page                                                                              |
| `403 Forbidden`             | The client does not have access rights to the content                                                               |
| `404 Not Found`             | The server can not find the requested resource                                                                      |
| `405 Method Not Allowed`    | The request method is known by the server but has been disabled and cannot be used                                  |
| `408 Request Timeout`       | This response is sent on an idle connection by some servers, even without any previous request by the client        |
| **Server error responses**  |                                                                                                                     |
| `500 Internal Server Error` | The server has encountered a situation it doesn't know how to handle                                                |
| `502 Bad Gateway`           | The server, while working as a gateway to get a response needed to handle the request, received an invalid response |
| `504 Gateway Timeout`       | The server is acting as a gateway and cannot get a response in time                                                 |
The following shows an example of requesting a page in a Linux terminal using the [cURL](https://en.wikipedia.org/wiki/CURL) utility, and receiving the server response while using the `-I` flag, which displays the headers:

Web Servers

```shell-session
xeloki@htb[/htb]$ curl -I https://academy.hackthebox.com

HTTP/2 200
date: Tue, 15 Dec 2020 19:54:29 GMT
content-type: text/html; charset=UTF-8
...SNIP...
```

While this `cURL` command example shows us the source code of the webpage:

Web Servers

```shell-session
xeloki@htb[/htb]$ curl https://academy.hackthebox.com

<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>Cyber Security Training : HTB Academy</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Some of the most common relational databases include:

|Type|Description|
|---|---|
|[MySQL](https://en.wikipedia.org/wiki/MySQL)|The most commonly used database around the internet. It is an open-source database and can be used completely free of charge|
|[MSSQL](https://en.wikipedia.org/wiki/Microsoft_SQL_Server)|Microsoft's implementation of a relational database. Widely used with Windows Servers and IIS web servers|
|[Oracle](https://en.wikipedia.org/wiki/Oracle_Database)|A very reliable database for big businesses, and is frequently updated with innovative database solutions to make it faster and more reliable. It can be costly, even for big businesses|
|[PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)|Another free and open-source relational database. It is designed to be easily extensible, enabling adding advanced new features without needing a major change to the initial database design|
Some of the most common `NoSQL` databases include:

|Type|Description|
|---|---|
|[MongoDB](https://en.wikipedia.org/wiki/MongoDB)|The most common `NoSQL` database. It is free and open-source, uses the `Document-Based` model, and stores data in `JSON` objects|
|[ElasticSearch](https://en.wikipedia.org/wiki/Elasticsearch)|Another free and open-source `NoSQL` database. It is optimized for storing and analyzing huge datasets. As its name suggests, searching for data within this database is very fast and efficient|
|[Apache Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra)|Also free and open-source. It is very scalable and is optimized for gracefully handling faulty values|

Other common `NoSQL` databases include: `Redis`, `Neo4j`, `CouchDB`, and `Amazon DynamoDB`.