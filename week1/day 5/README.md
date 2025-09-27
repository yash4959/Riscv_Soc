# 📘 Day 5 — Advanced Synthesis and Optimization Constructs

This document covers the **Day 5** topics, exploring how different Verilog constructs like `if`, `case`, and loops are interpreted by the synthesis tool and how they can be used to create optimized hardware.

---

## ⚖️ `if` and `case` Constructs in Synthesis

Both `if` and `case` statements describe conditional logic, but the synthesis tool can create very different hardware from them.

* **`if-else-if` Chain:** This construct is synthesized into a **priority encoder**. Conditions are evaluated in order, meaning the first condition has the highest priority. This can create a long chain of logic that may result in slower timing paths.

* **`case` Statement:** This is typically synthesized into a balanced **multiplexer (MUX)**. 

### Incomplete Specification and Latches
A common pitfall is an "incomplete" `if` or `case` statement, where a signal is not assigned a value in every possible branch.
* **What Happens?** To ensure the signal retains its value, the synthesis tool must infer memory. This creates an unintended **latch**.
* **Why are Latches Bad?** Latches are generally avoided in synchronous designs because they are transparent (not edge-triggered), can be susceptible to glitches, and complicate static timing analysis. Always ensure all paths assign a value to every signal or include a `default` case.

---

## 🔁 `for loop` vs. `for generate`

While they look similar, `for loop` and `for generate` describe fundamentally different hardware structures.

### `for loop`
A `for loop` describes sequential behavior inside an `always` block. During synthesis, the tool **unrolls the loop** to create a large, replicated block of combinational logic. It cannot be used to create multiple instances of modules.

* **Analogy:** One worker performing a series of repetitive tasks. The result is one large piece of work.

### `for generate`
A `for generate` is a declarative statement used to create multiple instances of hardware. The tool **elaborates** the loop to create parallel, duplicated structures like modules, registers, or logic blocks. It is ideal for building regular, repetitive hardware like register files or connecting multiple cores.

* **Analogy:** Hiring multiple workers to perform the same task in parallel. The result is multiple identical pieces of hardware.

---

## 🔬 Labs Overview

* **Labs on "Incomplete `if`/`case`":** You will write code with incomplete conditional assignments, synthesize it, and observe the unintended latches in the generated netlist. This helps in understanding how to write latch-free code.

## Lab on incomplete if
### steps of incomplete_if
1. open gtkwave
   ```bash
   iverilog incomp_if.v tb_incomp_if.v 
   ./a.out
   gtkwave tb_incomp_if.vcd
   ```
2. output of gtkwave
[incomplete_if_gtkwave](output_day_5/incomp_if_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of incomplete_if
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog incomp_if.v
   synth -top incomp_if
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
[incomplete_if_synthesis](output_day_5/incomp_if_synthesis.png)

### steps of incomplete_if2
1. open gtkwave
   ```bash
   iverilog incomp_if2.v tb_incomp_if2.v 
   ./a.out
   gtkwave tb_incomp_if2.vcd
   ```
2. output of gtkwave
[incomplete_if2_gtkwave](output_day_5/incomp_if2_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of incomplete_if2
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog incomp_if2.v
   synth -top incomp_if2
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
[incomplete_if2_synthesis](output_day_5/incomp_if2_synthesis.png)


* **Labs on "Incomplete overlapping `case`":** These labs will explore how priority or overlapping case conditions are synthesized, often resulting in priority logic similar to `if-else` chains.

## Lab on incomplete case
### Steps of incomp_case

1. open gtk wave
   ```bash
   iverilog incomp_case.v tb_incomp_case.v 
   ./a.out
   gtkwave tb_incomp_case.vcd
   ```
2. output of gtk wave
[incomp_case_gtkwave](output_day_5/incomp_case_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of incomp_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog incomp_case.v
   synth -top incomp_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
[incomp_case_synthesis](output_day_5/incomp_case_synthesis.png)

### Steps of comp_case

1. open gtk wave
   ```bash
   iverilog comp_case.v tb_comp_case.v 
   ./a.out
   gtkwave tb_comp_case.vcd
   ```
2. output of gtk wave
[comp_case_gtkwave](output_day_5/comp_case_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of comp_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog comp_case.v
   synth -top comp_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
[comp_case_synthesis](output_day_5/comp_case_synthesis.png)

### Steps of partial_case_assign

1. open gtk wave
   ```bash
   iverilog partial_case_assign.v tb_partial_case_assign.v 
   ./a.out
   gtkwave tb_partial_case_assign.vcd
   ```
2. output of gtk wave
[partial_case_assign_gtkwave](output_day_5/partial_case_assign_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of partial_case_assign
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog partial_case_assign.v
   synth -top partial_case_assign
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   show
   ```
5. output of synthesis
[partial_case_assign_synthesis](output_day_5/partial_case_assign_synthesis.png)

### Steps of bad_case

1. open gtk wave
   ```bash
   iverilog bad_case.v tb_bad_case.v
   ./a.out
   gtkwave tb_bad_case.vcd
   ```
2. output of gtk wave
[bad_case_gtkwave](output_day_5/bad_case_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of bad_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog bad_case.v
   synth -top bad_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr blocking_cavnet_net.v
   show
   ```
5. output of synthesis
[bad_case_synthesis](output_day_5/bad_case_synthesis.png)

6. gls for bad_case
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_case_net.v tb_bad_case.v
   ./a.out
   gtkwave tb_bad_case.vcd
   ```

7. output of gls
[bad_case_gls](output_day_5/gls_bad_case_gtkwave.png)

* **Labs on "`for loop`" and "`for generate`":** You will implement a function (like a multi-bit shifter or adder) using both constructs. By comparing the synthesized schematics, you will see the clear structural difference between a single unrolled combinational block (`for loop`) and multiple parallel instances (`for generate`).

## Labs on for and for genrater
### Steps of mux_generate

1. open gtk wave
   ```bash
   iverilog mux_generate.v tb_mux_generate.v
   ./a.out
   gtkwave tb_mux_generate.vcd
   ```
2. output of gtk wave
[mux_generate_gtkwave](output_day_5/mux_genrate_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of mux_generate
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog mux_generate.v
   synth -top mux_generate
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr mux_generate_net.v
   show
   ```
5. output of synthesis
[mux_generate_synthesis](output_day_5/mux_generate_synthesis.png)

6. gls for mux_generate
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v mux_generate_net.v tb_mux_generate.v
   ./a.out
   gtkwave tb_mux_generate.vcd
   ```

7. output of gls
[mux_generate_gls](output_day_5/gls_mux_generate_gtkwave.png)

### Steps of demux_case

1. open gtk wave
   ```bash
   iverilog demux_case.v tb_demux_case.v
   ./a.out
   gtkwave tb_demux_case.vcd
   ```
2. output of gtk wave
[demux_case_gtkwave](output_day_5/demux_case_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of demux_case
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog demux_case.v
   synth -top demux_case
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr demux_case.v
   show
   ```
5. output of synthesis
[demux_case_synthesis](output_day_5/demux_case_synthesis.png)

6. gls for demux_case
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v demux_case_net.v tb_demux_net.v
   ./a.out
   gtkwave tb_demux_case.vcd
   ```

7. output of gls
[demux_case_gls](output_day_5/gls_demux_case_gtkwave.png)


### Steps of demux_generate

1. open gtk wave
   ```bash
   iverilog demux_generate.v tb_demux_generate.v
   ./a.out
   gtkwave tb_demux_generate.vcd
   ```
2. output of gtk wave
[demux_generate_gtkwave](output_day_5/demux_generate_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of demux_generate
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog demux_generate.v
   synth -top demux_generate
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr demux_generate_net.v
   show
   ```
5. output of synthesis
[demux_generate_synthesis](output_day_5/demux_genrate_synthesis.png)

6. gls for demux_generate
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v demux_generate_net.v tb_demux_generate.v
   ./a.out
   gtkwave tb_demux_generate.vcd
   ```

7. output of gls
[demux_generate_gls](output_day_5/gls_demux_generate_gtkwave.png)

### Steps of ripple_carry_adder

1. open gtk wave
   ```bash
   iverilog fa.v rca.v tb_rca.v
   ./a.out
   gtkwave tb_rca.vcd
   ```
2. output of gtk wave
[ripple_carry_adder_gtkwave](output_day_5/rca_gtkwave.png)

3. open yosys
   ```bash
   yosys
   ```
4. synthesis of ripple_carry_adder
   ```bash
   read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   read_verilog fa.v rca.v
   synth -top fa rca
   abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
   write_verilog -noattr rca_net.v
   select top rca
   show
   ```
5. output of synthesis
[ripple_carry_adder_synthesis](output_day_5/rca_synthesis.png)

6. gls for ripple_carry_adder
   ```bash
   iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v rca_net.v tb_rca.v
   ./a.out
   gtkwave tb_rca.vcd
   ```

7. output of gls
[ripple_carry_adder_gls](output_day_5/gls_rca_gtkwave.png)

# 🗝️ Key Learnings from Day 5

Here is a summary of the essential synthesis concepts covered today.

* **`if-else-if` vs. `case`:** `if-else-if` chains create **priority logic**, which can be slow, while `case` statements generally synthesize into faster, parallel **multiplexers (MUXes)**.

* **Inferred Latches:** Failing to assign a value to a signal in every possible branch of an `if` or `case` statement causes the synthesis tool to create unintended **latches**.

* **`for loop`:** This construct is **unrolled** by the synthesis tool to create a single, large block of combinational logic.

* **`for generate`:** This is a powerful structural construct used to create multiple, parallel **instances** of hardware (like modules, logic, or registers).
