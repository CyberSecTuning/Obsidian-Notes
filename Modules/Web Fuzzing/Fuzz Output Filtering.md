[[Web Fuzzing Cheat Sheet]]
```shell-session
ffuf -u http://83.136.255.228:50442//post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -v -s 200,301
```
Filtering Fuzzing Output

```shell-session
# Show only successful requests and redirects:
xeloki@htb[/htb]$ wenum -w wordlist.txt --sc 200,301,302 -u https://example.com/FUZZ

# Hide responses with common error codes:
xeloki@htb[/htb]$ wenu -w wordlist.txt --hc 404,400,500 -u https://example.com/FUZZ

# Show only short error messages (responses with 5-10 words):
xeloki@htb[/htb]$ wenum -w wordlist.txt --sw 5-10 -u https://example.com/FUZZ

# Hide large files and focus on smaller responses:
xeloki@htb[/htb]$ wenum -w wordlist.txt --hs 10000 -u https://example.com/FUZZ

# Filter for responses containing specific information:
xeloki@htb[/htb]$ wenum -w wordlist.txt --sr "admin\|password" -u https://example.com/FUZZ
Filtering Fuzzing Output Examples

```shell-session
# Show only successful requests and redirects:
xeloki@htb[/htb]$ wenum -w wordlist.txt --sc 200,301,302 -u https://example.com/FUZZ

# Hide responses with common error codes:
xeloki@htb[/htb]$ wenu -w wordlist.txt --hc 404,400,500 -u https://example.com/FUZZ

# Show only short error messages (responses with 5-10 words):
xeloki@htb[/htb]$ wenum -w wordlist.txt --sw 5-10 -u https://example.com/FUZZ

# Hide large files and focus on smaller responses:
xeloki@htb[/htb]$ wenum -w wordlist.txt --hs 10000 -u https://example.com/FUZZ

# Filter for responses containing specific information:
xeloki@htb[/htb]$ wenum -w wordlist.txt --sr "admin\|password" -u https://example.com/FUZZ

### Gobuster

`Gobuster` offers various filtering options depending on the module being run, to help you focus on specific responses and streamline your analysis. There is a small caveat, the `-s` and `-b` options are only available in the `dir` fuzzing mode.

|Flag|Description|Example Scenario|
|---|---|---|
|`-s` (include)|Include only responses with the specified status codes (comma-separated).|You're looking for redirects, so you filter for codes `301,302,307`|
|`-b` (exclude)|Exclude responses with the specified status codes (comma-separated).|The server returns many 404 errors. Exclude them with `-b 404`|
|`--exclude-length`|Exclude responses with specific content lengths (comma-separated, supports ranges).|You're not interested in 0-byte or 404-byte responses, so use `--exclude-length 0,404`|

By strategically combining these filtering options, you can tailor `Gobuster's` output to your specific needs and focus on the most relevant results for your security assessments.

Filtering Fuzzing Output

```shell-session
# Find directories with status codes 200 or 301, but exclude responses with a size of 0 (empty responses)
xeloki@htb[/htb]$ gobuster dir -u http://example.com/ -w wordlist.txt -s 200,301 --exclude-length 0
```

## FFUF

`FFUF` offers a highly customizable filtering system, enabling precise control over the displayed output. This allows you to efficiently sift through potentially large amounts of data and focus on the most relevant findings. `FFUF's` filtering options are categorized into multiple types, each serving a specific purpose in refining your results.

|Flag|Description|Example Scenario|
|---|---|---|
|`-mc` (match code)|Include only responses that match the specified status codes. You can provide a single code, multiple codes separated by commas, or ranges of codes separated by hyphens (e.g., `200,204,301`, `400-499`). The default behavior is to match codes 200-299, 301, 302, 307, 401, 403, 405, and 500.|After fuzzing, you notice many 302 (Found) redirects, but you're primarily interested in 200 (OK) responses. Use `-mc 200` to isolate these.|
|`-fc` (filter code)|Exclude responses that match the specified status codes, using the same format as `-mc`. This is useful for removing common error codes like 404 Not Found.|A scan returns many 404 errors. Use `-fc 404` to remove them from the output.|
|`-fs` (filter size)|Exclude responses with a specific size or range of sizes. You can specify single sizes or ranges using hyphens (e.g., `-fs 0` for empty responses, `-fs 100-200` for responses between 100 and 200 bytes).|You suspect the interesting responses will be larger than 1KB. Use `-fs 0-1023` to filter out smaller responses.|
|`-ms` (match size)|Include only responses that match a specific size or range of sizes, using the same format as `-fs`.|You are looking for a backup file that you know is exactly 3456 bytes in size. Use `-ms 3456` to find it.|
|`-fw` (filter out number of words in response)|Exclude responses containing the specified number of words in the response.|You're filtering out a specific number of words from the responses. Use `-fw 219` to filter for responses containing that amount of words.|
|`-mw` (match word count)|Include only responses that have the specified amount of words in the response body.|You're looking for short, specific error messages. Use `-mw 5-10` to filter for responses with 5 to 10 words.|
|`-fl` (filter line)|Exclude responses with a specific number of lines or range of lines. For example, `-fl 5` will filter out responses with 5 lines.|You notice a pattern of 10-line error messages. Use `-fl 10` to filter them out.|
|`-ml` (match line count)|Include only responses that have the specified amount of lines in the response body.|You're looking for responses with a specific format, such as 20 lines. Use `-ml 20` to isolate them.|
|`-mt` (match time)|Include only responses that meet a specific time-to-first-byte (TTFB) condition. This is useful for identifying responses that are unusually slow or fast, potentially indicating interesting behavior.|The application responds slowly when processing certain inputs. Use `-mt >500` to find responses with a TTFB greater than 500 milliseconds.|

You can combine multiple filters. For example:

Filtering Fuzzing Output

```shell-session
# Find directories with status code 200, based on the amount of words, and a response size greater than 500 bytes
xeloki@htb[/htb]$ ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200 -fw 427 -ms >500

# Filter out responses with status codes 404, 401, and 302
xeloki@htb[/htb]$ ffuf -u http://example.com/FUZZ -w wordlist.txt -fc 404,401,302

# Find backup files with the .bak extension and size between 10KB and 100KB
xeloki@htb[/htb]$ ffuf -u http://example.com/FUZZ.bak -w wordlist.txt -fs 0-10239 -ms 10240-102400

# Discover endpoints that take longer than 500ms to respond
xeloki@htb[/htb]$ ffuf -u http://example.com/FUZZ -w wordlist.txt -mt >500
```


### wenum

`wenum` offers a robust filtering system to help you manage and refine the vast amount of data generated during fuzzing. You can filter based on status codes, response size/character count, word count, line count, and even regular expressions.

|Flag|Description|Example Scenario|
|---|---|---|
|`--hc` (hide code)|Exclude responses that match the specified status codes.|After fuzzing, the server returned many 400 Bad Request errors. Use `--hc 400` to hide them and focus on other responses.|
|`--sc` (show code)|Include only responses that match the specified status codes.|You are only interested in successful requests (200 OK). Use `--sc 200` to filter the results accordingly.|
|`--hl` (hide length)|Exclude responses with the specified content length (in lines).|The server returns verbose error messages with many lines. Use `--hl` with a high value to hide these and focus on shorter responses.|
|`--sl` (show length)|Include only responses with the specified content length (in lines).|You suspect a specific response with a known line count is related to a vulnerability. Use `--sl` to pinpoint it.|
|`--hw` (hide word)|Exclude responses with the specified number of words.|The server includes common phrases in many responses. Use `--hw` to filter out responses with those word counts.|
|`--sw` (show word)|Include only responses with the specified number of words.|You are looking for short error messages. Use `--sw` with a low value to find them.|
|`--hs` (hide size)|Exclude responses with the specified response size (in bytes or characters).|The server sends large files for valid requests. Use `--hs` to filter out these large responses and focus on smaller ones.|
|`--ss` (show size)|Include only responses with the specified response size (in bytes or characters).|You are looking for a specific file size. Use `--ss` to find it.|
|`--hr` (hide regex)|Exclude responses whose body matches the specified regular expression.|Filter out responses containing the "Internal Server Error" message. Use `--hr "Internal Server Error"`.|
|`--sr` (show regex)|Include only responses whose body matches the specified regular expression.|Filter for responses containing the string "admin" using `--sr "admin"`.|
|`--filter`/`--hard-filter`|General-purpose filter to show/hide responses or prevent their post-processing using a regular expression.|`--filter "Login"` will show only responses containing "Login", while `--hard-filter "Login"` will hide them and prevent any plugins from processing them.|

