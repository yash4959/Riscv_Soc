# 📘 Day 4 — GLS, Blocking vs. Non-blocking, and Synthesis-Simulation Mismatch

This document covers the **Day 4** topics, focusing on the critical differences between Verilog assignment types, how their misuse leads to simulation-synthesis mismatches, and how to verify the final design using Gate-Level Simulation (GLS).

---

## 🚦 Blocking (`=`) vs. Non-blocking (`<=`) Statements

Understanding the difference between these two assignment operators is crucial for writing correct and synthesizable RTL code.

### Blocking Assignments (`=`)
A **blocking assignment** (`=`) is executed sequentially, just like in a standard programming language. The execution of the next statement is "blocked" until the current one is complete. The variable on the left-hand side is updated immediately.

* **Analogy:** Following a recipe step-by-step. You must complete step 1 before you can begin step 2.
* **Best Used For:** **Combinational logic** (`always @(*)`), where the logic flow is sequential.

### Non-blocking Assignments (`<=`)
A **non-blocking assignment** (`<=`) is executed concurrently. The simulator evaluates all the right-hand side expressions first and then, at the end of the time step, assigns those evaluated values to the left-hand side variables.

* **Analogy:** A snapshot in time. All events are captured simultaneously and updated together.
* **Best Used For:** **Sequential logic** (`always @(posedge clk)`), as it accurately models how flip-flops in hardware all change state at the same time on a clock edge.

---


## ⚠️ Synthesis-Simulation Mismatch

A **synthesis-simulation mismatch** is a critical design bug where the pre-synthesis RTL simulation behaves differently from the post-synthesis gate-level simulation (and the final silicon).

The most common cause is the **improper use of blocking (`=`) assignments to model sequential logic**.

* **How it Happens:** In simulation, the blocking assignments create a sequential chain of events within a single clock cycle. However, the synthesis tool often interprets these as parallel hardware elements (like independent flip-flops) that should trigger simultaneously. This difference between the simulated sequential behavior and the synthesized parallel hardware is the source of the mismatch. 

---
## Lab on Synthesis simulation mismatch

### Lab steps for synthesis and gls on ternary_operator
1. open gtk wave
   ```bash
   iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
   ./a.out
   gtkwave tb_ternary_operator_mux.vcd
   ```
2. output of gtk wave
[ternary_operator_gtkwave](output_day_4/ternary_operator_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of ternary_operator
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog ternary_operator_mux.v
   synth -top ternary_operator_mux
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr ternary_operator_mux_net.v
   show
   ```
5. output of synthesis
[ternary_operator_synthesis](output_day_4/ternary_operator_synthesis.png)

6. gls simulation for ternary_operator
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
   ./a.out
   gtkwave tb_ternary_operator_mux.vcd
   ```
7. output of gls
[ternary_operator_gls](output_day_4/gls_ternary_operator_gtkwave.png)

### Lab steps for synthesis and gls on bad_mux
1. open gtk wave
   ```bash
   iverilog bad_mux.v tb_bad_mux.v
   ./a.out
   gtkwave tb_bad_mux.vcd
   ```
2. output of gtk wave
[bad_mux_gtkwave](output_day_4/bad_mux_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of bad_mux
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog bad_mux.v
   synth -top bad_mux
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr bad_mux_net.v
   show
   ```
5. output of synthesis
[bad_operator_synthesis](output_day_4/bad_mux_synthesis.png)

6. gls simulation for bad_mux
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
   ./a.out
   gtkwave tb_bad_mux.vcd
   ```
7. output of gls
[bad_mux_gls](output_day_4/gls_bad_mux_gtkwave.png)

## 🔬 Labs Overview

### Labs on GLS and Synthesis-Simulation Mismatch
These labs will guide you through running a **Gate-Level Simulation (GLS)** on a synthesized design. The primary goal is to compare the GLS waveform against the initial RTL simulation waveform to confirm that the synthesized hardware is functionally correct and matches the design intent.

---
## Lab steps for synthesis and gls on blocking_caveat
1. open gtk wave
   ```bash
   iverilog blocking_caveat.v tb_blocking_caveat.v
   ./a.out
   gtkwave tb_blocking_caveat.vcd

   ```
2. output of gtk wave
[blocking_caveat_gtkwave](output_day_4/blocking_caveat_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of blocking_caveat
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog blocking_caveat.v
   synth -top blocking_caveat
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr blocking_cavnet_net.v
   show
   ```
5. output of synthesis
[blocking_caveat_synthesis](output_day_4/blocking_caveat_synthesis.png)

6. gls simulation for blocking_caveat
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_cavnet_net.v tb_blocking_caveat.v
   ./a.out
   gtkwave tb_blocking_caveat.vcd
   ```
7. output of gls
[blockinh_caveat_gls](output_day_4/gls_blocking_caveat_gtkwave.png)

