# ASIC Design Class
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


## RISC-V COMPILER OUTPUT and DEBUGGING

Find the output of the C program on the RISC V Compiler using the Spike command and debug the code

## Check the Output and compare with previous output.
In our previous lab, we compiled our C code using both gcc and a RISC-V compiler. In assignment 3 we are going to have find the result of n numbers on the RISCV compiler using the SPIKE command.

### Code for compiling the objdump file
```bash
spike pk sum1ton.o
```
### The compiled code using SPIKE command along with object dump file
![Similar result for GCC and RISCV compiler when run the program](https://github.com/user-attachments/assets/7a290394-20ed-4082-a01b-4ce624035b23)
* Here we can see that we get the same output in both the process.

![objectdump that is to be debugged](https://github.com/user-attachments/assets/4f80656e-beaa-4453-b348-0072af306c4a)

## Debug using SPIKE debugger

### Code for debugging the assembly code 
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

## The same thing happens for -O1 as well

```bash
until pc 0 10184
```

![Screenshot 2024-07-21 220856](https://github.com/user-attachments/assets/d852482f-7f0a-42ca-85f1-5c8de1839ed1)
![Screenshot 2024-07-21 220943](https://github.com/user-attachments/assets/abbe402d-6c31-436b-8d7e-cec5f1690be2)

## ASSIGNMENT 3
### 1. Identifying Instruction Types

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
   
  
  ## Decoding each instruction type provided: 

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

