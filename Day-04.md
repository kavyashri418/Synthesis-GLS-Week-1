# 4 GLS, Blocking vs Non-Blocking and Synthesis Simulation Mismatches

## 4.1 Introduction to GLS

What is GLS?
- Running the test bench with netlist as design under test.
- Netlist is logically same as RTL code.
   - Same test bench will align with the design.

Why GLS?
- Verify the logical correctness of design after synthesies.
- Ensuring the timing of the design is met.
  - For this GLS needs to be run with delay annotation.
 
When is GLS Performed?
- After synthesis: Once the RTL is converted into a gate-level netlist.
- Before physical design: To catch issues early, before layout.

Types of GLS
- Functional GLS: Logic-only simulation, often with zero or unit delays.
- Timing GLS: Uses annotated timing data to check real-world timing behavior.

### GLS using Iverilog

<img width="996" height="398" alt="image" src="https://github.com/user-attachments/assets/ed57e4ac-14b3-4195-a48e-e366a18bce4e" />


## 4.2 Synthesis Simulation Mismatch

A synthesis–simulation mismatch happens when the RTL design behaves differently in simulation (pre-synthesis) compared to how it works after synthesis (post-synthesis netlist simulation or on actual hardware).This usually means something in the RTL code is not synthesizable as you intended, or the synthesis tool optimized it differently than expected.

It happens because of the following reasons

- Missing sensitivity list
- Blocking vs non-blocking assignments
- Non-standard verilog coding

a) Missing sensitivity list
always block is evaluated only when sel is changing. So output y is not evaluated when sel is not changing although i0and i1 are changing. Rather it acts like a latch. In this case always is evaluated for any signal changes.

b) Blocking vs Non-Blocking statements in verilog

  Blocking --> =
  - Executes the statements in the order it is written.
  - So the first statements is evaluated before the second statement.
  - Suitable for Combinational logic (e.g., always @(*)).

  Non-Blocking --> <=
  - Executes all the RHS when always block is entered and assigns to LHS.
  - Parallel evaluation.
  - Suitable for Sequential logic (e.g., always @(posedge clk)).

## LAB - 08 GLS and Synthesies Simulation Mismatch

a) Simulation

Step 1: Compile Design and Testbench

```bash
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_ternary_operator_mux.vcd
```

<img width="1210" height="500" alt="image" src="https://github.com/user-attachments/assets/5415d53b-7d7b-4b57-b3b8-3ccb46650a4d" />





b) Synthesis with Yosys

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog ternary_operator_mux.v
```

Step 4: Define the top Module

```bash
synth -top ternary_operator_mux
```

Step 5: abc logic synthesis

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

Step 6: Netlist Export

```bash
write_verilog -noattr ternary_operator_mux_net.v 
```

Step 7: View the schematic

```bash
show
```

<img width="1210" height="612" alt="image" src="https://github.com/user-attachments/assets/2861f621-1812-49a2-ad51-d579557ec472" />





c) Gate-Level Simulation of MUX

Step 1: Compile Design and Testbench

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v   //find primitives and sky130_fd_sc_hd.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_ternary_operator_mux.vcd
```

<img width="1211" height="499" alt="image" src="https://github.com/user-attachments/assets/e6815889-1ac7-4ec8-8604-16f285d312d8" />


## Bad Mux Example

Verilog code with intentional issues:

```bash
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```
Issues: 

- Incomplete sensitivity list: Should include i0, i1, and sel.
- Non-blocking assignment in combinational logic: Should use blocking assignments (=).

Corrected version:

```bash
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

In this lab we are using bad_mux.v file as DUT and tb_bad_mux.v as testbench.

<img width="1211" height="499" alt="image" src="https://github.com/user-attachments/assets/6dab1e32-7528-44d9-a08e-3f7760c106cc" />

<img width="1211" height="623" alt="image" src="https://github.com/user-attachments/assets/1c19bc41-1dc7-4eeb-847d-e4ac37a0ab92" />




Perform GLS on the bad_mux.

<img width="1211" height="507" alt="image" src="https://github.com/user-attachments/assets/c324d60f-08cb-4b8f-8c60-ec5bcb5d4230" />

## LAB - 09 Synthesis-Simulation Mismatch for Blocking Statements

Verilog code:
```bash
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

Issues:

- The order of assignments causes d to use the old value of x—not the newly computed value.

Corrected version:

```bash
always @ (*) begin
  x = a | b;
  d = x & c;
end
```

<img width="1211" height="499" alt="image" src="https://github.com/user-attachments/assets/f371c372-7bc9-467f-8388-40278ec13e31" />

<img width="1211" height="499" alt="image" src="https://github.com/user-attachments/assets/3a9a72ea-4e31-42d4-9d09-60fa8449dfef" />

<img width="1211" height="651" alt="image" src="https://github.com/user-attachments/assets/6745987e-ae39-41a3-8ffa-3e4d17a50274" />
