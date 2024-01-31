# Metalang
An overview, written by genr8eofl

## Machine Code
The machine itself thinks in binary op-codes and operands, 80 01 03 05 07. 83 FF FF FF FF. 61 AA 55 AA 55.
But we can refer to the instructions by mnemonics (names), ie MOVSB, ADD, SUB, so a mere human can use named instructions in a fancy assembler program.

## Assembly
The language our machine overlords chose for us in 1960 is still viable as a bridge to the future of humanity!

### ISA
The most accurate representation of the Instruction Set Architecure (ISA) - go directly to the source, the Intel® 64 and IA-32 Architectures Software Developer Manuals, and AMD64 Architecture Programmer's Manual.
This maps very closely to our real hardware CPUs, and should be considered as close to bare-metal and as low as you can touch in software.

### ABI
The System-V ABI sets a uniform convention we live and die by. What to do with registers when calling functions.

### Calling Conventions
The C calling convention is cdecl, stdcall, but theres also fastcall, (and purecall from MS Windows).
It relates to how entry/exit function boilerplate is handled, and what is done with the Stack, Return values and Registers. Standardized.

### Stack
[rsp]
Normally it is used for program execution and control flow.
The stack pointer is going to be some hex address in memory that contains a small working area, 8KB ?
You can pass large parameters to functions, on the stack, if it won't fit in a register. We can take control over this area for our main purposes.

### Low-level / High-level languages
After binary machine code, the only real low level language used is Assembly. C was often thought of as a high level language because of how advanced it was at the time, 1980s, but now we would consider it a low level language due to it being so closely tied to real physical hardware, where you the developer handle the physical memory management and CPU caches. But thats up for debate elsewhere. For my purposes I think C is in the middle, an "Intermediary" Language. Fact is though, C still remains heavily used, and C NEEDS to be used to bootstrap other high level languages and high level environments we want. Assembly is also a key component in bootstrapping anything, from C itself, and beyond.
Most high-level languages are now doing memory management and garbage collection for you. Nobody reverts back to assembly and its crude manual ways by choice, once we can get advanced compilers running. Almost nobody. And why not automate it?

### Syscalls
Our OS relies on the kernel to handle the hardware and interrupts, and sycalls to transfer execution between userspace and kernel mode. They are numbered 1-400+ and designated to implement important libraries any OS would need, but realistically only a handful would be relevant to a minimal implementation: 
fread, fwrite, printf, etc.

#### libc
Standard library stdio.h includes very common functions for common tasks and programs. Most functionality falls into these categories: File I/O, Character I/O (TTY), Strings, Math, Random, Malloc, Debug Assert, Time, Signals...
String functionality in string.h. Note, includes the Allocator, for memory management, _start, exit, other helpers for managing the environment. Contains all typedefs and primitives necessary for use by high level language.

#### POSIX (portability)
The benefit is being able to write one codebase and ship it to millions of people across differently architected machines. However that is annoying because now you have to follow POSIX rules meant to account for weird architectures.
Personally I feel like this tool has outlived its usefulness, and more effort should be spent on architecture specific tooling. Cross platform code is increasingly difficult. A hard decision needs to be made, and I've chosen AMD64.

#### OS: Linux / ELF64 (not portable but widespread)
Running your program is usually done from inside an operating system. This is the best one to choose for programming language and development. The Standard can't help us, Portability is out the window, but Linux is a common known quantity.
The Linux kernel ABI and the binary executable format (ELF) is dictated already and we will just conform to the de-facto standard; this is fine - although potentially confusing due to increasing complexity.

#### Rust
Rust is a compiled language that aims to try to fix C's biggest mistake, memory safety. Do not be fooled by that, this is a distraction. The rust compiler requires a lot of Box'ing and Unsafe operations. Its basically operating an instruction set ponzi scheme. The funky syntax and always thinking about borrowing and mutability, just leads to constantly begging the compiler just to let one piece of your own code access your own object, with increasingly verbose syntax and semantics. Nobody can even be sure what they did is correct due to lack of introspection and debugging code inspection tools. Rust needs to be deconstructed down to first principles and re-applied back on top of assembly, and then we can talk about what we can keep. We dont need Rust, we need to know what Rust does.

#### AI
Machine readability of code and machine learning are going to transform the ecosystem very very soon. I am eagerly awaiting tools to convert one language to another language and bindings for executing small repeatable payloads of code fron source A that we know will work on target B. Think ShellCode Generators but for every program, library, function, and algorithm in the world. Hopefully it would take a functional language approach so it can also work on "correctness", and proving what was transliterated is formally verifiable (proofs) - and not lossy, so we can do bidirectional translation (and recursion).

#### Assembler, Compiler, Preprocessor, Linker, Loader, LD, "SELF-HOSTED" GCC + Binutils, elfutils, etc.
These tools need to be present on the system but their significance can not be overlooked. Understand how these packages work to provide a safe and full compilation environment for C and asm, so we can replicate the functionality elsewhere
The system is fully working but the tools provide a way for the programmer to use the tools they just created to make more programs, closing the loop. You may now consume your own dogfood.

#### Lowering (compiler design)
A compiler can be designed to convert higher level constructs into an equivalent sequence of lower level constructs. For example like unrolling for/while loops, where a loop pattern is detected, matched, transformed, and rewritten without the author needing to say so. This USUALLY falls under the category of Optimization, -O3 for example would autovectorize any loops (-ftree-vectorize) and by changing the instruction order, changes the performance. Is it Compiler Magic or normal syntactic sugar? Our compiler COULD take high level ASM for human purposes, and lower it to a more low level ASM, for machine purposes.

#### High Level Constructs (compiler design)
We think of Data types, variables, arrays, pointers, strings, vectors - but really ALL the control flow structures, while, for, if,else, functions, all of the specifics of the high level language will ALL need to be specified. Boring.
There is an esolang checklist somewhere on HN,

#### Notes
##### External Sources:
check out "Bob Morgan's Building an Optimizing Compiler (1998)"
GitHub Repository @ github.com/grassator/mass = Meta assembly language aims to provide the developer flexibility to adjust every step of the compilation process. Tokenization, parsing, type checking, optimizations, instruction generation, and binary output should be accessible and augmentable. 
check out this guys' blog - https://mass.handmade.network/ 