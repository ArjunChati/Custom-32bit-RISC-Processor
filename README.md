# 🚀 32-bit Custom Processor: RTL Design & Verification
<div align="center">

[![Verilog](https://img.shields.io/badge/HDL-Verilog-brightgreen?style=for-the-badge&logo=verilog)](https://github.com/ArjunChati/Custom-32-bit-Processor-RTL-Design)
[![Vivado](https://img.shields.io/badge/Tools-Xilinx_Vivado-orange?style=for-the-badge&logo=xilinx)](https://github.com/ArjunChati/Custom-32-bit-Processor-RTL-Design)
[![FPGA](https://img.shields.io/badge/Platform-FPGA-lightgrey?style=for-the-badge)](https://github.com/ArjunChati/Custom-32-bit-Processor-RTL-Design)

**A specialized 32-bit CPU architecture featuring a 5-stage FSM and custom ISA, developed for high-efficiency FPGA resource utilization.**
</div>

---

## 📖 The Design Philosophy
As systems complexity grows in the AI and Cloud era, the need for custom centralized controllers is mandatory. While soft-processors like MicroBlaze are common, they often consume a large amount of FPGA resources. 

I built this **Custom CPU** to balance complexity and resource consumption, allowing for independent data processing and control requirements without the overhead of a standard hard processor.

## 🏗️ System Architecture
The processor is built on a 16-bit data path with a 32-bit instruction width, utilizing a Harvard-style approach with integrated Program and Data memory.

### **The 5-Stage Control Path (FSM)**
To ensure stable timing and memory synchronization, I implemented a robust **Finite State Machine (FSM)**:
1. **Idle**: System reset and initialization of the Program Counter (PC).
2. **Fetch**: Loading the instruction from `inst_mem` into the Instruction Register (IR).
3. **Decode/Execute**: Processing the opcode and driving the execution unit.
4. **Delay/Next**: A critical delay state to meet resource utilization constraints and ensure data stability before the next fetch.
5. **Sense Halt**: Managing termination logic without hanging the system bus.



### **The Special Register (SGPR)**
One of my key implementation notes: since a 16-bit multiplication results in a 32-bit value, I engineered a **Special General Purpose Register (SGPR)** to capture the upper bits, preventing data loss.

---

## 🕹️ Instruction Set Architecture (ISA)
I developed a custom ISA that supports both register-direct and immediate addressing modes via a dedicated `imm_mode` bit.

| Category | Opcodes | Description |
| :--- | :--- | :--- |
| **Arithmetic** | `ADD`, `SUB`, `MUL`, `MOV` | Handles core computations and data movement. |
| **Logical** | `OR`, `AND`, `XOR`, `NAND`, `NOT` | Bitwise operations for signal processing. |
| **Control** | `JUMP`, `JCarry`, `JZero`, `JOverflow` | Multi-condition branching for algorithm control. |
| **Memory** | `Storedin`, `Storereg`, `Sendreg` | Synchronizing data between memory and the GPR. |

---

## 🛠️ Verification & Flag Logic
[cite_start]Targeting **Validation and Verification** roles[cite: 2], I focused heavily on the integrity of hardware status flags. The CPU monitors four critical condition flags used for branching:

* **Carry Flag**: Set when the 17-bit `temp_sum` detects an arithmetic carry-out.
* **Zero Flag**: Monitored across both the GPR and SGPR to ensure accurate zero-detection.
* **Sign Flag**: Tracks the MSB of the result for signed arithmetic.
* **Overflow Flag**: Implemented logic to detect two's complement arithmetic violations:
    $$Overflow = (\neg A_{msb} \wedge \neg B_{msb} \wedge Out_{msb}) \vee (A_{msb} \wedge B_{msb} \wedge \neg Out_{msb})$$


---

## 📁 Repository Structure
* **`/src`**: Contains `top.v`, the primary RTL design.
* **`/tb`**: Contains the functional testbench and `inst_data.mem`.
* **`/docs`**: FSM mapping and block diagrams.

---

## 👤 Author
**Arjun Chati** *Electrical & Computer Engineering, The University of Texas at Austin*   
[LinkedIn](https://linkedin.com/in/arjun-chati/) | [GitHub](https://github.com/ArjunChati) [cite: 2]
