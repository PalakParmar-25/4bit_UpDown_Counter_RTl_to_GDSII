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

#### Verilog code used  for design : 
```bash
module pri_enc ( 
input Clock, 
input [3:0] in, 
output reg [1:0] out, 
output reg valid 
); 
// Internal registers 
reg [3:0] reg_in; 
reg [1:0] encoded_out; 
reg valid_i; 
// for input 
always @(posedge Clock) begin 
reg_in <= in; 
end 
// for output 
always @(posedge Clock) begin 
out   <= encoded_out; 
valid <= valid_i; 
end 
// Combinational logic for priority encoding 
always @(*) begin 
valid_i = 1'b1; 
case z (reg_in) 
4'b1???: encoded_out = 2'b11;  // Input[3] is highest priority 
4'b01??: encoded_out = 2'b10;  // Input[2] 
4'b001?: encoded_out = 2'b01;  // Input[1] 
4'b0001: encoded_out = 2'b00;  // Input[0]
default: begin 
encoded_out = 2'b00; 
valid_i = 1'b0; 
end 
endcase 
end 
endmodule  
```
#### Verilog code used  for testbench : 
```bash
`timescale 1ns/1ps 
`include "pri_enc_rtl.v" 
module tb_pri_enc; 
reg Clock; 
reg [3:0] in; 
wire [1:0] out; 
wire valid; 
pri_enc dut ( 
.Clock(Clock), 
.in(in), 
.out(out), 
.valid(valid) 
); 
// Clock generation 
initial Clock = 0; 
always #5 Clock = ~Clock;  // 10ns clock period 
initial begin 
$fsdbDumpvars(); 
$monitor("Time=%0dns in=%b -> out=%b valid=%b", $time, in, out, valid); 
in = 4'b0000; #10; 
in = 4'b0001; #10; 
in = 4'b0010; #10; 
in = 4'b0100; #10; 
in = 4'b1000; #10; 
in = 4'b1100; #10; 
in = 4'b0110; #10; 
in = 4'b1010; #10; 
in = 4'b1111; #10; 
$finish; 
end 
endmodule 
```
### 2️⃣ RTL Simulation & Functional Verification
The RTL design is verified using **Synopsys VCS** to ensure correct functionality before synthesis. **Verdi** is used to analyze waveforms and visualize the design behavior, helping catch logic errors early in the flow.

<img width="750" height="316" alt="image" src="https://github.com/user-attachments/assets/6ca6b0e9-f5cf-4119-a63c-6096cb6e2b78" />
<img width="635" height="287" alt="image" src="https://github.com/user-attachments/assets/8823a58e-2091-4a20-abfb-938d05f4d2e3" />

<img width="673" height="273" alt="image" src="https://github.com/user-attachments/assets/754134bc-1be9-40c9-90de-8ec0de1d9ede" />


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
