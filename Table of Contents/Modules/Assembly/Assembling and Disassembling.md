[[Assembly Cheat Sheet]]

## Assembling

First, we will copy the code below into a file called `helloWorld.s`.

Note: assembly files usually use the `.s` or the `.asm` extensions. We will be using `.s` in this module.

We don't have to keep using tabs to separate parts of an assembly file, as this was only for demonstration purposes. We can write the following code into our `helloWorld.s` file:


```assembly
global _start

section .data
    message db "Hello HTB Academy!"
    length equ $-message

section .text
_start:
    mov rax, 1
    mov rdi, 1
    mov rsi, message
    mov rdx, length
    syscall

    mov rax, 60
    mov rdi, 0
    syscall
```
Note how we used `equ` to dynamically calculate the length of `message`, instead of using a static `18`. This will become very handy later on. Once we do, we will assemble the file using `nasm`, with the following command:

Assembling & Disassembling

```shell-session
xeloki@htb[/htb]$ nasm -f elf64 helloWorld.s
```
Note: The `-f elf64` flag is used to note that we want to assemble a 64-bit assembly code. If we wanted to assemble a 32-bit code, we would use `-f elf`.

The final step is to link our file using `ld`. The `helloWorld.o` object file, though assembled, still cannot be executed. This is because many references and labels used by `nasm` need to be resolved into actual addresses, along with linking the file with various OS libraries that may be needed.

This is why a Linux binary is called `ELF`, which stands for an `Executable and Linkable Format`. To link a file using `ld`, we can use the following command:

Assembling & Disassembling

```shell-session
xeloki@htb[/htb]$ ld -o helloWorld helloWorld.o
```

Note: if we were to assemble a 32-bit binary, we need to add the '`-m elf_i386`' flag.

We have successfully assembled and linked our first assembly file. We will be assembling, linking, and running our code frequently through this module, so let us build a simple `bash` script to make it easier:

Code: bash

```bash
#!/bin/bash

fileName="${1%%.*}" # remove .s extension

nasm -f elf64 ${fileName}".s"
ld ${fileName}".o" -o ${fileName}
[ "$2" == "-g" ] && gdb -q ${fileName} || ./${fileName}
```

Now we can write this script to `assembler.sh`, `chmod +x` it, and then run it on our assembly file. It will assemble it, link it, and run it:

Assembling & Disassembling

```shell-session
xeloki@htb[/htb]$ ./assembler.sh helloWorld.s
Hello HTB Academy!
```