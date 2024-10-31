
**Basic GET request using python**
```python
import requests

response = requests.get('http://example.com/path') 
print(response.status_code) # Print HTTP status code 
print(response.text) # Print response body content
```
![[Pasted image 20241031102815.png]]
Print headers using 
`response.headers`
Organize the header using a for loop
```python
for k, v in response.headers.items():
	print(f"{k}L {v}")
```
Organize response text using 
`response.text.splitlines()[0]`
extract domain using for loop:
```python
for line in response.text.splitlines():
	if "<a" in line
		print(line)
```
Can modify to include only the URL using:
```python
		print(line.split('"')[1])
```

**Basic POST request using python**
```python
requets.post("http://127.0.0.1")

```
request headers using:
```python
requets.post("http://127.0.0.1", headers={"Host": "Hacked"})

```
Request using specified parameters (data):
```python
flag = requests.post("http://127.0.0.1", data={"a": "8cdce2933fae7131af4074b1fb4ec45a"})
```
- `data={"a": "8cdce2933fae7131af4074b1fb4ec45a"}`: Pass the data as a dictionary where `"a"` is the key and `"8cdce2933fae7131af4074b1fb4ec45a"` is the value.
- `flag.text`: Print the server’s response body.