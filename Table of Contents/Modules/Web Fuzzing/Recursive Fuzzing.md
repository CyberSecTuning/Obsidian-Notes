
[[Web Fuzzing Cheat Sheet]]

ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://94.237.63.215:33315/FUZZ -e .html -recursion 
recursive fuzzing templates 
```shell-session
$ ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://IP:PORT/FUZZ -e .html -recursion 
```


```shell-session
$ ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```

- `-w` ➡ Flag for defining a wordlist.
- `-u` ➡ Flag for defining a URL with a FUZZ keyword.
- `-v` ➡ Flag for displaying the full URLs in response, verbose output.
- `-ic` ➡ Ignore comments in wordlists. (lines starting with “#”)

While recursive fuzzing is a powerful technique, it can also be resource-intensive, especially on large web applications. Excessive requests can overwhelm the target server, potentially causing performance issues or triggering security mechanisms.

To mitigate these risks, `ffuf` provides options for fine-tuning the recursive fuzzing process:

- `-recursion-depth`: This flag allows you to set a maximum depth for recursive exploration. For example, `-recursion-depth 2` limits fuzzing to two levels deep (the starting directory and its immediate subdirectories).
- `-rate`: You can control the rate at which `ffuf` sends requests per second, preventing the server from being overloaded.
- `-timeout`: This option sets the timeout for individual requests, helping to prevent the fuzzer from hanging on unresponsive targets.


## How Recursive Fuzzing Works

Recursive fuzzing is an automated way to delve into the depths of a web application's directory structure. It's a pretty basic 3 step process:

1. `Initial Fuzzing`:
    - The fuzzing process begins with the top-level directory, typically the web root (`/`).
    - The fuzzer starts sending requests based on the provided wordlist containing the potential directory and file names.
    - The fuzzer analyzes server responses, looking for successful results (e.g., HTTP 200 OK) that indicate the existence of a directory.
2. `Directory Discovery and Expansion`:
    - When a valid directory is found, the fuzzer doesn't just note it down. It creates a new branch for that directory, essentially appending the directory name to the base URL.
    - For example, if the fuzzer finds a directory named `admin` at the root level, it will create a new branch like `http://localhost/admin/`.
    - This new branch becomes the starting point for a fresh fuzzing process. The fuzzer will again iterate through the wordlist, appending each entry to the new branch's URL (e.g., `http://localhost/admin/FUZZ`).
3. `Iterative Depth`:
    - The process repeats for each discovered directory, creating further branches and expanding the fuzzing scope deeper into the web application's structure.
    - This continues until a specified depth limit is reached (e.g., a maximum of three levels deep) or no more valid directories are found.