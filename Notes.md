## Puzzle 5 :
Analyzing Byte Positions
Each opcode and its associated data (if any) occupy a specific number of bytes. Let's break down the bytecode and count the positions accurately.

34: CALLVALUE (1 byte)
80: DUP1 (1 byte)
02: MUL (1 byte)
61 0100: PUSH2 0x0100 (3 bytes, 1 for PUSH2 and 2 for 0100)
14: EQ (1 byte)
60 0C: PUSH1 0x0C (2 bytes, 1 for PUSH1 and 1 for 0C)
57: JUMPI (1 byte)
FD: REVERT (1 byte)
FD: REVERT (1 byte)
5B: JUMPDEST (1 byte)
00: STOP (1 byte)
FD: REVERT (1 byte)
FD: REVERT (1 byte)

Byte Positions
Let's list the opcodes with their positions:

34: Position 0
80: Position 1
02: Position 2
61 0100: Position 3-5
14: Position 6
60 0C: Position 7-8
57: Position 9
FD: Position 10
FD: Position 11
5B: Position 12
00: Position 13
FD: Position 14
FD: Position 15

## Puzzle 7 :

This is the most important aspect of contract deployement and CREATE Opcode as mentioned in [OpenZeppelin post](https://blog.openzeppelin.com/deconstructing-a-solidity-contract-part-ii-creation-vs-runtime-6b9d60ecb44c):

> "[...] The creation code gets executed in a transaction, which returns a copy of the runtime code, which is the actual code of the contract. As we will see, the constructor is part of the creation code, and not part of the runtime code. The contract’s constructor is part of the creation code; it will not be present in the contract’s code once it is deployed."

When the CREATE opcode is executed, only the code returned by the RETURN opcode will be the "runtime code" that will be executed in the future when the deployed contract will be called. The other part of the bytecode is just used once, only for the constructor part.

We want to return runtime code with 1 instruction

Let's see how the RETURN opcode works: it pops 2 values from the stack to use them as input for:

- memory offset from where to start to read
- memory size in bytes to read and return

PUSH1 00
PUSH 00
MSTORE8

PUSH1 01
PUSH1 00
RETURN

## Puzzle 8:
Same logic as 7 but need to force the call to fail. There are many solutions like using an unrecognisable opcode.
