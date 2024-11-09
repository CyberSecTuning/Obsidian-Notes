[[Assembly Cheat Sheet]]
[[Registers]]
## Memory Addresses

As previously mentioned, x86 64-bit processors have 64-bit wide addresses that range from `0x0` to `0xffffffffffffffff`, so we expect the addresses to be in this range. However, RAM is segmented into various regions, like the Stack, the heap, and other program and kernel-specific regions. Each memory region has specific `read`, `write`, `execute` permissions that specify whether we can read from it, write to it, or call an address in it.

Whenever an instruction goes through the Instruction Cycle to be executed, the first step is to fetch the instruction from the address it's located at, as previously discussed. There are several types of address fetching (i.e., addressing modes) in the x86 architecture:

|Addressing Mode|Description|Example|
|---|---|---|
|`Immediate`|The value is given within the instruction|`add 2`|
|`Register`|The register name that holds the value is given in the instruction|`add rax`|
|`Direct`|The direct full address is given in the instruction|`call 0xffffffffaa8a25ff`|
|`Indirect`|A reference pointer is given in the instruction|`call 0x44d000` or `call [rax]`|
|`Stack`|Address is on top of the stack|`add rsp`|

In the above table, lower is slower. The less immediate the value is, the slower it is to fetch it.

Even though speed isn't our biggest concern when learning basic Assembly, we should understand where and how each address is located. Having this understanding will help us in future binary exploitation, with Buffer Overflow exploits, for example. The same understanding will have an even more significant implication with advanced binary exploitation, like ROP or Heap exploitation.