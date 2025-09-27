# 📘 Day 2 — Timing, Synthesis Methodologies & Flop Design

This document covers the **Day 2** material, focusing on the role of timing libraries, comparing different synthesis strategies, and exploring efficient coding styles for flip-flops.

---

## 🕒 Introduction to Timing Libraries (`.lib`)

### 📝 Theory
A **timing library file (`.lib`)** is a critical component in the digital design flow. It is a text file provided by the semiconductor foundry that contains detailed performance characteristics of all the standard cells available in a specific Process Design Kit (PDK).

**Key Characteristics in a `.lib` file:**
- **Timing Information:** This includes propagation delays for gates, setup and hold times for flip-flops, and other timing arcs. The data is often presented in lookup tables based on input transition times and output load capacitance.
- **Power Consumption:** It defines the leakage power and dynamic power consumed by each cell under various conditions.
- **Physical Area:** The physical footprint of each standard cell.
- **PVT Corners:** Libraries are provided for different **Process, Voltage, and Temperature (PVT)** corners (e.g., fast, slow, typical) to ensure the design works reliably across all expected operating conditions.

The synthesis tool (like Yosys) and the Static Timing Analysis tool (like OpenSTA) heavily rely on these `.lib` files to translate RTL into an optimized gate-level netlist and to verify its performance against timing constraints.

---
## Lab on timeing libraries
1. To open netlist
   ```bash
   cd lib/
   vim sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

## 🏗️ Hierarchical vs. Flat Synthesis

### 📝 Theory
Synthesis is the process of converting high-level RTL code into a gate-level netlist. The two primary methodologies for this process are flat and hierarchical synthesis.

#### Flat Synthesis
In **flat synthesis**, the tool removes all levels of hierarchy in the design and synthesizes the entire project as a single, massive module.
-   **Analogy:** Building a car by dumping all its individual nuts, bolts, and panels into one giant pile and assembling it from scratch.
-   **Pros:** It allows the synthesis tool to perform optimizations across the entire design without being limited by module boundaries. This can often lead to the best possible timing and area results (**Quality of Results - QoR**).
-   **Cons:** This approach is very slow and consumes a huge amount of memory, making it impractical for large, complex designs like a modern SoC.

#### Hierarchical Synthesis
In **hierarchical synthesis**, the design is synthesized module by module, preserving the original hierarchy defined in the RTL code.
-   **Analogy:** Building a car by first assembling the engine, then the chassis, and then the interior, and finally putting these pre-built sub-assemblies together.
-   **Pros:** It is significantly faster and uses less memory. It is also scalable, allowing large teams to work on different blocks of the design simultaneously.
-   **Cons:** Since optimization is contained within each module, the final result might be slightly less optimal compared to a flat synthesis.

For almost all modern, complex designs, **hierarchical synthesis** is the standard industry practice.

---
## Lab on Falt and hierarchical Synthesis

### Lab steps
Here we use an multiple modules in this file we run flat and hierarchical synthesis both

### hirachical synthesis
1. open yosys
   ```bash
   yosys
   ```
2. synthesis code for hierarchical
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog multiple_modules.v
   synth -top multiple_modules 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show multiple_modules
   ```
3. Output of the hierarchical synthesis
[hierarchical synthesis](output_day_2/hirachical_synthesis.png)

### flatten synthesis
1. open yosys
   ```bash
   yosys
   ```
2. synthesis code for flatten
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog multiple_modules.v
   synth -top multiple_modules 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   flatten
   show 
   ```
3. Output of the hierarchical synthesis
[flatten synthesis](output_day_2/flatten_synthesis.png)

4. synthesis the submodule 1 and submodule 2 seprate
   ```bash
   synth -top sub_module1
   synth -top sub_module2
   ```

5. output of the submodule
[sub_module_1](output_day_2/module1_synthesis.png)

## 💡 Various Flop Coding Styles and Optimization

### 📝 Theory
Flip-flops are the fundamental storage elements in synchronous digital circuits. The way you write the Verilog code for a flip-flop directly influences the hardware that the synthesis tool creates.

# 📘 Synchronous vs. Asynchronous Inputs: Key Definitions

This document provides a quick reference for the fundamental concepts of synchronous and asynchronous inputs in the context of flip-flops and sequential logic.

---

## ⏰ Synchronous Inputs

A **synchronous** input can only affect a flip-flop's state on the active edge of a clock signal. This means the input's effect is synchronized with the system clock, ensuring predictable and stable behavior.

---

## Lab on synchronous dff
### Lab steps
1. Open gtkwave
   ```bash
   iverilog dff_syncres.v tb_dff_syncres.v 
   ./a.out
   gtkwave tb_dff_syncres.vcd
   ```
2. output gtkwave
[syncronus_gtkwave](output_day_2/dff_syncronus_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. for synthesis
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_syncres.v
   synth -top dff_syncres
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
    ```
5. synthesis output
[syncronus_synthesis](output_day_2/syncronus_synthesis.png)

   
## ⚡ Asynchronous Inputs

An **asynchronous** input affects a flip-flop's state immediately, without waiting for a clock edge. These inputs override the clock and synchronous data.

-   **Asynchronous Set:** A specific asynchronous input that immediately forces a flip-flop's output to a logic '1' state, regardless of the clock.

---
## Lab on asynchronous_set dff
### Lab steps
1. Open gtkwave
   ```bash
   iverilog dff_async_set.v tb_dff_async_set.v
   ./a.out
   gtkwave tb_dff_async_set.vcd
   ```
2. output gtkwave
[asyncronus_set_gtkwave](output_day_2/asyncronus_set_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. for synthesis
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_async_set.v
   synth -top dff_async_set
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
    ```
5. synthesis output
[asyncronus_set_synthesis](output_day_2/asyncronus_set_synthesis.png)


-   **Asynchronous Reset (or Clear):** The counterpart to set, this input immediately forces the flip-flop's output to a logic '0'.

---
## Lab on asynchronous dff
### Lab steps
1. Open gtkwave
   ```bash
   iverilog dff_asyncres.v tb_dff_asyncres.v
   ./a.out
   gtkwave tb_dff_asyncres.vcd
   ```
2. output gtkwave
[asyncronus_gtkwave](output_day_2/dff_asyncronus_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. for synthesis
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog dff_asyncres.v
   synth -top dff_asyncres
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
    ```
5. synthesis output
[syncronus_synthesis](output_day_2/asyncronus_synthesis.png)

#### Optimization
Synthesis tools are highly optimized to recognize standard, clean coding styles. Writing unconventional or complex logic for simple elements like flip-flops can confuse the tool, leading to inefficient hardware. Using a **clock enable** is a common and efficient optimization technique to reduce power by preventing the flop from toggling unnecessarily on every clock cycle.

## Lab on optimaization mul_2
### Lab steps
1. synthesis of mul_2
   ```bash
   yosys
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog mult_2.v
   synth -top mul2
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
2. output synthesis
[mul_2_synthesis](output_day_2/mul_2 synthesis.png)
4. open netlist
   ```bash
   write_verilog -noattr mult2_net.v
   !gvim mul2
   ```
5. output of the netlist
[mul_2_netlist](output_day_2/mul_2_netlist.png)

## Lab on optimaization mul_8
### Lab steps
1. synthesis of mul_8
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog mult_8.v
   synth -top mult8
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
2. output synthesis
[mul_2_synthesis](output_day_2/mul_8 synthesis.png)
4. open netlist
   ```bash
   write_verilog -noattr mult8_net.v
   !gvim mult8
   ```
5. output of the netlist
[mul_2_netlist](output_day_2/mul_8_netlist.png)



