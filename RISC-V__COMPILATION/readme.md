# ASIC Design Class
## RISC-V Compilation Of a simple C Program
### Step 1
Compile the C code on RISC-V compiler
### Step 2
See the dump of generated object file in assembly language

### Step 2
* Run the executable program and see the output in the terminal window.
* For the "main" section, we can calculate the number of instructions either by counting each individual instruction or we can subtract the address of the first instruction in the next section with the first instruction of the main section and divide the difference with 4 since it is a byte addressable memory, so 4 memory block form one instruction
* E.g.: No. of instruction in main block = (0x101c0 - 0x10184)/4 = 0x38/4 = 0xE = 14 instructions

### Step 3
* Compile the code again with the compiler flag set as **-Ofast** and observe the generated assembly code

Assembly code generated with **-Ofast** compiler flag

### Observation
Here we can observe that the number of instructions are reduced to 6 as compared to 14 in the previous case
* -O1 is moderate in it's code optimization while -Ofast is highly aggressive to achieve highest possible performance
* -O1 maintains strict adherence to standards while -Ofast may violate some standards to achieve better performance


