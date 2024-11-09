1.)  What operating system is the computer using? What version?  
![[Pasted image 20241106190441.png]]
	WinXPSP2x86

2.) How much RAM is included in the analysis?  



3.) View the running processes. Does this look like your average box?  
 ![[Pasted image 20241106191446.png]]

While the System seems to start as normal, there are many processes that do not belong on an average box. 

a. What processes look abnormal? What makes them abnormal?  

These processes stand out when compared to an average box. 
```
hxdef100.exe- backdoor rootkit
cryptcat.exe- Listener that can be used to create reverse shells.
bircd.exe- IRC that can be used for malicious communication through IRC.
iroffer.exe- IRC file transfer service
poisonivy.exe- RAT trojan backdoor
nc.exe- netcat listener to create reverse shells. 
```

These processes can also be linked to IP addresses associated with their PID's.

![[Pasted image 20241106191729.png]]
1728 is iroffer.exe
1480 is bircd.exe
480 is poisonivy.exe

4.) Can you find user account names? Passwords? If not why not?  

Our hive-scan revealed hex locations we can use to scan for users since hive-dump alone provided no results. 


![[Pasted image 20241107072138.png]]

The hex did not reveal much, but with a google search on how to identify registry hives for user accounts, we can begin to search for `SAM\Domains\Account\Users`

Using Volatility's print-key feature we revealed the sub-keys under the registry. 
The `Names` key, stands out. 

![[Pasted image 20241107072932.png]]
![[Pasted image 20241107073311.png]]
Traversing to the /Names registry, we can now see the names of all the users.

Using filescan, we can further investigate the location offset of the Sam registery

![[Pasted image 20241107073908.png]]

![[Pasted image 20241107075314.png]]
![[Pasted image 20241107075607.png]]



We can dump these files in our specified directory using the dumpfiles flag and specifying the SAM offset
 
![[Pasted image 20241107074412.png]]


5.) View the Dynamically Linked Libraries. Does this look like your average box?  

a. What DLLs look abnormal?  
```
rundll32.exe
fldrclnr.dll
Wizard_RunDLL
```

6.)Can you associate any Processes (PIDs), DLLs, and executables?  

7.) View the files associated with the processes.  
![[Pasted image 20241106195612.png]]
![[Pasted image 20241106195642.png]]
![[Pasted image 20241106195659.png]]

a. Do any files or file paths look abnormal? Reference the file path if available.  

Many files stand out as suspicious when viewing the process list. specifically the files
hxdef100.exe, birc.exe and iroffer.exe. 
`c:\hxdefrootkit\hxdef100.exe hxdef100.ini`
Cryptcat.exe was used with the command:
`C:\hxdefrootkit\cryptcat.exe -L -p 666 -e cmd.exe`
Once the shell was created using cryptcat on port 666, the bircd.exe was executed.
`C:\hidden\bewareircd-win32\bircd.exe`
then
`C:\hidden\ir\iroffer.exe`


8.) Explain what you think happened to this box. Have you seen anything before

Based on the evidence found during the memory analysis, we can visualize the hackers footsteps. 
Windows services appear to start up as normal. We can view the pstree and see the processes which are associated with the system start. 

We see the intruder gained a foothold using the rootkit Hacker Defender: `hxdef100.exe`
This is a well known rootkit used to allow a user to hide files, processes, system services, system drivers, registry keys and values, open ports, cheat with free disk space. Program also masks its changes in memory and hides handles of hidden processes. Program installs hidden back-doors, register as hidden system service and installs hidden system driver.

The intruder begins by editing the hxdef100.ini configuration file. Shortly after the intruder set up a reverse shell listening device on port 666 using crpytcat.exe.
Next  `bircd.exe`  was executed.
`bircd.exe` is he executable for beware ircd, a lightweight IRC that can be used for malicious exploits. In this case it was stored in the `/hidden` directory indicating nefarious use. This program can be particularly strong for an intruder due to the program not being visible; there is no window, or tray icon. It just runs. 

Now that the intruder has gained a foothold with a rootkit and set up an invisible IRC. This can be used by hackers as a command and control to communicate with multiple machines.

Next the `iroffer.exe` executable was ran from the `/hidden` directory.
iroffer is a software program that acts as a fileserver for IRC.
It is similar to a FTP server or WEB server, but users can download
files using the DCC protocol of IRC instead of a web browser, giving the intruder access to transfer files into the machine. 

Poisonivy.exe can then be seen running from the /System32 directory. This was found in the previous lab and can be associated with a known backdoor known as a Remote Administration Tool (RAT).

Shortly after, the intruder executed another reverse shell using netcat with:
`C:\inetpub\ftproot\nc.exe -L -p 6666 -e cmd.exe` 

It seems this reverse shell was used to execute the commands for winvnc4.exe likely to gain remote access and the lock.bat file, which was found in the previous lab to lock down the system when needed. 
Before the command line logs end the command `C:\WINDOWS\System32\rundll32.exe fldrclnr.dll,Wizard_RunDLL` was ran which can be the DLL's used to run malicious code from DLL's or hide evidence. 







![[Pasted image 20241106191605.png]]
![[Pasted image 20241106191648.png]]
![[Pasted image 20241106191729.png]]



refrences
https://github.com/dinoex/iroffer-dinoex
https://github.com/claudiouzelac/rootkit.com/blob/master/hf/hxdef100r/readmeen.txt
https://www.trendmicro.com/vinfo/us/threat-encyclopedia/malware/poisonivy