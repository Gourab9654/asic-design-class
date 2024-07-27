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



# ASIC Design Class
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


# RISC-V COMPILER OUTPUT and DEBUGGING

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
