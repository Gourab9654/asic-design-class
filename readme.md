# ASIC Design Class
### Tools used
- **Software Tools:**
  - GCC (GNU Compiler Collection)
  - RISC-V GNU Compiler Toolchain
  - Spike RISC-V Simulator
  - Ubuntu OS
  - MakerChip IDE

<details>
<summary> Assignment 1</summary>
<br>
  
## GCC Compilation Of a simple C Program
### Step 1
* In the Linux Environment, create a new C Program file using any editor. Here the leafpad editor is used.
  1. **Code Snippet:**
    ```c
    #include <stdio.h>

    int main() {
        int i, n=5, sum=0;
        for(i=1; i<=n; i++){
          sum = sum + i;
        }
        printf("The sum from 1 to %d is %d\n", n, sum);
        return 0;
    }
    ```
* Save the program and compile your code in the terminal window using GCC compiler.
  
![Screenshot 2024-07-16 230409](https://github.com/user-attachments/assets/84f6f628-42a9-4a0d-8139-8605f1749a35)

### Step 2
Run the executable program and see the output in the terminal window.
### Output
The picture below represents the C code and its output

![Screenshot 2024-07-16 220224](https://github.com/user-attachments/assets/4dbde6dc-0ff0-43c4-92a3-2d6c839e3f7e)
</details>
<details>
<summary> Assignment 2</summary>
<br>

## RISC-V Compilation Of a simple C Program
### Step 1
1. **Code Snippet:**
    ```c
    #include <stdio.h>

    int main() {
        int i, n=5, sum=0;
        for(i=1; i<=n; i++){
          sum = sum + i;
        }
        printf("The sum from 1 to %d is %d\n", n, sum);
        return 0;
    }
    ```
* Compile the C code on RISC-V compiler
![Screenshot 2024-07-17 145045](https://github.com/user-attachments/assets/1440d896-d10e-4be8-84a3-2274caaeefb3)
* Now create the object file (.o) that is the output of the compiler as shown in the procedure shown below.
![Screenshot 2024-07-17 145313](https://github.com/user-attachments/assets/120bef44-0acf-4fcd-88b9-02b48cc71609)

### Step 2
* Run the executable program and see the output in the terminal window.
![Screenshot 2024-07-17 145751](https://github.com/user-attachments/assets/34bd3441-d2da-49b2-914b-6672252fda8e)

* For the "main" section, we can calculate the number of instructions either by counting each individual instruction or we can subtract the address of the first instruction in the next section with the first instruction of the main section and divide the difference with 4 since it is a byte addressable memory, so 4 memory block form one instruction
![Screenshot 2024-07-17 150053](https://github.com/user-attachments/assets/a2d28e2d-07e8-4aec-9f56-e94585f9cc2d)

* E.g.: No. of instruction in main block = (0x101C0 - 0x10184)/4 = 0x3C/4 = 0xF = 15 instructions
![Screenshot 2024-07-17 150210](https://github.com/user-attachments/assets/bec2004a-440f-4d5b-af8f-8b9d12efb600)
### Step 3
* Compile the code again with the compiler flag set as **-Ofast** and observe the generated assembly code
![Screenshot 2024-07-17 150613](https://github.com/user-attachments/assets/c808bb07-659a-44c3-b75f-1e51c5e27ea8)

* Assembly code generated with **-Ofast** compiler flag
![Screenshot 2024-07-17 150839](https://github.com/user-attachments/assets/33aa5b33-b0cf-4198-ac25-97d080ea8a75)
![Screenshot 2024-07-17 150920](https://github.com/user-attachments/assets/726b7516-90fa-45fb-ba2e-39ef6f1735f1)
### Observation
Here we can observe that the number of instructions are reduced to 12 as compared to 15 in the previous case
* -O1 is moderate in it's code optimization while -Ofast is highly aggressive to achieve highest possible performance
* -O1 maintains strict adherence to standards while -Ofast may violate some standards to achieve better performance
</details>
<details>
<summary> Assignment 3</summary>
<br>
  
##  RISC-V COMPILER OUTPUT and DEBUGGING

Find the output of the C program on the RISC V Compiler using the Spike command and debug the code

### Check the Output and compare with previous output.
In our previous lab, we compiled our C code using both gcc and a RISC-V compiler. In assignment 3 we are going to have find the result of n numbers on the RISCV compiler using the SPIKE command.

### Code for compiling the objdump file
```bash
spike pk sum1ton.o
```
### The compiled code using SPIKE command along with object dump file
![Similar result for GCC and RISCV compiler when run the program](https://github.com/user-attachments/assets/7a290394-20ed-4082-a01b-4ce624035b23)
* Here we can see that we get the same output in both the process.

![objectdump that is to be debugged](https://github.com/user-attachments/assets/4f80656e-beaa-4453-b348-0072af306c4a)

### Debug using SPIKE debugger

Code for debugging the assembly code 
```bash
spike -d pk sum1ton.o
```
We will allow the Spike debugger to run until the main function, specifically until the 100b0 instruction. After that, we will manually continue debugging and inspect the a0 register before and after the execution. We observe that the instruction lui a0, 0x21 updates the a0 register from 0x0000000000000001 to 0x0000000000021000
```bash
until pc 0 100bo
```
Next, we will manually debug the next instruction i.e., addi sp, sp, -16. This instruction decrements the stack pointer (sp) by 16. Before executing this instruction, the sp register held the value 0x0000003ffffffb50, which is then updated to 0x0000003ffffffb40.
```bash
reg 0 sp
```
In the assembly code we can see that the value of the stack pointer is being reduced by 10 in hexadecimal we is equivalent to being reduced by 16 in decimal notation.
![Screenshot 2024-07-21 213722](https://github.com/user-attachments/assets/48ca0385-d9e0-43f4-aa4f-d420becdc79d)

### The same thing happens for -O1 as well

```bash
until pc 0 10184
```

![Screenshot 2024-07-21 220856](https://github.com/user-attachments/assets/d852482f-7f0a-42ca-85f1-5c8de1839ed1)
![Screenshot 2024-07-21 220943](https://github.com/user-attachments/assets/abbe402d-6c31-436b-8d7e-cec5f1690be2)
</details>
<details>
<summary> Assignment 4 </summary>
<br>

## ASSIGNMENT 4
### 1.Identifying Instruction Types

* As the activity suggests, intruction types are being indentified for the instructions provided. The 32bit code is identified to do so. Each instruction type has it's own instruction format.

**What are instruction formats in RISCV?** 

Instruction formats can be considered as a 'contract' betwwen the assembly language and the hardware where, if the assembly language 'demands' to execute the instruction, the hardware knows exactly what to do with it. Therefore, there exists certain instructions and their respective format for the hardware to understand. They are made up of series of 0s and 1s depending upon their format, which includes the type of operation, location of data, etc.

There exists 6 types of instruction formats in RISCV.

* R type

    + 'R' here stands for register. 
    + This type inculcates all arithmetic and logical operations.
    +  They are used for operations that involve 3 registers.
    +  The format of R-type instructions is consistent and includes fields for specifying two source registers, one destination register, a function code to specify the operation, and an opcode.
    +  Examples: ADD, SUB, OR, XOR, etc.
    +  The instruction format is as follows: 
  
  ![rtype](https://github.com/user-attachments/assets/518e5b3b-046d-452b-90a4-4dc22a78accf)
    + funct7 (7 bits): Function code for additional instruction differentiation.
    + rs2 (5 bits): Second source register.
    + rs1 (5 bits): First source register.
    + funct3 (3 bits): Function code for primary instruction differentiation.
    + rd (5 bits): Destination register.
    + opcode (7 bits): Basic operation code for R-type instructions (0110011 for integer operations).
* I type

    + I-type instructions in the RISC-V architecture are used for operations that involve an immediate value along with one or two registers.
    +  These instructions typically perform operations such as arithmetic with immediate values, load operations, and certain branch instructions.
    +  The format of I-type instructions includes fields for a source register, destination register, an immediate value, a function code, and an opcode.
    +  The instruction format is as follows:
  
  ![itype](https://github.com/user-attachments/assets/044814e2-9f7b-453a-8541-d2650c4e15ab)
    + immediate (12 bits): Immediate value used for operations.
    + rs1 (5 bits): Source register.
    + funct3 (3 bits): Function code for instruction differentiation.
    + rd (5 bits): Destination register.
    + opcode (7 bits): Basic operation code for I-type instructions.
  
* S Type 
  
    + S-type instructions in the RISC-V architecture are used for store operations, where data is stored from a register into memory.
   + The format of S-type instructions includes fields for two source registers, an immediate value that determines the memory offset, a function code, and an opcode.
   +  The format is as follows: 

  ![stype](https://github.com/user-attachments/assets/6d2c61ec-2ab2-46f3-98a7-a351d43387a2)
    + imm[11:5] (7 bits): Upper 7 bits of the immediate value.
    + rs2 (5 bits): Second source register (contains the data to be stored).
    + rs1 (5 bits): First source register (base address register).
    + funct3 (3 bits): Function code for instruction differentiation.
    + imm[4:0] (5 bits): Lower 5 bits of the immediate value.
    + opcode (7 bits): Basic operation code for S-type instructions.

* B Type

    + B-type instructions in the RISC-V architecture are used for conditional branch operations.
    +  These instructions are designed to alter the flow of execution based on comparisons between two registers. 
    +  The format of B-type instructions includes fields for two source registers, an immediate value that determines the branch offset, a function code, and an opcode.
    +  Following is the instruction format:
    
     ![btype](https://github.com/user-attachments/assets/f8c4c251-9d0e-43bb-bc2d-5d62f6546a94)
    + imm[12] (1 bit): The 12th bit of the immediate value.
    + imm[10:5] (6 bits): The 10th to 5th bits of the immediate value.
    + rs2 (5 bits): Second source register.
    + rs1 (5 bits): First source register.
    + funct3 (3 bits): Function code for instruction differentiation.
    + imm[4:1] (4 bits): The 4th to 1st bits of the immediate value.
    + imm[11] (1 bit): The 11th bit of the immediate value.
    + opcode (7 bits): Basic operation code for B-type instructions

* U Type

    + U-type instructions in the RISC-V architecture are used for operations involving large immediate values, typically for loading upper immediate values or computing addresses.
    +  The format of U-type instructions includes fields for a destination register, a large immediate value, and an opcode.
    +  The instruction format is as follows:
  
     ![utype](https://github.com/user-attachments/assets/27da568d-cc13-49f7-b291-fe9bec1e0a0f)

    + immediate[31:12] (20 bits): The upper 20 bits of the immediate value.
    + rd (5 bits): Destination register.
    + opcode (7 bits): Operation code for U-type instructions.
    + The immediate value is stored in the upper 20 bits of a 32-bit word, with the lower 12 bits set to zero when used in calculations.

* J type
    + J-type instructions in the RISC-V architecture are used for jump operations, allowing for altering the program control flow by jumping to a specified address.
    + These instructions are typically used for unconditional jumps, like calling functions or implementing loops.
    + Following is the instruction format: 
     
    ![jtype](https://github.com/user-attachments/assets/d2b575bf-f23f-4064-8cce-015d24a712c4)
    + imm[20] (1 bit): The 20th bit of the immediate value.
    + imm[10:1] (10 bits): The 10th to 1st bits of the immediate value.
    + imm[11] (1 bit): The 11th bit of the immediate value.
    + imm[19:12] (8 bits): The 19th to 12th bits of the immediate value.
    + rd (5 bits): Destination register where the return address is stored.
    + opcode (7 bits): Operation code for J-type instructions.
   
  
  ### Decoding each instruction type provided: 

1. ```ADD r0, r1, r2```

    + Opcode for ADD = 0110011
    + rd = r0 = 00000
    + rs1 = r1 = 00001
    + rs2 = r2 = 00010
    + func3 = 000
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00010_00001_000_00000_0110011

2. ```SUB r2, r0, r1```

    + Opcode for SUB = 0110011
    + rd = r2 = 00010
    + rs1 = r0 = 00000
    + rs2 = r1 = 00001
    + func3 = 000
    + func7 = 0100000
    + **R Type**
    + 32 Bit Instruction: 0100000_00001_00000_000_00010_0110011

3. ```AND r1, r0, r2```

    + Opcode for AND = 0110011
    + rd = r1 = 00001
    + rs1 = r0 = 00000
    + rs2 = r2 = 00010
    + func3 = 111
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00010_00000_111_00001_0110011

4. ```OR r8, r1, r5```

    + Opcode for OR = 0110011
    + rd = r8 = 01000
    + rs1 = r1 = 00001
    + rs2 = r5 = 00101
    + func3 = 110
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00101_00001_110_01000_0110011

5. ```XOR r8, r0, r4```

    + Opcode for XOR = 0110011
    + rd = r8 = 01000
    + rs1 = r0 = 00000
    + rs2 = r4 = 00100
    + func3 = 100
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00100_00000_100_01000_0110011

6. ```SLT r0, r1, r4```

    + Opcode for SLT = 0110011
    + rd = r0 = 00000
    + rs1 = r1 = 00001
    + rs2 = r4 = 00100
    + func3 = 010
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00100_00001_010_00000_0110011

7. ```ADDI r2, r2, 5```

    + Opcode for ADDI = 0010011
    + rd = r2 = 00010
    + rs1 = r2 = 00010
    + imm = 000000000101
    + func3 = 000
    + **I Type**
    + 32 Bit Instruction: 000000000101_00010_000_00010_0010011

8. ```SW r2, r0, 4```

    + Opcode for SW = 0100011
    + rs1 = r0 = 00000
    + rs2 = r2 = 00010
    + imm = 0000000 0100
    + func3 = 010
    + **S Type**
    + 32 Bit Instruction: 0000000_00010_00000_010_00100_0100011

9. ```SRL r6, r1, r1```

    + Opcode for SRL = 0110011
    + rd = r6 = 00110
    + rs1 = r1 = 00001
    + rs2 = r1 = 00001
    + func3 = 101
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00001_00001_101_00110_0110011

10. ```BNE r0, r0, 20```

    + Opcode for BNE = 1100011
    + rs1 = r0 = 00000
    + rs2 = r0 = 00000
    + imm = 000000 001010
    + func3 = 001
    + **B Type**
    + 32 Bit Instruction: 0000000_00000_00000_001_01010_1100011

11. ```BEQ r0, r0, 15```

    + Opcode for BEQ = 1100011
    + rs1 = r0 = 00000
    + rs2 = r0 = 00000
    + imm = 000000 001111
    + func3 = 000
    + **B Type**
    + 32 Bit Instruction: 0000000_00000_00000_000_01111_1100011

12. ```LW r3, r1, 2```

    + Opcode for LW = 0000011
    + rd = r3 = 00011
    + rs1 = r1 = 00001
    + imm = 000000000010
    + func3 = 010
    + **I Type**
    + 32 Bit Instruction: 000000000010_00001_010_00011_0000011

13. ```SLL r5, r1, r1```

    + Opcode for SLL = 0110011
    + rd = r5 = 00101
    + rs1 = r1 = 00001
    + rs2 = r1 = 00001
    + func3 = 001
    + func7 = 0000000
    + **R Type**
    + 32 Bit Instruction: 0000000_00001_00001_001_00101_0110011
  
| Instruction       | Type | 32-bit Representation                 |
|-------------------|------|---------------------------------------|
| ADD r0, r1, r2    | R    | 0000000_00010_00001_000_00000_0110011 |
| SUB r2, r0, r1    | R    | 0100000_00001_00000_000_00010_0110011 |
| AND r1, r0, r2    | R    | 0000000_00010_00000_111_00001_0110011 |
| OR r8, r1, r5     | R    | 0000000_00101_00001_110_01000_0110011 |
| XOR r8, r0, r4    | R    | 0000000_00100_00000_100_01000_0110011 |
| SLT r00, r1, r4   | R    | 0000000_00100_00001_010_00000_0110011 |
| ADDI r02, r2, 5   | I    | 000000000101_00010_000_00010_0010011 |
| SW r2, r0, 4      | S    | 0000000_00010_00000_010_00100_0100011 |
| SRL r06, r01, r1  | R    | 0000000_00001_00001_101_00110_0110011 |
| BNE r0, r0, 20    | B    | 0000000_00000_00000_001_01010_1100011 |
| BEQ r0, r0, 15    | B    | 0000000_00000_00000_000_01111_1100011 |
| LW r03, r01, 2    | I    | 000000000010_00001_010_00011_0000011 |
| SLL r05, r01, r1  | R    | 0000000_00001_00001_001_00101_0110011 |

| Assembly Instruction | Hexadecimal Representation |
|----------------------|----------------------------|
| ADD r0, r1, r2       | 0x00208033                 |
| SUB r2, r0, r1       | 0x40100033                 |
| AND r1, r0, r2       | 0x00207833                 |
| OR r8, r1, r5        | 0x0050C833                 |
| XOR r8, r0, r4       | 0x00408433                 |
| SLT r00, r1, r4      | 0x00402833                 |
| ADDI r02, r2, 5      | 0x00510093                 |
| SW r2, r0, 4         | 0x002020A3                 |
| SRL r06, r01, r1     | 0x0010A833                 |
| BNE r0, r0, 20       | 0x00012A63                 |
| BEQ r0, r0, 15       | 0x00000F63                 |
| LW r03, r01, 2       | 0x00210883                 |
| SLL r05, r01, r1     | 0x00109233                 |

There are some differences between RISCV ISA and Hardcoded ISA.So for the above Instructions the difference between RISCV ISA and Hardcoded ISA is
| Operation           | Standard RISC-V ISA | Standard RISC-V ISA (Binary)               | Hardcoded ISA     | Hardcoded ISA (Binary)                  |
|---------------------|---------------------|--------------------------------------------|-------------------|-----------------------------------------|
| ADD R6, R2, R1      | 32'h00110333        | 000000000001 00010 000 00110 0110011       | 32'h02208300      | 000000100010 00000 100 00000 01100000  |
| SUB R7, R1, R2      | 32'h402083b3        | 010000000010 00000 000 00111 0110011       | 32'h02209380      | 000000100010 01000 100 10000 01100000  |
| AND R8, R1, R3      | 32'h0030f433        | 000000000011 00000 111 01000 0110011       | 32'h0230a400      | 000000100011 01000 101 00100 01100000  |
| OR R9, R2, R5       | 32'h005164b3        | 000000000101 00010 110 01001 0110011       | 32'h02513480      | 000000100101 00010 110 10100 01100000  |
| XOR R10, R1, R4     | 32'h0040c533        | 000000000100 00000 100 01010 0110011       | 32'h0240c500      | 000000100100 00000 100 11000 01100000  |
| SLT R1, R2, R4      | 32'h0045a0b3        | 000000000100 00010 010 00001 0110011       | 32'h02415580      | 000000100100 00010 101 01010 01100000  |
| ADDI R12, R4, 5     | 32'h004120b3        | 000000000101 00010 010 01100 0010011       | 32'h00520600      | 000000100100 00010 010 00010 01100000  |
| BEQ R0, R0, 15      | 32'h00000f63        | 000000000000 00000 000 00000 1100011       | 32'h00f00002      | 000000000000 00000 000 00000 11000000  |
| SW R3, R1, 2        | 32'h0030a123        | 000000000011 00000 010 00001 0100011       | 32'h00209181      | 000000100010 00000 010 00001 01100000  |
| LW R13, R1, 2       | 32'h0020a683        | 000000000010 00000 010 01101 0000011       | 32'h00208681      | 000000100010 00000 010 01100 01100000  |
| SRL R16, R14, R2    | 32'h0030a123        | 000000000011 00000 010 00001 0100011       | 32'h00271803      | 000000100010 00000 011 00110 01100000  |
| SLL R15, R1, R2     | 32'h002097b3        | 000000000010 00000 111 01111 0110011       | 32'h00208783      | 000000100010 00000 111 01111 01100000  |
### 2. Identifying Instruction Types
A.The given hardcoded instructions are:

![Screenshot 2024-07-29 120125](https://github.com/user-attachments/assets/7c95b29a-40ca-4ce3-a8a9-fd8858bbfb79)

`ADD R6, R2, R1`

This is the waveform of the given hardcoded verilog program:

![ADD](https://github.com/user-attachments/assets/bf8c2366-2cdd-409e-912e-9f8326e8bd87)

This is the waveform of our verilog program:


`SUB R7, R1, R2`

This is the waveform of the given hardcoded verilog program:

![SUB](https://github.com/user-attachments/assets/facff573-4992-4bbe-96f2-11f9128ada45)

This is the waveform of our verilog program:


`AND R8, R1, R3`

 This is the waveform of the given hardcoded verilog program:

![AND](https://github.com/user-attachments/assets/5cf420f5-1bfa-4b9f-9133-415dfc7baf21)

 This is the waveform of our verilog program:



 `OR R9, R2, R5`

  This is the waveform of the given hardcoded verilog program:

  ![OR](https://github.com/user-attachments/assets/f191347d-1866-4e74-aac2-2d4558114f18)

  This is the waveform of our verilog program:


  `XOR r10, r1, r4`

  This is the waveform of the given hardcoded verilog program:

![XOR](https://github.com/user-attachments/assets/defbe708-cd86-4de6-9f72-20c31d26ec06)

  This is the waveform of our verilog program:

 

  `SLT r1, r2, r4 `

  This is the waveform of the given hardcoded verilog program:

![SLT](https://github.com/user-attachments/assets/e21bc999-f45c-4713-b9a9-91cb2894fe93)  

  This is the waveform of our verilog program:


  `ADDI r12, r4, 5 `

  This is the waveform of the given hardcoded verilog program:

![ADDI](https://github.com/user-attachments/assets/9c599bde-01e6-4cc9-a138-ee22457a89b6)

  This is the waveform of our verilog program:



  `SW r3, r1, 2`

   This is the waveform of the given hardcoded verilog program:

![SW](https://github.com/user-attachments/assets/c7dc6c31-3e07-4ce2-880b-cffa18b32adf)

   This is the waveform of our verilog program:



   `LW r13, r01, 2`

![LW](https://github.com/user-attachments/assets/1640b1a1-7693-453c-9338-dd2a6e589428)

   This is the waveform of our verilog program:

  

   `BEQ r0, r0, 15`

   ![BEQ](https://github.com/user-attachments/assets/2868d2aa-09a0-4670-96f5-2535efdb9e6f)

   This is the waveform of our verilog program:

B.The given custom instructions are:

![Screenshot 2024-07-29 142902](https://github.com/user-attachments/assets/590e675d-343e-4223-95dc-e0f27be14af7)

`ADD R0, R1, R2`

This is the waveform of the given hardcoded verilog program:

![add](https://github.com/user-attachments/assets/ab6d346b-e69f-42a3-8719-bf7f07a66140)

This is the waveform of our verilog program:


`SUB R2, R0, R1`

This is the waveform of the given hardcoded verilog program:

![sub](https://github.com/user-attachments/assets/4e54ff97-2aa1-4884-b334-996edff4ac7c)

This is the waveform of our verilog program:


`AND R1, R0, R2`

 This is the waveform of the given hardcoded verilog program:

![and](https://github.com/user-attachments/assets/781c3125-a6aa-4881-b8c3-dd9e0475b5b8)

 This is the waveform of our verilog program:



 `OR R8, R1, R5`

  This is the waveform of the given hardcoded verilog program:

![or](https://github.com/user-attachments/assets/6f9c4a18-d244-4b8f-8213-723d775011a2)

  This is the waveform of our verilog program:


  `XOR r8, r0, r4`

  This is the waveform of the given hardcoded verilog program:

![xor](https://github.com/user-attachments/assets/3c520ef7-8a64-4a2b-9c60-7c36511f9953)

  This is the waveform of our verilog program:

 

  `SLT r00, r1, r4 `

  This is the waveform of the given hardcoded verilog program:

  ![slt](https://github.com/user-attachments/assets/10667b1b-95ee-4368-8d55-d037c24ea465)

  This is the waveform of our verilog program:


  `ADDI r2, r2, 5 `

  This is the waveform of the given hardcoded verilog program:

![ADDI](https://github.com/user-attachments/assets/4de3ad07-947e-4d7d-a535-0a738b8f2c36)

  This is the waveform of our verilog program:



  `SW r2, r0, 4`

   This is the waveform of the given hardcoded verilog program:

![SW](https://github.com/user-attachments/assets/5845f1b1-0f53-4749-b7ba-181f07faf9a5)

   This is the waveform of our verilog program:



   `LW r03, r01, 2`

![lw](https://github.com/user-attachments/assets/999490c3-2e8f-47bd-986e-1f0c0e5113a0)

   This is the waveform of our verilog program:

  

   `BEQ r0, r0, 15`

   

![BEQ](https://github.com/user-attachments/assets/ae0dd3ed-e09c-468e-a3c3-a81e410d822a)

   This is the waveform of our verilog program:
</details>
<details>
<summary> Assignment 5</summary>
<br>

## Compile C application with GCC and RISC-V GCC
### **Application Name** - Binary To Decimal Converter
1. **Code Snippet:**
    ```c
    #include <stdio.h>
   #include <string.h>
   #include <math.h>
   int binaryToDecimal(const char *binary) {
    int decimal = 0;
    int length = strlen(binary);

    for (int i = 0; i < length; i++) {
        if (binary[i] == '1') {
            decimal += 1 << (length - 1 - i); 
        }
    }

    return decimal;
   }

   int main() {
    char binary[65];  
    printf("Enter a binary number: ");
    scanf("%64s", binary);  

    int decimal = binaryToDecimal(binary);
    printf("The decimal equivalent of %s is %d\n", binary, decimal);

    return 0;
   }

    ```


2. **Compilation of the program with GCC compiler**
![Screenshot 2024-08-13 210507](https://github.com/user-attachments/assets/bbb9deb3-8111-4358-b98e-30632b86d248)


3. **Compilation of the Program with RISC-V Compiler**
![Screenshot 2024-08-13 211320](https://github.com/user-attachments/assets/1c9cb625-28e0-4266-8cf8-8f2d7f655809)

4. **Creating the Objdump file**
![Screenshot 2024-08-14 110030](https://github.com/user-attachments/assets/b684d855-c008-466d-b84b-73b53a57994a)

![Screenshot 2024-08-14 105945](https://github.com/user-attachments/assets/f237132f-da25-45f1-bb3a-723849205cfc)

</details>
<details>
<summary> Assignment 6</summary>
<br>

**Introduction to TL-Verilog and Makerchip:**
Makerchip supports the Transaction-Level Verilog (TL-Verilog) standard, which represents a significant advancement by removing the need for the legacy features of traditional Verilog and introducing a more streamlined syntax. TL-Verilog enhances design efficiency by adding powerful constructs for pipelines and transactions, making it easier to develop complex digital circuits.

## Combinational Circuits in TL-Verilog
![Screenshot 2024-08-19 183054](https://github.com/user-attachments/assets/36b3aec3-a78c-4080-8ab6-988590695d7d)
![Screenshot 2024-08-19 183101](https://github.com/user-attachments/assets/27f0ac73-2684-481f-b8a5-828bae0edcc4)

### inverter
![Screenshot 2024-08-19 181713](https://github.com/user-attachments/assets/f6c32b6e-6142-48da-a821-336332a5b0c4)
### 2 input AND gate
![Screenshot 2024-08-19 182051](https://github.com/user-attachments/assets/6546a442-6413-491b-beec-8d755c0f8799)
### 2 input OR gate
![Screenshot 2024-08-19 182250](https://github.com/user-attachments/assets/7db4e4e2-a1f7-4708-ba7a-02ba05c4ae0f)
### 2:1 MUX
![Screenshot 2024-08-19 182401](https://github.com/user-attachments/assets/9238fc16-a219-4dd0-9ef3-d9f016f99d1a)
### 2:1 MUX using Vectors
![Screenshot 2024-08-19 182501](https://github.com/user-attachments/assets/d616ae66-0769-4a10-889c-b1823a115332)

## Sequential Circuits in TL-Verilog
### Sequential Calculator
This gives us the following waveforms and block diagram:-
![sequential calc](https://github.com/user-attachments/assets/01a5a7ab-6f8f-468b-badb-36ad7c085c7b)
## Pipelined Logic:-
The output for the following code is as follows:-
![pipelined logic](https://github.com/user-attachments/assets/6b3728e4-6aee-4a13-91d0-47664c4d15ba)
### Cycle Calculator
The output for the cycle calculator is as follows:-
![cycle calculator](https://github.com/user-attachments/assets/9cbe36a6-6f96-4770-a1aa-61fa5a8616d9)
## Validity
When we generate a waveform as in all the previous cases we are receiving a result for all the clock cycles. Here there are no compilation errors but it is quite possible that logical errors can be present in these cases. These errors will be ignored during compile time and it will be difficult to debug them by simply looking at the waveforms. Also there might be certain cases where a dont care condition comes up. These cases are insignificant to us and thus should be neglected . In order to do so we use the Validity. The global clock is also running all the time. There might be instances in our code when we do not need a particular case to run but still does as the clock triggers it. In order to execute a clock physically voltage or current sources are used. These sources use some power during that clock cycle. In complex circuits if such cases are ignored a lot of power will be wasted. So in order to reduce power consumption we remove the clock during such cycles and this process is called as clock gating. The validity helps us with this.
The output for the following code is as follows:-

![validity on cycle calculator](https://github.com/user-attachments/assets/ee77ba38-6e1a-483b-9b38-c63dcf1033ba)

### Total Distance Calculator
The output for the above code is as follows:-
![total distance](https://github.com/user-attachments/assets/a6a5dd73-ae55-47c9-ad2a-f9f752462087)

### Validity on Cycle Calculator
The output is as follows:-
![validity on cycle calculator](https://github.com/user-attachments/assets/ee77ba38-6e1a-483b-9b38-c63dcf1033ba)


## Basic RISC V architecture

![basic risc-v architecture](https://github.com/user-attachments/assets/712abd37-acee-41bc-b65c-9f6a605aa0f0)


  
### Program Counter

The program counter is supposed to increase its value by 4 to fetch the next instruction from the memory. The below image specifies the same. In case a reset is triggered the program counter will be initialised to zero for the next instruction.

The following diagram explains the working of the program counter

![program counter](https://github.com/user-attachments/assets/37196a54-a585-41e3-a8c7-922741e6ffe7)

The following is the code for the working of the program counter

```bash
$pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );
```
We get the following output after executing the code:-

![image](https://github.com/user-attachments/assets/9eef0a38-71d1-4fa4-bbf6-aa98283bc534)


### Adding the instruction memory

The program counter points to the next address where the instruction is present in the instruction memory. We need to fetch this instruction in order to process it and make further calculations.

![adding to instruction memory](https://github.com/user-attachments/assets/63eb64d5-3bba-4d9b-ba93-1d3940cfa1a2)
![fetch](https://github.com/user-attachments/assets/1f17d4be-a45f-4ed8-b8cc-4a5e3ad9b765)
```bash
$imem_rd_en = >>1$reset ? 0 : 1;
$imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
$instr[31:0] = $imem_rd_data[31:0];
```



### Decoding the instruction

We have decoded the instruction on the basis of all the 6 types of RISC V instruction set. The code for decoding is as follows:-

```bash
$is_i_instr = $instr[6:2] ==? 5'b0000x ||
              $instr[6:2] ==? 5'b001x0 ||
              $instr[6:2] ==? 5'b11001;
$is_r_instr = $instr[6:2] ==? 5'b01011 ||
              $instr[6:2] ==? 5'b011x0 ||
              $instr[6:2] ==? 5'b10100;
$is_s_instr = $instr[6:2] ==? 5'b0100x;
$is_b_instr = $instr[6:2] ==? 5'b11000;
$is_j_instr = $instr[6:2] ==? 5'b11011;
$is_u_instr = $instr[6:2] ==? 5'b0x101;
```

![decode](https://github.com/user-attachments/assets/e32df76c-3fd0-4f74-b3cc-57be82c5791c)
### Immediate Decode Logic

The instruction sets have an immediate field. In order to decoder this field we use the following code:-

```bash
$imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                      32'b0;
```



### Decode logic for other fields

Apart from the immediate we have other fields which also need to be decoded. The code for the same is as follows:-

```bash
$rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
```
At a time only one instruction is passed on to for decode. This instruction can be of any 1 of the 6 instruction set types. Thus we need to validate that it belongs to the respective category or else there may be a clash of different instruction set types.


### Decoding Individual Instructions

We are decoding the individual instructions using the following code

```bash
$dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
$is_beq = $dec_bits ==? 11'bx_000_1100011;
$is_bne = $dec_bits ==? 11'bx_001_1100011;
$is_blt = $dec_bits ==? 11'bx_100_1100011;
$is_bge = $dec_bits ==? 11'bx_101_1100011;
$is_bltu = $dec_bits ==? 11'bx_110_1100011;
$is_bgeu = $dec_bits ==? 11'bx_111_1100011;
$is_addi = $dec_bits ==? 11'bx_000_0010011;
$is_add = $dec_bits ==? 11'b0_000_0110011;
```



### Register File Read and Enable

Here we read the instructions from the respective instruction memory and store it in the registers. We have 2 register slots the read the instructions from the memory. We send these stored instructions to the ALU after this process.

The code is as follows:-

```bash
$rf_rd_en1 = $rs1_valid;
$rf_rd_index1[4:0] = $rs1;
$rf_rd_en2 = $rs2_valid;
$rf_rd_index2[4:0] = $rs2;

$src1_value[31:0] = $rf_rd_data1;
$src2_value[31:0] = $rf_rd_data2;
```
![Register read and write](https://github.com/user-attachments/assets/a8ca4f26-7447-4106-b966-fb78e7bd3931)

### Arithmetic and Logic Unit

Used to perform arithmetic operations on the values stored in the registers. The code for the same is as follows:-

```bash
$result[31:0] = $is_addi ? $src1_value + $imm :
                $is_add ? $src1_value + $src2_value :
                32'bx ;
```

Here we have written code for the addi and add operation.


### Register File Write

Once the ALU performs the operations on the values stored in ther registers we may need to put these values back into these registers based. For this we use the register file write. We also have to make sure that we should not write into the register if the destination register is x0 as it is always meant to be 0. The code is as follows:-

```bash
$rf_wr_en = $rd_valid && $rd != 5'b0;
$rf_wr_index[4:0] = $rd;
$rf_wr_data[31:0] = $result;
```


### Branch instructions

Based on the control input we may need to jump to some different address after a particular instruction based on some condition generated during run-time. This is when we use the branch instructions. The code is as follows:-

```bash
$taken_branch = $is_beq ? ($src1_value == $src2_value):
	        $is_bne ? ($src1_value != $src2_value):
	        $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
	        $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bltu ? ($src1_value < $src2_value):
                $is_bgeu ? ($src1_value >= $src2_value):
	        1'b0;
$br_target_pc[31:0] = $pc +$imm;
```
![Branch Logic](https://github.com/user-attachments/assets/c9725498-e388-4e7f-9f3d-455ea8510e02)


## RISC-V CPU CORE WITHOUT PIPELINE
![Screenshot 2024-08-21 125355](https://github.com/user-attachments/assets/e6f46a89-fe5f-4be3-98f7-1f0292c8db67)
```bash
\m4_TLV_version 1d: tl-x.org
\SV
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module
\TLV

   m4_asm(ADD, r10, r0, r0)
   m4_asm(ADD, r14, r10, r0)
   m4_asm(ADDI, r12, r10, 1010)
   m4_asm(ADD, r13, r10, r0)
   m4_asm(ADD, r14, r13, r14)
   m4_asm(ADDI, r13, r13, 1)
   m4_asm(BLT, r13, r12, 1111111111000)
   m4_asm(ADD, r10, r14, r0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_gour = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>1$taken_branch ? >>1$br_target_pc :
                     >>1$pc + 32'd4;
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         $opcode[6:0] = $instr[6:0];
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         $funct7_valid = $is_r_instr;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         $br_target_pc[31:0] = $pc +$imm;
         *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9);
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   |cpu
      m4+imem(@1)
      m4+rf(@1, @1)
   m4+cpu_viz(@4)
\SV
   endmodule
```
## RISC-V CPU CORE WITH PIPELINE
```bash
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 0 to 9 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_GOUR = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```


![Screenshot 2024-08-21 130349](https://github.com/user-attachments/assets/3d7c1b0a-1748-4fb5-8142-931e9e7dfe97)

**The Sum  accumulated in R14 register completed in 58 cycles**
![Screenshot 2024-08-26 160915](https://github.com/user-attachments/assets/ff2dcb0f-4334-41f4-9691-9ea38d138e3a)
**clock as clk_gour**
![Screenshot 2024-08-22 105043](https://github.com/user-attachments/assets/f4cd9bae-d1b5-441b-afb0-5c840f1e117c)


</details>
<details>
<summary> Assignment 7</summary>
<br>

## Conversion from TLV into Verilog using Sandpiper-SaaS compiler.Following the conversion, pre-synthesis simulations will be conducted using the GTKWave simulator to verify the design.

### Step-by-Step Procedure:

1. **Install Required Packages:**
Begin by installing the necessary packages using pip:
```bash
pip3 install pyyaml click sandpiper-saas
```
2. **Clone the github repo:** 
clone this repo containing VSDBabySoC design files and testbench. Move into the VSDBabySoc directory
```bash
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoc
```

3. **Replace the rvmyth.tlv file in the VSDBabySoC Directory:** 
replace in src/module with the rvmth.tlv.

4. **Convert .tlv to .v using converter:**
Now we have written the code in TL-Verilog .tlv which is a high level language and we want to convert into low level verilog that is to translate .tlv definition of rvmyth into .v definition. To do so Run the following command as follows

```bash
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

4. **Make the pre_synth_sim.vcd:**
We will create the pre_synth_sim.vcd by running the following command
```bash
make pre_synth_sim
```
The result of the simulation i.e the pre_synth_sim.vcd will be stored in the output/pre_synth_sim directory


5 .**Now to compile and simulate RISC-V design run the following code:**
To compile and simulate vsdbabysoc design.

```bash
iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
cd output
./pre_synth_sim.out
```
To generate pre_synth_sim.vcd file,which is our simulation waveform file.


6. **To open the Simulation file in gtkwave tool:**
To do so run the follwowing command 
```bash
gtkwave pre_synth_sim.vcd
```
The following diagram contains:-
- clk_Gour: This is the clock input to the RISC-V core.
- reset: This is the input reset signal to the RISC-V core.
- OUT[9:0]: This is the 10-bit output [9:0] OUT port of the RISC-V core. This port comes from the RISC-V register #14, originally.

![Screenshot 2024-08-26 160001](https://github.com/user-attachments/assets/c1757704-c42b-4463-97f2-10d09434f87e)

</details>
