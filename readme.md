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

![synth_image](https://github.com/user-attachments/assets/5a53f7b9-1406-4b55-906f-ce33e8775a52)

  ------
The netlist generated in the terminal window is shown below:

![netlist_terminal](https://github.com/user-attachments/assets/0681ac71-daa7-4739-8ae1-c9757f9f30fc)

![netlist2](https://github.com/user-attachments/assets/83a7d12a-1052-4e5c-84b8-9b20608174b7)

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
![terminal_gtk](https://github.com/user-attachments/assets/3b934499-adc0-43cd-96e0-ff8576eca7ea)

----
The simulation waveforms are:

1. clk_gour,reset,VCO_IN & Output signals:
-----
In the above waveforms, we can see the following signals:

**clk_gour:**     
**reset:**     
**RV_to_DAC[9:0]:** 
**OUT:** 

**The pre synthesis simulation waveforms and the post synthesis simulation waveforms were found to be identical.
The pre synthesis simulation waveforms are shown here for reference:**


      
</details>
