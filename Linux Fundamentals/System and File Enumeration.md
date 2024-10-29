[[Linux Cheat Sheet]]
`apropos`.-a tool that can be useful in the beginning. Each manual page has a short description available within it. This tool searches the descriptions for instances of a given keyword.
![[apropos sudo example.png]]


Running `uname -a` will print all information about the machine in a specific order: kernel name, hostname, the kernel release, kernel version, machine hardware name, and operating system. The `-a` flag will omit `-p` (processor type) and `-i` (hardware platform) if they are unknown.
![[Pasted image 20241028061619.png]]


![[Pasted image 20241028062600.png]]

![[Pasted image 20241028063447.png]]
![[Pasted image 20241028065914.png]]
ls -i    to locate incode number of files. 

find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null

![[Pasted image 20241028071705.png]]
