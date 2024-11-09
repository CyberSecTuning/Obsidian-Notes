Download ./licence binary and open with ghidra to examine binary. 

![[Pasted image 20241030020609.png]]
We can analyze the main function and find the first password which is given as `PasswordNumeroUno`

We can now use ltrace and strace to begin debugging our program. 
ltrace shows library calls and strace shows system calls. 

working through the program with ltrace we notice that an incorrect input, reveals the correct answer. 

![[Pasted image 20241030021024.png]]
![[Pasted image 20241030021053.png]]

Using this logic we were able to find all 3 passwords to run the program correctly.
Running the program alone with the correct answers, still does not yeild a flag.

Since the challenge spawns a docker, we can assume it can be communicated with via netcat. 

![[Pasted image 20241030021510.png]]
Once netcat is connected to the host we will have to answer questions about the file.
Using the file command many of the answers will be answered. 
ldd will display the shared library dependencies.
![[Pasted image 20241030020849.png]]

the Xor key used can be seen in the source code as 0x13 which is 19 in decimal form.
![[Pasted image 20241030021655.png]]
Examining the binary we can see there was 5 put calls in the main function. 
![[Pasted image 20241030022123.png]]

![[Pasted image 20241030021450.png]]
Once all answers are correct, the flag is output.
![[Pasted image 20241030021623.png]]



