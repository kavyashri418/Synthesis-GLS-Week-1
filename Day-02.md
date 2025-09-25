# 1 Timing libs, Hierarchical vs flat synthesis and efficient flop coding types

## 1.1 Introduction to timing.libs

A timing library (usually with extension .lib, often called a Liberty file) is a standard cell characterization file used in digital design.
It describes the electrical, functional, and timing behavior of each cell in a given standard cell library under specific process-voltage-temperature (PVT) conditions.

## LAB - 04 Introduction to dot lib

Step 1: Install Neovim Text Editor

```bash
sudo apt install neovim -qt
```

Step 2: Open sky130_fd_sc_hd__tt_025C_1v80.lib

```bash
gvim sky130_fd_sc_hd__tt_025C_1v80.lib
```
## 1.2 Hierarchical vs Flat Synthesis

HIERARCHICAL SYNTHESIS: in hierarchical synthesis, the RTL design is synthesized block by block (module by module) instead of flattening everything into a single netlist.Each block is optimized independently, and then the blocks are connected together at the top level.

Advantages
- Large designs are easier to manage in smaller pieces.
- Already-synthesized blocks can be reused in multiple projects.

Disadvantages
- Since blocks are optimized individually, cross-boundary optimizations (sharing logic, retiming, etc.) are limited.
- Block-to-block connections may cause extra buffers and delay.

FLAT SYNTHESIS: in flat synthesis, the entire RTL design hierarchy is flattened into one big netlist before synthesis.The synthesis tool sees the whole design as a single module and optimizes globally.

Advanatages
- Global optimization gives potentially better timing, area, and power.
- Logic sharing, retiming, restructuring across module boundaries is possible.

Disadvantages
- Large designs take much longer and require more compute resources.
- Debugging and floorplanning are harder.

## LAB - 05 Hier Synthesis and Flat Synthesis

# Hierarchical Synthesis LAB

Step 1: Open Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```

Step 3: Read verilog design

```bash
read_verilog multiple_modules.v      
```

Step 4: Define the top Module

```bash
synth -top multiple_modules  
```

Step 5: abc logic synthesis

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: View the Schematic

```bash
show multiple_modules
```

# Flat Synthesis LAB

Step 1: Open Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```

Step 3: Read verilog design

```bash
read_verilog multiple_modules.v     
```

Step 4: Define the top Module

```bash
synth -top multiple_modules  
```

Step 5: abc logic synthesis

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: Flatten synthesis

```bash
flatten
```

Step 7: View the schematic

```bash
show multiple_modules
```

<img width="1280" height="542" alt="image" src="https://github.com/user-attachments/assets/0d4d247c-a2b5-4b15-9ad3-c2b583848b12" />


## 1.3 Various Flop Coding Styles and Optimization

WHY FLOP?
A flip-flop (DFF, register) is a sequential element that stores data on a clock edge.In RTL coding, synthesis tools infer flops from always blocks with clock sensitivity.

- Asynchronous Reset: the Reset (or Clear) signal forces the flip-flop output Q = 0 immediately, regardless of the clock.
  Asynchronous Reset D Flip Flop

```bash
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk, posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

- Asynchronous Set: the Set signal forces the flip-flop output Q = 1 immediately, regardless of the clock. It is typically active-high (or active-low depending on library).
  Asynchronous Set D Flip Flop

 ```bash
module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk, posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```

- Synchronous ReSet: the flip-flop output Q = 0, but only on the active clock edge. The reset signal is sampled at the clock edge — if asserted, the flop outputs 0 at that edge
 Synchronous Set D Flip Flop

 ```bash
module dff_syncres (input clk, input async_reset, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

## LAB - 5.1 DFF

Step 1: Compile Design and Testbench

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_dff_asyncres.vcd
```

![WhatsApp Image 2025-09-26 at 3 30 06 AM](https://github.com/user-attachments/assets/a6370f46-8941-41eb-8b3a-7879fac3bc25)


Synthesis with Yosys

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog /path/to/dff_asyncres.v
```

Step 4: Define the top Module

```bash
synth -top dff_asyncres
```

Step 5: Mapping flip flop

```bash
dfflibmap -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: abd logic synthesis

```bash
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 7: View the schematic

```bash
show
```

![WhatsApp Image 2025-09-26 at 3 30 05 AM](https://github.com/user-attachments/assets/92c6947b-93f0-4c83-9467-bab8cdce208a)


# dff_async_set.v as DUT

![WhatsApp Image 2025-09-26 at 3 30 04 AM](https://github.com/user-attachments/assets/f9bbb226-bc01-4c0b-8f85-a1ef8c973e1f)

![WhatsApp Image 2025-09-26 at 3 30 03 AM](https://github.com/user-attachments/assets/c3216d9a-c924-4cd5-b49e-7059988aefe1)


# dff_syncres.v as DUT


![WhatsApp Image 2025-09-26 at 3 30 02 AM](https://github.com/user-attachments/assets/6dc3a9cb-463a-4311-bdc7-0fd16d06ec5f)


<img width="1280" height="583" alt="image" src="https://github.com/user-attachments/assets/8a515139-0fc4-4763-8f49-bef01d01e858" />








  

















