# Open-Source Simulator Verilog

## 1. Introduction to Open-Source Simulator Verilog

### 1.1 Introduction to Icarus Verilog (iverilog), Design and Testbench

**Simulator** - RTL design is checked for adherence to the spec by simulating the design. Simulator is the tool used for simulating the design.  
- *iverilog* is the tool used in this course.

**Design** - Design is the actual Verilog code or set of Verilog codes which has the intended functionality to meet the required specifications.

**Testbench** - Testbench is the setup to apply stimulus (test_vector) to the design to check its functionality.

**How simulator works?**  
- Simulator looks for changes on the input signals.  
- Upon change to the input, the output is evaluated.  
- If no changes to the input, no change to the output!  
- Simulator is looking for changes in the values of input.


<img width="3257" height="1526" alt="Screenshot 2025-09-22 235437" src="https://github.com/user-attachments/assets/b5aa8efe-56c1-4fdc-9f39-cd8e8f98b5a7" />


**Note:**  
- Design may have one or more primary inputs, one or more primary outputs.  
- Testbench doesn’t have a primary input or primary outputs.

**Iverilog based simulation flow**

### 1.2 Introduction to Yosys

**Synthesizer** - Tool used for converting the RTL to netlist.  
- *Yosys* is the synthesizer used in this course.

**Verify the synthesis**  

**Note:**  
- The set of primary inputs / primary outputs will remain same between the RTL design and synthesized netlist.  
- The same testbench can be used!

## 1.3 Introduction to Logic Synthesis


<img width="2653" height="1835" alt="Screenshot 2025-09-23 022104" src="https://github.com/user-attachments/assets/28fa8736-e0d1-4c11-8284-c68d4ea1450d" />


**RTL Design** - Behavioral representation of the required specification.

**Synthesis**  
- RTL to gate-level translation.  
- The design is converted into gates and the connections are made between the gates.  
- This is then output as a file called **netlist**.


<img width="1181" height="1778" alt="Screenshot 2025-09-23 022147" src="https://github.com/user-attachments/assets/e9c572b9-f27b-4d74-9418-a084dcbb570b" />


### What is `.lib`?  
- `.lib` is a collection of logical modules, includes basic logic gates like AND, OR, NOT, etc.  
- Different flavours of the same gate:  

**2-input AND gate**  
- Slow  
- Medium  
- Fast  

**3-input AND gate**  
- Slow  
- Medium  
- Fast  

### Why different flavours of gate?  
- Combinational delay in logic path determines the maximum speed of operation of a digital logic circuit.  
- We need cells that work fast to make **Tcombi** small.


<img width="2659" height="792" alt="Screenshot 2025-09-23 022215" src="https://github.com/user-attachments/assets/cd16b876-cecb-4279-a6b4-5d21780105a5" />


### Why we need slow cells?  
- To ensure that there are no **HOLD** issues at DFF_B, we need cells that work slowly.  
- Hence, we need cells that work fast to meet the required performance and slow to meet HOLD.  
- The collection of these cells forms the **.lib**.

### Faster Cells vs Slower Cells  
- **Load in digital logic circuit** → capacitance  
- Faster the charging/discharging of capacitance → lesser the cell delay  
- To change/discharge the capacitance fast, we need transistors capable of sourcing more current → **wide transistors**  
- Wider transistors → low delay → more area and power as well  
- Faster cells don’t come free; they come at the penalty of **area and power**.

### Selection of cells  
- Need to guide the synthesizer to select the flavour of cells that is optimum for the implementation of the logic circuit.  

**More use of faster cells**  
- Bad circuit in terms of power and area  
- Hold time violations  

**More use of slower cells**  
- Sluggish circuit, may not meet performance needs  

**The guidance offered to the synthesizer → "constraints"**

### LAB - 01 Introduction to iverilog Design Testbench

## Simulation of 2:1 Multiplexer

Clone the repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

Step 1: Navigate to verilog_files folder

```bash
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

Step 2: Design simulation

- We are using good_mux.v DUT file for sumulation.
- tb_good_mux.v is the testbench file.
- To compile Design and Testbench use

```bash
iverilog good_mux.v tb_good_mux.v
```

To run simulation 

```bash
./a.out
```

### LAB - 02 Introduction to iverilog gtkwave

Step 1: Open gtkwave to view waveform

```bash
gtkwave tb_good_mux.vcd
```
Step 2: Launches the graphical version of Vim

```bash
gvim tb_good_mux.v -o good_mux.v
```

### LAB - 03 Introduction to Yosys and sky130 PDKs

Step 1: Open Yosys

```bash
yosys
```

Step 2: Load standerd cell library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  //use path to locate sky130_fd_sc_hd__tt_025C_1v80.lib file
```

Step 3: Read verilog design

```bash
read_verilog good_mux.v //Loads your HDL design
```

Step 4: Define the top Module

```bash
synth -top good_mux  //Synthesize your RTL into generic gate
```

Step 5: Invoke abc synthesis and optimization tool

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: Gate-Level Netlist

```bash
write_verilog good_mux_netlist.v
```

Step 7: View the Schematic

```bash
show
```






