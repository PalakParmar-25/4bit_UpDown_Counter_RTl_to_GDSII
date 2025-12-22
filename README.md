# 🔷 4-Bit Priority Encoder | RTL to GDSII Flow

## 📌 Project Overview
This repository presents the complete **RTL to GDSII implementation of a 4-bit Priority Encoder**, designed using **Verilog HDL** and implemented using **Synopsys industry-grade EDA tools**.

A priority encoder is a fundamental combinational digital circuit that outputs the binary representation of the **highest-priority asserted input**, along with a **valid signal** indicating whether any input is active. This project demonstrates how a simple RTL design is transformed into a **fabrication-ready physical layout (GDSII)** using a standard ASIC design flow.

---

## 🧠 About the RTL to GDSII Workshop
This project was developed during a **hands-on RTL to GDSII VLSI Workshop** conducted at **Pandit Deendayal Energy University (PDEU)** in collaboration with the **ChipIN Centre**.

The workshop provided structured guidance on:
- Understanding the complete VLSI backend flow  
- Working with Synopsys tools in a Linux environment  
- Executing each design stage using Tcl-based automation  

The workshop acted as a **guided framework**, while the complete design implementation and analysis were carried out as part of this project.

---

## ⚙️ Design Description – 4-Bit Priority Encoder
- **Inputs:** 4-bit input vector  
- **Outputs:**  
  - 2-bit encoded output  
  - `valid` signal indicating presence of any active input  
- **Priority Scheme:** MSB has the highest priority  
- **Clocked Design:** Inputs and outputs are registered for synchronous operation  

---

## 🔁 RTL to GDSII Flow (Humanized Explanation)

### 1️⃣ RTL Coding (Verilog HDL)
The design begins with a **Register Transfer Level (RTL)** description written in Verilog. The priority encoder logic is implemented using combinational logic, while registers are used to synchronize inputs and outputs with the clock.

### 2️⃣ RTL Simulation & Functional Verification
The RTL design is verified using **Synopsys VCS** to ensure correct functionality before synthesis. **Verdi** is used to analyze waveforms and visualize the design behavior, helping catch logic errors early in the flow.

### 3️⃣ Logic Synthesis (Gate-Level Netlist)
Using **Synopsys Design Compiler**, the verified RTL is converted into a **gate-level netlist** mapped to the **SAED 32nm standard cell library**. Timing constraints are applied, and reports for **area, power, and timing slack** are generated.

### 4️⃣ Physical Design (Floorplan to Routing)
The synthesized netlist is imported into **Synopsys IC Compiler II (ICC2)**, where the physical implementation takes place. This includes:
- Floorplanning and power planning  
- Standard cell placement  
- Clock Tree Synthesis (CTS)  
- Signal routing  

At this stage, the logical design becomes an actual **chip layout**.

### 5️⃣ Static Timing Analysis (STA)
Post-layout timing verification is performed using **Synopsys PrimeTime**. Parasitic information extracted after routing is used to confirm that the design meets all timing requirements under realistic conditions.

### 6️⃣ GDSII Generation
Finally, a clean **GDSII layout** is generated, representing the final physical design ready for silicon fabrication.

---

## 🛠️ Tools & Environment

### Design & Verification
- **Verilog HDL** – RTL design  
- **Synopsys VCS** – RTL simulation  
- **Verdi** – Waveform and schematic debugging  

### Synthesis
- **Synopsys Design Compiler (DC Shell)**  
- **SAED 32nm RVT Standard Cell Library**

### Physical Design
- **Synopsys IC Compiler II (ICC2)**  

### Timing Analysis
- **Synopsys PrimeTime (PT Shell)**  

### Environment
- **Linux (Rocky Linux)**  
- **Shell-based flow with Tcl scripting**

---

## 📁 Repository Structure
```bash
4bit-priority-encoder/
├── rtl/
│   └── pri_enc_rtl.v
├── testbench/
│   └── pri_enc_tb.v
├── constraints/
│   └── pri_enc.sdc
├── dc/
│   └── run_dc.tcl
├── icc2/
│   └── scripts/
├── pt/
│   └── run_pt.tcl
├── reports/
│   ├── timing/
│   ├── area/
│   └── power/
└── results/
    └── pri_enc.gds
