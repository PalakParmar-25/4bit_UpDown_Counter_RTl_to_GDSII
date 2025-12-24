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
- The RTL design is verified using **Synopsys VCS** to ensure correct functionality before synthesis. **Verdi** is used to analyze waveforms and visualize the design behavior, helping catch logic errors early in the flow.

- The below give image is Verdi schematic view showing RTL-level connectivity and synthesized logic mapping of the priority encoder, highlighting signal flow from inputs through combinational logic to outputs.
<img width="750" height="316" alt="image" src="https://github.com/user-attachments/assets/6ca6b0e9-f5cf-4119-a63c-6096cb6e2b78" />

<img width="635" height="287" alt="image" src="https://github.com/user-attachments/assets/8823a58e-2091-4a20-abfb-938d05f4d2e3" />

- Verdi debug environment showing hierarchy, RTL source code, and simulation waveforms of the priority encoder, verifying correct output encoding and valid signal behavior.
  
<img width="673" height="273" alt="image" src="https://github.com/user-attachments/assets/754134bc-1be9-40c9-90de-8ec0de1d9ede" />


### 3️⃣ Logic Synthesis (Gate-Level Netlist)
Using **Synopsys Design Compiler**, the verified RTL is converted into a **gate-level netlist** mapped to the **SAED 32nm standard cell library**. Timing constraints are applied, and reports for **area, power, and timing slack** are generated.

<img width="542" height="342" alt="image" src="https://github.com/user-attachments/assets/defa39f7-2c57-449b-a03a-6766dd0f5fa6" />

<img width="678" height="375" alt="image" src="https://github.com/user-attachments/assets/7aeb26fb-0451-4aa0-b685-d91dcc5045cc" />

### 4️⃣ Physical Design (Floorplan to Routing)

The synthesized netlist is imported into **Synopsys IC Compiler II (ICC2)**, where the physical implementation takes place. This includes:
#### Floorplanning :
  - Initial chip layout defining core area, aspect ratio, and I/O placement.
  - Major blocks are arranged to optimize area utilization and reduce interconnect congestion.
    
  <img width="643" height="377" alt="image" src="https://github.com/user-attachments/assets/7fa3e8a7-bc74-4a49-8778-c60d66a9f6a6" />
  
#### Power planning :
  - Initial chip layout defining core area, aspect ratio, and I/O placement.
  - Major blocks are arranged to optimize area utilization and reduce interconnect congestion.
    
  <img width="619" height="424" alt="image" src="https://github.com/user-attachments/assets/4a707c44-18a6-48bd-88ef-404049176d70" />
  
#### Standard cell placement :
  - Standard cells are placed within the core while meeting timing and congestion constraints.
  - The placement is optimized to reduce wirelength and improve performance.
    
  <img width="619" height="424" alt="image" src="https://github.com/user-attachments/assets/faaacf1e-5e11-4def-bf73-8cc06eb7c43a" />

#### Clock Tree Synthesis (CTS) :
- The pink-highlighted routing represents the global clock distribution network.
- It delivers the clock signal uniformly to all sequential elements (D flip-flops) in the design, ensuring synchronous operation and minimized clock skew across registers.
  
  <img width="619" height="424" alt="image" src="https://github.com/user-attachments/assets/1bf5327f-5f69-490d-a9bd-be59cf51a95c" />

  <img width="619" height="424" alt="image" src="https://github.com/user-attachments/assets/9a2580d0-f2bf-4f14-b3c1-06437c96f076" />

#### Signal routing :
  - All signal, clock, and power connections are fully routed across metal layers.
  - This stage completes physical connectivity while satisfying DRC and timing requirements.
  - At this stage, the logical design becomes an actual **chip layout**.
  <img width="619" height="424" alt="image" alt="image" src="https://github.com/user-attachments/assets/9e608061-3f57-4d9e-a822-80c3c17bae90" />



### 5️⃣ Static Timing Analysis (STA) :
PrimeTime (PT) Shell is used in the RTL-to-GDSII flow for performing static timing analysis (STA) after synthesis or place-and-route. It verifies that the design meets timing constraints like setup, hold, recovery, and removal times. PT Shell reads in gate-level netlists, standard cell libraries, and timing constraints (SDC) to report worst-case paths and slack. This ensures the final chip layout will function correctly at the target clock frequency.
The slack got at the PT shell is 0.0355473 ns.


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
