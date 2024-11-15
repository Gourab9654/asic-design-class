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
![Screenshot 2024-08-26 181442](https://github.com/user-attachments/assets/d1b5a147-9f76-4c50-a682-fd8bbec84a2c)
The following diagram contains:-
- clk_Gour: This is the clock input to the RISC-V core.
- reset: This is the input reset signal to the RISC-V core.
- OUT[9:0]: This is the 10-bit output [9:0] OUT port of the RISC-V core, coming from RISC-V register #14, originally.

![Screenshot 2024-08-26 160001](https://github.com/user-attachments/assets/c1757704-c42b-4463-97f2-10d09434f87e)

TLV waveforms as generated from MakerChip IDE
![Screenshot 2024-08-26 160915](https://github.com/user-attachments/assets/12901fbf-ab42-45e7-b0ec-31279ed24fcd)
</details>
<details>
<summary> Assignment 8</summary>
<br>

## Addition of Peripherals to convert the Digital output to analog output using DAC and PLL

 In this assignment we are adding two peripherals to convert the digital output to the analog output namely PLL and DAC. 
 
 - **Phase locked loop:-** The crystal oscillator present on the board is capable of giving a clock of frequency between 12 - 20 MHZ. The processor operates at frequency near 100MHZ and thus we need an IP/Peripheral to convert this low frequency clock to a high frequency clock. Here the PLL comes into picture. The input to the PLL is the crystal oscillator clock and returns a high frequency clock to our risc v core. This clock is then appended by my name CPU_clk_GOUR_a0.
 - **Digital to Analog Converter:-** The processor works with digital input but we transmit or receive signals in analog form. So in order to convert the digital signal in our risc v core to analog signal we are using the digital to analog converter IP.

Commands used to run the rvmyth.v file

```bash
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
```

After this we dump the ./pre_synth_sim.out file to create the .vcd file using the following command

```bash
./pre_synth_sim.out
```

We then run this .vcd file on gtkwave to observe the output

```bash
gtkwave pre_synth_sim.vcd
```

The above process has been executed by me in the following way

![Screenshot from 2024-09-01 14-36-34](https://github.com/user-attachments/assets/d8e12fb0-66f8-494f-8320-67cf5cf0b868)



The output of the above code is as follows:-

![Screenshot from 2024-09-01 16-55-34](https://github.com/user-attachments/assets/66641f77-e2ca-4c8c-9051-d6f9954d2e0c)






</details>
<details>
<summary> Assignment 9</summary>
	
## RTL design using Verilog with SKY130 Technology
Simulator is a tool used to check if it adheres to the designed specifications by simualating the code. Simulator looks for the changes on the input signals and upon change to the input the output is evaluated. RTL design is the Verilog code that implements a circuit. To verify it, a testbench is written and simulated using Icarus Verilog. The VCD(Value Change Dump) file generated is viewed using GTKWave to debug and verify the design's functionality. GTKWave allows users to load and inspect waveforms generated during the simulation, helping them understand signal interactions, timing relationships, and overall circuit behavior.

The below is the Iverilog based Simulation Flow

![image](https://github.com/user-attachments/assets/7da43121-8525-4e69-9543-694f9b843260)
 <details>
	  <summary>Initial Setup</summary>
	 Enter the following commands in the Ubuntu terminal as depicted in the screenshot
	  
```
sudo -i
sudo apt-get install git
ls
cd /home/gourab
mkdir VLSI
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
ls
```

We can observe the list of files present in the directory. 

![Screenshot from 2024-10-19 14-43-24](https://github.com/user-attachments/assets/b22d3e89-762e-4390-9841-2cd28ff3aeaf)

</details>
 <details>
	  <summary>Day 1: Installing the files, running the RTL Design in iverilog & GTKWave and thereby converting into netlist using Yosys synthesizer</summary>
		  
  <li>
	  Introduction to iverilog and GTKWave: This tutorial involved learning about how to simulate the design and testbench for a 2x1 multiplexer, using iverilog, and displaying the waveform on GTKWave.
	  
```
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
![Screenshot from 2024-10-19 14-56-59](https://github.com/user-attachments/assets/67bf13c1-5d60-4625-a3c9-c37e06e14dfb)
   	  
![Screenshot from 2024-10-19 14-58-00](https://github.com/user-attachments/assets/c198a352-5b68-489b-a601-e30fbbdf9f7b)

  ```

  //Design 
  module good_mux (input i0, input i1, input sel, output reg y);
	  always@(*)
	  begin
	  	if(sel)
			y<=i1;
		else
			y<=i0;
	  end
  endmodule
  //Testbench
  module tb_good_mux;
	reg i0,i1,sel;
	wire y;

     	good_mux uut(.sel(sel),.i0(i0),.i1(i1),.y(y));

	initial begin
		$dumpfile("tb_good_mux.vcd");
		$dumpvars(0,tb_good_mux);
		sel=0;
		i0=0;
		i1=0;
		#300 $finish;
	end
	always #75 sel = ~sel;
	always #10 i0 = ~i0;
	always #55 i1 = ~i1;
  endmodule
  ```
  </li>
  <li>
	  Introduction to Yosys: This tutorial involved the use of Yosys for synthesising the design we created in Verilog, viewing its netlists and the cells that are generated for the purpose of creating the circuit. The following commands are used:

   ```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr good_mux_netlist.v
!gvim good_mux_netlist.v
  ```

1. Opens Yosys Tool
2. Reads the technology library file (Liberty format) required for synthesis using the specified path.
3. Loads the Verilog file good_mux.v for synthesis.
4. Performs synthesis on the design, with good_mux as the top module.
5. Optimizes the synthesized design using the ABC tool and the specified technology library.
6. Displays the synthesized design as a schematic.
7. Writes the synthesized netlist to the file good_mux_netlist.v without attributes.
8. Opens the netlist file good_mux_netlist.v in the gvim text editor.

```
//Generated Netlist
module good_mux(i0, il, sel, y);
	wire _0_;
	wire _1_;
	wire _2_;
	wire_3_;
	input i0; wire i0;
	input il; wire il;
	input sel; wire sel;
	output y; wire y;
	
	sky130_fd_sc_hd__mux2_1 _4_ (.AO(_0_),.A1(_1_),.S(_2_),.X(_3_));
	
	assign_0_ = 10;
	assign 1 = il;
	assign 2 = sel;
	assign y = _3_;
endmodule
```

![Screenshot from 2024-10-19 15-11-28](https://github.com/user-attachments/assets/c27eb3e6-1444-4c5e-ad1f-ac3a8bde3f47)

![Screenshot from 2024-10-19 15-12-17](https://github.com/user-attachments/assets/9c5b47dd-af15-491c-ac25-51cb8810ebcc)





![Screenshot from 2024-10-19 15-14-35](https://github.com/user-attachments/assets/7909d585-d794-4345-99e8-6cede3fde17d)



![Screenshot from 2024-10-19 15-18-07](https://github.com/user-attachments/assets/e841cb82-7268-4181-b72f-892a875b882d)

![Screenshot from 2024-10-19 16-53-54](https://github.com/user-attachments/assets/baa7d5fd-d72c-4e7e-bdfa-ea8de5454bb5)

![Screenshot from 2024-10-19 16-06-51](https://github.com/user-attachments/assets/f1d2674f-f14f-4f58-ae7f-25b2a056097e)

![Screenshot from 2024-10-19 16-24-46](https://github.com/user-attachments/assets/78d46533-dab0-4882-a153-a9037d20b415)
  </li>
  
  </details>

  <details>
	  <summary>Day 2: Hierarchial vs Flat Synthesis, Flop coding styles and optimisation</summary>
  
<li>
	   Yosys Synthesis for Multiple Modules: This tutorial involved the synthesis of a design file that has more than one module.

```
//Design

module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule

module multiple_modules (input a, input b, input c, output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1)); //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y)); //y = netic,ie y = a&b + c;
endmodule
```

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog multiple_modules.v
4. synth -top multiple_modules
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. show
7. write_verilog -noattr multiple_modules_netlist.v
8. !gvim multiple_modules_netlist.v
```

   </li>

```
//Generated Netlist
module multiple_modules (a, b, c, y);
	input a; wire a;
	input b; wire b;
	input c; wire c;
	wire net1;
	output y; wire y;

	sub_modulel ul (.a(a),.b(b),.y(net1));
	sub_module2 u2 (.a(net1),.b(c),.y (y));
endmodule

module sub_modulel (a, b, y);
	wire _0_;
	wire _1_;
	wire _2_;
	input a; wire a;
	input b; wire b;
	output y; wire y;
	
	sky130_fd_sc_hd_and2_0_3_(.A(_1_),.B(_0_),.X(_2_));
	
	assign _1_ = b;
	assign _0_ = a;
	assign y = _2_;
endmodule

module sub_module2 (a, b, y);
	wire _0_;
	wire _1_;
	wire _2_;
	input a; wire a;
	input b; wire b;
	output y;wire y;

	sky130_fd_sc_hd_or2_0_3_ (A(_1_), .B( 0 ), .X( 2 ));
	assign _1_ = b;
	assign _0_ = a;
	assign y = _2_;
endmodule
```


![Screenshot from 2024-10-20 00-18-44](https://github.com/user-attachments/assets/1358819b-aa3a-49df-ac14-772e20a18157)

![Screenshot from 2024-10-20 00-19-44](https://github.com/user-attachments/assets/fcc9546e-1421-44ec-97a8-da41347f4e18)

![Screenshot from 2024-10-20 10-56-11](https://github.com/user-attachments/assets/b8894646-2dd2-4218-b0ae-a6480900cc99)

![Screenshot from 2024-10-20 00-20-21](https://github.com/user-attachments/assets/66245a17-892a-4221-8c86-9ca2b96e7b12)

![Screenshot from 2024-10-20 00-20-57](https://github.com/user-attachments/assets/4cefbd3b-1699-4b86-bb00-7202994a090b)

![Screenshot from 2024-10-20 00-07-15](https://github.com/user-attachments/assets/bb638174-2d5a-4bc0-a7a6-217cc9232c18)
<li>
	Use of Module Level Synthesis: This method is preferred when multiple instances of same module are used. The synthesis is carried out once and is replicate multiple times, and the multiple instances of the same module are stitched together in the top module. This method is helpful when making use of divide and conquer algorithm

 ```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog multiple_modules.v
4. synth -top sub_module1
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. show
```
![Screenshot from 2024-10-20 00-21-53](https://github.com/user-attachments/assets/928fe246-1a6d-4ea1-9292-3c219723027d)
</li>

<li>
	Use of Flattening: Merges all hierarchical modules in the design into a single module to create a flat netlist.
 ```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog multiple_modules.v
4. synth -top multiple_modules
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. flatten
7. show
8. write_verilog -noattr multiple_modules_netlist.v
9. gvim multiple_modules_netlist.v
```

```
//Generated Netlist
module multiple_modules (a, b, c, y);
	wire _0_; wire _1_;
	wire _2_; wire _3_;
	wire _4_; wire _5_;
	input a; wire a;
	input b; wire b;
	input c; wire c;
	wire net1;
	wire \ul.a;
	wire \ul.b;
	wire \ul.y;
	wire \u2.a;
	wire \u2.b;
	wire \u2.y;
	output y; wire y;
	
	sky130_fd_sc_hd_and2_0 _6_ (.A(1),.B(0),.X(_2_));
	sky130_fd_sc_hd_or2_0 _7_(.A(4),.B(_3_),.X(5));

	assign 4 = \u2.b ;
	assign 3 = \u2.a ;
	assign \u2.y = _5_;
	assign \u2.a = net1;
	assign \u2.b = c;
	assign y = \u2.y;
	assign 1 = \u1.b;
	assign 0 = \ul.a ;
	assign \ul.y = _2_;
	assign \ul.a = a;
	assign \u1.b = b;
	assign net1 = \u1.y;
endmodule
```

![Screenshot from 2024-10-20 00-24-13](https://github.com/user-attachments/assets/e033e09c-e4ab-4a2d-ad4b-38794444b2c8)
![Screenshot from 2024-10-20 00-08-53](https://github.com/user-attachments/assets/fba372b6-5fb7-47ca-8b4e-41559fe8a291)
![Screenshot from 2024-10-19 23-50-39](https://github.com/user-attachments/assets/7dd8e450-7e1f-45f2-91fd-f0f4c4230011)
</li>

<li>
	Simulation of D-Flipflop using Iverilog and GTKWave: Performed simulations for 3 types of D-Flipflops - Asynchronous Reset, Asynchronous Set and Synchronous Reset.

Asynchronous Reset

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

```
//Design
module dff_asyncres(input clk, input async_reset, input d, output reg q);
	always@(posedge clk, posedge async_reset)
	begin
		if(async_reset)
			q <= 1'b0;
		else
			q <= d;
	end
endmodule
//Testbench
module tb_dff_asyncres; 
	reg clk, async_reset, d;
	wire q;
	dff_asyncres uut (.clk(clk),.async_reset (async_reset),.d(d),.q(q));

	initial begin
		$dumpfile("tb_dff_asyncres.vcd");
		$dumpvars(0,tb_dff_asyncres);
		// Initialize Inputs
		clk = 0;
		async_reset = 1;
		d = 0;
		#3000 $finish;
	end
		
	always #10 clk = ~clk;
	always #23 d = ~d;
	always #547 async_reset=~async_reset; 
endmodule
```

![Screenshot from 2024-10-20 00-35-37](https://github.com/user-attachments/assets/674eb7bf-84a8-4c99-97a4-b26e429589af)

![Screenshot from 2024-10-20 00-33-36](https://github.com/user-attachments/assets/c6a96a2e-527a-4525-aae7-26f5b31f2bd3)

      From the waveform, it can be observed that the Q output changes to zero when the asynchronous reset is set high, independent of the positive/negative clock edge.

Asynchronous Set

```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

```
//Design
module dff_async_set(input clk, input async_set, input d, output reg q);
	always@(posedge clk, posedge async_set)
	begin
		if(async_set)
			q <= 1'b1;
		else
			q <= d;
	end
endmodule
//Testbench
module tb_dff_async_set; 
	reg clk, async_set, d;
	wire q;
	dff_async_set uut (.clk(clk),.async_set (async_set),.d(d),.q(q));

	initial begin
		$dumpfile("tb_dff_async_set.vcd");
		$dumpvars(0,tb_dff_async_set);
		// Initialize Inputs
		clk = 0;
		async_set = 1;
		d = 0;
		#3000 $finish;
	end
		
	always #10 clk = ~clk;
	always #23 d = ~d;
	always #547 async_set=~async_set; 
endmodule
```

![Screenshot from 2024-10-20 00-35-37](https://github.com/user-attachments/assets/674eb7bf-84a8-4c99-97a4-b26e429589af)

![Screenshot from 2024-10-20 00-34-53](https://github.com/user-attachments/assets/fe61c8d9-0d36-497b-9932-db0e5f96ae4e)

From the waveform, it can be observed that the Q output changes to one when the asynchronous set is set high, independent of the positive/negative clock edge.

Synchronous Reset

```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

```
//Design
module dff_syncres(input clk, input sync_reset, input d, output reg q);
	always@(posedge clk)
	begin
		if(sync_reset)
			q <= 1'b0;
		else
			q <= d;
	end
endmodule
//Testbench
module tb_dff_syncres; 
	reg clk, syncres, d;
	wire q;
	dff_asyncres uut (.clk(clk),.sync_reset (sync_reset),.d(d),.q(q));

	initial begin
		$dumpfile("tb_dff_syncres.vcd");
		$dumpvars(0,tb_dff_syncres);
		// Initialize Inputs
		clk = 0;
		sync_reset = 1;
		d = 0;
		#3000 $finish;
	end
		
	always #10 clk = ~clk;
	always #23 d = ~d;
	always #547 sync_reset=~async_reset; 
endmodule
```
      
![Screenshot from 2024-10-20 00-35-37](https://github.com/user-attachments/assets/674eb7bf-84a8-4c99-97a4-b26e429589af)

![Screenshot from 2024-10-20 00-36-05](https://github.com/user-attachments/assets/76b47b02-743c-465a-951d-0056f2f8b1ce)

From the waveform, it can be observed that the Q output changes to zero when the synchronous reset is set high, only at the positive clock edge.

</li>

<li>
	Synthesis of D-Flipflop using Yosys: Synthesized 3 types of D-Flipflops - Asynchronous Reset, Asynchronous Set and Synchronous Reset.
	
Asynchronous Reset
	
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_asyncres.v
4. synth -top dff_asyncres
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
8. write_verilog -noattr dff_asyncres_netlist.v
9. gvim dff_asyncres_netlist.v
```

```
//Generated Netlist   		
module dff_asyncres (clk, async_reset, d, q);
	wire _0_;
	wire _1_;
	wire _2_;
	input async_reset;
	input clk;
	input d;
	output q;
	
	sky130_fd_sc_hd__clkinv_1 _3_ (.A(_0_),.Y(_1_));
	sky130_fd_sc_hd__dfrtp_1 _4_ (.CLK(clk),.D(d),.RESET_B(_2_),.Q(q));
	assign _0_ = async_reset;
	assign _2_ = _1_;
endmodule
```

![Screenshot from 2024-10-20 00-45-05](https://github.com/user-attachments/assets/9fef968f-69a7-48ec-a826-b73720db62f4)
![Screenshot from 2024-10-20 00-44-22](https://github.com/user-attachments/assets/8d157f34-8466-4d8e-a956-83cb8f1afd35)

Asynchronous Set		
  
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_async_set.v
4. synth -top dff_async_set
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
8. write_verilog -noattr dff_async_set_netlist.v
9. gvim dff_async_set_netlist.v
```

```
//Generated Netlist   		
module dff_async_set (clk, async_set, d, q);
	wire _0_;
	wire _1_;
	wire _2_;
	input async_set;
	input clk;
	input d;
	output q;
	
	sky130_fd_sc_hd__clkinv_1 _3_ (.A(_0_),.Y(_1_));
	sky130_fd_sc_hd__dfrtp_1 _4_ (.CLK(clk),.D(d),.RESET_B(_2_),.Q(q));
	assign _0_ = async_set;
	assign _2_ = _1_;
endmodule
```

![Screenshot from 2024-10-20 00-56-43](https://github.com/user-attachments/assets/dd23a723-6358-4d07-acc1-57dedc039210)
Synchronous Reset
  
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_syncres.v
4. synth -top dff_syncres
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
8. write_verilog -noattr dff_syncres_netlist.v
9. gvim dff_syncres_netlist.v
```

```
//Generated Netlist   		
module dff_syncres (clk, sync_reset, d, q);
	wire _0_;
	wire _1_;
	wire _2_;
	input sync_reset;
	input clk;
	input d;
	output q;
	
	sky130_fd_sc_hd__clkinv_1 _3_ (.A(_0_),.Y(_1_));
	sky130_fd_sc_hd__dfrtp_1 _4_ (.CLK(clk),.D(d),.RESET_B(_2_),.Q(q));
	assign _0_ = sync_reset;
	assign _2_ = _1_;
endmodule
```

![Screenshot from 2024-10-20 01-03-23](https://github.com/user-attachments/assets/12b84888-85d6-4ab0-bc03-fb1fe70f89b0)

</li>

<li>
	Multiplication by 2: This tutorial, we get to know that specific multiplier hardware is not required for multiplication of a number by 2. It can simply be achieved by concatenating the number itself with a zero in the LSB.
 
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog mult_2.v
4. synth -top mul2
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. show
7. write_verilog -noattr mul2_net.v
8. gvim mul2_net.v
```

```
//Design
module mul2(input [2:0]a, output [3:0]y);
	assign y=a*2;
endmodule
```

```
//Generated Netlist
module mul2(a,y);
	input [2:0]a; wire [2:0]a;
	output [3:0]y; wire [3:0]y;

	assign y = {a,1'h0};
endmodule
```

![Screenshot from 2024-10-20 01-05-24](https://github.com/user-attachments/assets/5ccb7409-3bf7-44d5-9f87-3aede1ba570a)

![Screenshot from 2024-10-20 01-06-00](https://github.com/user-attachments/assets/2dd47629-9d35-409f-aae7-f6f6edafabe0)

</li>

<li>
	Multiplication by 9: This tutorial, we get to know that specific multiplier hardware is not required for multiplication of a number by 9. It can simply be achieved by concatenating the number with itself.
 
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog mult_9.v
4. synth -top mult9
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. show
7. write_verilog -noattr mul9_net.v
8. gvim mul9_net.v
```

```
//Design
module mul2(input [2:0]a, output [5:0]y);
	assign y=a*9;
endmodule
```

```
//Generated Netlist
module mul9(a,y);
	input [2:0]a; wire [2:0]a;
	output [5:0]y; wire [5:0]y;

	assign y = {a,a};
endmodule
```

![Screenshot from 2024-10-20 01-35-32](https://github.com/user-attachments/assets/9a0513b8-82a6-4af8-bfed-925e97d54f09)
![Screenshot from 2024-10-20 01-29-38](https://github.com/user-attachments/assets/59cf5edc-ced5-4e92-b06d-bd574ffb56ac)

</li>

    
  </details>
<details>
	  <summary>Day 3: Combinational and Sequential Optimisations</summary>
Optimization of Various Designs

<li>
	Design infers 2 input AND Gate:

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog opt_check.v
4. synth -top opt_check
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. show
```

5. Removes unused or redundant logic in the design and purges any dangling wires or gates.
 
```
//Design
module opt_check(input a, input b, output y);
	assign y = a?b:0;
endmodule
```

![Screenshot from 2024-10-20 12-08-05](https://github.com/user-attachments/assets/d18e235a-452a-4628-8604-98f07b4477cf)

![Screenshot from 2024-10-20 12-18-23](https://github.com/user-attachments/assets/25834df5-3ea0-4c5e-b0e2-4ac89e86859a)

![Screenshot from 2024-10-20 12-09-30](https://github.com/user-attachments/assets/1140c306-1f2c-4b2a-9d88-66481d14754d)

</li>

<li>
	Design infers 2 input OR Gate:

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog opt_check2.v
4. synth -top opt_check2
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. show
```

```
//Design
module opt_check2(input a, input b, output y);
	assign y = a?1:b;
endmodule
```

![Screenshot from 2024-10-20 12-10-34](https://github.com/user-attachments/assets/76558d1c-856d-4681-9eff-f2658d62a113)

![Screenshot from 2024-10-20 12-11-03](https://github.com/user-attachments/assets/76f173a1-e161-485b-a401-b4b937720947)

![Screenshot from 2024-10-20 12-11-19](https://github.com/user-attachments/assets/99ca2145-e75f-471e-ad85-037a21a9d69a)
</li>	

<li>
	Design infers 3 input AND Gate:

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog opt_check3.v
4. synth -top opt_check3
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. show
```

```
//Design
module opt_check2(input a, input b, input c, output y);
	assign y = a?(b?c:0):0;
endmodule
```

![Screenshot from 2024-10-20 12-13-14](https://github.com/user-attachments/assets/a34875a6-9813-4e1b-af1d-c403cf1bb6b5)

![Screenshot from 2024-10-20 12-13-47](https://github.com/user-attachments/assets/0e296d77-fc24-46bf-b459-bb693fd67582)

![Screenshot from 2024-10-20 12-14-01](https://github.com/user-attachments/assets/2f03c177-8a13-4889-b1e6-fb494c8d4ae5)

</li>

<li>
	Design infers 2 input XNOR Gate (3 input Boolean Logic)

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog opt_check4.v
4. synth -top opt_check4
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. show
```

```
//Design
module opt_check2(input a, input b, input c, output y);
	assign y = a ? (b ? ~c : c) : ~c;
endmodule
```

![Screenshot from 2024-10-20 12-16-33](https://github.com/user-attachments/assets/5f43c59a-7cfa-48ca-982a-adba4609891f)
![Screenshot from 2024-10-20 12-16-20](https://github.com/user-attachments/assets/79c137fe-bbe6-4322-ad2b-ca8e5aa3dcda)
![Screenshot from 2024-10-20 12-15-58](https://github.com/user-attachments/assets/8c16efa0-8c2c-4cf3-9266-0e4a38567bb0)

</li>

<li>
	Multiple Module Optimization-1

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog multiple_module_opt.v
4. synth -top multiple_module_opt
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. show
```

```
//Design

module sub_module1(input a, input b, output y);
	assign y = a & b;
endmodule

module sub_module2 (input a, input b output y);
	assign y = a^b;
endmodule

module multiple_module_opt(input a, input b input c, input d output y);
	wire n1,n2, n3;

	sub_module1 U1 (.a(a), .b(1'b1), .y(n1));
	sub_module2 U2 (.a(n1), .b(1'b0), .y(n));
	sub_module2 U3 (.a(b), .b(d), .y(n3));

	assign y = c | (b & n1);
endmodule
```

![Screenshot from 2024-10-20 13-39-25](https://github.com/user-attachments/assets/eff942f7-5eec-4a39-9aa8-3b71891a5df8)


![Screenshot from 2024-10-20 13-40-34](https://github.com/user-attachments/assets/e27a1cda-d65f-4a4b-8c3c-aeffe538bb44)

![Screenshot from 2024-10-20 13-40-53](https://github.com/user-attachments/assets/6f20feb6-10c5-42fd-b5f7-5ea01d6dc3a0)
</li>

<li>
	Multiple Module Optimization-2

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog multiple_module_opt2.v
4. synth -top multiple_module_opt2
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. show
```

```
//Design
module sub_module(input a input b output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a, input b input c, input d, output y);
	wire n1,n2, n3;

	sub_module U1 (.a(a), .b(1'b0), y(n));
	sub_module U2 (.a(b), .b(c), .y(n2));
	sub_module U3 (.a(n2), .b(d), .y(n));
	sub_module U4 (.a(n3), .b(n1), .y(y));
endmodule
```

<li>
	D-Flipflop Constant 1 with Asynchronous Reset (active low)
	
```
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
```

```
//Design
module dff_const1(input clk, input reset, output reg q); 
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
//Testbench
module tb_dff_const1; 
	reg clk, reset;
	wire q;

	dff_const1 uut (.clk(clk),.reset(reset),.q(q));

	initial begin
		$dumpfile("tb_dff_const1.vcd");
		$dumpvars(0,tb_dff_const1);
		// Initialize Inputs
		clk = 0;
		reset = 1;
		#3000 $finish;
	end

	always #10 clk = ~clk;
	always #1547 reset=~reset;
endmodule
```
 
![Screenshot from 2024-10-20 13-44-14](https://github.com/user-attachments/assets/527a0166-d846-4131-a8c0-e7014b1eeb2b)

![Screenshot from 2024-10-20 13-44-51](https://github.com/user-attachments/assets/ab2cfd0f-123d-4372-a89a-45e656176485)

From the waveform, it can be observed that the Q output is always high when reset is zero, and reset doesn't depend on clock edge.
  
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_const1.v
4. synth -top dff_const1
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 13-47-03](https://github.com/user-attachments/assets/5f32f280-08c6-4d3e-a4db-b46237ff67bf)

![Screenshot from 2024-10-20 13-47-22](https://github.com/user-attachments/assets/1d9d4114-44cd-4673-a9c3-c4babb82732a)
</li>

<li>
	D-Flipflop Constant 2 with Asynchronous Reset (active high)

```
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const2.vcd
```

```
//Design
module dff_const2(input clk, input reset, output reg q); 
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
//Testbench
module tb_dff_const2; 
	reg clk, reset;
	wire q;

	dff_const2 uut (.clk(clk),.reset(reset),.q(q));

	initial begin
		$dumpfile("tb_dff_const1.vcd");
		$dumpvars(0,tb_dff_const1);
		// Initialize Inputs
clk = 0;
		reset = 1;
		#3000 $finish;
	end

	always #10 clk = ~clk;
	always #1547 reset=~reset;
endmodule
```
 
![Screenshot from 2024-10-20 13-47-54](https://github.com/user-attachments/assets/917abe8b-085d-4eaa-9ed2-6f6f098f9938)

![Screenshot from 2024-10-20 13-48-25](https://github.com/user-attachments/assets/3d17fa5d-3135-43be-ac17-aa6e2423163d)
From the waveform, it can be observed that the Q output is always high irrespective of reset.
  
```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_const2.v
4. synth -top dff_const2
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 13-51-56](https://github.com/user-attachments/assets/bf4eb299-b590-4e3a-9482-47901535ff71)
![Screenshot from 2024-10-20 13-50-35](https://github.com/user-attachments/assets/b53eaaf7-bdc5-409c-b528-96b96aaca217)

</li>

<li>
	D-Flipflop Constant 3 with Asynchronous Reset (active low)

```
//Design
module dff_const3(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_const3.v
4. synth -top dff_const3
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 13-52-56](https://github.com/user-attachments/assets/4ef16aba-d82b-4158-8f62-e9346db7190d)

![Screenshot from 2024-10-20 13-53-38](https://github.com/user-attachments/assets/4e6881a1-776b-4568-894b-dda3f32ad83b)

This module defines a D flip-flop, for a positive edge of reset, q is set to 1 and q1 is set to 0. On each clock cycle, q1 is set to 1, and q is updated with the value of q1.


When synthesized, the design will result in a flip-flop where q becomes 1 after the first clock cycle post-reset and stays 1 afterward.

</li>

<li>
	D-Flipflop Constant 4 with Asynchronous Reset (active high)

```
//Design
module dff_const4(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b1;
			q1 <= 1'b1;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_const4.v
4. synth -top dff_const4
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 13-54-42](https://github.com/user-attachments/assets/8ddb0d7e-6b72-4156-a37d-c6df99fb1d37)

![Screenshot from 2024-10-20 13-55-18](https://github.com/user-attachments/assets/b1cd8666-1059-4abd-b31c-94c3eb77fd2b)

This module defines a D flip-flop that sets both q and q1 to 1 on a positive edge of reset. On each clock cycle, q1 remains 1, and q is updated with the value of q1 (which is always 1).

When synthesized, the design will result in a flip-flop where q is always 1, regardless of the reset or clock state.

</li>

<li>
	D-Flipflop Constant 5 with Asynchronous Reset

```
//Design
module dff_const5(input clk, input reset, output reg q); 
	reg q1;

	always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
		else
		begin	
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog dff_const5.v
4. synth -top dff_const5
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 13-56-47](https://github.com/user-attachments/assets/3b5096df-07e8-4c91-b76d-ba0f9e9e6016)

![Screenshot from 2024-10-20 13-57-12](https://github.com/user-attachments/assets/05d5df3c-6860-424c-b723-b8f0ef025d68)
This module defines a D flip-flop that resets both q and q1 to 0 on a positive edge of reset. On each clock cycle, it sets q1 to 1 and then updates q with the value of q1 (which will always be 1 after the first cycle).

When synthesized, the design will result in a flip-flop where q is always 1 after the first clock cycle post-reset.

</li>

<li>
	Counter Optimization 1:

```
//Design	
module counter_opt (input clk, input reset, output q);
	reg [2:0] count;
	assign q = count[0];
	
	always @(posedge clk,posedge reset)
	begin
		if(reset)
			count <= 3'b000;
		else
			count <= count + 1;
	end
endmodule
```

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog counter_opt.v
4. synth -top counter_opt
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 13-58-16](https://github.com/user-attachments/assets/91822b70-16c8-4925-b05a-4b53559e78f6)

![Screenshot from 2024-10-20 13-58-30](https://github.com/user-attachments/assets/a30bb60b-9377-4e2f-9b05-e0e76f7e9133)
</li>

<li>
	Counter Optimization 2:

```
//Design	
module counter_opt2 (input clk, input reset, output q);
	reg [2:0] count;
	assign q = (count[2:0] == 3'b100);
	
	always @(posedge clk,posedge reset)
	begin
		if(reset)
			count <= 3'b000;
		else
			count <= count + 1;
	end
endmodule
```

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog counter_opt2.v
4. synth -top counter_opt2
5. dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
7. show
```

![Screenshot from 2024-10-20 14-04-56](https://github.com/user-attachments/assets/233723bf-fbd6-4b38-bd7a-33eadf112094)

![Screenshot from 2024-10-20 14-05-45](https://github.com/user-attachments/assets/06d71651-0967-4069-8f51-6f0606e2456d)
 
</li>

</li>

  </details>

  <details>
	  <summary>Day 4: GLS , Blocking vs Non-blocking and Synthesis-Simulation mismatch</summary>
<li>
	Design of 2x1 MUX using Ternary Operator:
	
```
//Design
module ternary_operator_mux(input i0, input i1, input sel, output y);
	assign y = sel?i1:i0;
endmodule
```

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

These commands perform iverilog and GTKWave simulation.

![Screenshot from 2024-10-20 14-47-41](https://github.com/user-attachments/assets/135af422-39af-4181-96cb-4cd570be26cb)

![Screenshot from 2024-10-20 14-48-07](https://github.com/user-attachments/assets/d9311642-5568-4b86-b387-06313afb4866)

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog ternary_operator_mux.v
4. synth -top ternary_operator_mux
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. write_verilog -noattr ternary_operator_mux_net.v
8. !gvim ternary_operator_mux_net.v
9. show
```

```
//Generated Netlist
module ternary_operator_mux(i0, il, sel, y);
	wire _0_;
	wire _1_;
	wire _2_;
	wire _3_;
	input i0; wire i0;
	input il; wire il;
	input sel; wire sel;
	output y; wire y;
	
	sky130_fd_sc_hd_mux2_1 _4_ (.AO(_0_),.A1(_1_),.S(_2_),.X(_3_));

	assign _0_ = i0;
	assign _1_ = il;
	assign _2_ = sel;
	assign y = _3_;
endmodule
```

![Screenshot from 2024-10-20 14-51-09](https://github.com/user-attachments/assets/ee242f1e-b4be-4490-9eec-c41c64a52603)

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![Screenshot from 2024-10-20 14-52-00](https://github.com/user-attachments/assets/24ec3204-b9b1-401a-8923-bf9d3bab757c)

![Screenshot from 2024-10-20 15-13-10](https://github.com/user-attachments/assets/84d3783d-83bb-41de-bba5-9d47657ef1dd)

These waveforms correspond to the GATE LEVEL SYNTHESIS for the Ternary Operator MUX.

</li>	

<li>
	Design of a Bad 2x1 MUX:

```
//Design
module bad_mux(input i0, input i1, input sel, output reg y);
	always@(sel)
	begin
		if(sel)
			y <= i1;
		else
			y <= i0;
	end
endmodule
```

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![Screenshot from 2024-10-20 15-03-49](https://github.com/user-attachments/assets/aee93f19-1fe0-4271-93a5-86bbada7acd9)
![Screenshot from 2024-10-20 15-03-22](https://github.com/user-attachments/assets/009ae054-238f-41af-82db-5541626878ac)

From the waveform it can be observed that the output y changes only when there is a change in select line, completely ignoring the change in i0 and i1, which should also change the output y. Thus, this design is that of a bad MUX.

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog bad_mux.v
4. synth -top bad_mux
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. write_verilog -noattr bad_mux_net.v
8. !gvim bad_mux_net.v
9. show
```

```
//Generated Netlist
module bad_mux(i0, il, sel, y);
	wire _0_;
	wire _1_;
	wire _2_;
	wire _3_;
	input i0; wire i0;
	input il; wire il;
	input sel; wire sel;
	output y; wire y;
	
	sky130_fd_sc_hd_mux2_1 _4_ (.AO(_0_),.A1(_1_),.S(_2_),.X(_3_));

	assign _0_ = i0;
	assign _1_ = il;
	assign _2_ = sel;
	assign y = _3_;
endmodule
```

![Screenshot from 2024-10-20 15-07-21](https://github.com/user-attachments/assets/acf4336d-398d-4366-883f-ae9d121bd55f)

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![Screenshot from 2024-10-20 15-49-52](https://github.com/user-attachments/assets/013350b6-9761-41b4-a0e4-245e15dc4418)

![Screenshot from 2024-10-20 15-09-13](https://github.com/user-attachments/assets/850e946b-813e-4038-af0b-714b38a46173)

These waveforms correspond to the GATE LEVEL SYNTHESIS for the Bad MUX.

</li>

<li>
	Blocking Caveat:

```
//Design
module blocking_caveat(input a, input b, input c, output reg d);
	reg x;

	always@(*)
	begin
		d = x & c;
		x = a | b;
	end
endmodule
```

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![Screenshot from 2024-10-20 15-57-26](https://github.com/user-attachments/assets/1c3acb0f-915f-4212-87db-6535cb8f8705)

![Screenshot from 2024-10-20 16-02-54](https://github.com/user-attachments/assets/fbbf0035-d8e2-4935-9be4-d9adb49efe21)

![Screenshot from 2024-10-20 16-02-47](https://github.com/user-attachments/assets/e3b2aa99-cd89-4a5c-b428-26d2f6880780)

As depicted in the waveform, when A and B go zero, the OR gate output should be zero (X equal to zero), and the AND gate output should also be zero (same as D output). But, the AND gate input of X takes the previous value of A|B equal to one, based on the design created by the blocking statement, hence the discrepancy in the output.

```
1. yosys
2. read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
3. read_verilog blocking_caveat.v
4. synth -top blocking_caveat
5. abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
6. opt_clean -purge
7. write_verilog -noattr blocking_caveat_net.v
8. !gvim blocking_caveat_net.v
9. show
```

```
//Generated Netlist
module blocking_caveat(a,b,c,d);
	wire _0_;
	wire _1_;
	wire _2_;
	wire _3_;
	wire _4_;
	input a; wire a;
	input b; wire b;
	input c; wire c;
	input d; wire d;
	output d; wire d;
	
	sky130_fd_sc_hd__o21a_1 _5_ (.A1(_2_),.A2(_1_),.B1(_3_),.X(_4_));

	assign _2_ = b;
	assign _1_ = a;
	assign _3_ = c;
	assign d = _4_;
endmodule
```

![Screenshot from 2024-10-20 16-07-43](https://github.com/user-attachments/assets/14860dcb-b550-4679-8ebf-512dbff3820f)
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![Screenshot from 2024-10-20 16-04-20](https://github.com/user-attachments/assets/0a41a5f7-738c-448d-b88f-b548474cc14b)

![Screenshot from 2024-10-20 16-05-32](https://github.com/user-attachments/assets/3e9cde6d-af32-4c44-aaa6-4bb96397c244)
These waveforms correspond to the GATE LEVEL SYNTHESIS for the Blocking Caveat.

</li>

  </details>
</details>
<details>
<summary> Assignment 10</summary>
<br>

## Synthesis of RISC-V using yosys and Post synthesis simulation of Babysoc using iverilog GTKwave
First step in the Post synthesis simulation design flow is to synthesize the generated RTL code of RISC-V and after that we will simulate the result. This way we can find more about our code and its bugs. So in this section we are going to synthesize our code then do a post-synthesis simulation to look for any issues. The post and pre (modeling section) synthesis results should be identical.


  To perform the synthesis process do the following:
  ```
cd ~/VSDBabySoC/src
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib ../lib/avsddac.lib
read_liberty -lib ../lib/avsdpll.lib  
read_verilog ../verilog_files/vsdbabysoc.v
read_verilog ../verilog_files/rvmyth.v
read_verilog ../verilog_files/clk_gate.v 
synth -top vsdbabysoc
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show vsdbabysoc
write_verilog -noattr vsdbabysoc.synth.v
```

These commands will generate the vsdbaby soc top level netlist file vsdbabysoc.synth.v which can be used for the post synthesis simulation of the RISC-V processor.
The synthesized module is shown below:

![Screenshot from 2024-10-24 01-02-48](https://github.com/user-attachments/assets/fc1dcb7f-fa78-4814-b341-101dda636b63)
Generated Netlist
![Screenshot from 2024-10-24 02-18-24](https://github.com/user-attachments/assets/15473c9c-2084-46ba-8728-4cc582f5a3d8)
![Screenshot from 2024-10-24 02-18-39](https://github.com/user-attachments/assets/8ec27eea-c232-4417-8ee9-d1be174a758f)
![Screenshot from 2024-10-24 02-18-49](https://github.com/user-attachments/assets/69582f72-cdb0-45ff-ad34-273fa42daaf5)
  ------
The netlist generated in the terminal window is shown below:

![Screenshot from 2024-10-24 01-16-41](https://github.com/user-attachments/assets/34f0de64-fa51-4e20-9574-7caa0ef2d947)


![Screenshot from 2024-10-24 01-18-32](https://github.com/user-attachments/assets/9b8d4b31-333a-4513-a9ed-40cd466d12cd)

![Screenshot from 2024-10-24 01-18-44](https://github.com/user-attachments/assets/9330c041-cb37-46cf-8105-a044a660a86e)

![Screenshot from 2024-10-24 01-20-04](https://github.com/user-attachments/assets/71b2684a-295e-4851-a0ce-b2f54080d015)

-----
## Post-synthesis simulation (GLS)
--------
For Post-synthesis simulation, we use the vsdbabysoc.v as the top module, which includes RISC-V, DAC and PLL as submodules and the testbench which we used for the pre synthsis simulation of the vsdbabysoc.

The commands for simulating the synthesized module of RISC-V are:
```
cd ~/VSDBabySoC

mkdir -p output/post_synth_sim && iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I src/module/include -I src/module -I src/gls_model src/module/testbench.v && cd output/post_synth_sim && ./post_synth_sim.out
```

The result of the simulation (i.e. post_synth_sim.vcd) will be stored in the output/post_synth_sim directory and the waveform could be seen by the following command:
```
gtkwave post_synth_sim.vcd
```
![GLS](https://github.com/user-attachments/assets/bfc6e04a-a577-4eca-b1f3-bfd7f2020ed0)

----
The simulation waveforms are:

1. clk_gour,reset,VCO_IN & Output signals:
![Screenshot from 2024-10-24 01-04-41](https://github.com/user-attachments/assets/44fea0ce-e9d8-4ea8-9bd2-6cfab6bf91a9)
![Screenshot from 2024-10-24 01-05-17](https://github.com/user-attachments/assets/7d6c3fd0-c1ce-48a3-93a5-c64909f994cb)
-----
In the above waveforms, we can see the following signals:

**clk_gour:**     
**reset:**     
**RV_to_DAC[9:0]:** 
**OUT:** 

**The pre synthesis simulation waveforms and the post synthesis simulation waveforms were found to be identical.
The pre synthesis simulation waveforms are shown here for reference:**

![Screenshot from 2024-10-23 20-17-25](https://github.com/user-attachments/assets/5fd5a4be-aa0c-4dd9-99a0-5447c80cdc1d)
![Screenshot from 2024-10-23 20-16-58](https://github.com/user-attachments/assets/9d626725-cdf8-44cb-b4c3-949748b8a21f)

      
</details>
<details>
<summary> Assignment 11</summary>
<br>
	
 ## STA	
	
Static Timing Analysis (STA) is a method used in digital circuit design to verify the timing performance of a circuit without requiring dynamic simulation. It checks whether the circuit meets its timing constraints by analyzing the timing paths in the design. Here are some key aspects of STA:

1. Timing Paths: STA evaluates all possible paths through a circuit from input to output, taking into account the propagation delays of gates and interconnects.

2. Setup and Hold Times: It checks for setup and hold time violations. The setup time is the minimum time before the clock edge that the input data must be stable, while the hold time is the minimum time after the clock edge that the data must remain stable.

3. Clock Constraints: STA incorporates clock definitions, including the clock frequency, period, and any variations (like skew or jitter).

4. Worst-case Scenario: STA assumes worst-case conditions for delay values (like maximum load, temperature, and voltage) to ensure that the circuit will perform correctly under all expected operating conditions.

### Why STA is performed ?

Static Timing Analysis (STA) is performed for several critical reasons in digital circuit design:

1. Timing Verification: STA ensures that the design meets its specified timing constraints. It verifies that data signals can propagate through the circuit within the required time limits, ensuring that outputs are stable and valid when needed.

2. Identify Timing Violations: It helps identify setup and hold time violations, which can lead to incorrect operation of flip-flops and other sequential elements. Detecting these violations is crucial to ensure the reliability of the circuit.

3. Performance Optimization: By analyzing the timing paths, designers can identify critical paths that limit the maximum operating frequency. This information can be used to optimize the design by resizing gates, adjusting the layout, or modifying the clock strategy.

4. Early Detection of Issues: STA allows for early detection of timing issues during the design process, reducing the risk of costly iterations and revisions in later stages, such as post-layout or during fabrication.

5. Power Consumption Analysis: Timing analysis can also help in understanding the impact of clock frequency on power consumption. By ensuring that the design runs at optimal speeds, designers can balance performance and power efficiency.

6. Design Validation: STA provides a level of assurance that the design will work correctly under the specified operating conditions. It validates the design against its intended specifications and requirements.

7. Automation: STA tools can automatically analyze complex designs, making it more efficient than traditional dynamic simulations, especially for large-scale integrated circuits.

8. Support for Variability: STA can incorporate variations in manufacturing processes, temperature, and voltage (PVT variations) to ensure robust performance across different conditions.

In summary, STA is essential for ensuring the functionality, reliability, and performance of digital circuits, enabling designers to create high-quality, efficient designs.

![Screenshot from 2024-10-29 00-05-02](https://github.com/user-attachments/assets/c6af4bc7-c98e-420a-8658-69bdb7d5c221)
![Screenshot from 2024-10-29 00-15-10](https://github.com/user-attachments/assets/b858a290-01be-4249-86af-f210f0e1e95d)

### What is reg2reg Path ?

A reg2reg path (register-to-register path) refers to a timing path in a digital circuit that connects two sequential elements, specifically flip-flops or registers. This path is crucial in the context of Static Timing Analysis (STA) because it represents the flow of data from one register to another through combinational logic.

Reg2reg paths are essential for ensuring proper data flow and synchronization in digital circuits, especially in designs with pipelining or sequential operations. Analyzing these paths helps in verifying that the data processing occurs correctly across clock cycles, thereby ensuring the overall functionality and reliability of the circuit.

#### Key characteristics of reg2reg Path

1. **Sequential Logic**: Reg2reg paths are part of sequential circuits where data is stored in registers and passed from one register to another after being processed by combinational logic.

2. **Setup and Hold Timing**:
   * **Setup Time**: Reg2reg paths are analyzed for setup time constraints to ensure that the data output from the first register (FF1) arrives at the second register (FF2) before the clock edge that triggers FF2.
   * **Hold Time**: These paths are also evaluated for hold time constraints to ensure that the data remains stable at the input of FF2 for a specified period after the clock edge that triggers FF1.

3. **Combinational Logic Delay**: The timing analysis of a reg2reg path includes the propagation delay through the combinational logic that connects the two registers. This delay can vary based on the logic elements and their configuration.

4. **Critical Paths**: Reg2reg paths can often be critical paths if they take longer than other paths in the design, which can limit the maximum operating frequency of the circuit.

5. **Path Analysis**: STA tools evaluate reg2reg paths to check for timing violations, allowing designers to optimize the circuit by adjusting the logic, resizing gates, or modifying the layout.

6. **Clock Domain Crossing**: If the two registers belong to different clock domains, additional considerations for metastability and synchronization are needed, which complicates the reg2reg timing analysis.

### What is clk2reg Path ?

A clk2reg path (clock-to-register path) refers to a timing path in a digital circuit that connects the clock signal to a register (flip-flop). This path is crucial for ensuring that the register operates correctly in response to clock events. Here are the key aspects of clk2reg paths:

1. **Clock Signal Propagation**: The clk2reg path represents the time it takes for the clock signal to reach the register from the clock source, including any delays introduced by clock buffers or routing.

2. **Setup Timing**: In the context of setup timing analysis, the clk2reg path is important for determining when the data signal must arrive at the register relative to the clock edge. The analysis ensures that the clock arrives at the register before the data input becomes stable, meeting the setup time requirement.

3. **Clock Delay**: This path is evaluated for the delay introduced by any clock distribution elements, such as buffers and inverters, that may be part of the clock tree. The total delay impacts the timing of when the register captures the input data.

4. **Critical Paths**: A clk2reg path can become a critical path if the delay through this path is significant enough to affect the maximum frequency of operation for the circuit.

5. **Hold Timing**: Although clk2reg paths are primarily associated with setup time analysis, they can also be relevant for hold time analysis, especially in cases where the clock signal may have some jitter or variations that could affect timing margins.

6. **Clock Diagram Crossing**: If the register is part of a different clock domain, the clk2reg analysis will also involve considerations for synchronization and potential metastability issues.   

6. Tools: There are various tools for performing STA, such as Synopsys PrimeTime, Cadence Tempus, and others, which automate the process and provide detailed reports on timing violations.

Overall, STA is crucial for ensuring that digital circuits operate reliably at the intended speeds and for identifying potential timing issues early in the design process.
## Tools Installation
**CUDD**
Download CUDD from **[here](https://github.com/davidkebo/cudd/blob/main/cudd_versions/cudd-3.0.0.tar.gz)** and move downloaded file to `home` directory
```
cd
tar xvfz cudd-3.0.0.tar.gz
cd cudd-3.0.0
./configure
make
```
**openSTA**
```
cd
sudo apt-get install cmake clang gcc tcl swig bison flex

git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
cmake -DCUDD_DIR=/home/gourab/cudd-3.0.0
make
app/sta
```


```
cd /home/gourab/OpenSTA
mkdir lab11
```
Download all the **[required files](https://github.com/thelikith/asic-design-class/tree/main/Codes/Lab%2010)** to directory `lab11`

**Steps to do Timing Analysis**
- Clock period = 9.00ns
- Setup uncertainty and clock transition will be 5% of clock = 0.45ns.
- Hold uncertainty and data transition will be 8% of clock = 0.72ns.


```
cd /home/gourab/OpenSTA/app
./sta


read_liberty /home/gourab/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /home/gourab/OpenSTA/lab11/gourab_riscv_netlist.v 
link_design rvmyth

create_clock -name clk -period 9.00 [get_ports clk]
set_clock_uncertainty [expr 0.05 * 9.00] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * 9.00] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * 9.00] [get_clocks clk]
set_input_transition [expr 0.08 * 9.00] [all_inputs]

report_checks -path_delay max
report_checks -path_delay min
```
![Screenshot from 2024-10-28 21-47-06](https://github.com/user-attachments/assets/cd9cffaf-52e6-4e12-992f-d4792d0d0777)

### reg2reg setup report--max timing report
![Screenshot from 2024-10-28 23-43-24](https://github.com/user-attachments/assets/957c8610-1a2c-4b86-9190-32404c378f7f)
### reg2reg hold report--min timing report
![Screenshot from 2024-10-28 21-47-12](https://github.com/user-attachments/assets/efedb849-2b69-4c4b-a379-d18c05a32704)

</details>
<details>
<summary> Assignment 12</summary>
<br>
	
### Download all the necessary libraries.
```
cd /home/gourab/OpenSTA
mkdir lab12
cd lab12
mkdir lib
mkdir output
```

- Download and copy the files from this **[folder](https://github.com/Subhasis-Sahu/SFAL-VSD/skywater-pdk-libs-sky130_fd_sc_hd/timing)** to `lib` directory
- Download and copy the files from this **[folder](https://github.com/manili/VSDBabySoC/src/lib)** to `lib` directory

  ### vsdbabysoc_synthesis.sdc
- Clock period = 9.00ns
- Setup uncertainty and clock transition is 5% of clock
- Hold uncertainty and data transition is 8% of clock.
  
```
# Create clock with new period
create_clock [get_pins pll/CLK] -name clk -period 9.00 -waveform {0 4.5}

# Set loads
set_load -pin_load 0.5 [get_ports OUT]
set_load -min -pin_load 0.5 [get_ports OUT]

# Set clock latency
set_clock_latency 1 [get_clocks clk]
set_clock_latency -source 2 [get_clocks clk]

# Set clock uncertainty
set_clock_uncertainty 0.45 [get_clocks clk]  ; # 5% of clock period for setup
set_clock_uncertainty -hold 0.72 [get_clocks clk] ; # 8% of clock period for hold

# Set maximum delay
set_max_delay 9.00 -from [get_pins dac/OUT] -to [get_ports OUT]

# Set input delay for VCO_IN
set_input_delay -clock clk -max 4 [get_ports VCO_IN]
set_input_delay -clock clk -min 1 [get_ports VCO_IN]

# Set input delay for ENb_VCO
set_input_delay -clock clk -max 4 [get_ports ENb_VCO]
set_input_delay -clock clk -min 1 [get_ports ENb_VCO]

# Set input delay for ENb_CP
set_input_delay -clock clk -max 4 [get_ports ENb_CP]
set_input_delay -clock clk -min 1 [get_ports ENb_CP]

# Set input transition for VCO_IN
set_input_transition -max 0.45 [get_ports VCO_IN] ; # 5% of clock
set_input_transition -min 0.72 [get_ports VCO_IN] ; # adjust if needed

# Set input transition for ENb_VCO
set_input_transition -max 0.45 [get_ports ENb_VCO] ; # 5% of clock
set_input_transition -min 0.72 [get_ports ENb_VCO] ; # adjust if needed

# Set input transition for ENb_CP
set_input_transition -max 0.45 [get_ports ENb_CP] ; # 5% of clock
set_input_transition -min 0.72 [get_ports ENb_CP] ; # adjust if needed

```

### sta.tcl
```
set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__tt_100C_1v80.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty /home/gourab/OpenSTA/lab12/lib/$list_of_lib_files($i)
read_liberty -min /home/gourab/OpenSTA/lab12/lib/avsdpll.lib
read_liberty -max /home/gourab/OpenSTA/lab12/lib/avsdpll.lib
read_liberty -min /home/gourab/OpenSTA/lab12/lib/avsddac.lib
read_liberty -max /home/gourab/OpenSTA/lab12/lib/avsddac.lib
read_verilog  /home/gourab/OpenSTA/lab12/vsdbabysoc_synth.v
link_design vsdbabysoc
read_sdc /home/gourab/OpenSTA/lab12/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /home/gourab/OpenSTA/lab12/output/min_max_$list_of_lib_files($i).txt

exec echo "$list_of_lib_files($i)" >> /home/gourab/OpenSTA/lab12/output/sta_worst_max_slack.txt
report_worst_slack -max -digits {4} >> /home/gourab/OpenSTA/lab12/output/sta_worst_max_slack.txt

exec echo "$list_of_lib_files($i)" >> /home/gourab/OpenSTA/lab12/output/sta_worst_min_slack.txt
report_worst_slack -min -digits {4} >> /home/gourab/OpenSTA/lab12/output/sta_worst_min_slack.txt

exec echo "$list_of_lib_files($i)" >> /home/gourab/OpenSTA/lab12/output/sta_tns.txt
report_tns -digits {4} >> /home/gourab/OpenSTA/lab12/output/sta_tns.txt

exec echo "$list_of_lib_files($i)" >> /home/gourab/OpenSTA/lab12/output/sta_wns.txt
report_wns -digits {4} >> /home/gourab/OpenSTA/lab12/output/sta_wns.txt
}

```
### Commands to perform STA
```
cd /home/gourab/OpenSTA/app
./sta
source /home/gourab/OpenSTA/lab12/sta.tcl
```

![Screenshot from 2024-11-05 11-16-31](https://github.com/user-attachments/assets/86456dd8-9ea0-4402-be39-aa46c3553063)


| Library                          | TNS         | WNS         | Worst max Slack | Worst min Slack |
|----------------------------------|-------------|-------------|-------------|-------------------|
| sky130_fd_sc_hd__tt_025C_1v80.lib | -18.5196    | -0.5108     | -0.5108     | -0.4104           |
| sky130_fd_sc_hd__tt_100C_1v80.lib | -11.0892    | -0.3565     | -0.3565     | -0.4055           |
| sky130_fd_sc_hd__ff_100C_1v65.lib | 0.0000      | 0.0000      | 1.6045      | -0.4709           |
| sky130_fd_sc_hd__ff_100C_1v95.lib | 0.0000      | 0.0000      | 3.1168      | -0.5240           |
| sky130_fd_sc_hd__ff_n40C_1v56.lib | -4.5127     | -0.1719     | -0.1719     | -0.4285           |
| sky130_fd_sc_hd__ff_n40C_1v65.lib | 0.0000      | 0.0000      | 0.9612      | -0.4649           |
| sky130_fd_sc_hd__ff_n40C_1v76.lib | 0.0000      | 0.0000      | 1.9801      | -0.4957           |
| sky130_fd_sc_hd__ff_n40C_1v95.lib | 0.0000      | 0.0000      | 3.1419      | -0.5325           |
| sky130_fd_sc_hd__ss_100C_1v40.lib | -3474.7312  | -19.5716    | -19.5716    | 0.1853            |
| sky130_fd_sc_hd__ss_100C_1v60.lib | -1461.3009  | -10.3240    | -10.3240    | -0.0780           |
| sky130_fd_sc_hd__ss_n40C_1v28.lib | -15386.5898 | -65.0132    | -65.0132    | 1.1096            |
| sky130_fd_sc_hd__ss_n40C_1v35.lib | -9325.6338  | -42.1470    | -42.1470    | 0.6275            |
| sky130_fd_sc_hd__ss_n40C_1v40.lib | -6688.1201  | -32.1437    | -32.1437    | 0.4049            |
| sky130_fd_sc_hd__ss_n40C_1v44.lib | -5212.2480  | -26.3579    | -26.3579    | 0.2709            |
| sky130_fd_sc_hd__ss_n40C_1v60.lib | -2006.3447  | -13.0590    | -13.0590    | -0.0572           |
| sky130_fd_sc_hd__ss_n40C_1v76.lib | -828.1414   | -6.8319     | -6.8319     | -0.2162           |

![Screenshot from 2024-11-05 13-58-31](https://github.com/user-attachments/assets/3347f5f6-ae42-4789-8c81-9500351d253b)
![Screenshot from 2024-11-05 14-04-45](https://github.com/user-attachments/assets/1d154555-64dc-49fe-a1df-60bb3c5caa74)
![Screenshot from 2024-11-05 14-17-00](https://github.com/user-attachments/assets/e4244faa-a3dc-454b-aef9-fd31da29bef5)
![Screenshot from 2024-11-05 14-17-17](https://github.com/user-attachments/assets/8b62ea5b-6bc2-4f3d-bc93-e8025fa433d5)


</details>
<details>
<summary> Assignment 13</summary>
<br>


![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow)1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/92eb860b-7a88-4c6f-8143-ad3e09fd9c5b)
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow) (1)1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4285c5e4-d5df-43e4-b460-ead45ff67f9b)
# Digital VLSI SoC Design and Planning

![Static Badge](https://img.shields.io/badge/OS-linux-orange)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-navy)
![Static Badge](https://img.shields.io/badge/languages-verilog%2C_bash%2C_TCL-crimson)
![GitHub last commit](https://img.shields.io/github/last-commit/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub language count](https://img.shields.io/github/languages/count/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub top language](https://img.shields.io/github/languages/top/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub repo size](https://img.shields.io/github/repo-size/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub repo file count (file type)](https://img.shields.io/github/directory-file-count/fayizferosh/soc-design-and-planning-nasscom-vsd)
<!---
Comments
-->

> 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM

## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK 

### Theory

<details>
  <summary>
Expand or Collapse
  </summary>

#### Package

* In any embedded board we have seen, the part of the board we consider as the chip is only the ***PACKAGE*** of the chip which is nothing but a protective layer or packet bound over the actual chip and the actual manufatured chip is usually present at the center of a package wherein, the connections from package is fed to the chip by ***WIRE BOUND*** method which is none other than basic wired connection.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7562205a-7435-46c7-a66e-de1626911f14)
![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7005a9e3-79da-4590-bea0-eb3768127a3d)
![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/70b1c678-2a2e-484f-9181-812dbcd5f0a3)

#### Chip

* Now, taking a look inside the chip, all the signals from the external world to the chip and vice versa is passed through ***PADS***. The area bound by the pads is ***CORE*** where all the digital logic of the chip is placed. Both the core and pads make up the ***DIE*** which is the basic manufacturing unit in regards to semiconductor chips.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d65a0ddf-2f86-4bbc-8d36-b02e09a1483e)

* ***FOUNDRY*** is the place where the semiconductor chips are manufactured and ***FOUNDRY IP's*** are Intellectual Properties based on a specific foundry and these IP's require a specific level of intelligence to be produced whereas, repeatable digital logic blocks are called ***MACROS***.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/ed1cd25e-6270-4b84-8f0d-f0ea7c8a7ef8)

#### ISA (Intruction Set Architecture)

* A C program which has to be run on a specific hardware layout which is the interior of a chip in your laptop, there is certain flow to be followed.
* Initially, this particular C program is compiled in it's assembly language program which is nothing but ***RISC-V ISA (Reduced Instruction Set Compting - V Intruction Set Architecture)***.
* Following this, the assembly language program is then converted to machine language program which is the binary language logic 0 and 1 which is understood by the hardware of the computer.
* Directly after this, we've to implement this RISC-V specification using some ***RTL (a Hardware Description Language)***. Finally, from the RTL to ***Layout*** it is a standard PnR or RTL to GDSII flow.

![Screenshot (278)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/7dc4601a-e386-48e5-9d1f-7fa5b47ca0ba)

* For an application software to be run on a hardware there are several processes taking place. To begin with, the apps enters into a block called system software and it converts the application program to binary language. There are various layers in system software in which the major layers or components are OS (Operating System), Compiler and Assembler.
* At first the OS outputs are small function in C, C++, VB or Java language which are taken by the respective compiler and converted into instructions and the syntax of these instructions varies with the hardware architecture on which the system is implemented.
* Then, the job of the assembler is to take these instructions and convert it into it's binary format which is basically called as a machine language program. Finally, this binary language is fed to the hardware and it understands the specific functions it has to perform based on the binary code it receives.

![Screenshot (279)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/19e8b634-f209-41a6-928d-6fba66f5b177)

* For example, if we take a stopwatch app on RISC-V core, then the output of the OS could be a small C function which enters into the compiler and we get output RISC-V instructions following this, the output of the assembler will be the binary code which enters into your chip layout.

![Screenshot (280)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/7d4570ca-82a6-4abe-81d2-067ebb9b2c15)

* For the above stopwatch the following are the input and output of the compiler and assembler.

![Screenshot (281)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/d7b7fd1b-21ee-46b7-9a91-8314bd753a51)

* The output of the compiler are instructions and the output of the assembler is the binary pattern. Now, we need some RTL (a Hardware Description Language) which understands and implements the particular instructions. Then, this RTL is synthesised into a netlist in form of gates which is fabricated into the chip through a physical design implementation.

![Screenshot (282)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/e349cb06-45e3-4ae4-b85f-9020a0a62737)

* There are mainly 3 different parts in this course. They are:
1. RISC-V ISA
2. RTL and synthesis of RISC-V based CPU core - picorv32
3. Physical design implementation of picorv32

![Screenshot (283)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/832f0ea6-2d60-4d9a-937c-a2dedd5f8cac)

#### Open-source Implementation

* For open-source ASIC design implemantation, we require the following enablers to be readily available as open-source versions. They are:-
1. RTL Designs
2. EDA Tools
3. PDK Data

* Initially in the early ages, the design and fabrication of IC's were tightly coupled and were only practiced by very few companies like TI, Intel, etc.
* In 1979, Lynn Conway and Carver Mead came up with an idea to saperate the design from the fabrication and to do this they inroduced structured design methodologies based on the λ-based design rules and published the first VLSI book "Introduction to VLSI System" which started the VLSI education.
* This methodology resulted in the emergence of the design only companies or ***"Fabless Companies"*** and fabrication only companies that we usually refer to as ***"Pure Play Fabs"***.
* The inteface between the designers and the fab by now became a set of data files and documents, that are reffered to as the ***"Process Design Kits (PDKs)"***.
* The PDK include but not limited to Device Models, Technology Information, Design Rules, Digital Standard Cell Libraries, I/O Libraries and many more.
* Since, the PDK contained variety of informations, and so they were distributed only under NDAs (Non-Disclosure Agreements) which made it in-accessible to the public.
* Recently, Google worked out an agreement with skywater to open-source the PDK for the 130nm process by skywater Technology, as a result on 30 June 2020 Google released the first ever open-source PDK.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/87384374-e66b-4ec6-b9c4-3fb92ad4d275)

* ASIC design is a complex step that involves tons of steps, various methodologies and respective EDA tools which are all required for successful ASIC implementation which is achieved though an ASIC flow which is nothing but a piece of software that pulls different tools togather to carry out the design process.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1762d6d6-c5f8-4bd9-8a3d-968eb4360889)

#### OpenLANE Open-source ASIC Design Implementation Flow

* The main objective of the ASIC Design Flow is to take the design from the RTL (Register Transfer Level) all the way to the GDSII, which is the format used for the final fabrication layout.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/533f58ee-4524-4a18-abb5-36b4d6a56b1f)

* Synthesis is the process of convertion or translation of design RTL into circuits made out of Standard Cell Libraries (SCL) the resultant circuit is described in HDL and is usually reffered to as the Gate-Level Netlist.
* Gate-Level Netlist is functionally equivalent to the RTL.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e43891a0-ab09-4df2-898d-7e843158e936)

* The fundemental building blocks which are the standard cells have regular layouts.
* Each cell has different views/models which are utilised by different EDA tools like liberty view with electrical models of the cells, HDL behavioral models, SPICE or CDL views of the cells, Layout view which include GDSII view which is the detailed view and LEF view which is the abstract view.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/48df884c-c894-4a2a-a511-09321c208d6b)

* Chip Floor Planning

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/38ecd866-ac83-42c7-83ba-2ba995f9ba4e)

* Macro Floor Planning

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/35ac760c-bba9-4bd1-9c65-e8ceeee2ccf5)

* Power Planning typically uses upper metal layers for power distribution since thay are thicker than lower metal layers and so have lower resistance and PP is done to avoid electron migration and IR drops.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f18013a7-e4c7-4a6d-ba53-33362c04689a)

* Placement

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3654398b-40bc-4e42-9205-77f673d5584c)

* Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/817c504d-0b8e-4a0a-9c3c-e9d110c23535)

* Clock Tree Synthesis

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6db284d4-065f-450a-9200-a6e6cbfd7fbb)

* Clock skew is the time difference in arrival of clock at different components.
* Routing

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7458495f-b527-4f21-813e-8dbcbf29ed9b)

* skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer the following 5 layers are all Aluminium layers.

![stackup](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e4438c12-d83e-4083-a58f-33a410a47927)

* Global and Detailed Routing

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b54ebd4c-127a-441f-829e-6000531e9b8d)

* Once done with the routing the final layout can be generated which undergoes various Sign-Off checks.
* Design Rules Checking (DRC) which verifies that the final layout honours all design fabrication rules.
* Layout Vs Schematic (LVS) which verifies that the final layout functionality matches the gate-level netlist that we started with.
* Static Timing Analysis (STA) to verify that the design runs at the designated clock frequency.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d8bc72cd-2fe9-4ae2-9431-68a6fa77c671)

</details>

### Implementation

Section 1 tasks:- 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the flop ratio.

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```


#### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

Screenshots of running each commands
![Screenshot from 2024-11-13 15-36-03](https://github.com/user-attachments/assets/7708a678-9cdb-4bbc-87eb-4387fb58021f)


![Screenshot from 2024-11-13 15-56-06](https://github.com/user-attachments/assets/faf93e24-338a-4e8b-b7d1-1a00d1aa868e)
#### 2. Calculate the flop ratio.

Screenshots of synthesis statistics report file with required values highlighted

![Screenshot from 2024-11-13 15-59-15](https://github.com/user-attachments/assets/739e09a2-55a0-454e-af0e-9d560a6add8e)
![Screenshot from 2024-11-13 15-59-18](https://github.com/user-attachments/assets/6ef7d30a-553c-4cf3-a8ec-463706959c5c)


Calculation of Flop Ratio and DFF % from synthesis statistics report file

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```

## Section 2 - Good floorplan vs bad floorplan and introduction to library cells

### Theory

### Implementation

Section 2 tasks:- 
1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

```math
Area\ of\ die\ in\ microns = Die\ width\ in\ microns * Die\ height\ in\ microns
```


#### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshot of floorplan run
![Screenshot from 2024-11-13 16-10-20](https://github.com/user-attachments/assets/12096132-cdb6-47c5-942f-4e1da58d2746)

![Screenshot from 2024-11-13 16-10-44](https://github.com/user-attachments/assets/1ad24bbd-b081-408b-9055-db06f0d161a9)

![Screenshot from 2024-11-13 16-10-54](https://github.com/user-attachments/assets/5433b9df-9648-44c4-803d-b660a010af99)

![Screenshot from 2024-11-13 16-11-04](https://github.com/user-attachments/assets/462d4980-77fe-48b1-8756-b94897c88edf)

![Screenshot from 2024-11-13 16-11-06](https://github.com/user-attachments/assets/6d29cb3b-356b-414e-9983-9410ad2cefa6)



#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

![Screenshot from 2024-11-13 16-16-46](https://github.com/user-attachments/assets/80a4af19-14e7-4ec7-adda-0d40288d1bd0)


According to floorplan def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-15/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![Screenshot from 2024-11-13 16-33-36](https://github.com/user-attachments/assets/4ea5efe6-7564-4281-be89-d774d8219545)

Equidistant placement of ports

![Screenshot from 2024-11-13 17-31-23](https://github.com/user-attachments/assets/4033cb3b-0cdf-4456-a5b2-6968504a543a)


Decap Cells and Tap Cells
![Screenshot from 2024-11-13 17-31-23](https://github.com/user-attachments/assets/4033cb3b-0cdf-4456-a5b2-6968504a543a)


Diogonally equidistant Tap cells

![Screenshot from 2024-11-13 17-48-34](https://github.com/user-attachments/assets/1e18f1e6-9be9-436c-87e1-47516d9b7e3b)

Unplaced standard cells at the origin

![Screenshot from 2024-11-13 17-43-35](https://github.com/user-attachments/assets/65473230-e45a-423f-b993-42fa06e95621)
#### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run

![Screenshot from 2024-11-13 18-53-09](https://github.com/user-attachments/assets/010fb628-d7c9-4d97-913c-a1edef21d87a)
![Screenshot from 2024-11-13 18-44-30](https://github.com/user-attachments/assets/21aea7c2-3c33-4751-91b8-f4f2c3231ba2)

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-15/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![Screenshot from 2024-11-13 19-11-27](https://github.com/user-attachments/assets/c7c64216-1229-467d-96b6-aa3862ec3b37)


Standard cells legally placed 

![Screenshot from 2024-11-13 19-29-33](https://github.com/user-attachments/assets/1ba583ef-afa8-460f-b7bc-29274d88bb7d)

Inverter with my name--> sky130_gourab_vsdinv
![Screenshot from 2024-11-14 22-58-08](https://github.com/user-attachments/assets/c7b573cd-f72a-45c7-9114-d28e3c1c7a54)

![Screenshot from 2024-11-15 15-18-33](https://github.com/user-attachments/assets/df20932b-e27f-4d92-a48a-82151b9d616a)
Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

## Section 3 - Design library cell using Magic Layout and ngspice characterization 
### Theory

### Implementation

* Section 3 tasks:-
1. Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.


#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

![Screenshot from 2024-11-13 19-32-44](https://github.com/user-attachments/assets/bd6a35c2-34b1-4737-9247-d74e3c5de6e8)

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Screenshot from 2024-11-13 19-33-29](https://github.com/user-attachments/assets/7216261f-d30c-4ae8-b92a-4ca8e300f8f3)
NMOS and PMOS identified

![Screenshot from 2024-11-13 20-05-58](https://github.com/user-attachments/assets/b7c42b24-758b-4aed-9e62-5f3e9c05c6b1)
![Screenshot from 2024-11-13 20-06-48](https://github.com/user-attachments/assets/501d9920-a0a2-43a3-ab71-e9419de1f3ea)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot from 2024-11-13 20-10-07](https://github.com/user-attachments/assets/603fff31-51ec-4c1b-b1fd-1c58bbe0e0c6)
PMOS source connectivity to VDD (here VPWR) verified

![Screenshot from 2024-11-13 20-10-34](https://github.com/user-attachments/assets/e5d67ede-7cdd-4f3c-97b0-13dc1d1c240f)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot from 2024-11-13 20-12-21](https://github.com/user-attachments/assets/46782333-096d-4238-8767-c35a156e200e)
Deleting necessary layout part to see DRC error

![Screenshot from 2024-03-19 01-10-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/861912e4-eef9-4226-b563-db7f49ca6632)

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

![Screenshot from 2024-11-13 20-39-57](https://github.com/user-attachments/assets/b18481da-6290-46e3-abe4-cbb0377a885e)

Screenshot of created spice file

![Screenshot from 2024-11-13 20-39-44](https://github.com/user-attachments/assets/6a72e731-5c9f-4af7-838f-b9743553e912)

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Screenshot from 2024-11-13 20-48-10](https://github.com/user-attachments/assets/1c96960c-0388-42e3-9d31-6290dd1d6459)

Final edited spice file ready for ngspice simulation

![Screenshot from 2024-11-13 21-47-15](https://github.com/user-attachments/assets/d2c9bffb-ac83-4b28-9ed3-deee778a060b)

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

![Screenshot from 2024-11-13 22-03-09](https://github.com/user-attachments/assets/f95a12a5-6c95-4508-99cf-83f58241df32)

Screenshot of generated plot

![Screenshot from 2024-11-13 22-07-50](https://github.com/user-attachments/assets/1d675349-26b7-433c-ac94-f53a90a94f66)

Rise transition time calculation

```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

20% Screenshots

![Screenshot from 2024-11-13 22-31-48](https://github.com/user-attachments/assets/39b9c8e0-3b4e-4c1c-a168-981ea0af0407)
![Screenshot from 2024-11-13 22-32-14](https://github.com/user-attachments/assets/aca69d67-b828-40e4-9be4-35f0fb5c776d)
80% Screenshots

![Screenshot from 2024-11-13 22-35-48](https://github.com/user-attachments/assets/5ee90d7b-ae91-4879-931f-99ea290b596b)
![Screenshot from 2024-11-13 22-37-00](https://github.com/user-attachments/assets/780c3ba5-93ed-48e0-b88a-d0b0a790e7c2)

```math
Rise\ transition\ time = 2.245 - 2.18209 = 0.06291\ ns = 62.91\ ps
```

Fall transition time calculation

```math
Fall\ transition\ time = Time\ taken\ for\ output\ to\ fall\ to\ 20\% - Time\ taken\ for\ output\ to\ fall\ to\ 80\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

20% Screenshots

![Screenshot from 2024-11-13 22-51-06](https://github.com/user-attachments/assets/cf732259-7171-41ab-8a9b-68f025a06cd6)
![Screenshot from 2024-11-13 22-51-13](https://github.com/user-attachments/assets/bcb7cbc5-4559-4b4f-b14f-3a006491df37)

80% Screenshots

![Screenshot from 2024-11-13 22-55-19](https://github.com/user-attachments/assets/b2d60cd0-3424-4af5-9de4-f22a70281c0e)
![Screenshot from 2024-11-13 22-55-30](https://github.com/user-attachments/assets/62a48644-c3b5-45d3-bdfc-b551a9e35155)
```math
Fall\ transition\ time = 4.0956 - 4.0543  = 0.0413\ ns = 41.3\ ps
```

Rise Cell Delay Calculation

```math
Rise\ Cell\ Delay = Time\ taken\ for\ output\ to\ rise\ to\ 50\% - Time\ taken\ for\ input\ to\ fall\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

50% Screenshots

![Screenshot from 2024-11-13 22-59-23](https://github.com/user-attachments/assets/4ffd3bfe-b82b-42e4-ae9f-7d594847f2ec)
![Screenshot from 2024-11-13 22-59-29](https://github.com/user-attachments/assets/1e39fa1d-8f58-48d8-80ca-e41d5f9a1899)

```math
Rise\ Cell\ Delay = 2.21165 - 2.15 = 0.06165\ ns = 61.65\ ps
```

Fall Cell Delay Calculation

```math
Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

50% Screenshots
![Screenshot from 2024-11-13 23-05-54](https://github.com/user-attachments/assets/8c7766fb-85c9-40f9-8c2f-d0d2ab570c2e)
![Screenshot from 2024-11-13 23-06-11](https://github.com/user-attachments/assets/86875067-4e2e-43d0-a718-cb2e5d17e5a7)

```math
Fall\ Cell\ Delay = 4.07 - 4.05 = 0.02\ ns = 20\ ps
```

#### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```

Screenshots of commands run

![Screenshot from 2024-11-13 23-11-42](https://github.com/user-attachments/assets/a5319fed-17f6-4f66-ad97-47f65b830771)

Screenshot of .magicrc file

![Screenshot from 2024-11-13 23-12-13](https://github.com/user-attachments/assets/0702ba32-7489-4a41-b10a-b5ea62fa4e71)

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

![Screenshot from 2024-11-13 23-32-47](https://github.com/user-attachments/assets/07e08b7f-a115-4aaa-b3a4-32ee3cf49ddb)

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

![Screenshot from 2024-11-14 00-41-35](https://github.com/user-attachments/assets/b0e7fd8c-df99-45ab-9b6c-9d0ea717e117)
![Screenshot from 2024-11-14 00-42-54](https://github.com/user-attachments/assets/1b42dd7d-e624-4493-914c-f51aa69e8c00)
![Screenshot from 2024-11-14 01-31-38](https://github.com/user-attachments/assets/7eb46dcf-b2c6-451d-9b9a-82ac98522f23)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-21 23-58-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/dc30df54-3282-42f0-8e7d-fc4d8877ed64)
![Screenshot from 2024-03-21 23-09-50](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d112ca07-854c-41e7-8822-c269e21defea)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-21 23-13-11](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b18e8e07-ef0f-40fb-9b6d-8aae878a23c6)
![Screenshot from 2024-11-14 01-46-24](https://github.com/user-attachments/assets/70b2090f-8b98-4540-a481-7181c721732c)

**Incorrectly implemented difftap.2 simple rule correction**

Screenshot of difftap rules

![Screenshot from 2024-11-14 01-40-10](https://github.com/user-attachments/assets/7be4dd74-9207-45f3-9362-68f842a54995)
Incorrectly implemented difftap.2 rule no drc violation even though spacing < 0.42u

![Screenshot from 2024-11-14 02-26-56](https://github.com/user-attachments/assets/6512f778-f754-4fd0-a996-9531be113b65)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-22 00-26-43](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b5892f9b-9c5d-4b1b-baa2-6fe45f3965b1)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```


**Incorrectly implemented nwell.4 complex rule correction**

Screenshot of nwell rules

![Screenshot from 2024-11-14 02-30-15](https://github.com/user-attachments/assets/6bd3b3c2-3cbe-4a54-905f-cd9966c4a691)

Incorrectly implemented nwell.4 rule no drc violation even though no tap present in nwell

![Screenshot from 2024-11-14 02-31-33](https://github.com/user-attachments/assets/559d8612-5d2f-40ec-b223-4ce998f0cc9a)
![Screenshot from 2024-11-14 02-52-09](https://github.com/user-attachments/assets/de62851c-807a-4102-b3a6-28f838316fa0)

New commands inserted in sky130A.tech file to update drc

![Screenshot 2024-11-15 145641](https://github.com/user-attachments/assets/b5bbfffb-3411-453d-95a5-c8529b15deac)
![Screenshot 2024-11-15 145551](https://github.com/user-attachments/assets/120069b0-32a8-4a13-bb38-ead1b859310e)


Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```


## Section 4 - Pre-layout timing analysis and importance of good clock tree

### Theory

### Implementation

* Section 4 tasks:-
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.
2. Save the finalized layout with custom name and open it.
3. Generate lef from the layout.
4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.
5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.
6. Run openlane flow synthesis with newly inserted custom inverter cell.
7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.
9. Do Post-Synthesis timing analysis with OpenSTA tool.
10. Make timing ECO fixes to remove all violations.
11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.
12. Post-CTS OpenROAD timing analysis.
13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.


#### 1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

Conditions to be verified before moving forward with custom designed cell layout:
* Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
* Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
* Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.

Commands to open the custom inverter layout

```bash
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of tracks.info of sky130_fd_sc_hd

![Screenshot from 2024-11-14 03-03-52](https://github.com/user-attachments/assets/08dbdd5e-e866-4fb8-a9a8-dd23ac28f8f6)

Commands for tkcon window to set grid as tracks of locali layer

```tcl
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of commands run

![Screenshot from 2024-11-14 03-10-13](https://github.com/user-attachments/assets/ef082f46-0f85-489c-8c3e-d41a6722d1d3)

Condition 1 verified

![Screenshot from 2024-11-14 03-14-31](https://github.com/user-attachments/assets/1a4e090d-b339-402c-accf-a40a0ba256a6)

Condition 2 verified

```math
Horizontal\ track\ pitch = 0.46\ um
```

![Screenshot from 2024-11-14 03-17-06](https://github.com/user-attachments/assets/3906ae44-491f-4f30-89cb-39f0bcc3474c)
```math
Width\ of\ standard\ cell = 1.38\ um = 0.46 * 3
```

Condition 3 verified

```math
Vertical\ track\ pitch = 0.34\ um
```

![Screenshot from 2024-11-14 03-18-31](https://github.com/user-attachments/assets/8c802256-848a-41ed-a331-f9ebea111bab)
```math
Height\ of\ standard\ cell = 2.72\ um = 0.34 * 8
```

#### 2. Save the finalized layout with custom name and open it.

Command for tkcon window to save the layout with custom name

```tcl
# Command to save as
save sky130_gourab_vsdinv.mag
```

Command to open the newly saved layout

```bash
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

Screenshot of newly saved layout

![Screenshot from 2024-11-15 01-12-05](https://github.com/user-attachments/assets/e72326e6-0b79-4569-ac9a-69fb5b9e9554)

#### 3. Generate lef from the layout.

Command for tkcon window to write lef

```tcl
# lef command
lef write
```

Screenshot of command run

![Screenshot from 2024-11-14 03-26-51](https://github.com/user-attachments/assets/12a23a12-40c1-4319-bcc1-4e8a951eb9df)

Screenshot of newly created lef file

![Screenshot from 2024-11-14 03-31-48](https://github.com/user-attachments/assets/e1b16b80-4655-43ea-8fde-48ed9bdd6e34)

#### 4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

```bash
# Copy lef file
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

#### 5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow

```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```


#### 6. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshots of commands run
![Screenshot from 2024-11-14 03-57-49](https://github.com/user-attachments/assets/fe38af18-31cf-4007-998c-e2831cf0741f)
![Screenshot from 2024-11-15 01-22-31](https://github.com/user-attachments/assets/5c9b6c5d-1983-4e7f-85cb-abdb6ecbb748)
![Screenshot from 2024-11-15 01-23-55](https://github.com/user-attachments/assets/3f917dcd-6ad6-48d5-a8aa-d82be89fb700)

#### 7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Noting down current design values generated before modifying parameters to improve timing

![Screenshot from 2024-11-15 01-22-31](https://github.com/user-attachments/assets/5c9b6c5d-1983-4e7f-85cb-abdb6ecbb748)
![Screenshot from 2024-11-15 01-23-55](https://github.com/user-attachments/assets/3f917dcd-6ad6-48d5-a8aa-d82be89fb700)

Commands to view and change parameters to improve timing and run synthesis

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 13-11_10-35 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshot of merged.lef in `tmp` directory with our custom inverter as macro

![Screenshot from 2024-11-15 16-03-02](https://github.com/user-attachments/assets/6d6101e9-774b-4f74-969c-20a357b4aa46)
Screenshots of commands run

![Screenshot from 2024-11-15 01-32-41](https://github.com/user-attachments/assets/586d2abe-8308-4ffa-941b-031af8211bf6)
![Screenshot from 2024-11-15 01-33-02](https://github.com/user-attachments/assets/e08935d7-cb83-4723-9131-abb425f65ee2)
![Screenshot from 2024-11-15 01-33-11](https://github.com/user-attachments/assets/5fb690a2-46e4-4283-850b-56b6752813fc)

Comparing to previously noted run values area has increased and worst negative slack has become 0
![Screenshot from 2024-11-15 01-34-34](https://github.com/user-attachments/assets/67f80a45-237c-41a6-81db-bc64b8e7895d)
![Screenshot from 2024-11-15 01-34-28](https://github.com/user-attachments/assets/0abc66fb-2d4a-420b-b533-579ab3d79c4b)

#### 8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command

```tcl
# Now we can run floorplan
run_floorplan
```

Screenshots of command run
![Screenshot from 2024-11-15 01-37-16](https://github.com/user-attachments/assets/55e16625-29ec-4815-bb61-992572381b8a)
![Screenshot from 2024-11-15 01-37-02](https://github.com/user-attachments/assets/8fa93848-571c-4a8d-8482-5fd2e7171293)

Since we are facing unexpected un-explainable error while using `run_floorplan` command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on `Floorplan Commands` section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```tcl
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or
```

Screenshots of commands run

![Screenshot from 2024-11-15 01-39-12](https://github.com/user-attachments/assets/8807821a-aef9-45ce-a28f-24f463616580)
![Screenshot from 2024-11-15 01-37-16](https://github.com/user-attachments/assets/9b2e709f-3427-4eab-8972-b6f0b9324777)

Now that floorplan is done we can do placement using following command

```tcl
# Now we are ready to run placement
run_placement
```

Screenshots of command run

![Screenshot from 2024-11-15 01-39-40](https://github.com/user-attachments/assets/c2dd00a2-bcc4-46c0-81a8-5936bd6656fd)
![Screenshot from 2024-11-15 01-40-36](https://github.com/user-attachments/assets/1d57e466-558b-49ec-b32c-517a0cda87b6)

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-35/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of placement def in magic
![Screenshot from 2024-11-14 04-37-36](https://github.com/user-attachments/assets/0674736d-ffad-4e75-bf04-259587a5f3b1)
Inverter instancewith my name
![Screenshot from 2024-11-14 05-49-31](https://github.com/user-attachments/assets/7e2e2e2e-ce99-4449-84e1-9aee26439322)
Screenshot of inverter with my name 
![Screenshot from 2024-11-15 15-24-14](https://github.com/user-attachments/assets/4b018eb3-7941-414f-b621-8fcfa2fcc6c9)
![Screenshot from 2024-11-15 15-24-29](https://github.com/user-attachments/assets/6b5a72d3-0e30-4aaa-bded-bd157c31f5e7)
Command for tkcon window to view internal layers of cells

```tcl
# Command to view internal connectivity layers
expand
```
#### 9. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands run final screenshot

![Screenshot from 2024-11-14 07-54-02](https://github.com/user-attachments/assets/b505fcdf-7f25-4891-b747-16d9e526558a)


Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

![Screenshot from 2024-11-14 08-27-58](https://github.com/user-attachments/assets/db798aa8-ca99-4267-9cff-94e7216defbd)
![Screenshot from 2024-11-14 09-05-08](https://github.com/user-attachments/assets/35665566-d1ae-4d09-a5b8-d71928f69489)

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis 

```tcl
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 13-11_10-15 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

![Screenshot from 2024-11-14 09-55-48](https://github.com/user-attachments/assets/a19c50fa-edf9-49f6-883d-397b05a4c89d)

#### 10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts

![Screenshot from 2024-11-14 09-57-32](https://github.com/user-attachments/assets/92080e95-41e0-4f22-9791-59f4d2be0e5b)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11675_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-11-14 09-58-11](https://github.com/user-attachments/assets/d17b46da-df25-4a2d-8a78-e6f492652810)

OR gate of drive strength 2 is driving 4 fanouts

![Screenshot from 2024-11-14 09-59-41](https://github.com/user-attachments/assets/329e3510-b55f-4090-8ad4-31a985c993ba)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-11-14 10-00-15](https://github.com/user-attachments/assets/6992e92d-4ac0-4410-8d96-f45500922c68)
![Screenshot from 2024-11-14 10-01-56](https://github.com/user-attachments/assets/cf2548dc-62b7-431c-a9c4-2909961ee67f)
OR gate of drive strength 2 driving OA gate has more delay

![Screenshot from 2024-03-26 10-22-10](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7669eeef-7b93-4cfd-a152-17eec772496a)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```


OR gate of drive strength 2 driving OA gate has more delay

![Screenshot from 2024-03-26 10-32-27](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4185dc98-a065-49d3-a92f-fb4b02f23ee5)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-11-14 10-14-01](https://github.com/user-attachments/assets/b9454abe-d2ad-483a-8062-003a51f4c0c5)
![Screenshot from 2024-11-14 10-14-22](https://github.com/user-attachments/assets/3e98350f-3bde-40c7-ac25-2fe8b393df60)

Commands to verify instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

```tcl
# Generating custom timing report
report_checks -from _30537_ -to _30440_ -through _14509_
```

Screenshot of replaced instance

![Screenshot from 2024-11-14 10-18-37](https://github.com/user-attachments/assets/fb4f678b-06ea-4d49-b162-338b0c421c2c)

*We started ECO fixes at wns -22.72 and now we stand at wns -21.45 we reduced around 1.27 ns of violation*

#### 11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use `write_verilog` and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist

```bash
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

Commands to write verilog

```tcl
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

Screenshot of commands run

![Screenshot from 2024-11-14 10-33-13](https://github.com/user-attachments/assets/1a055e65-5c90-481e-99e0-016c1711ae84)


Verified that the netlist is overwritten by checking that instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

![Screenshot from 2024-11-14 10-33-45](https://github.com/user-attachments/assets/a5a96f48-1a53-446a-a8d2-fb8926092a40)

Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 13-11_10-15 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```

Screenshots of commands run

![Screenshot from 2024-11-14 10-41-04](https://github.com/user-attachments/assets/9ae410e3-1967-4613-955f-6f6ed8b87021)
![Screenshot from 2024-11-14 10-41-13](https://github.com/user-attachments/assets/df7b3240-e183-4791-a8e1-ee395fd0864c)
![Screenshot from 2024-11-14 10-43-37](https://github.com/user-attachments/assets/869dfa46-4998-4746-b60c-894a1596aac4)
![Screenshot from 2024-11-14 10-45-52](https://github.com/user-attachments/assets/9011c610-78ea-4553-9667-741f11206e4e)
#### 12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_10-15/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_10-15/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_10-15/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
screenshot
![Screenshot from 2024-11-14 10-54-05](https://github.com/user-attachments/assets/40d94ac8-8198-4665-81db-f117a5b09c64)

#### 13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing `CTS_CLK_BUFFER_LIST`

```tcl
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/13-11_10-15/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_10-15/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_10-15/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```

Screenshots of commands run and timing report generated

![Screenshot from 2024-11-14 10-46-11](https://github.com/user-attachments/assets/1c3c5110-8a2d-4e19-9d42-c8c508436878)
![Screenshot from 2024-11-14 10-50-30](https://github.com/user-attachments/assets/55c65534-1339-4da6-bad7-848b99a2c82a)
![Screenshot from 2024-11-14 10-50-32](https://github.com/user-attachments/assets/b35edd1d-c026-4294-ac74-a35877c15d85)
![Screenshot from 2024-11-14 10-50-35](https://github.com/user-attachments/assets/a7bbd6bd-eec3-48af-af05-4b991291a185)
![Screenshot from 2024-11-14 10-54-05](https://github.com/user-attachments/assets/40d94ac8-8198-4665-81db-f117a5b09c64)


## Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA 

### Theory

### Implementation

* Section 5 tasks:-
1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
2. Perfrom detailed routing using TritonRoute.
3. Post-Route parasitic extraction using SPEF extractor.
4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.



#### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn 
```

Screenshots of power distribution network run
![Screenshot from 2024-11-14 11-28-20](https://github.com/user-attachments/assets/37e0cc12-fab2-4725-a208-39ca89f3bbf9)
![Screenshot from 2024-11-14 11-28-27](https://github.com/user-attachments/assets/31f4cb41-7daa-4b1a-bd1e-6498000c7b27)

Commands to load PDN def in magic in another terminal

```bash
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-15/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

Screenshots of PDN def
![Screenshot from 2024-11-14 11-35-33](https://github.com/user-attachments/assets/4c10adf6-f768-4415-aaaf-35619176c9dc)
Inverter with my name
![Screenshot from 2024-11-14 23-45-58](https://github.com/user-attachments/assets/abaf49b6-56b4-4597-903d-4f3a13b1d2ce)
![Screenshot from 2024-11-14 11-36-41](https://github.com/user-attachments/assets/a3006dbd-b711-446c-86ab-219ceff4750f)
![Screenshot from 2024-11-14 11-37-32](https://github.com/user-attachments/assets/5feb84d6-e78e-44c1-ac0b-e7f93b597147)

#### 2. Perfrom detailed routing using TritonRoute and explore the routed layout.

Command to perform routing

```tcl
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```

Screenshots of routing run

![Screenshot from 2024-11-14 11-41-27](https://github.com/user-attachments/assets/1c12d7b7-d8db-4728-a861-0ecaaf48b03b)
![Screenshot from 2024-11-14 11-55-21](https://github.com/user-attachments/assets/43ad6f3d-2195-4b08-af18-72565755771c)

Commands to load routed def in magic in another terminal


```bash
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-15/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

Screenshots of routed def

![Screenshot from 2024-11-14 11-59-56](https://github.com/user-attachments/assets/993c6bee-0147-4f35-a498-d302ebdae409)

![Screenshot from 2024-11-14 12-00-28](https://github.com/user-attachments/assets/5c436a19-8c6f-4daf-86a8-1067d839db8a)
![Screenshot from 2024-11-14 12-01-11](https://github.com/user-attachments/assets/963561cf-42a8-4509-9725-0009f324b8b9)
![Screenshot from 2024-11-14 12-02-15](https://github.com/user-attachments/assets/707798fc-e95d-48a6-93d2-f630f805aee6)
![Screenshot from 2024-11-14 12-02-19](https://github.com/user-attachments/assets/6fc1c89d-894a-457c-8b6f-9235b33e1f65)

Screenshot of fast route guide present in `openlane/designs/picorv32a/runs/13-11_10-15/tmp/routing` directory

![Screenshot from 2024-11-14 12-09-02](https://github.com/user-attachments/assets/fbc0ef75-84d2-49d1-baa7-34e05fc7ebbe)

#### 3. Post-Route parasitic extraction using SPEF extractor.

Commands for SPEF extraction using external tool

```bash
# Change directory
cd Desktop/work/tools/SPEF_EXTRACTOR

# Command extract spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-15/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_10-15/results/routing/picorv32a.def
```

#### 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_10-15/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/13-11_10-15/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_10-15/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of commands run and timing report generated
![Screenshot from 2024-11-14 13-08-06](https://github.com/user-attachments/assets/fbef85fe-e78f-4515-b6cf-ec39c93889fd)
![Screenshot from 2024-11-14 13-08-11](https://github.com/user-attachments/assets/03ad52de-51f7-41d6-8230-ae1cb098db39)
![Screenshot from 2024-11-14 13-08-14](https://github.com/user-attachments/assets/3327da0b-8830-483f-8126-2cd336adf1d0)
![Screenshot from 2024-11-14 13-09-34](https://github.com/user-attachments/assets/b4947947-d87a-47fb-835b-f4f1d39711a9)

</details>
