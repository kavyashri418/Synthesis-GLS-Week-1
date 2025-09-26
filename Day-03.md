# 3 Combinational and Sequential Optimizations

## 3.1 Introduction to Optimization
Optimization in design means transforming logic/netlists into more efficient implementations without changing functionality.

- Area minimization (fewer gates, smaller die size).
- Power reduction (lower switching activity & leakage).
- Performance improvement (higher speed, meet timing).
- Testability and reliability (support DFT, reduce critical paths).

## 3.2 Combinational Logic Optimization

Works on pure logic (no flops).

## Constant Propagation
Constant propagation is a combinational logic optimization technique where signals or expressions that are known to have constant values are replaced with those constants throughout the logic. This reduces unnecessary logic and can simplify circuits.

## Boolean Logic Propagation
Boolean logic propagation is a combinational optimization technique where known logical relationships are used to simplify expressions and eliminate redundant logic. It leverages Boolean algebra rules to optimize circuits.

## 3.3 Sequential Logic Optimization

Works on designs with flip-flops / registers.

## State Optimization
State optimization is a sequential logic optimization technique aimed at reducing the number of states in a finite state machine (FSM) without changing its functional behavior. Fewer states mean simpler logic for next-state and output functions, leading to reduced area, power, and potentially faster timing.

## Retiming
Retiming is a sequential optimization technique where registers (flip-flops) in a circuit are repositioned across combinational logic to improve timing performance, area, or power without changing the functional behavior of the circuit.

## Sequential Logic Cloning
Sequential logic cloning is a sequential optimization technique where parts of combinational logic between registers are duplicated (cloned) to reduce fan-out, improve timing, or optimize physical placement, without changing the circuit’s functional behavior.

# Combinational vs Sequential Optimization

| Aspect         | Combinational Optimization             | Sequential Optimization                    |
|----------------|--------------------------------------|--------------------------------------------|
| Focus          | Pure logic simplification             | Registers + logic across cycles            |
| Main Benefit   | Area & power reduction, timing depth | Performance (timing closure, pipelining)  |
| Techniques     | Boolean simplification, const-prop    | Retiming, FSM state reduction, cloning     |
| When Applied   | During logic synthesis                | During advanced synthesis / physical opt  |

## LAB - 06 Combinational Logic Optimization

Simulation
Step 1: Compile Design and Testbench

```bash
iverilog opt_check.v tb_opt_check.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_opt_check.vcd
```

Synthesis with Yosys

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog opt_check.v
```

Step 4: Define the top Module

```bash
synth -top opt_check
```

Step 5: Mapping flip flop

```bash
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

Step 7: View the schematic

```bash
show
```

![WhatsApp Image 2025-09-26 at 3 30 00 AM](https://github.com/user-attachments/assets/bd608826-db98-436d-ab06-966532545e5b)


opt_check2.v


![WhatsApp Image 2025-09-26 at 3 29 59 AM](https://github.com/user-attachments/assets/bc42de47-3822-4b67-a18c-9df68f74e435)


opt_check3.v

![WhatsApp Image 2025-09-26 at 3 29 58 AM](https://github.com/user-attachments/assets/6fc0d437-6ecc-4c4a-a475-5eb2bfc35e8e)


Simulation for multiple_module_opt.v

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog multiple_module_opt.v
```

Step 4: Define the top Module

```bash
synth -top multiple_module_opt
```

Step 5: Use of Flatten

```bash
flatten
```

Step 6: Logic synthesis

```bash
opt_clean -purge
```

Step 7: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 8: View the schematic

```bash
show
```

![WhatsApp Image 2025-09-26 at 3 29 57 AM (1)](https://github.com/user-attachments/assets/cedff470-9a60-4a47-9e7d-5ff74bdecccc)


## LAB - 07 Sequential Logic Optimisation

Simulation

Step 1: Compile Design and Testbench

```bash
iverilog dff_const1.v tb_dff_const1.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_dff_const1.vcd
```

![WhatsApp Image 2025-09-26 at 3 29 57 AM](https://github.com/user-attachments/assets/6395be2c-9e40-4a88-9203-bb1b9c6aa3b5)


Synthesis with Yosys

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog dff_const1.v
```

Step 4: Define the top Module

```bash
synth -top dff_const1
```

Step 5: Mapping flip flop

```bash
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 7: View the schematic

```bash
show
```

![WhatsApp Image 2025-09-26 at 3 29 56 AM](https://github.com/user-attachments/assets/18097ba6-59e7-48e3-a2d2-6dfbff3bff3b)


# dff_const2.v


![WhatsApp Image 2025-09-26 at 3 29 55 AM](https://github.com/user-attachments/assets/2b09e2f5-0769-48c0-95a4-940668c85de7)


![WhatsApp Image 2025-09-26 at 3 29 54 AM](https://github.com/user-attachments/assets/8a1358c2-78c7-464a-8e33-36408feab4fd)


# dff_const3.v


![WhatsApp Image 2025-09-26 at 3 29 52 AM](https://github.com/user-attachments/assets/4f851768-86ee-41c8-a2b2-e1825c78a21d)


![WhatsApp Image 2025-09-26 at 3 29 51 AM](https://github.com/user-attachments/assets/9f0b333b-fabe-4fe1-908d-7bc0b7374d7e)


## 3.4 Sequential Optimisation for Unused Outputs

Sequential Optimization for Unused Outputs is a technique used in digital design synthesis to simplify and optimize circuits by analyzing sequential elements (like flip-flops) whose outputs are not contributing to the final design outputs.

Unused Optimization Lab

Simulation

Step 1: Compile Design and Testbench

```bash
iverilog counter_opt.v tb_counter_opt.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_counter_opt.vcd
```


![WhatsApp Image 2025-09-26 at 3 29 49 AM](https://github.com/user-attachments/assets/0dbd5eb0-b406-446d-a8c7-779e1ab6ec99)


Synthesis with Yosys

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog counter_opt.v
```

Step 4: Define the top Module

```bash
synth -top counter_opt
```

Step 5: Mapping flip flop

```bash
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 7: View the schematic

```bash
show
```

![WhatsApp Image 2025-09-26 at 3 29 48 AM](https://github.com/user-attachments/assets/e9a9e2bc-6be6-4e8d-9137-eba009166388)


Simulation

Step 1: Compile Design and Testbench

```bash
iverilog counter_opt2.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave counter_opt2.v tb_counter_opt2.vcd
```

Synthesis with Yosys

Step 1: Invoke Yosys

```bash
yosys
```

Step 2: read_liberty standard library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 3: Read verilog design

```bash
read_verilog counter_opt2.v
```

Step 4: Define the top Module

```bash
synth -top counter_opt
```

Step 5: Mapping flip flop

```bash
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 6: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Step 7: View the schematic

```bash
show
```














