###### Task 1

Which TCP port is hosting a database server?
Scan the IP using nmap 
`nmap -sV -sC <IP>`
![[Pasted image 20241104210947.png]]
###### Task 2

What is the name of the non-Administrative share available over SMB?
connect to the IP over smbclient 
`smbclient -N -L 10.129.98.5`
-N : No password
-L : This option allows you to look at what services are available on a server
###### Task 3

What is the password identified in the file on the SMB share?

Using smbclient, we can navigate it the share backups, which was shown with no comments.
using the command
`smbclient -N \\\\10.129.98.5\\backups`
once logged into the client, we can enumerate the database using the dir command. 
prod.dtsConfig is found in the directory and can be obtained using the get command
get prod.dtsConfig

![[Pasted image 20241105164323.png]]
using cat prod.dtsConfig shows the configuration contents which shows the username and password. 

Password= M3g4c0rp123

Task 4

What script from Impacket collection can be used in order to establish an authenticated connection to a Microsoft SQL Server?

reviewing the prod.dtsConfig contents, it shows the user type as sql_svc, the impacket tool 

a google search will reveal the impacket for sql_svc is 
`mssqlclient.py`
using the command
`python 3 mssqlclient.py ARCHETYPE/sql_svc@10.129.74.31 --windows-auth`
we can use this` script can be used to gain a foothold on the targets SQL server. 
