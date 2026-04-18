# RISC_V
every instruction a CPU executes goes through the following workflow in order: 

- Fetch : basically grab the next instruction from memory
- Decode : figure out what instruction it is and read the registers
- Execute : do the actual math (ALU!!!!)
- Memory : read or write to RAM if needed...
- Writeback : put the result back into a register

## 1. Fetch and PC:

##### deliverable: a module that takes a clock, outputs a 32-bit instruction and current PC every cycle, accepts a "jump to this address" signal from execute
```
inputs:  clk, rst, branch_taken, branch_target
outputs: instruction[31:0], pc[31:0]
```
## 2. Decode and Register File
#### deliverable: a module that takes a 32-bit instruction, outputs decoded fields and two register values
```
inputs:  instruction[31:0], clk, wr_en, rd_addr, wr_data
outputs: rs1_data[31:0], rs2_data[31:0], immediate[31:0], alu_op
```
## 3. ALU
#### deliverable: a module that takes two 32-bit inputs and an operation code, outputs a 32-bit result
```
inputs:  a[31:0], b[31:0], alu_op[3:0]
outputs: result[31:0], zero_flag, negative_flag
ALU operations to implement:
ADD, SUB, AND, OR, XOR, SLL (shift left), SRL (shift right), SLT (set less than)
```
## 4. Memory, Writeback and Integration
- data memory : a simple RAM for load (LW) and store (SW) instructions
- writeback mux : decides whether to write ALU result or memory data back to register file
- most importantly: connecting all 4 modules together into one top-level CPU
- testbench that runs an actual RISC-V program through the whole thing
#### deliverable: top level module, memory module and final testbench

## Goals

- PicoRV32[https://github.com/YosysHQ/picorv32] is our reference implementation. when we build our fetch stage, open picorv32.v and find the fetch logic.

- CVA6[https://github.com/openhwgroup/cva6] is our stretch goal. the 6-stage pipeline our team is building is essentially a smol version of what CVA6 does.
