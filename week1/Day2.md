# Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles
## 1. Introduction to timing .libs


The *sky130_fd_sc_hd__tt_025C_1v80.lib* filename encodes important information such as   

tt → for typical design & synthesis → Represents average (nominal) transistor behavior  
025C → Temperature = 25 °C → Timing and leakage vary with temperature (higher temp → slower transistors, more leakage)  
1v80 → Voltage = 1.8 V → Gate delays depend on supply voltage (higher voltage → faster switching, lower → slower)  

### Library structure analysis
go to the directory where the lib files are
```bash
cd Documets/vsd/sky130RTLDesignAndSynthesisWorkshop/lib
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="851" height="749" alt="Screenshot 2025-09-27 014136" src="https://github.com/user-attachments/assets/c59f8dc9-152e-4b19-a92a-131336a8ddc0" />

I searched for the gate using find feature

<img width="855" height="744" alt="Screenshot 2025-09-27 014210" src="https://github.com/user-attachments/assets/ca9df607-e8bf-488a-9dc6-6f69bae3d537" />



## 2. Hierarchical vs Flat Synthesis

🟦 Hierarchical Analysis Implemetation

You might have multiple .lib files, each describing cells for a different block.

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```
```bash
write_verilog -noattr multiple_modules_hier.v
!gvim multiple_modules_hier.v
```
output 
<img width="1124" height="952" alt="Screenshot 2025-09-27 020207" src="https://github.com/user-attachments/assets/99662997-63d9-46b0-b69d-a089452b0898" />
<img width="811" height="643" alt="Screenshot 2025-09-27 020341" src="https://github.com/user-attachments/assets/8a406cc4-fff0-4be6-8a2a-938853f12736" />
<img width="1137" height="966" alt="Screenshot 2025-09-27 020649" src="https://github.com/user-attachments/assets/1c54dabc-ff76-4c73-be94-8c1bc4e2d8ab" />


🟩 Flat Analysis Implementation

Usually, one merged .lib is used for the whole flattened netlist.

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
flatten
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
```bash
write_verilog -noattr multiple_modules_flat.v
!gvim multiple_modules_flat.v
```

output <img width="1168" height="834" alt="Screenshot 2025-09-27 021113" src="https://github.com/user-attachments/assets/573072ec-cc68-4bb1-a8d7-f791970dc400" />

<img width="1136" height="955" alt="Screenshot 2025-09-27 020825" src="https://github.com/user-attachments/assets/55ba3dde-291f-4ed6-9a8c-36d8d47b7438" />



| Aspect           | **Hierarchical Analysis**                                           | **Flat Analysis**                                 |
| ---------------- | ------------------------------------------------------------------- | ------------------------------------------------- |
| **Structure**    | Keeps the **design hierarchy** (modules/submodules) during analysis | Breaks down **everything into one big netlist**   |
| **View**         | Looks at each block/module separately                               | Sees the entire chip as a single-level circuit    |
| **Speed**        | Faster for large designs — only analyzes modules you care about     | Slower — more gates and nets to analyze at once   |




## 3. Various Flop coding styles and optimizations


### Asynchronous Flip Flop
<img width="850" height="1002" alt="Screenshot 2025-09-27 022812" src="https://github.com/user-attachments/assets/6af1eff8-e8bc-4519-9ce9-5e5f8efd1595" />

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave  ./tb_dff_asyncres.vcd

```
output 
<img width="1126" height="972" alt="Screenshot 2025-09-27 023205" src="https://github.com/user-attachments/assets/75d52c66-b7af-400f-a194-05062ee11703" />

### Asynchronous set


```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave  ./tb_dff_async_set.vcd
```
<img width="623" height="456" alt="image" src="https://github.com/user-attachments/assets/dc5f6b3c-4eee-4f83-b597-b63a65c88ee3" />


### Synchronous Reset

```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave  ./tb_dff_syncres.vcd
```
<img width="626" height="451" alt="image" src="https://github.com/user-attachments/assets/873e9351-27b5-4538-bf26-d21f581255e6" />


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show -format png dff_syncres
```



<img width="839" height="816" alt="Screenshot 2025-09-27 024327" src="https://github.com/user-attachments/assets/773bfc71-80c6-489d-8109-921aec7ecd42" />




##  Some more Optimisations
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mult_2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show -format png mult_2
write_verilog -noattr mul2_net.v
```


```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mult_8
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mul8_net.v
```
