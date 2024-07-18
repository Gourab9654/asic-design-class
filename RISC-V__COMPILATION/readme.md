# ASIC Design Class
## RISC-V Compilation Of a simple C Program
### Step 1
Compile the c code on RISC-V compiler
### Step 2
See the dump of generated object file in assembly language
* In the Linux Environment, create a new C Program file using any editor. Here the leafpad editor is used. 
* Save the program and compile your code in the terminal window using GCC compiler
 

### Step 2
*Run the executable program and see the output in the terminal window.
*For the "main" section, we can calculate the number of instructions either by counting each individual instruction or we can subtract the address of the first instruction in the next section with the first instruction of the main section and divide the difference with 4 since it is a byte addressable memory, so 4 memory block form one instruction



E.g.: No. of instruction in main block = (0x101bc - 0x10184)/4 = 0x38/4 = 0xE = 14 instructions
### Output
The picture below represents the C code and its output

![Screenshot 2024-07-16 220224](https://github.com/user-attachments/assets/4dbde6dc-0ff0-43c4-92a3-2d6c839e3f7e)
