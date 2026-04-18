# RISC_V
every instruction a CPU executes goes through the following workflow in order: 
```
Fetch 
  |
  V
Decode 
  |
  V
Execute 
  |
  V
Memory 
  |
  V
Writeback
```

Fetch : basically grab the next instruction from memory
Decode : figure out what instruction it is and read the registers
Execute : do the actual math (this is where your ALU!!!!)
Memory : read or write to RAM if needed...
Writeback : put the result back into a register

## 1. Fetch and PC:
- program counter (PC): a register that holds the address of the current instruction
- instruction memory: a simple block of memory holding your program
- fetching the 32-bit instruction and passing it to decode
- handling jumps and branches (updating PC to a new address when needed)
##### deliverable: a module that takes a clock, outputs a 32-bit instruction and current PC every cycle, accepts a "jump to this address" signal from execute
```
inputs:  clk, rst, branch_taken, branch_target
outputs: instruction[31:0], pc[31:0]
```
## 2. Decode and Register File
- parsing the 32-bit instruction into fields (opcode, rs1, rs2, rd, immediate)
- the register file : all 32 registers (x0 through x31), reads two at once, writes one
- figuring out what operation the execute stage needs to do
- x0 is always hardwired to zero
Cool, so this is the RISC-V instruction format (shamelessly copied from a clanker)
```
R-type: [funct7|rs2|rs1|funct3|rd|opcode]  ← register operations
I-type: [imm[11:0]|rs1|funct3|rd|opcode]   ← loads, immediate ops
S-type: [imm|rs2|rs1|funct3|imm|opcode]    ← stores
B-type: [imm|rs2|rs1|funct3|imm|opcode]    ← branches
```
#### deliverable: a module that takes a 32-bit instruction, outputs decoded fields and two register values
```
inputs:  instruction[31:0], clk, wr_en, rd_addr, wr_data
outputs: rs1_data[31:0], rs2_data[31:0], immediate[31:0], alu_op
```
## 3. ALU
- the ALU (Arithmetic Logic Unit) : does ADD, SUB, AND, OR, XOR, shifts, comparisons
- branch condition evaluation (is rs1 == rs2? is rs1 < rs2?)
- forwarding/hazard detection if you want to get fancy
- deliverable: a module that takes two 32-bit inputs and an operation code, outputs a 32-bit result
- inputs:  a[31:0], b[31:0], alu_op[3:0]
- outputs: result[31:0], zero_flag, negative_flag
- ALU operations to implement:
```
ADD
SUB
AND
OR
XOR
SLL (shift left)
SRL (shift right)
SLT (set less than)
```
## 4. Memory, Writeback and Integration
- data memory : a simple RAM for load (LW) and store (SW) instructions
- writeback mux : decides whether to write ALU result or memory data back to register file
- most importantly: connecting all 4 modules together into one top-level CPU
- testbench that runs an actual RISC-V program through the whole thing
#### deliverable: top level module, memory module and final testbench
