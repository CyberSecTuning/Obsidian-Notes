###### Task 1

Which TCP port is hosting a database server?
Scan the IP using nmap 
`nmap -sV -sC <IP>`
![[Pasted image 20241104210947.png]]
###### Task 2

What is the name of the non-Administrative share available over SMB?
connect to the IP over smbclient 
`smbclient -N -L 10.129.98.5`

###### Task 3

What is the password identified in the file on the SMB share?

Using smbclient, we can navigate ito the share backups, which was unprotected.



Password=M3g4c0rp123