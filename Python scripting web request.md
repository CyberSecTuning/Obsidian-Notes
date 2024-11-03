###### **Basic GET request using python**
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
	print(f"{k}: {v}")
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

###### **Basic POST request using python**
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

###### **Multiple parameters passed through post data:**
```python
flag = requests.post("http://127.0.0.1", data={"a": "896ff7cd481e04849f2cb1ef814629e7", "b": "0ea9d74a 485dadf1 6f559b71 23348b7a48"})
print(flag.text)
```

###### **POST request sending JSON data:**

```python
flag = requests.post("http://127.0.0.1", json={"a": "c678f238662ed61a01b667e0eb3e6336"})
```

###### Complex POST JSON data request including multiple variables and 2 objects including a list:

```python
flag = requests.post("http://127.0.0.1", json={"a":"e2028ffe9e25adb522a7fc11564c2f35","b":{"c":"ac2a2ba6","d":["2efc3c6a","5397df21 b36e9e3b&3d74dc9d#eb744276"]}})
```


###### **Scripting Stateful request:**
