# RISC-V MYTH

RISC-V MYTH workshop is offered by [Vlsi system design](https://www.vlsisystemdesign.com/) and [Redwood EDA](https://www.redwoodeda.com).

- In this workshop, we learn about RISC-V ISA , Basic of TL-Verilog , use of [makerchip](https://www.makerchip.com) by Redwood EDA. We finally end the workshop with a capstone project making a RISC-V CPU core using TL-verilog in makerchip platform.

# CONTENTS
| DAY         | TOPIC         |
|-------------|---------------|
| 1 | [Introduction to RISC-V ISA and GNU compiler toolchain](#day-1---introduction-to-risc-v-isa-and-gnu-compiler-toolchain) |
| 2 | [Re-writing the C program using ASM language](#day-2---re-writing-the-c-program-using-asm-language) |
| 3 | [Digital Logic with TL-Verilog and Makerchip](#day-3---digital-logic-with-tl-verilog-and-makerchip) |
| 4 | [Basic RISC-V CPU micro-architecture](#day-4---basic-risc-v-cpu-micro-architecture) |
| 5 | [Complete Pipelined RISC-V CPU micro-architecture](#day-5---complete-pipelined-risc-v-cpu-micro-architecture) |


# DAY 1 - Introduction to RISC-V ISA and GNU compiler toolchain

## C-program to compute sum from one to n

Open your linux and nagivate to the ternimal

- Open your prefered text editor and write the C program code to compute the sum of N numbers.

   - ![code](images/d1_sc_1.png)

- Now, Run it using GCC command

  - ![code](images/d1_sc_2.png)

- Next step is to use the RISCV complier run the c code and look at the assembly code.
- First we use the below command to run the code . 
  
```
riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
   - ![riscv complier command](images/d1_sc_3.png)

This converts the code into assembly language.

```
riscv64-unknown-elf-objdump -d sum1ton.o | less
```
  - ![riscv assemply](images/d1_sc_4.png)

- Now, change the complier command and observe the number of instructuons taken to run the same code.

```
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

  - ![riscv](images/d1_sc_5.png)

We can observe that here, it has taken 12 instructions to run the main fucntion compared to 15 instructions using the -o1 in the complier command.


- Next step is to use SPIKE to run the object file we created to get the output of the C program. Use  the below command for that
```
spike pk sum1ton.o
```
  - ![spike command](images/d1_sc_6.png)


We can also run each instruction manually. For that we fiest enter the manual control using SPIKE with the below command
```
spike -d pk sum1ton.o
```

Then we run the program counter til the first instruction using the below command
```
until pc 0 100b0
```

From there we can manually run the program by going through each instruction just by pressing enter in the keyboard.

You can also view the conents in the resgisters using the below command 
```
reg 0 a2
```
Here a is the register.

- Below we have used certain SPIKE commands to show the contents in specific registers.
   - ![spike command](images/d1_sc_7.png)


## C Code for highest and lowest signed and unsigned numbers

   - ![c code](images/d1_sc_8.png)

Now we run it using the RISCV ISA based compiler

```
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o intg.o integers.c
```
```
spike -pk intg.o
```
  - ![output](images/d1_sc_9.png)


# DAY 2 - Re-writing the C program using ASM language

- First we modify the C code to get the sum of numbers as below.
  - ![d2_sc_1](images/d2_sc_1.png)

- Then we write an Assembly language program for the same code as below.
  - ![d2_sc_2](images/d2_sc_2.png)

- Now we run the C code using RISCV ISA with GCC complier.
  - ![d2_sc_3](images/d2_sc_3.png)

  - ![d2_sc_4](images/d2_sc_4.png)


## Converting C code to HEX format

- Next, we convert the c code to hex format using the below shell script code and pass it to the picorv32 core for computation.
  For this first nagivate to the labs folder inside the riscv_workshop_collateral folder.

Use the below command to view the script
```
cat rv32im.sh
```

  - ![shell script](images/d2_sc_5.png)

- Now use the below commands to execute the script

```
chmod 777 rv32im.sh

./rv32im.sh
```

  - ![shell script](images/d2_sc_6.png)


# DAY 3 - Digital Logic with TL-Verilog and Makerchip

## Introduction

**TL-Verilog** (Transaction-Level Verilog) is an extension of SystemVerilog designed to simplify hardware design by introducing transaction-level modeling concepts directly into RTL development. It helps reduce complexity, improve reusability, and enhance productivity by incorporating pipeline-driven design principles.

**Makerchip** is an online IDE that provides an interactive, cloud-based environment for TL-Verilog-based development by RedWood EDA.

# Table of LABS (DO NOT EDIT THE CODE IN THE SANDBOX LINKS)

| S.NO | LABS | Sandboxes (Links) |
|----------|----------|-----------------|
| 1 | Combinational circuits (lab till mux)   | [Output](#combinational-circuits) |
| 2 | Combinational calculator implementation   | [Sandbox](https://makerchip.com/sandbox/0zpfRhoYN/0oYhrkB#) |
| 3 | 32 bit counter   | [Sandbox](https://makerchip.com/sandbox/0n5fGhmRZ/0nZh7Qz) |
| 4 | Sequential Calculator   | [Sandbox](https://makerchip.com/sandbox/0n5fGhmRZ/0nZh7Qz) |
| 5 | Recreating the pipelined logic   | [Sandbox](https://makerchip.com/sandbox/0G6fJhk2x/0P1hKqp) |
| 6 | 2-Cycle Caculator   | [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/0Rghv83#) |
| 7 | 2-Cycle Caculator using validity   | [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/0Z4h5nP) |
| 8 | Caculator with single value memory   | [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/048hBAm) |
| 9 | 3-D pythagoras theorem   | [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/00ghG00) |


Below is an example of the interface of makerchip platform.

![makerchip ex1](images/d3_sc_1.png)

## Combinational circuits

- A combinational circuit is a type of digital circuit in which the output depends only on the current inputs and not on any past inputs. Basic logic gates such as not gate, or gate , and gate, etc. are examples of combinational cirtuit.

  - Here we have a TL-verilog example of a not gate

  - ![Not gate TL verilog](images/d3_sc_2.png)

- Next we use vectors using TL-verilog . Below is an example .

  - ![Vectors TL verilog](images/d3_sc_3.png)

- Implementation of a multiple bit <ins>**MUX**</ins> using TL-verilog in makerchip platform.

  - ![Vectors TL verilog](images/d3_sc_4.png)

- <ins>**Combinational calculator implementation**</ins> [Sandbox](https://makerchip.com/sandbox/0zpfRhoYN/0oYhrkB#)

  - ![calculator TL verilog](images/d3_sc_5.png)

## **Sequential circuits**

A sequential circuit is a type of digital circuit where the output depends not only on the current inputs but also on past inputs (i.e., it has memory)

### Lab 
- <ins>**32 bit counter**</ins> [ [Sandbox](https://makerchip.com/sandbox/0n5fGhmRZ/0nZh7Qz) ]

  - ![32 bit counter using TL-verilog in makerchip](images/d3_sc_6.png)


- <ins>**Sequential Calculator**</ins> [ [Sandbox](https://makerchip.com/sandbox/0n5fGhmRZ/0nZh7Qz) ]

  - ![Sequential Calculator using TL-verilog in makerchip](images/d3_sc_7.png)


## **PIPELINING**

Pipelining is a design technique used to improve performance by dividing a task into multiple stages, where each stage operates in parallel. It allows for higher throughput by processing multiple instructions or data elements simultaneously.

- <ins>**lab-1- Recreating the pipelined logic**</ins> [ [Sandbox](https://makerchip.com/sandbox/0G6fJhk2x/0P1hKqp) ]

  - ![Pipelined logic example](images/d3_sc_8.png)


- <ins>**2-Cycle Calculator**</ins> [ [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/0Rghv83#) ]

  - ![2-Cycle Calculator](images/d3_sc_9.png)

## **VALIDITY**

Validity in TL-Verilog is a mechanism that ensures logic executes only when needed. It acts like an implicit enable signal, preventing unnecessary operations and improving efficiency.


- Validity controls execution: Logic runs only when the stage is valid.

- Uses ! (validity assertion): Marks a signal or stage as valid only when needed.

- Improves efficiency: Prevents unnecessary computations and reduces power consumption.


- <ins>**2-Cycle Caculator using validity**</ins> [ [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/0Z4h5nP) ]

  - ![2-Cycle Caculator using validity](images/d3_sc_10.png)


- <ins>**Caculator with single value memory**</ins> [ [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/048hBAm) ]

  - ![Caculator with single value memory](images/d3_sc_11.png)


## **HIERARCHY**

In TL-Verilog, hierarchy is used to organize complex designs into reusable modules. Unlike traditional Verilog, TL-Verilog naturally supports hierarchical design through nested pipelines and modular structures.

- <ins>**3-D pythagoras theorem**</ins> [ [Sandbox](https://makerchip.com/sandbox/0mZf5hQJx/00ghG00) ]

  - ![3-D pythagoras theorem](images/d3_sc_12.png)

# Day 4 - Basic RISC-V CPU micro-architecture

- Now we will start to code the RISC-V cpu core . Below is a rough architecture diagram of the core.
   - ![CPU core architecture](images/d4_sc_1.png)

- First step is to add a program counter (PC) to the design . Open up the RISC-V lab boiler plate from [here](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Frisc-v_shell.tlv)
   - ![PC addition](images/d4_sc_2.png)

- Next Step is enable the pre-defined **imem** blocks and assign the signals respectively. After that, your blocks should look like this.
   - ![Adding imem](images/d4_sc_3.png)
  

 



# Day 5 - Complete Pipelined RISC-V CPU micro-architecture
