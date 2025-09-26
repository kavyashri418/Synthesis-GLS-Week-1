# 5 Optimization in Synthesis

## 5.1 If Case Constructs

a) If-else Statement 

- The if statement allows conditional execution of code blocks based on Boolean expressions.
- Syntax allows multiple else if branches and an optional else block.
- Evaluates conditions sequentially from top to bottom.

```bash
if (condition) begin
    // statements if condition is true
end else if (another_condition) begin
    // statements if another_condition is true
end else begin
    // statements if none of the above are true
end
```

b) case Statement

- The case statement provides multi-way selection based on the value of an expression.
- Evaluates the expression once and selects the matching case.
- An optional default block handles unmatched values.

```bash
case (expression)
    value1: begin
        // statements when expression == value1
    end
    value2: begin
        // statements when expression == value2
    end
    default: begin
        // statements when no case matches
    end
endcase

```
## 5.2 Inferred Latches in Verilog
An inferred latch occurs when a combinational block does not specify all output assignments for all possible input conditions. Synthesis tools interpret this as a need to store the previous value, effectively creating a level-sensitive latch.


## LAB - 10 Incomplete If-Case
```bash
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```
Simulation

Step 1: Compile Design and Testbench

```bash
iverilog incomp_if.v tb_incomp_if.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_incomp_if.vcd
```

<img width="1212" height="566" alt="image" src="https://github.com/user-attachments/assets/a60b10f4-a56c-4b8e-a642-769367ea26cc" />


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
read_verilog incomp_if.v
```

Step 4: Define the top Module

```bash
synth -top incomp_if
```

Step 5: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

Step 6: View the schematic

```bash
show
```

<img width="1212" height="728" alt="image" src="https://github.com/user-attachments/assets/25109bc3-41ec-4b5b-949b-420943e324d0" />


incomp_if2.v DUT

<img width="1212" height="519" alt="image" src="https://github.com/user-attachments/assets/45d1ac45-e4c0-49d5-a198-2a974b1908d2" />

<img width="1212" height="625" alt="image" src="https://github.com/user-attachments/assets/4e0cfe15-15a7-4934-99f8-b9e399832a12" />


## LAB - 11 Incomplete Overlapping Case

Simulation

Step 1: Compile Design and Testbench

```bash
iverilog incomp_case.v tb_incomp_case.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_incomp_case.vcd
```

<img width="1212" height="566" alt="image" src="https://github.com/user-attachments/assets/f57d6187-8fba-45dc-883b-992a9f75cdbf" />


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
read_verilog read_verilog incomp_case.v  
```

Step 4: Define the top Module

```bash
synth -top incomp_case
```

Step 5: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

Step 6: View the schematic

```bash
show
```

<img width="1212" height="595" alt="image" src="https://github.com/user-attachments/assets/a366e3a0-428f-49a9-9e8d-6b8d97c99edd" />


comp_case.v DUT
```bash
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```

<img width="1212" height="562" alt="image" src="https://github.com/user-attachments/assets/20a26dc5-177e-4940-aa1e-fd256f1be1b8" />

<img width="1212" height="596" alt="image" src="https://github.com/user-attachments/assets/3e25c443-d21f-4efa-8dc4-a50d1ef30b39" />


bad_case.v DUT

Steps for GLS
```bash
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // '?' is a wildcard; be careful with incomplete cases!
    endcase
end
endmodule
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
read_verilog bad_case.v 
```

Step 4: Define the top Module

```bash
synth -top bad_case 
```

Step 5: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

Step 6: Generate Verilog Netlist

```bash
  write_verilog -noattr bad_case_net.v
```

Step 7: View the schematic

```bash
show
```

<img width="1212" height="596" alt="image" src="https://github.com/user-attachments/assets/02b81f6a-75ba-4f37-9ba7-41b02a21b0ad" />


<img width="1212" height="775" alt="image" src="https://github.com/user-attachments/assets/f5c66fa2-8a11-476d-ae0b-4cf81c947547" />


## 5.3 for Loops in Verilog 
The for loop in Verilog is used to repeat a block of code multiple times, commonly for iterating over vectors, arrays, or registers.

## Lab - 12 Loops and Generate Blocks

```bash
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```

<img width="1209" height="606" alt="image" src="https://github.com/user-attachments/assets/a8861301-2769-4d21-9703-04b83b89fa4b" />



## LAB - 13 Demux using for loop

<img width="1209" height="528" alt="image" src="https://github.com/user-attachments/assets/aa0af01f-2af8-4440-85d6-f78d372b7bbe" />


## LAB - 14 for rcv with generate block

8-bit Ripple Carry Adder 
```bash
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```
Full Adder Module
```bash
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```

Simulation

Step 1: Compile Design and Testbench

```bash
iverilog iverilog rca.v tb_rca.v
```

Step 2: Run simulation

```bash
./a.out
```

Step 3: gtkwave view waveform

```bash
gtkwave tb_rca.vcd
```

<img width="1210" height="515" alt="image" src="https://github.com/user-attachments/assets/776c5d14-c5e7-411c-ab3a-d6c4f9d32257" />


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
read_verilog rca.v tb_rca.v
```

Step 4: Define the top Module

```bash
synth -top rca 
```

Step 5: abc logic synthesis

```bash
  abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```

Step 6: View the schematic

```bash
show
```
<img width="1209" height="800" alt="image" src="https://github.com/user-attachments/assets/4ea79872-737b-45b5-b806-a68da1f22716" />











