
[[Web Fuzzing Cheat Sheet]]
## FFUF
[[Web Fuzzing Cheat Sheet]]

`FFUF` (`Fuzz Faster U Fool`) is a fast web fuzzer written in Go. It excels at quickly enumerating directories, files, and parameters within web applications. Its flexibility, speed, and ease of use make it a favorite among security professionals and enthusiasts.

## Gobuster

`Gobuster` is another popular web directory and file fuzzer. It's known for its speed and simplicity, making it a great choice for beginners and experienced users alike.

## FeroxBuster

`FeroxBuster` is a fast, recursive content discovery tool written in Rust. It's designed for brute-force discovery of unlinked content in web applications, making it particularly useful for identifying hidden directories and files. It's more of a "forced browsing" tool than a fuzzer like `ffuf`.


## wfuzz/wenum

`wenum` is a actively maintained fork of `wfuzz`, a highly versatile and powerful command-line fuzzing tool known for its flexibility and customization options. It's particularly well-suited for parameter fuzzing, allowing you to test a wide range of input values against web applications and uncover potential vulnerabilities in how they process those parameters.

If you are using a penetration testing Linux distribution like PwnBox or Kali, `wfuzz` may already be pre-installed, allowing you to use it right away if desired. However, there are currently complications when installing `wfuzz`, so you can substitute it with `wenum` instead. The commands are interchangeable, and they follow the same syntax, so you can simply replace `wenum` commands with `wfuzz` if necessary.