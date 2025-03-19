# RISCV-MYTH
workshop on RISCV ISA using OpenSource tools

## DAY-1 - Introduction to RISC-V ISA and GNU compiler toolchain

#### C-program to compute sum from one to n

Open your linux and nagivate to the ternimal

Open your prefered text editor and write the C program code to compute the sum of N numbers.

![code](images/d1_sc_1.png)

Now, Run it using GCC command

![code](images/d1_sc_2.png)

Next step is to use the RISCV complier run the c code and look at the assembly code.
First we use the below command to run the code

```
riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
![riscv complier command](images/d1_sc_3.png)

This converts the code into assembly language.

```
riscv64-unknown-elf-objdump -d sum1ton.0 | less
```
![riscv assemply](images/d1_sc_4.png)

Now, change the complier command and observe the number of instructuons taken to run the same code.

```
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

![riscv](images/d1_sc_5.png)

We can observe that here, it has taken 12 instructions to run the main fucntion compared to 15 instructions using the -o1 in the complier command.
