Download Link: https://assignmentchef.com/product/solved-cs224-lab-4-mips-single-cycle-datapath-and-controller
<br>
In this lab you will use the digital design engineering tools (System Verilog HDL, Xilinx Vivado) to modify the single-cycle MIPS processor.  You will expand the instruction set of the “MIPS-lite” processor by adding new instructions to it. To do this, you must first determine the RTL expressions of the new instructions then modify the datapath and control unit of the MIPS.  To implement the new instructions will require you to modify some System Verilog modules in the HDL model of the processor.  To test and prove correctness, you will simulate the microarchitecture

<strong>Summary </strong>

<strong>Part 1</strong> : Preliminary Report/Preliminary Design Report: System Verilog model for Original10 + New instructions (Due date of this part is the same for all).

<strong>Part 2</strong> : Simulation of the MIPS-lite processor and Implementation and Testing of new instructions .

<strong>DUE DATE OF PART 1: SAME FOR ALL SECTIONS</strong> Dear students please bring and drop your preliminary work into the box provided in front of the lab before 10:40 AM on Wednesday. No late submission!

<strong>LAB WORK SUBMISSION TIMING:</strong> You have to show your lab work to your TA by <strong>12:15</strong> in the morning lab and by <strong>17:15</strong> in the afternoon lab. Note that you cannot wait for the last moment to do this. If you wait for the last moment and show your work after the deadline time 20 points will be taken off.

In this lab and the next one, we assume SystemVerilog knowledge, as well as the ability to use tools such as Xilinx Vivado, since you all took CS223 – Digital Design. If you are not familiar with these, please get used to them as soon as possible

<strong> </strong>

<strong>Part 1. Preliminary Work / Preliminary Design Report </strong>

Be sure to make a copy of the report, to use during the lab.  Your PDR should contain the following 7 items, one per page:

<ol>

 <li>a) Cover page, with university name, department name, and course name and number at the top, “Preliminary Design Report”, Lab # (e.g. 4), Section #, and your name and ID# in the middle, and the date of your lab at the bottom.</li>

 <li>b) Determine the assembly language equivalent of the machine codes given in the imem module in the “Complete MIPS model.txt” file posted on Unilica for this lab. In the given System Verilog module for imem, the hex values are the MIPS machine language instructions for a small test program. Dis-assemble these codes into the equivalent assembly language instructions and give a 3-column table for the program, with one line per instruction, containing its location, machine instruction (in hex) and its assembly language equivalent. [Note: you may dis-assembly by hand or use a program tool like the one in Unilica.]</li>

 <li>c) <strong> </strong>Register Transfer Level (RTL) expressions for each of the <u>new instructions</u> that you are adding (see list below for your section), including the fetch and the updating of the PC.</li>

 <li>d) Make any additions or changes to the datapath which are needed in order to make the RTLs for the instructions possible. The base datapath should be in black, with changes marked in red and other colors (one color per new instruction).</li>

 <li>e) Make a new row in the main control table for each <u>new instruction</u> being added, and if necessary, add new columns for any new control signals that are needed (input or output). Be sure to completely fill in the table—all values must be specified. If any changes are needed in the ALU decoder table, give this table in its new form (with new rows, columns, etc). The base table should be in black, with changes marked in red and other colors. {Note: if you need new ALUOp bits to encode new values, you should also give a new version of Table 7.1, showing the new encodings}</li>

 <li>f) <strong>] </strong>Write a test program in MIPS assembly language, that will show whether the new instructions are working or not, and that will confirm that all existing old instructions still continue to work. Don’t use any pseudo-instructions; use only real MIPS instructions that will be recognized by the new control unit.</li>

</ol>

<strong> </strong>

<strong>Table of instructions to implement– by section</strong>

<table>

 <tbody>

  <tr>

   <td width="62">Section</td>

   <td width="263">MIPS instructions</td>

  </tr>

  <tr>

   <td width="62">Sec 1</td>

   <td width="263">Base: Original10New: “lui”, “jalm”</td>

  </tr>

  <tr>

   <td width="62">Sec 2</td>

   <td width="263">Base: Original10New: “ble”, “push”</td>

  </tr>

  <tr>

   <td width="62">Sec 3</td>

   <td width="263">Base: Original10New: “jm”, ”subi”</td>

  </tr>

  <tr>

   <td width="62">Sec 4</td>

   <td width="263">Base: Original10New: “sw+”, “bge”</td>

  </tr>

 </tbody>

</table>

<strong> </strong>

The Original10 instructions in “MIPS-lite” are add, sub, and, or, slt, lw, sw, beq, addi and j.

Instructions in quotes (e.g. ”push”) are not defined in the MIPS instruction set. They don’t exist in any MIPS documentation; they are completely new to MIPS.  You will create them, according to the definitions below, then implement them.

ble, bge: these I-type instructions do what you would expect—branch to the target address, if the condition is met. Otherwise, the branch is not taken. Example: ble $t2, $t7, TopLoop

jm: this I-type instruction is a jump, to the address stored in the memory location indicated in the standard way. Example: jm 40($s3)

subi: this I-type instruction subtracts, using a sign-extended immediate value. subi $t2, $t7, 4

sw+: this I-type instruction does the normal store, as expected, plus an increment (by 4, since it is a word transfer) of the base address in RF[rs]. {Note: this kind of auto-increment instructions are useful when moving through an array of data words.}

jalm: this I-type instruction is a jump, to the address stored in the memory location indicated in the standard way.  But it also puts the return address into the register specified.  Example: jalm $t5, 40($s3)




push: this I-type instruction does what you would expect—push a register value onto stack. Example: push $a3 {Note: the assembler automatically puts 29 in the rs field, and 0 into the immediate field, of the machine code instruction.}

<strong>Part 2:  Simulation and Implementation</strong>

<ol>

 <li>a) Complete the System Verilog model of single-cycle MIPS by designing a 32-bit ALU module (one is partly specified already in “Complete MIPS model.txt”) and save this ALU module by itself in a new file with a meaningful name. Make this file the basis of a new Xilinx Vivado project. In simulation, check its syntax, and then simulate this ALU, using a testbench that you will write. When you are sure that the 32-bit ALU is working correctly in simulation, you can now use it in the MIPS-lite datapath.  When you have integrated your working 32-bit ALU into the “Complete MIPS model.txt” file, you are ready to simulate the MIPS-lite single-cycle processor in Xilinx Vivado.</li>

 <li>b) Make a New Project, giving it a meaningful name, for your single-cycle MIPS-lite. Do Add Source for the System Verilog modules given in “Complete MIPS model.txt” (modified with your working ALU), and Save everything (you don’t have to use Complete MIPS model.txt, you can write your code if you find it more convenient). Make the necessary changes in order to support newly added instructions.</li>

 <li>c) Study the small test program loaded into instruction memory (in the imem module) that you dis-assembled in part b) of your Preliminary Design Report. What is the program attempting to do? Extend the imem module so that it will also include newly added instructions as well.</li>

 <li>d) Now make a System Verilog testbench file and using Xilinx Vivado, simulate your MIPS-lite processor executing the test program. Study the results given in the simulation window. Find each instruction, and understand its values. Why is writedata undefined for some of the early instructions in the program?</li>

 <li>e) Now modify the simulation, in order to show more information. Make changes to the System Verilog modules as needed so that the 32-bit values of PC and the Instruction are made to be outputs of the top-level module. Then modify the testbench file, so that they are displayed in the simulation.</li>

 <li>f) When you have studied the simulation results and can explain, for any instruction, the values of PC, instruction, writedata, dataaddr, and the memwrite signal, then <u>call the TA, show your simulation demo and answer questions for grade.</u> The purpose of the questions is to determine your knowledge level and test your ability to explain your demo, the System Verilog code and the reasons behind it, and to see if you can explain what would happen if certain changes were made to it. <strong>To get full points from the Oral Quiz, you must know and understand everything about what you have done.</strong></li>

</ol>




<strong> </strong>

<strong> </strong>

<strong>Part 3. Submit your code for MOSS similarity testing</strong>

In Part 2, you created a new top-level module for your overall system, and modified several System Verilog modules for parts of the single-cycle MIPS that needed to change in order to implement the new instructions.  Now it is time to combine all the new and modified System Verilog codes into a file called <strong>StudentID_StudentFirstName_StudentLastName_SecNo_LabNo_LAB.txt. </strong> Add the System Verilog code of the testbench module used for your simulation in part f), plus the top-level module file that you used for part f), in their final form.  Also, add all the Verilog modules whose code is new or modified from “Complete MIPS model.txt”.  DO NOT include any unmodified System Verilog modules, only those whose code you changed or created.