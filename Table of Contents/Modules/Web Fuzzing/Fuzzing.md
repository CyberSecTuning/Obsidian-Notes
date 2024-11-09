[[Web Fuzzing Cheat Sheet]]

ffuf directory template

ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://<target-ip:<port>/webfuzzing_hidden_path/flag/FUZZ -e .txt,.php,.html -v

## ffuf 


We will use `ffuf` for this fuzzing task. Here's how `ffuf` generally works:

1. `Wordlist`: You provide `ffuf` with a wordlist containing potential directory or file names.
2. `URL with FUZZ keyword`: You construct a URL with the `FUZZ` keyword as a placeholder where the wordlist entries will be inserted.
3. `Requests`: `ffuf` iterates through the wordlist, replacing the `FUZZ` keyword in the URL with each entry and sending HTTP requests to the target web server.
4. `Response Analysis`: `ffuf` analyzes the server's responses (status codes, content length, etc.) and filters the results based on your criteria.

For example, if you want to fuzz for directories, you might use a URL like this:

Code: http

```http
http://localhost/FUZZ
```

`ffuf` will replace `FUZZ` with words like "`admin`," "`backup`," "`uploads`," etc., from your chosen wordlist and then send requests to `http://localhost/admin`, `http://localhost/backup`, and so on.

### Directory Fuzzing

The first step is to perform directory fuzzing, which helps us discover hidden directories on the web server. Here's the ffuf command we'll use:

Directory and File Fuzzing

```shell-session
xeloki@htb[/htb]$ ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://IP:PORT/FUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-399
________________________________________________

[...]

w2ksvrus                [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 0ms]
:: Progress: [220559/220559] :: Job [1/1] :: 100000 req/sec :: Duration: [0:00:03] :: Errors: 0 ::
```

- `-w` (wordlist): Specifies the path to the wordlist we want to use. In this case, we're using a medium-sized directory list from SecLists.
- `-u` (URL): Specifies the base URL to fuzz. The `FUZZ` keyword acts as a placeholder where the fuzzer will insert words from the wordlist.

The output above shows that `ffuf` has discovered a directory called `w2ksvrus` on the target web server, as indicated by the 301 (Moved Permanently) status code. This could be a potential entry point for further investigation.

### File Fuzzing

While directory fuzzing focuses on finding folders, file fuzzing dives deeper into discovering specific files within those directories or even in the root of the web application. Web applications use various file types to serve content and perform different functions. Some common file extensions include:

- `.php`: Files containing PHP code, a popular server-side scripting language.
- `.html`: Files that define the structure and content of web pages.
- `.txt`: Plain text files, often storing simple information or logs.
- `.bak`: Backup files are created to preserve previous versions of files in case of errors or modifications.
- `.js`: Files containing JavaScript code add interactivity and dynamic functionality to web pages.

By fuzzing for these common extensions with a wordlist of common file names, we increase our chances of discovering files that might be unintentionally exposed or misconfigured, potentially leading to information disclosure or other vulnerabilities.