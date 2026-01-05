![Verilog](https://img.shields.io/badge/HDL-Verilog-brightgreen)
![Vivado](https://img.shields.io/badge/Tools-Xilinx_Vivado-orange)
![License](https://img.shields.io/badge/License-MIT-blue)
![Platform](https://img.shields.io/badge/Platform-FPGA-lightgrey)


32-bit Custom Processor RTL Design Overview
A Verilog-based implementation of a 32-bit RISC-style processor designed for FPGA deployment. The architecture uses a 16-bit data path and a custom ISA optimized for minimal resource utilization compared to standard soft-core processors like MicroBlaze.

Hardware Specifications
Instruction Width: 32-bit fixed-length instructions.

Data Width: 16-bit internal data path.

Registers: 32 General Purpose Registers (GPR) + 1 Special Register (SGPR) for 32-bit multiplication results.

Control Logic: 5-stage Finite State Machine (Idle, Fetch, Decode/Execute, Delay, Next/Halt).

Memory: On-chip Program (Instruction) and Data Memory arrays.

Instruction Set (ISA)
The CPU implements a custom 32-bit instruction format: [31:27] Opcode | [26:22] R-Dest | [21:17] R-Src1 | [16] Mode | [15:0] Immediate/R-Src2

Supported Operations:
Arithmetic: ADD, SUB, MUL (stores upper bits in SGPR), MOV.

Logical: Bitwise AND, OR, XOR, XNOR, NAND, NOR, NOT.

Control Flow: JUMP and conditional branching based on four hardware flags: Carry, Zero, Sign, and Overflow.

Memory I/O: STORE (Register to DM), LOAD (DM to Register), and external bus support (DIN/DOUT).

Verification Strategy
Verification was conducted via a testbench (tb.v) targeting the following corner cases:

Flag Logic: Validating Two's Complement overflow and carry-out detection during ALU operations.

Branching: Testing conditional jump accuracy under various flag states.

FSM Timing: Ensuring stable memory reads by utilizing a delay_next_inst state to meet setup/hold requirements.

Directory Structure
src/: Verilog RTL source code (top.v).

tb/: Testbench and memory initialization files (tb.v, inst_data.mem).

docs/: Architecture diagrams and FSM state maps.