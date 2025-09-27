# Combinational and Sequential optimizations

## Introduction to optimizations

Making a design better that uses less area, less power, or runs faster without changing what it does.

Example (Before Optimization)
```v
assign y = (a & b) | (a & b);
```
Here the same logic is twice — two AND gates and one OR gate.

After Optimization
```v
assign y = a & b;
```
extra logic is removed 




## combinational logic optimizations
Go to the directory
```bash
cd Documents/vsd/sky130RTLDesignAndSynthesisWorkshop/lib
```

### First optimization Check

```bash
gedit opt_check.v
```
<img width="859" height="1006" alt="Screenshot 2025-09-27 101307" src="https://github.com/user-attachments/assets/37202f0c-92c9-4283-ab15-6a75ce9e920a" />

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v 
opt_clean -purge
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="854" height="751" alt="Screenshot 2025-09-27 101637" src="https://github.com/user-attachments/assets/a5b976c4-d9d9-432a-91d4-0be0b22519e5" />

### Second Check

<img width="853" height="1007" alt="Screenshot 2025-09-27 101354" src="https://github.com/user-attachments/assets/f1a58249-31ab-4c81-af9c-adbcd031590d" />

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v 
opt_clean -purge
synth -top opt_check2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="858" height="769" alt="Screenshot 2025-09-27 101846" src="https://github.com/user-attachments/assets/931c5ea7-e58d-4a56-88b7-d2252451275d" />

### Third check

<img width="857" height="767" alt="Screenshot 2025-09-27 102110" src="https://github.com/user-attachments/assets/4bfc1215-63f1-4840-9bee-8f02aa92b86c" />


### Fourth Check

<img width="851" height="730" alt="Screenshot 2025-09-27 102228" src="https://github.com/user-attachments/assets/baaf7c7c-49b0-4f4a-9730-c1f02537fa1f" />


## Sequential logic optimizations
verilog code for dff_const1.v
```v
module dff_const1 (input clk, input rst_n, output reg q);
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n)
            q <= 1'b0;
        else
            q <= 1'b1;
    end
endmodule
```

```bash
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
output
<img width="795" height="739" alt="Screenshot 2025-09-27 102329" src="https://github.com/user-attachments/assets/42c07e4f-70b6-49f9-84f9-668fb5f04973" />


verilog code for dff_const2.v  

```v
module always_high_reg (input clk, input rst_n, output reg q);
    always @(posedge clk or negedge rst_n) begin
        if (!rst_n)
            q <= 1'b1;
        else
            q <= 1'b1;
    end
endmodule
```

output  

<img width="563" height="677" alt="Screenshot 2025-09-27 102456" src="https://github.com/user-attachments/assets/f55fd918-c426-4272-b0ca-0189a45753a3" />



## Sequential optimizations for unused outputs

verilog code for counter_opt.v
```v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

output

<img width="1106" height="705" alt="Screenshot 2025-09-27 102653" src="https://github.com/user-attachments/assets/21f56fc0-9ace-4ee2-ae6d-d3f0fb98f52c" />

### For Counter_opt2
<img width="905" height="519" alt="Screenshot 2025-09-27 103029" src="https://github.com/user-attachments/assets/d2ad097e-6977-416e-b3a6-c0132e84c021" />

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt2.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

output

<img width="1098" height="703" alt="Screenshot 2025-09-27 103316" src="https://github.com/user-attachments/assets/9defe8b6-398d-49b2-8f51-d65086bc0ab4" />



