[[Assembly Cheat Sheet]]

The second type of basic instructions is Arithmetic Instructions. With Arithmetic Instructions, we can perform various mathematical computations on data stored in registers and memory addresses. These instructions are usually processed by the ALU in the CPU, among other instructions. We will split arithmetic instructions into two types: instructions that take only one operand (`Unary`), instructions that take two operands (`Binary`).

## Unary Instructions

The following are the main Unary Arithmetic Instructions (we will assume that `rax` starts as `1` for each instruction):

| Instruction | Description    | Example                                         |
| ----------- | -------------- | ----------------------------------------------- |
| `inc`       | Increment by 1 | `inc rax` -> `rax++` or `rax += 1` -> `rax = 2` |
| `dec`       | Decrement by 1 | `dec rax` -> `rax--` or `rax -= 1` -> `rax = 0` |


