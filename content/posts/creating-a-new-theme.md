---
author:
  name: "Deekshitha Reddy Prakash"
date: 2024-06-28
linktitle: Designing a 5 stage pipelined 64 bit RISC-V Processor
type:
- post
- posts
title: Designing a 5 stage pipelined 64 bit RISC-V Processor
weight: 10
series:
- Hugo 101
---


## Introduction

I’ve decided to embark on an exciting journey of building a 64-bit pipelined RISC-V processor. My implementation will adhere to the RV64I base integer instruction set, the foundational core of the RISC-V architecture.

If you’re curious about the details of the RISC-V specification, feel free to check out the [official RISC-V Instruction Set Manual](https://riscv.org/technical/specifications/).

The '64' in RV64I means that registers, memory addresses, and integer operations are performed with values of 64-bit width. The 'I' refers to the Integer base instruction set, which includes instructions for integer operations only.

### Instruction Formats in RV64I

1. **R-Type (Register Operations):**  
   Used for operations like arithmetic (e.g., `ADD`, `SUB`), logical (e.g., `AND`, `OR`), or shifts (e.g., `SLL`, `SRL`).

2. **I-Type (Immediate Operations):**  
   Used for arithmetic with immediate values (e.g., `ADDI`), load instructions (e.g., `LW`, `LD`), and system calls (e.g., `ECALL`).

3. **S-Type (Store Operations):**  
   Includes store instructions like `SW` (store word) and `SD` (store double word).

4. **U-Type (Upper Immediate Operations):**  
   Includes `LUI` (load upper immediate) and `AUIPC` (add upper immediate to program counter).

5. **B-Type (Branch Instructions):**  
   Used for conditional branches like `BEQ` (branch if equal) and `BNE` (branch if not equal).  
   These are often used in loops and decision-making. The program counter (PC) is updated based on whether a condition is met.

6. **J-Type (Jump Instructions):**  
   Used for unconditional jumps to a specific target address or for function calls.  
   Examples include `JAL` (jump and link) and `JALR` (jump and link register). Unlike branch instructions, the PC is always updated to the target address.


### Architecture Overview

The design of the 64-bit pipelined RISC-V processor will follow these key architectural principles:

#### 1. Classic 5-Stage Pipeline
   - The processor will implement the classic **5-stage pipeline**:
     - **Fetch**
     - **Decode**
     - **Execute**
     - **Memory Access**
     - **Writeback**
   - This pipeline will be **in-order**.

#### 2. RV64I ISA Compliance
   - The processor will be fully compliant with the **RV64I** instruction set architecture.

#### 3. Separate Data and Control Paths
   - The architecture will separate the **data path** and **control path**, optimizing the handling of data and control signals.

#### 4. ALU Design
   - Instead of using a single **64-bit ALU**, the design will incorporate a **group of three 32-bit ALUs**. 
   - **ALU 1** will perform operations on the **lower 32 bits** of the operands.
   - The **carry** generated from ALU 1 will serve as a control signal, determining which of the remaining ALUs (ALU 2 or ALU 3) should handle the subsequent computation. Specifically:
     - If **carry = 0**, ALU 2 will be selected.
     - If **carry = 1**, ALU 3 will be selected.
   - This design improves the handling of 64-bit operations by dividing the computation across smaller ALUs, reducing complexity and potentially improving performance for specific instructions.

-----

In the next post, I’ll dive deeper into the design phase, focusing on how to translate the architectural concepts.

Have a good day!!