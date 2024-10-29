[[Web Fuzzing Cheat Sheet]]

gobuster vhost examples:

gobuster vhost -u http://inlanefreight.htb:44892 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain

gobuster vhost -u http://inlanefreight.htb:44892 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain | grep "web-"

gobuster dns -d inlanefreight.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt 


## Gobuster

`Gobuster` is a versatile command-line tool renowned for its directory/file and DNS busting capabilities. It systematically probes target web servers or domains to uncover hidden directories, files, and subdomains, making it a valuable asset in security assessments and penetration testing.

`Gobuster's` flexibility extends to fuzzing for various types of content:

- `Directories`: Discover hidden directories on a web server.
- `Files`: Identify files with specific extensions (e.g., `.php`, `.txt`, `.bak`).
- `Subdomains`: Enumerate subdomains of a given domain.
- `Virtual Hosts (vhosts)`: Uncover hidden virtual hosts by manipulating the `Host` header.


Let's dissect the `Gobuster` vhost fuzzing command:

Virtual Host and Subdomain Fuzzing

```shell-session
xeloki@htb[/htb]$ gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```

- `gobuster vhost`: This flag activates `Gobuster's vhost fuzzing mode`, instructing it to focus on discovering virtual hosts rather than directories or files.
- `-u http://inlanefreight.htb:81`: This specifies the base URL of the target server. `Gobuster` will use this URL as the foundation for constructing requests with different vhost names. In this example, the target server is located at `inlanefreight.htb` and listens on port 81.
- `-w /usr/share/seclists/Discovery/Web-Content/common.txt`: This points to the wordlist file that `Gobuster` will use to generate potential vhost names. The `common.txt` wordlist from SecLists contains a collection of commonly used vhost names and subdomains.
- `--append-domain`: This crucial flag instructs `Gobuster` to append the base domain (`inlanefreight.htb`) to each word in the wordlist. This ensures that the `Host` header in each request includes a complete domain name (e.g., `admin.inlanefreight.htb`), which is essential for vhost discovery.

In essence, `Gobuster` takes each word from the wordlist, appends the base domain to it, and then sends an HTTP request to the target URL with that modified `Host` header. By analyzing the server's responses (e.g., status codes, response size), `Gobuster` can identify valid `vhosts` that might not be publicly advertised or documented.

### Gobuster Subdomain Fuzzing

While often associated with vhost and directory discovery, `Gobuster` also excels at subdomain enumeration, a crucial step in mapping the attack surface of a target domain. By systematically testing variations of potential subdomain names, `Gobuster` can uncover hidden or forgotten subdomains that might host valuable information or vulnerabilities.

Let's break down the `Gobuster` subdomain fuzzing command:

Virtual Host and Subdomain Fuzzing

```shell-session
xeloki@htb[/htb]$ gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt 
```

- `gobuster dns`: Activates `Gobuster's` DNS fuzzing mode, directing it to focus on discovering subdomains.
- `-d inlanefreight.com`: Specifies the target domain (e.g., `inlanefreight.com`) for which you want to discover subdomains.
- `-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`: This points to the wordlist file that `Gobuster` will use to generate potential subdomain names. In this example, we're using a wordlist containing the top 5000 most common subdomains.

Under the hood, `Gobuster` works by generating subdomain names based on the wordlist, appending them to the target domain, and then attempting to resolve those subdomains using DNS queries. If a subdomain resolves to an IP address, it is considered valid and included in the output.