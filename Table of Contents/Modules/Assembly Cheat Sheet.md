
---
[[HTB Modules]]
Table of Contents
Fundamentals
[[Ram Architecture]]
[[CPU Architecture]]
[[ISA]]
[[Registers]]
[[Assembly File Structure]]


[[Assembling]]
[[Disassembling]]
[[GDB]]
[[GDB Debugging]]
[[Data Movement]]
[[Arithmetic Instructions -Assembly]]


## Compilation Stages

![Compilation Stages](https://academy.hackthebox.com/storage/modules/85/assembly_Compilation_Stages_1.jpg)

If we run this Python line, it would be essentially executing the following `C` code:

Code: c

```c
#include <unistd.h>

int main()
{
    write(1, "Hello World!", 12);
    _exit(0);
}
```

Note: the actual `C` source code is much longer, but the above is the essence of how the string '`Hello World!`' is printed. If you are ever interested in knowing more, you can check out the source code of the Python3 print function at this [link](https://github.com/python/cpython/blob/0332e569c12d3dc97171546c6dc10e42c27de34b/Python/bltinmodule.c#L1829) and this [link](https://github.com/python/cpython/blob/9975cc5008c795e069ce11e2dbed2110cc12e74e/Objects/fileobject.c#L119)

The above `C` code uses the Linux `write` syscall, built-in for processes to write to the screen. The same syscall called in Assembly looks like the following:

Code: nasm

```nasm
mov rax, 1
mov rdi, 1
mov rsi, message
mov rdx, 12
syscall

mov rax, 60
mov rdi, 0
syscall
```

Code: shellcode

```shellcode
48 c7 c0 01
48 c7 c7 01
48 8b 34 25
48 c7 c2 0d
0f 05

48 c7 c0 3c
48 c7 c7 00
0f 05
```

Finally, for the processor to execute the instructions linked to this machine, it would have to be translated into binary, which would look like the following:

Code: binary

```binary
01001000 11000111 11000000 00000001
01001000 11000111 11000111 00000001
01001000 10001011 00110100 00100101
01001000 11000111 11000010 00001101 
00001111 00000101

01001000 11000111 11000000 00111100 
01001000 11000111 11000111 00000000 
00001111 00000101
```
In the above 'Hello World' example, which Assembly instruction will '00001111 00000101' execute?
syscall

