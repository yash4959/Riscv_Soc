# 🧠 [DAY-1] Basics of NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)

---

## 1. Introduction to Circuit Design and SPICE Simulations

### 1.1 Why do we need SPICE simulations?

SPICE (Simulation Program with Integrated Circuit Emphasis) is a powerful tool used to simulate and analyze electronic circuits before they are physically fabricated.  
It allows designers to predict how a circuit will behave under different voltage, current, and temperature conditions.  

SPICE helps engineers understand nonlinear device characteristics such as those found in transistors and diodes, and it greatly reduces the need for physical prototyping — saving both time and cost.  
Its models are built using mathematical equations derived from semiconductor device physics, giving accurate and realistic circuit predictions.

---
![Power_aware_cts](output_week4/power_aware_cts.jpeg
)
### 1.2 Basic Element in Circuit Design – NMOS

The NMOS (n-channel MOSFET) is a voltage-controlled semiconductor device used widely for switching and amplification.  
It has four terminals — **Gate (G)**, **Source (S)**, **Drain (D)**, and **Body (B)**.

When the gate-to-source voltage (Vgs) becomes greater than the threshold voltage (Vth), an inversion layer forms on the surface of the semiconductor substrate.  
This creates a conductive channel that allows electrons to move from the drain to the source terminal.

**Key parameters:**
- Vgs: Voltage between gate and source  
- Vds: Voltage between drain and source  
- Vth: Threshold voltage that determines when the transistor turns ON

In NMOS devices, the current flow is due to electrons (majority carriers), and the strength of conduction is controlled by the applied gate voltage.

---
![NMOS_Basic](output_week4/nmos_basic.jpeg)
![NMOS_Basic](output_week4/nmos_basic_1.jpeg)

### 1.3 Strong Inversion and Threshold Voltage

When the gate voltage exceeds the threshold voltage (Vgs > Vth), the MOSFET enters **strong inversion**.  
In this state, a dense electron channel forms under the gate oxide, and current flows easily between drain and source.

If the gate voltage is below Vth, the MOSFET operates in the **weak inversion** or **subthreshold region**, where only a small leakage current flows.  
In this region, the drain current increases exponentially with gate voltage but remains very small.

Thus, Vth marks the transition between OFF and ON states of the transistor.

---
![Threshold_voltage](output_week4/threshold_voltage_basic.jpeg)
![Threshold_voltage_ewuation](output_week4/threshold_voltage_equation.jpeg)

### 1.4 Threshold Voltage with Positive Substrate Potential

The threshold voltage of a MOSFET is not constant — it depends on the **body bias** or **source-to-body voltage (Vsb)**.  
When a positive substrate potential is applied (for NMOS), it increases the depletion region under the gate, requiring a higher gate voltage to invert the channel.

As a result, the threshold voltage increases with body bias.  
This phenomenon is known as the **body effect** or **substrate bias effect**.  
A higher body bias makes the transistor harder to turn ON since more gate voltage is required to form the conducting channel.

---

## 2. NMOS Resistive Region and Saturation Region of Operation

### 2.1 Resistive Region (Linear Region)

In the resistive or linear region, the MOSFET behaves like a **voltage-controlled resistor**.  
This occurs when the drain-to-source voltage (Vds) is smaller than the overdrive voltage (Vgs - Vth).  

In this region, the current through the device increases approximately linearly with Vds.  
The conductivity of the channel depends on how strongly the gate voltage enhances the channel — a higher Vgs results in a lower channel resistance and higher drain current.

---


### 2.2 Drift Current Theory

The drain current in a MOSFET mainly results from the **drift of electrons** under the influence of the electric field between drain and source.  
As the gate voltage increases, more electrons are attracted to the channel region, increasing the inversion charge.  
The movement of these electrons due to the applied drain-to-source voltage forms the drift current.

The strength of this current depends on:
- The mobility of electrons (how easily they move through the channel)
- The electric field strength along the channel
- The charge density of the inversion layer

---

### 2.3 Drain Current Model for Linear Region

In the linear region, the drain current increases almost proportionally with Vds.  
Initially, when Vds is small, the MOSFET channel is uniformly inverted, and current follows Ohm’s law.  
As Vds increases further, the voltage difference along the channel causes a gradual reduction in charge density near the drain.  
This slightly reduces the rate of current increase, leading to a nonlinear curve.

However, for small Vds, the relationship between Id and Vds remains nearly linear.

---
![NMOS_equations](output_week4/nmos_analysis.jpeg)

### 2.4 SPICE Conclusion to Resistive Operation

SPICE simulations verify the theoretical understanding of the linear region.  
They show that:
- The drain current increases linearly with small Vds.
- For higher gate voltages (Vgs), the slope of the Id–Vds curve increases, meaning channel resistance decreases.
- This confirms that Vgs effectively controls how easily current flows through the NMOS channel.

---

### 2.5 Pinch-Off Region Condition

As Vds increases, a point is reached where the voltage difference across the channel becomes large enough that the channel near the drain end gets depleted.  
This happens when Vds equals (Vgs - Vth).  
At this point, the MOSFET is said to be in the **pinch-off** condition.

Beyond this point, increasing Vds further does not significantly increase the current — instead, the voltage drop occurs mainly across the depleted region.  
This marks the transition from the linear to the **saturation region**.

---
![Pinch_off](output_week4/pinch_off.jpeg)
![Pinch_off](output_week4/pinch_off_1.jpeg)

### 2.6 Drain Current Model for Saturation Region

In the saturation region, the MOSFET channel is pinched off near the drain, and current becomes nearly constant.  
The drain current now depends mainly on the gate voltage rather than Vds.

Ideally, the current remains constant for increasing Vds, but in real devices, a small increase still occurs due to the **channel length modulation effect**, represented by a parameter called lambda (λ).  
This effect is similar to the Early effect in bipolar junction transistors (BJTs).

Thus, the MOSFET in saturation behaves like a **current source** controlled by Vgs.

---

## 3. Introduction to SPICE

### 3.1 Basic SPICE Setup

SPICE is a circuit simulation engine that uses **netlists** — text-based descriptions of circuit components and their connections.  
It allows designers to perform DC, AC, and transient analyses to study how a circuit behaves under various input conditions.

The typical process involves:
1. Defining the circuit elements (like transistors, resistors, capacitors, voltage sources, etc.).
2. Assigning model parameters for each device.
3. Choosing the type of analysis to perform (e.g., DC sweep for Id–Vds curves).

---

### 3.2 Circuit Description in SPICE Syntax

A simple NMOS simulation setup includes a transistor element, gate and drain voltage sources, and a control statement for DC analysis.  
The Sky130 model is specified to ensure accurate device characteristics.  

SPICE syntax describes connections and device parameters clearly, making it suitable for both schematic and textual simulation approaches.

---

### 3.3 Define Technology Parameters

The device model in SPICE uses parameters that represent physical properties of the transistor, such as:
- Electron mobility (how easily carriers move)
- Oxide capacitance per unit area (Cox)
- Device geometry (Width W and Length L)
- Threshold voltage (Vth)

These parameters come from the **Sky130 PDK (Process Design Kit)**, which provides accurate data for 130 nm CMOS technology.

---

### 3.4 First SPICE Simulation

The first simulation often involves a **DC sweep** to analyze how drain current varies with Vds and Vgs.  
From the results, we can observe two main regions:
- **Linear region:** Id increases almost linearly with Vds.  
- **Saturation region:** Id becomes constant after the pinch-off point.

This helps in understanding and validating the theoretical current models derived from MOSFET physics.

---
![ids_vds_nmos_pmos_curve.jpeg](output_week4/ids_vds_nmos_pmos_curve.jpeg)
![Load_curve_nmos_pmos](output_week4/Load_curve_nmos_pmos.jpeg)
![inverter_out_in_curve](output_week4/inverter_out_in_curve.jpeg)

### 3.5 SPICE Lab with Sky130 Models

The **Sky130 open-source process** provides accurate transistor models for NMOS and PMOS devices.  
Using these models, SPICE can generate realistic Id–Vds and Id–Vgs curves that closely match actual silicon behavior.

By simulating NMOS characteristics using the Sky130 models, one can clearly see the transition between resistive and saturation regions, validating both theoretical and experimental understanding.

---
## Steps for simulation:
### step:1 clone the github repositery
```bash
https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```
go inside the design folder and go the design folder in that you see the design fles.
inside the design folder run the following commend
```bash
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```
### output
![spice_netlist_day1](output_week4/spice_netlist_day1.png)
![ngspice_w5_l2](output_week4/ngspice_w5_l2.png)

## 🧩 Summary Table

| Region | Condition | Behavior |
|--------|------------|-----------|
| **Cutoff** | Vgs < Vth | Transistor OFF, no current flows |
| **Linear (Resistive)** | Vds < (Vgs - Vth) | Acts like a voltage-controlled resistor, Id increases linearly |
| **Saturation** | Vds ≥ (Vgs - Vth) | Id becomes nearly constant, transistor acts as a current source |

---

# 🧠 [Day-2] Velocity Saturation and Basics of CMOS Inverter Voltage Transfer Characteristics (VTC)

---

## 1. SPICE Simulation for Lower Nodes and Velocity Saturation Effect

### 1.1 Introduction

As CMOS technology scales down to smaller feature sizes (sub-micron and nanometer nodes), the behavior of MOSFETs deviates from the long-channel model.  
One of the most important non-ideal effects observed in short-channel devices is **velocity saturation**.  

In long-channel MOSFETs, carrier velocity increases linearly with the electric field.  
However, in short-channel devices, at high electric fields, carrier velocity no longer increases linearly — it **saturates** to a maximum limit known as **saturation velocity (vsat)**.  
This phenomenon significantly impacts current drive and device performance.

---

### 1.2 SPICE Simulation for Lower Nodes

When simulating advanced technology nodes (like 130 nm or smaller) using SPICE, the effects of mobility degradation and velocity saturation must be included.  

The **Sky130 PDK** models already account for these effects through advanced physical parameters in the BSIM (Berkeley Short-channel IGFET Model).  

SPICE simulations for lower nodes reveal that:
- Drain current saturates earlier compared to long-channel devices.
- The transition between linear and saturation regions is sharper.
- The transconductance (gm) decreases due to reduced carrier velocity.

This allows accurate modeling of real transistor behavior under high-field conditions.

---

### 1.3 Drain Current vs Gate Voltage for Long and Short Channel Devices

In long-channel MOSFETs, the drain current in saturation increases quadratically with gate voltage.  
However, in short-channel MOSFETs, due to velocity saturation, the relationship becomes nearly linear for high Vgs.

**Key differences:**
- **Long-channel:** Current ∝ (Vgs - Vth)²  
- **Short-channel:** Current ∝ (Vgs - Vth)  

This means that as device dimensions shrink, the drain current no longer grows as fast with increasing Vgs, resulting in reduced gain and transconductance.

SPICE simulations can plot Id–Vgs curves for both long and short channel devices to visualize this change in slope.

---
![velocity_saturation](output_week4/velocity_saturation_1.jpeg)
![velocity_saturation](output_week4/velocity_saturation_2.jpeg)

### 1.4 Velocity Saturation at Lower and Higher Electric Fields

In the **low electric field region**, carrier drift velocity is proportional to the electric field (v = μE).  
At **higher fields**, carriers experience scattering and their velocity approaches a limiting value — the **saturation velocity (vsat)**.

The field at which this transition occurs depends on:
- The semiconductor material (e.g., silicon, GaAs)
- Carrier mobility
- Channel length

For silicon, typical saturation velocity is around 10⁷ cm/s.  
Once the field exceeds the critical value (Ec), velocity saturation dominates and limits the drain current increase.

---
![velocity_saturation](output_week4/velocity_saturation_3.jpeg)

### 1.5 Velocity Saturation Drain Current Model

In presence of velocity saturation, the current expression is modified to include the effect of limited carrier velocity.  
Instead of quadratic dependence on gate voltage, the model assumes that velocity reaches vsat beyond a certain field, making Id nearly linear with (Vgs - Vth).

Key points of this model:
- It is more accurate for deep submicron devices.
- Current is limited by the maximum drift velocity.
- Channel-length dependence is reduced.
- Drain current increases less sharply with gate voltage.

SPICE simulations based on BSIM models automatically include this effect.

---

## 2. CMOS Voltage Transfer Characteristics (VTC)

### 2.1 Introduction

A **CMOS inverter** is the fundamental building block of all digital logic circuits.  
It consists of a **PMOS** and **NMOS** transistor connected in a complementary fashion.  
Understanding its **Voltage Transfer Characteristic (VTC)** helps analyze logic behavior, switching points, and noise margins.

---

### 2.2 MOSFET as a Switch

In digital circuits, a MOSFET operates mainly as a **voltage-controlled switch**:
- The NMOS conducts when its gate voltage exceeds the threshold voltage (logic 1).
- The PMOS conducts when its gate voltage is below its threshold voltage (logic 0).

Thus, one of the transistors is always ON, while the other is OFF, providing rail-to-rail output levels with low static power dissipation.

---

### 2.3 Standard MOS Voltage Current Parameters

The key parameters defining the inverter behavior include:
- **Vthn, Vthp:** Threshold voltages of NMOS and PMOS
- **Kn, Kp:** Process transconductance parameters (related to W/L ratios and mobility)
- **VDD:** Supply voltage
- **Vin, Vout:** Input and output voltages

These determine the inverter’s switching threshold and the shape of the VTC curve.

---

### 2.4 PMOS/NMOS Drain Current vs Drain Voltage

The PMOS and NMOS devices operate in complementary regions depending on the input voltage.  
For different Vin levels:
- NMOS current increases as Vin rises.
- PMOS current decreases as Vin rises.

At the switching point, both transistors conduct simultaneously — this defines the **transition region** of the inverter.

Plotting Id vs Vd for each device gives insight into how their currents balance to produce the inverter VTC.

---

### 2.5 Step 1 – Convert PMOS Gate-Source Voltage to Source-Voltage Relation

To analyze inverter behavior, it’s convenient to express the PMOS voltage relation in terms of the source voltage (since its source is connected to VDD).  
This helps compare the PMOS and NMOS currents under the same Vout condition.

When the input voltage Vin changes, the gate-source voltages of both devices change accordingly, affecting their conduction levels.

---

### 2.6 Step 2 & Step 3 – Convert PMOS and NMOS Drain-Source Voltage to Output Voltage

In the inverter configuration:
- The **output voltage (Vout)** is the drain voltage of both transistors.
- The NMOS source is connected to ground, and the PMOS source is connected to VDD.

By relating the drain current equations to Vout, we can analyze how current flows during switching.  
At equilibrium (static operation), the NMOS and PMOS currents are equal in magnitude, determining the output voltage for each input level.

---

### 2.7 Step 4 – Merge PMOS and NMOS Load Curves and Plot VTC

The **Voltage Transfer Characteristic (VTC)** is obtained by plotting Vout against Vin.  
This curve shows three distinct regions:

1. **NMOS Cutoff / PMOS ON:**  
   - Vin is low  
   - NMOS is OFF, PMOS pulls Vout to VDD (logic HIGH)

2. **Transition Region:**  
   - Vin is around the inverter threshold voltage  
   - Both transistors conduct partially  
   - The output voltage changes rapidly from HIGH to LOW

3. **PMOS Cutoff / NMOS ON:**  
   - Vin is high  
   - NMOS conducts fully, PMOS is OFF  
   - Vout is pulled to GND (logic LOW)

The point where both currents are equal defines the **switching threshold (Vm)** of the inverter.  
The steepness of the transition defines **noise margins** and the inverter’s switching speed.

---

### 2.8 SPICE Lab for CMOS Inverter VTC

### simulation for drain current v/s drain to source volatge:
### step-1:
```bash
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch 
```
![spice_netlist_idvds](output_week4/spice_netlist_idvds.png)
![ngspice_idvds_w0.375_l0.25.](output_week4/ngspice_idvds_w0.375_l0.25.png)
![ngspice_idvds_w1.8_l1.2](output_week4/ngspice_idvds_w1.8_l1.2.png)

### simulation for draing current v/s gate to source voltage
### step-1:
```bash
ngspice day2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```
![spice_netlist_idvgs](output_week4/spice_netlist_idvgs.png)
![ngspice_idvgs_w0.375_l0.25](output_week4/ngspice_idvgs_w0.375_l0.25.png)
![ngspice_idvgs_w1.8_l1.2](output_week4/ngspice_idvgs_w1.8_l1.2.png)


## 🧩 Summary Table

| Concept | Description | Key Observation |
|----------|--------------|----------------|
| Velocity Saturation | Limiting of carrier velocity under high fields | Reduces current and transconductance in short-channel devices |
| Long vs Short Channel | Short-channel devices show earlier current saturation | Id–Vgs becomes linear instead of quadratic |
| CMOS Inverter | Basic logic gate made from PMOS and NMOS | Converts input logic to complementary output |
| VTC | Graph of Vout vs Vin | Defines switching threshold and noise margins |
| SPICE Simulation | DC sweep of Vin | Verifies theoretical VTC and device behavior |

---

# ⚡[Day-3] CMOS Switching Threshold and Dynamic Simulations

---

## 1. Voltage Transfer Characteristics – SPICE Simulations

### 1.1 SPICE Deck Creation for CMOS Inverter

A SPICE deck is a text-based file that describes the CMOS inverter circuit. It defines the connections, components, and simulation commands required for running Ngspice.  
The CMOS inverter includes one NMOS and one PMOS transistor connected in a complementary configuration.  
The input voltage is swept from 0 to the supply voltage to obtain the DC transfer characteristic curve.  
This deck also specifies the power supply, transistor dimensions, and the model library from the Sky130 PDK.

The purpose of creating this deck is to simulate the voltage transfer characteristic (VTC) and study the static behavior of the inverter.

---

### 1.2 SPICE Simulation for CMOS Inverter

When the SPICE simulation is executed, it generates a plot of output voltage versus input voltage, known as the Voltage Transfer Characteristic.  
This curve shows how the inverter transitions from logic high to logic low as the input changes.  

At low input voltage, the NMOS is off and PMOS is on, so the output is high.  
At high input voltage, the NMOS turns on and PMOS turns off, pulling the output low.  
The middle region is the transition region, where both transistors conduct simultaneously.  

From this simulation, the switching threshold, gain, and noise margins can be observed and analyzed.

---

### 1.3 Labs – Sky130 SPICE Simulation for CMOS Inverter

### simulation for inverter transistor chateristices
### step-1:
```bash
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs time in
```
![netlist_spice_inv_trns](output_week4/netlist_spice_inv_trns.png)
![ngspice_inv_trns](output_week4/ngspice_inv_trns.png)

### simulation for inverter vtc chateristices
### step-1:
```bash
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs in
```
![netlist_spice_inv_vtc](output_week4/netlist_spice_inv_vtc.png)
![ngspice_inv_vtc](output_week4/ngspice_inv_vtc.png)

## 2. Static Behavior Evaluation – CMOS Inverter Robustness and Switching Threshold

### 2.1 Switching Threshold, Vm

The switching threshold is the input voltage at which the inverter output equals the input voltage.  
At this point, both the NMOS and PMOS transistors conduct the same amount of current, balancing the circuit.  
It determines how the inverter switches between logic states and plays a major role in defining noise immunity and stability.  

A well-designed inverter has its switching threshold close to the mid-point of the supply voltage, ensuring symmetrical switching characteristics.

---
![switch_threshold](output_week4/switch_threshold.jpeg)

### 2.2 Effect of Device Sizing on Switching Threshold

The switching threshold depends on the relative strengths of the NMOS and PMOS transistors.  
If both transistors are of equal size, the threshold might shift away from the center due to mobility differences.  
Since electrons move faster than holes, the NMOS tends to be stronger than the PMOS.  
To balance this, the PMOS transistor is usually made wider to equalize the drive current.  

Proper sizing results in a symmetrical transfer curve and equal noise margins for both logic levels.

---
![switch_threshold_comparision](output_week4/switch_threshold_comparision.jpeg)

### 2.3 Determining Transistor Sizing for Desired Threshold

By adjusting the width and length of the PMOS and NMOS devices, the inverter’s switching threshold can be positioned as desired.  
If the PMOS is made stronger, the threshold voltage shifts upward, and if the NMOS is stronger, the threshold shifts downward.  
This control helps in optimizing circuit behavior for speed, power, or robustness depending on the design target.  

This aspect is very important for digital design where uniform switching points ensure predictable logic operation across multiple gates.

---
![switch_threshold_equations](output_week4/switch_threshold_equations_1.jpeg)
![switch_threshold_equations_2](output_week4/switch_threshold_equations_2.jpeg)

### 2.4 Static and Dynamic Simulation of CMOS Inverter

The static simulation involves sweeping the input voltage slowly to observe the steady-state response, producing the VTC.  
It helps analyze static parameters such as noise margins, gain, and switching voltage.  

In contrast, the dynamic or transient simulation applies a time-varying input signal, such as a pulse, to observe how fast the output responds.  
From this, important timing parameters like rise time, fall time, and propagation delay are extracted.  
These values show how quickly the inverter can switch states, which directly affects the overall speed of digital circuits.

---

### 2.5 Simulation with Increased PMOS Width

Increasing the PMOS width improves the pull-up strength of the inverter, helping the output reach the high voltage faster.  
This modification results in a more balanced and symmetric transfer curve and reduces the rise time during dynamic operation.  
However, it also slightly increases the load capacitance, leading to a small increase in power consumption.  

In practice, designers tune the PMOS width carefully to achieve the desired trade-off between speed, symmetry, and power efficiency.

---

### 2.6 Applications of CMOS Inverter in Clock Network and STA

CMOS inverters are the most fundamental elements in digital design.  
They are used to build logic gates, buffers, and complex circuits like flip-flops and memory cells.  
In clock networks, inverters are employed to shape, buffer, and distribute the clock signal efficiently while maintaining proper timing.  

During static timing analysis (STA), inverter characteristics such as delay, slew rate, and transition time are used to calculate timing paths across the circuit.  
Hence, understanding the inverter’s static and dynamic behavior is essential for reliable digital system design.

---

## Summary

| Concept | Description |
|----------|-------------|
| SPICE Deck | Defines inverter circuit and simulation setup |
| VTC | Output vs Input curve showing switching behavior |
| Switching Threshold | Point where inverter output equals input |
| Device Sizing | Adjusts threshold and symmetry of VTC |
| Static Simulation | Finds steady-state characteristics |
| Dynamic Simulation | Measures delay, rise, and fall times |
| PMOS Width Increase | Improves pull-up strength and symmetry |
| Applications | Used in clock buffers, logic gates, and timing paths |

---

#  🔊 [Day-4] CMOS Noise Margin

## 📌 What is Noise Margin?

In digital circuits, logic signals pass through multiple logic gates. During transmission, these signals can become distorted due to electrical interference or noise. **Noise Margin** describes how tolerant a digital circuit is to this unwanted noise while still correctly interpreting logic levels.

---

## 🔧 CMOS Inverter and Noise

A **CMOS inverter** is the fundamental building block of CMOS logic. It consists of:
- A **PMOS transistor** connected to the supply voltage.
- An **NMOS transistor** connected to ground.

The inverter converts logic `1` to logic `0` and vice versa. But due to real-world effects like voltage drops and interference, signals aren't perfect. The output may not fully reach ideal values, causing **noise**. Noise Margin measures how reliably the inverter handles such noise disturbances.

---
![noise_margin_ideal_practical](output_week4/noise_margin_ideal_practical.jpeg)


## 🧭 Static Behavior and Voltage Levels

Digital signals are ideally binary but represented by voltage ranges:
- A higher voltage range represents logical `1` (HIGH).
- A lower voltage range represents logical `0` (LOW).

The **input-output voltage characteristics** of a CMOS inverter define how well the inverter distinguishes between valid logic states. Noise Margin is derived from these transitions and margins around valid logic levels.

---
![noise_margin](output_week4/noise_margin.jpeg)


## ✅ Importance of Noise Margin

- Ensures **signal reliability** in real circuits.
- Allows **safe logic level interpretation** even with disturbances.
- Provides **design robustness** against manufacturing variations and temperature changes.
- Improves **signal integrity** across long interconnect wires.

---

## 🔄 Factors Affecting Noise Margin

Noise Margin depends on several practical design factors:

| Factor | Effect |
|--------|--------|
| **Power supply voltage** | Lower voltage reduces signal difference and noise tolerance |
| **Transistor sizing** | Width and length impact drive strength and switching threshold |
| **PMOS/NMOS ratio** | Imbalanced sizing shifts switching threshold affecting stability |
| **Temperature** | Higher temperature increases resistance and noise susceptibility |
| **Process variations** | Variations during chip fabrication alter transistor behavior |
| **Load capacitance** | Large loads cause slower transitions and more noise sensitivity |

---

## ⚖️ PMOS Width Impact

The width of the PMOS transistor influences:
- **Switching threshold** of the inverter
- **Rise time** of the output signal
- **Voltage noise robustness**

Increasing PMOS width generally improves the ability of the circuit to drive logic HIGH, but may also increase power consumption and input capacitance. Proper sizing of PMOS relative to NMOS is key for balanced design and stable noise margins.

---
![noise_margin_comparison](output_week4/noise_margin_comparison.jpeg)
![noise_margin_summary](output_week4/noise_margin_summary.jpeg)

## 🔬 Practical Evaluation (Sky130)

### simulation for inverter noise margin 
### step-1:
```bash
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot -vdd#branch
```
### output 
![ngspice_noise_margin.png](output_week4/ngspice_noise_margin.png)

# 🧪 [Day 5] CMOS Power Supply and Device Variation Robustness 

## ⚡ Power Supply Variation

A digital circuit depends on a stable supply voltage to maintain correct switching. However, in real hardware, the supply voltage can fluctuate due to switching activity, IR drops, noise in the power grid, or external interference. These changes are called **power supply variations**.

### 🔧 Effects of Supply Variation

| Variation Type | Impact |
|----------------|--------|
| Lower supply voltage | Slower switching, reduced noise margin, possible logic errors |
| Higher supply voltage | Higher speed but increased power and reliability risk |
| Fluctuating supply | Unstable output behavior |

### ✅ Importance

Power supply robustness ensures:
- Stable logic levels
- Consistent performance
- Resistance to power noise
- Reliable circuit operation across conditions

---

## ⚙️ Smart SPICE Simulation for Supply Variation

Simulation tools like **Ngspice or SmartSPICE** are used to analyze how the inverter behaves when supply voltage changes. By testing under multiple voltage conditions, designers evaluate circuit stability. This step ensures the circuit remains functional under real-world power variation.

---
### steps for simulation:
### step-1:
```bash
ngspice day5_inv_supplyvariation_Wp1_Wn036.spice
plot out vs in
```
### output:
![spice_netlist_supply_variation](output_week4/spice_netlist_supply_variation.png)
![ng_spice_supply_variation](output_week4/ng_spice_supply_variation.png)


## ⚖️ Advantages and Disadvantages of Low Supply Voltage

Using lower supply voltage can reduce power consumption but increases risk of logic failure.

| ✅ Advantages | ⚠️ Disadvantages |
|---------------|------------------|
| Reduces dynamic power | Reduces noise margin |
| Less heat generated | Slows down switching |
| Improves efficiency | Increases sensitivity to variations |

---

## 🏗️ Device Variation

Even when using the same fabrication process, transistors on a chip are not exactly identical. Small physical differences occur during manufacturing, causing **device variations**.

### 🔬 Sources of Device Variations

| Source | Description |
|--------|-------------|
| Etching variations | Inaccurate pattern transfer during lithography |
| Oxide thickness variations | Uneven gate oxide affects transistor behavior |
| Channel doping variations | Changes in impurity concentration affect switching |
| Line edge roughness | Irregular transistor dimensions |

These variations change transistor behavior from its design intent, affecting circuit performance.

---
![device_variation](output_week4/device_variation.jpeg)

### inverter chain analysis:
![design_variation_inverter_chain](output_week4/design_variation_inverter_chain.jpeg)
![device_variation_oxidation_process](output_week4/device_variation_oxidation_process.jpeg)


## 🧪 Simulation of Device Variations

Smart SPICE simulations help observe inverter behavior under **process variations**. Random variations are applied to transistor dimensions and material properties to evaluate robustness. This ensures the design works even in worst-case silicon fabrication scenarios.

---
### simulation steps:
### step-1:
```bash
ngspice day5_inv_devicevariation_wp7_wn042.spice
plot out vs in
```

### output:
![spice_netlist_device_variation](output_week4/spice_netlist_device_variation.png)
![ngspice_device_variation](output_week4/ngspice_device_variation.png)

## ✅ Design Robustness

Robust CMOS design must tolerate:
- Power supply changes
- Manufacturing variations
- Temperature changes
- Electrical noise

To improve robustness:
- Proper transistor sizing
- Using guard bands
- Corner simulations
- Power integrity design

---

---

## ✅ Conclusion:

Throughout **Day 1 to Day 5** of the NgspiceSky130 learning journey, we explored the essential foundations of CMOS circuit behavior and robustness using simulations. We began by understanding the fundamental current–voltage characteristics of MOSFET devices and gradually moved toward complete CMOS inverter design. We analyzed how transistor physics, switching behavior, and voltage transfer influence digital logic functionality.

From there, we evaluated the **reliability** of CMOS inverters by studying **noise margins, power supply variations, and device-level fabrication variations**. These concepts are critical in real-world chip design, where circuits must operate correctly across changing environmental and manufacturing conditions. Through simulations, we gained insights into how design choices such as transistor sizing, supply voltage, and layout consistency impact circuit performance and stability.

This foundation is essential for progressing further into **digital IC design, timing analysis, and physical layout implementation**. With a firm understanding of CMOS behavior and robustness, we are now prepared to advance to more complex circuit design and verification challenges.

🚀 **This marks the completion of the fundamental CMOS inverter study using Sky130 and Ngspice — building a strong base for VLSI design and open-source chip development.**

---

