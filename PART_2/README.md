# 📘 Week 2 Task – BabySoC Fundamentals & Functional Modelling

## Objective
To build a solid understanding of SoC fundamentals and practice functional modelling of the BabySoC using simulation tools such as **Icarus Verilog** and **GTKWave**.

## PART 2 Labs on - (Hands-on Functional Modelling)

## 📂 Project Structure

```txt
VSDBabySoC/
├── src/
│   ├── include/      # Header files (*.vh)
│   ├── module/       # Verilog + TLV modules
│   │   ├── vsdbabysoc.v   # Top-level module
│   │   ├── rvmyth.v       # CPU
│   │   ├── avsdpll.v      # PLL
│   │   ├── avsddac.v      # DAC
│   │   └── testbench.v    # Testbench
└── output/           # Simulation outputs
```

---

## 🛠️ Setup

### 📥 Cloning the Project

```bash
cd ~/VLSI
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC/
```
---

## 🔧 TLV → Verilog Conversion

Since **RVMYTH** is written in **TL-Verilog (.tlv)**, we need to convert it to Verilog before simulating.

```bash
# Install tools
sudo apt update
sudo apt install python3-venv python3-pip

# Create virtual env
python3 -m venv sp_env
source sp_env/bin/activate

# Install SandPiper-SaaS
pip install pyyaml click sandpiper-saas

# Convert TLV → Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

✅ Now you’ll have `rvmyth.v` alongside your other Verilog files.

---

You need to add  two extra files for the synthesis which is ```avsddac_stub.v```and ```avsdpll_stub.v``` this is uploaded on this week2 part-2 (testbench and design file folder).

---

## 🧪 Simulation Steps

### pre-synthesis simulation

```bash
mkdir -p output/pre_synth_sim

iverilog -o output/pre_synth_sim/pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v

cd output/pre_synth_sim
./pre_synth_sim.out
```
### for see the gtk wave

```bash
gtkwave pre_synth_sim.vcd
```
### output GTKWaves
![pre_synthesis_uut_gtkwave][output_week2_part2/pre_synthesis_uut_gtkwave.png]

## Waveform Analysis

### 1. CPU Core and Memory Interface

This view inspects the `L1_CPU_dmem` module, which is the Level 1 Data Memory interface for the CPU core.

- **Hierarchy:** `vsdbabysoc_tb` -> `u_dut` -> `core` -> `L1_CPU_dmem`
- **Timestamp:** 37900 ns
- **Analysis:** The signals show the CPU performing a memory read operation, indicated by `CPU_dmem_rd_en_a5` being high (`1`). The key observation is that the top-level output signal `OUT[9:0]` has the hexadecimal value **`0x134`**. This demonstrates the CPU successfully placing data onto its output bus to control a peripheral.

---

![pre_synthesis_dac_gtkwave][output_week2_part2/pre_synthesis_dac_gtkwave.png]

## Waveform Analysis

### 2. Digital-to-Analog Converter (DAC) Verification

This view focuses on the `dac` module to confirm it correctly receives data from the CPU.

- **Hierarchy:** `vsdbabysoc_tb` -> `u_dut` -> `dac`
- **Timestamp:** 22470 ns
- **Analysis:** The DAC's input, `D[9:0]`, is connected to the CPU's `OUT[9:0]` bus and correctly shows the value **`0x134`**. The DAC's analog-style output (`OUT`) changes in response to this digital input. This view confirms that the DAC is receiving the intended digital value and producing the corresponding analog output, verifying the data path from the CPU to the DAC.

---

![pre_synthesis_pll_gtkwave][output_week2_part2/pre_synthesis_pll_gtkwave.png]

## Waveform Analysis

### 3. Phase-Locked Loop (PLL) Verification

This analysis verifies the `pll` module, which is responsible for generating the system clock.

- **Hierarchy:** `vsdbabysoc_tb` -> `u_dut` -> `pll`
- **Timestamp:** 18420 ns
- **Analysis:** The waveforms confirm the PLL is functioning correctly. The `REF` (reference clock) signal is used to generate a stable, oscillating output `CLK` signal. The internal `VCO_IN` (Voltage-Controlled Oscillator Input) shows the control voltage that determines the output frequency. This test verifies that the clock generation unit, which is critical for the entire SoC's operation, is stable and working as expected.

---

## 🧠 Yosys Synthesis Flow for VSDBabySoC

Here we do an module level synthesis beacuse it is not possible to work with gate level synthesis.

### --- Part 1: Read Libraries and Design ---

```bash
read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib src/lib/avsddac.lib
read_liberty -lib src/lib/avsdpll.lib
read_verilog src/module/vsdbabysoc.v
read_verilog -I src/include src/module/rvmyth.v
read_verilog -I src/include src/module/clk_gate.v
read_verilog src/module/avsddac_stub.v
read_verilog src/module/avsdpll_stub.v
```

### --- Part 2: Show the MODULE-LEVEL (RTL) design BEFORE synthesis for vsdbabysoc ---

```bash
show -format png -prefix vsdbabysoc_rtl vsdbabysoc
```

###  --- Part 3: Synthesize the Design (while preserving module boundaries) ---

```bash
synth -top vsdbabysoc
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
opt
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
### --- Part 4: Generate Gate-Level PNGs for Sub-modules (BEFORE flattening) ---

```bash
show -format png -prefix netlist_clk_gate clk_gate
show -format png -prefix netlist_avsdpll_stub avsdpll_stub
show -format png -prefix netlist_avsddac_stub avsddac_stub
```

### --- Part 5: Flatten the Design and Final Cleanup ---

```bash
flatten
setundef -zero
clean -purge
rename -enumerate
```
### --- Part 5: Write Final (Flattened) Netlist and Statistics ---

```bash
write_verilog -noattr reports/vsdbabysoc_netlist.v
stat -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
---

## Output photo

![vsdbabysoc_rtl][output_week2_part2/vsdbabysoc_rtl.png]
![netlist_clk_gate][output_week2_part2/netlist_clk_gate.png]
![output_synthesis][output_week2_part2/output_synthesis.png]
![printing_stestices_1][output_week2_part2/printing_stestices_1.png]
![printing_stestices_2][output_week2_part2/printing_stestices_2.png]
![printing_stestices_3][output_week2_part2/printing_stestices_3.png]

##  Post-Synthesis Verification (GLS)

To ensure the synthesis process did not alter the design's logic, we perform a **Gate-Level Simulation (GLS)**. This involves simulating the generated netlist with the same testbench used for the original RTL.

### **Command**: The `iverilog` command for GLS is more complex, as it requires the Verilog models for the Sky130 standard cells.

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o simulation/post_synth_sim.out src/gls_model/primitives.v src/gls_model/sky130_fd_sc_hd.v reports/vsdbabysoc_netlist.v src/module/avsdpll.v src/module/avsddac.v src/module/testbench.v
 ```

### **Execution**:

 ```bash
 cd simulation
 ./post_synth_sim.out
 gtkwave dump.vcd
 ```

### Output GTKWave

![post_synthesis_uut_gtkwave][output_week2_part2/post_synthesis_uut_gtkwave.png]
![post_synthesis_vsdbabysoc_gtkwave][output_week1_part_1/post_synthesis_vsdbabysoc_gtkwave.png]

## 🔑 Key Takeaways
- Yosys converts RTL design into a gate-level netlist using standard cell libraries.  
- Pre-flatten visualization helps understand each module’s internal structure.  
- Flattening merges all modules for physical design readiness.  
- Optimization and mapping ensure efficient hardware implementation.  
- The flow bridges RTL synthesis to final SoC integration smoothly.

