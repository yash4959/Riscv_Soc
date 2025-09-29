# 📘 Day 3 — Combinational and Sequential Optimizations

This document provides an overview of **Day 3** topics, focusing on the various optimization techniques that synthesis tools apply to combinational and sequential logic to improve a design's Power, Performance, and Area (PPA).

---

## 🎯 Introduction to Optimizations

**Logic optimization** is the process where a synthesis tool automatically transforms a gate-level circuit into a smaller, faster, and more power-efficient version without changing its logical function. The primary goal is to meet the design constraints for **timing (performance)**, **power consumption**, and **chip area**.

---

## ➗ Combinational Logic Optimizations

This involves optimizing logic that does not contain memory elements, where the output is purely a function of the current inputs.

* **Constant Propagation:** Simplifies logic by replacing parts of a circuit with constant values. For example, an AND gate with one input tied to logic '0' will always output '0', so the entire gate can be replaced by a direct connection to ground.
* **Boolean Logic Simplification:** Uses rules of Boolean algebra to reduce the number of gates. For instance, the expression `(A AND B) OR (A AND NOT B)` can be simplified to just `A`, eliminating the need for two AND gates and an OR gate. 
* **Common Sub-expression Elimination:** If an identical piece of logic is used in multiple places, the tool synthesizes it only once and shares the result. This reduces the total gate count and saves area.

---

## Lab on Combinational optimaization

### Lab on opt_check
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check.v
   synth -top opt_check
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
[opt_check_synthesis](output_day_3/opt_check.png)

### Lab on opt_check2
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check2.v
   synth -top opt_check2
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
[opt_check2_synthesis](output_day_3/opt_check2.png)

### Lab on opt_check3
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check3
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check3.v
   synth -top opt_check3
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
[opt_check3_synthesis](output_day_3/opt_check3.png)

### Lab on opt_check4
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of opt_check4
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog opt_check4.v
   synth -top opt_check4
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
[opt_check4_synthesis](output_day_3/opt_check4.png)


### Lab on multiple_module_opt
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of multiple_module_opt
   ```bash
   read_verilog multiple_module_opt.v
   synth -top multiple_module_opt
   flatten
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
[multiple_module_opt_synthesis](output_day_3/multiple_module_opt.png)

### Lab on multiple_module_opt2
1. open yosys
   ```bash
   yosys
   ```
2. synthesis of multiple_module_opt2
   ```bash
   read_verilog multiple_module_opt2.v
   synth -top multiple_module_opt2
   flatten
   opt_clean -purge
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output :
[multiple_module_opt2_synthesis](output_day_3/multiple_module_opt_2.png)


## 🔄 Sequential Logic Optimizations

This involves optimizing circuits that contain memory elements like flip-flops, where the output depends on both current inputs and past states.

* **Retiming:** Moves registers across combinational logic blocks to better balance timing paths. This helps to shorten the critical path delay, allowing the design to run at a higher clock frequency without changing the circuit's overall function.
* **State Minimization:** For Finite State Machines (FSMs), this technique identifies and merges equivalent states. Fewer states require fewer flip-flops to implement, which directly reduces the circuit's area.
* **Clock Gating:** A major power-saving technique where the clock signal to a group of flip-flops is temporarily turned off when they are not changing state. This eliminates the dynamic power consumed during unnecessary switching. 

---
## Lab on sequantial optimaization

### Lab on dff_const1
1. open gtkwave
   ```bash
   iverilog dff_const1.v tb_dff_const1.v
   ./a.out
   gtkwave tb_dff_const1.vcd
   ```
2. output of gtkwave
[const1_gtkwave](output_day_3/dff_const1_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const1
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const1.v
   synth -top dff_const1
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
[const1_synthesis](output_day_3/synthesis_dff_const1.png)

### Lab on dff_const2
1. open gtkwave
   ```bash
   iverilog dff_const2.v tb_dff_const2.v
   ./a.out
   gtkwave tb_dff_const2.vcd
   ```
2. output of gtkwave
[const2_gtkwave](output_day_3/dff_const2_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const2.v
   synth -top dff_const2
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
[const2_synthesis](output_day_3/synthesis_dff_const2.png)

### Lab on dff_const3
1. open gtkwave
   ```bash
   iverilog dff_const3.v tb_dff_const3.v
   ./a.out
   gtkwave tb_dff_const3.vcd
   ```
2. output of gtkwave
[const3_gtkwave](output_day_3/dff_const3_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const3
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const3.v
   synth -top dff_const3
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
[const3_synthesis](output_day_3/synthesis_dff_const3.png)

### Lab on dff_const4
1. open gtkwave
   ```bash
   iverilog dff_const4.v tb_dff_const3.v
   ./a.out
   gtkwave tb_dff_const3.vcd
   ```
2. output of gtkwave
[const4_gtkwave](output_day_3/dff_const4_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const4
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const4.v
   synth -top dff_const4
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
[const4_synthesis](output_day_3/synthesis_dff_const4.png)

### Lab on dff_const5
1. open gtkwave
   ```bash
   iverilog dff_const5.v tb_dff_const5.v
   ./a.out
   gtkwave tb_dff_const5.vcd
   ```
2. output of gtkwave
[const5_gtkwave](output_day_3/dff_const5_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of dff_const5
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog dff_const5.v
   synth -top dff_const5
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show 
   ```
5. output of synthesis
[const5_synthesis](output_day_3/synthesis_dff_const5.png)


## 🗑️ Sequential Optimizations for Unused Outputs

This is a powerful technique for reducing waste in a design.

* **Logic Trimming (or Dead Code Elimination):** If the output of a flip-flop or a block of logic is not connected to anything else in the design (i.e., it has no fan-out), it serves no purpose. The synthesis tool will identify and completely remove these unused registers and any combinational logic that solely drives them, leading to significant savings in both area and power.

---
## Lab on sequantial logic unused case

### lab on couter_opt
1. open yosys
   ```bash
   yosys
   ```
2. synthesis counter_opt
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog counter_opt.v
   synth -top counter_opt
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output of synthesis
[counter_opt_synthesis](output_day_3/unused_output_counter_opt.png)

### copy counter_opt file to counter_opt2 file
1. copy and open the file
   ```bash
   cp counter_opt.v counter_opt2.v
   gvim counter_opt2.v
   ```
### after open gvim counter_opt2.v change the line assign q = count[0]; to assign q = (count[2:0] == 3'b100); and save
### run the counter_opt2 file
1. open yosys
   ```bash
   yosys
   ```
2. synthesis counter_opt2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
   read_verilog counter_opt2.v
   synth -top counter_opt
   dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
3. output of synthesis
[counter_opt2_synthesis](output_day_3/unused_output_counter_opt2.png)

