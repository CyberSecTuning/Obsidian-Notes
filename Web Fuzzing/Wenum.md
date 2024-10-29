[[Web Fuzzing Cheat Sheet]]

Quick wenum fuzzing

`wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"`

`ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v`

ffuf -u http://94.237.63.215:33315/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v


wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://83.136.254.158:54267/get.php?x=FUZZ"
## Wenum fuzzing GET

In this section, we'll leverage `wenum` to explore both GET and POST parameters within our target web application, ultimately aiming to uncover hidden values that trigger unique responses, potentially revealing vulnerabilities.

To follow along, start the target system via the question section at the bottom of the page, replacing the uses of IP:PORT with the `IP`:`PORT` for your spawned instance. We will be using the `/usr/share/seclists/Discovery/Web-Content/common.txt` wordlists for these fuzzing tasks.

```

Then to begin, we will use `curl` to manually interact with the endpoint and gain a better understanding of its behavior:

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ curl http://IP:PORT/get.php

Invalid parameter value
x: 
```

The response tells us that the parameter `x` is missing. Let's try adding a value:

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ curl http://IP:PORT/get.php?x=1

Invalid parameter value
x: 1
```

The server acknowledges the `x` parameter this time but indicates that the provided value (`1`) is invalid. This suggests that the application is indeed checking the value of this parameter and producing different responses based on its validity. We aim to find the specific value to trigger a different and hopefully more revealing response.

Manually guessing parameter values would be tedious and time-consuming. This is where `wenum` comes in handy. It allows us to automate the process of testing many potential values, significantly increasing our chances of finding the correct one.

Let's use `wenum` to fuzz the "`x`" parameter's value, starting with the `common.txt` wordlist from SecLists:

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"

...
 Code    Lines     Words        Size  Method   URL 
...
 200       1 L       1 W        25 B  GET      http://IP:PORT/get.php?x=OA... 

Total time: 0:00:02
Processed Requests: 4731
Filtered Requests: 4730
Requests/s: 1681
```

- `-w`: Path to your wordlist.
- `--hc 404`: Hides responses with the 404 status code (Not Found), since `wenum` by default will log every request it makes.
- `http://IP:PORT/get.php?x=FUZZ`: This is the target URL. `wenum` will replace the parameter value `FUZZ` with words from the wordlist.

Analyzing the results, you'll notice that most requests return the "Invalid parameter value" message and the incorrect value you tried. However, one line stands out:

Code: bash

```bash
 200       1 L       1 W        25 B  GET      http://IP:PORT/get.php?x=OA...
```

This indicates that when the parameter `x` was set to the value "`OA...`," the server responded with a `200 OK` status code, suggesting a valid input.

If you try accessing `http://IP:PORT/get.php?x=OA...`, you'll see the flag.

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ curl http://IP:PORT/get.php?x=OA...

HTB{...}
```

### Wenum fuzzing POST request

Fuzzing POST parameters requires a slightly different approach than fuzzing GET parameters. Instead of appending values directly to the URL, we'll use `ffuf` to send the payloads within the request body. This enables us to test how the application handles data submitted through forms or other POST mechanisms.

Our target application also features a POST parameter named "`y`" within the `post.php` script. Let's probe it with `curl` to see its default behavior:

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ curl -d "" http://IP:PORT/post.php

Invalid parameter value
y: 
```

The `-d` flag instructs `curl` to make a POST request with an empty body. The response tells us that the parameter `y` is expected but not provided.

As with GET parameters, manually testing POST parameter values would be inefficient. We'll use `ffuf` to automate this process:

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : POST
 :: URL              : http://IP:PORT/post.php
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/common.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : y=FUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200
________________________________________________

[Status: 200, Size: 26, Words: 1, Lines: 2, Duration: 7ms]
| URL | http://IP:PORT/post.php
    * FUZZ: SU...

:: Progress: [4730/4730] :: Job [1/1] :: 5555 req/sec :: Duration: [0:00:01] :: Errors: 0 ::
```

The main difference here is the use of the `-d` flag, which tells `ffuf` that the payload ("`y=FUZZ`") should be sent in the request body as `POST` data.

Again, you'll see mostly invalid parameter responses. The correct value ("`SU...`") will stand out with its `200 OK` status code:

Code: bash

```bash
000000326:  200     1 L      1 W     26 Ch     "SU..."
```

Similarly, after identifying "`SU...`" as the correct value, validate it with `curl`:

Parameter and Value Fuzzing

```shell-session
xeloki@htb[/htb]$ curl -d "y=SU..." http://IP:PORT/post.php

HTB{...}
```