# 📘 Day 1 — Verilog RTL Design, Simulation & Synthesis

This document contains **Day 1** material of the RTL Design & Synthesis workshop using Verilog, Icarus Verilog, GTKWave, Yosys, and Sky130 PDKs.  

---

##  Introduction to Verilog RTL Design and Synthesis

### 📝 Theory
Verilog is a hardware description language (HDL) that models digital systems. RTL (Register-Transfer Level) is an abstraction where data flows between registers and through combinational logic.  

**Key Points:**
- RTL defines synchronous behavior (flip-flops, registers) and combinational logic.  
- Simulation validates correctness before hardware implementation.  
- Synthesis converts RTL into a gate-level netlist.  
- Tools: **Icarus Verilog (simulation)**, **GTKWave (waveform view)**, **Yosys (synthesis)**, **Sky130 PDK (standard cells)**.  

---

##  Introduction to open-source simulator iverilog

### 📝 Theory
**Icarus Verilog (iverilog)** is an open-source Verilog simulator.  
It compiles Verilog source code into an executable simulation. Combined with **GTKWave**, it provides visibility into signal waveforms.  

**Workflow:**
1. Write RTL and testbench.  
2. Compile with `iverilog`.  
3. Run the output executable.  
4. Open `.vcd` dump in GTKWave.  

This ensures functional correctness of the RTL before synthesis.

---

## Labs using iverilog and GTKWave

### 🧪 Lab Steps
In the Riscv_soc folder clone the repo and open the verilog_files to simulate.
1. Clone and navigate:
   ```bash
   git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
   cd sky130RTLDesignAndSynthesisWorkshop/verilog_files/
   ```
2. Compile and run:
   ```bash
   iverilog good_mux.v tb_good_mux.v
   ./a.out 
    gtkwave tb_good_mux.vcd
   ```
3. Output:
[GTK wave](output_day_1/good_mux_gtkwave.png)

## Introduction to Yosys and Logic Synthesis
📝 **Theory**

Yosys is an open-source logic synthesis tool.  
It transforms RTL into a gate-level netlist and can map designs to a PDK’s standard cells.

**Workflow:**
- Read RTL → `read_verilog your_design.v`  
- Synthesize Logic → `synth`  
- Map to Standard Cells → `abc -liberty your_lib.lib`  
- View Design (Optional) → `show`  
- Export Netlist → `write_verilog output.v`

---
## Lab using yosys

### 🧪 Lab Steps for synthesis
1. open yosys
   ```bash
   yosys
   ```
2. for synthesis run below comment in yosys
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog good_mux.v
    synth -top good_mux
    abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    show
   ```
3.Output:
[good_mux_synthesis](output_day_1/good_mux_synthesis.png)

4. For netlist:
   ```bash
   write_verilog -noattr good_mux_netlist.v
   !vim good_mux_netlist.v
   ```
5. Output of Netlist:
[good_mux_netlist](output_day_1/good_mux_netlist.png)

---

## 🗝️ Key Outcomes from Day 1

- Understood **RTL Design** concepts using Verilog.  
- Learned to simulate designs with **Icarus Verilog (iverilog)**.  
- Visualized waveforms using **GTKWave**.  
- Gained introduction to **Yosys** for logic synthesis.  
- Performed synthesis with **Sky130 PDK standard cells**.  
- Observed how RTL is transformed into a **gate-level netlist**.  

   

   

