# Metalang
An overview, written by genr8eofl

## Assembly
The language our machine overlords chose for us in 1960 is still viable as a bridge to the future of humanity!

### ISA
The most accurate representation of the Instruction Set Architecure (ISA) - go directly to the source, the Intel® 64 and IA-32 Architectures Software Developer Manuals and AMD64 Architecture Programmer's Manual
This maps very closely to our real hardware CPUs, and should be considered as low as you can go in software.
The machine itself thinks in binary op-codes and operands, 80 01 03 05 07 but we can refer to the instructions by mnemonics (names), ie MOVSB, ADD, SUB, so a mere human can use named instructions in a fancy assembler program.

### ABI
The System-V ABI sets a uniform convention we live and die by.


### Stack
[rsp]
Normally it is used for program execution and control flow.
The stack pointer is going to be some hex address in memory that contains a small working area, 8KB ?
you can pass large parameters to functions, on the stack, if it won't fit in a register.

### Calling Conventions
The C calling convention is cdecl, stdcall, but theres also fastcall, and purecall from windows. It relates to how entry/exit function boilerplate is handled, and what is done with the Stack, Return values and Registers.

#### OS: Linux / ELF64 

### Syscalls
Our OS relies on the kernel to handle the hardware and interrupts, and sycalls to transfer execution between userspace and kernel mode. They are numbered 1-400+ and designated to implement important libraries any OS would need, but realistically only a handful would be relevant to a minimal implementation: 
fread, fwrite, printf, etc.

#### libc
Standard library stdio.h includes very common functions for common tasks and programs. String functionality in string.h . Also includes the Allocator, for memory management other helpers for managing the environment.
Contains typedefs and primitives necessary for use in high level languages.


